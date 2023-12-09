---
layout: post
title: Porting RWKV to RK3588 NPU
subtitle: I want a low-power and fast local LLM
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [llm]
comments: true
---

{: .box-warning}
Agenda
1 **RK3588 NPU**
2 **RWKV**
3 Attempt 1 - Convert from **ONNX**
4 Attempt 2 - offloading MatMul from **GGML**
5 Conclusion

### RK3588 NPU
- Fixed pipeline convolution processor
- 3 cores
- `6TOPS @ INT8`
- `3TOPS @ FP16`
- HW sepc claims `INT4/INT16/FP32` capablity. No SDK support
- Really is designed for vision models
- Fixed pipeline
- Non programmable

![](/assets/img/npu-core.png)

### rknn-toolkit2
- Rockchip provides **RKNN-toolkit2** to compile **ONNX**
- Then load **RKNN** using Python or C
- Run the model

- Compiles **ONNX** into **RKNN** 
- Compile can crash if graph too complex
- Can only quantize image files as input
- Does not support dynamic input size or graph

### Matmul Performance
- As of **RKNPU 1.5.2**, single **NPU** core only
- Much better then naive or basic tiling
- As fast as **OpenBLAS multi-core**
- `Max K = 2048 @ FP16`

### RWKV - BlinkDL/RWKV-LM
- **RNN** with Transformer performance
- Easy to implement
- MatMul only
- arXiv: 2305.13048

### RWKV
- Channel Mix and Time Mix are just `4 matmul`
- Output of `layer N` goes to `layer N+1`
- ⨁ means element-wise multiplication
- To generate the next token
- Grab the current token.
- Feed into network
- Also use the intermid state
- No multihead attention
- `O(1)` context `O(N)` state size

![](/assets/img/rwkv.png)

### Much simpler images to scale

**Layer in RWKV**

![](/assets/img/layer-inrwkv.png)

**Layer in LLaMA2**

![](/assets/img/layer-in-llama2.png)

### Transformer Self Attention
- **RKNN** cannot do self attention
- Need to multiply 2 computed matrix
- But **RKNN** needs mat B be ordered
- **LLaMA** is impossible

![](/assets/img/llama2.png)

### Attempt 1 - Convert from *ONNX*
Fighting the toolchain

#### tpoisonooo/llama.onnx
- Like I said, **RKNN** can’t do attention
- Also contains **RWKV model**?
- Layers are split into different **ONNX** files

![](/assets/img/llama-oonx.png)

### Converting to *RKNN*
- Embed and head layer works well!
- Crash on mixing layers
- DieHard did not save it. Fuck
- Other compiler bugs..

![](/assets/img/converting-to-rknn.png)

### *ONNX* Graph hacking!
- Disable **FP16 conversion** , else face compiler bug. **RKNN** converts anyway
- Walk **ONNX** graph, find **MatMuls**
- Split graph
- Send a small, compute heavy graph to **RKNN**
- Keep the rest in **ONNX**

{: .box-note}
It works, but *SLOW*

- Very slow. `~500ms/token`, `430M param` 
- Expected, too much overhead

### Attempt 2 - offloading *MatMul* from *GGML*
This actually works

#### saharNooby/rwkv.cpp
- Used **GGML**, **C library** for inference
- **GGML** runs **LLaMA** and **Whisper**
- Support quantizatrion down to 2 bits
- Rockchip fixed their **MatMul C API** in `v1.5.2`
- Compiler still crash

### *GGML* Hacking
- Like Linux, moving target
- A day to learn how **GGML** works
- `ggml_compute_forward_mul_mat_f16_f32`
- Weight `fp16`, input `fp32`
- Allocate handles, memory and reorder during init

### Works, but...
- Slower then **CPU**
- `CPU: 61ms`
- `NPU: 83ms/token (load 53/300%)`
- `CPU load: 50%`
- **25%** load in kernel

### Using Multi *NPU*
- **RK3588** has **3 NPU cores**
- **Matmul** can only use 1
- Manually split along the **N axis** into 2
- Then stich back together
  
![](/assets/img/using-multi-npu.png)

Source: Hulalazz/A-_Guide_-to_Data_Sciecne_from_mathematics/tree/master

### FASTER...
- Now `76ms/token`
- 2 cores, total load 106/300%
- Still slower then **CPU**

### *3 NPU cores + 1 CPU core*
- 65ms/token
- Close. But not CPU speed yet.
- Unstable. Buggy
- Sad

### Tried every trick in the book
- Vectorize `fp32` to `fp16 conversion`
- Fixing overhead from **GGML** hack
- Reduce system call
- Use 3 NPU cores instead of 2 so on
- NO HELP

### Benchmarking
- Should have benchmarked first
- Daammn
- RKNN is extremely fast
- `M = K = N (y axis)`

![](/assets/img/benchmarking.png)

### *GGML* strike back
`M = 1`, `K = 1024`, `N = 1024`, mat B pre-transposed 

- GGML: 0.1ms
- RKNN: 0.2ms
- Issue is `M = 1`. For large M, **RKNN** is faster
- **NPU** needs to be better at **GEMV**
- But **GGML** is optmized for this

### Conclusion
#### RK3588 NPU

- As of `SDK v1.5.2`
- Good **MatMul**, bad at **GEMV**
- Driver too high latency/heavy
- Need larger K support to be usible in **LLM**
- SDK design flaws

{: .box-note}
If you just want any *LLM* to run...

- Add your accelerator to **GGML**
- Only need to support **MatMul**
- Maybe **LayerNorm**
- Enough to run **RWKV**
- Or just have a good **ONNX compiler**

{: .box-note}
If you just want *LLaMA* to run...

- Add accelerator to **GGML**
- Must support **MatMul** without reordering mat B
- Avoid transpose in attention
- Practically **MatMul** must support `K >= 4096`

{: .box-note}
If you want *RWKV* to run fast...

- Support parallel graph walk
- Unlikely full use HW on **Matmul** `K=2048`
- Optimize **GEMV**
- Ideally, `K >= 8192` for large **RWKV**
- Hardware support for **WKV** operation
- Support unconventional and deep op fusion
- `Mul → Add → Mul → Add → MatMul`
- Store intermid on chip

### These should be fusible

![](/assets/img/fusible1.png)

![](/assets/img/fusible2.png)

### Features helpful to *GGML*
- Accelerator can access Vritual Memory
- Run multiple ops at the same time
- Supports `fp32/fp16` multipling with `fp16/int8`
- Low latency driver/runtime
- Async runtime, able to wait for completion

### Single NPU core vs 2

GGML + RKNN 1 core

![](/assets/img/1-core.png)

GGML + RKNN 2 cores

![](/assets/img/2-cores.png)

### 3 NPU cores (split matrix into 4 pieces)

GGML + 3 NPU cores 

`70ms`

![](/assets/img/npu-3-cores.png)
