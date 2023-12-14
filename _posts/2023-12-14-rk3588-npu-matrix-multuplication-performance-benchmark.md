---
layout: post
title: RK3588 NPU Matrix Multiplication Performance Benchmark 
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [LLM]
comments: true
---

The goal is to run a fine-tuned large language model on my RK3588 development board for hacking/red teaming cases.

Right now, it appears that the RWKV model is the simplest to implement. I don't have to figure out a way to make **MultiHeadAttention** function on the NPU, at least. 

However, I found that quantization significantly affects inference speed when working with rwkv.cpp, which performs inference on the RK3588 using only CPU power. In fact, INT4 is roughly four times faster than FP16. Not encouraging. It suggests to me that the bottleneck is in the memory bandwidth. 

In that case, the NPU might not have a significant impact.

=> [rwkv.cpp - INT4/INT5/INT8 and FP16 Inference on CPU for RWKV Language Model](https://github.com/saharNooby/rwkv.cpp)

The first thing I need an answer to is how quickly the NPU can multiply matrices. 

The specification states that the RK3588's NPU can perform `0.5 TFLOPS` at `FP16` when matrix multiplication is used. Although `6TOPS` is mentioned in marketing materials, it only pertains to `INT4` and performs convolution. 

Let's check that against some benchmark data from PyTorch and NumPy.

Initially, with **RKNN2 v1.5.0**. The NPU can only be controlled through ONNX. There is a complete breakdown in the low-level matrix multiplication API. Thus, I created a quick script that multiplies an input by a random matrix to create ONNX models. Next, the RKNN model was exported.

```python
# part of genmatmul.py
def make_matmul(M, N, K):
    matmul_node = helper.make_node(
        'MatMul',
        inputs=['A', 'B'],
        outputs=['C'],
    )
    b_mat = np.random.randn(N, K).astype(np.float32)
    b_node = helper.make_node(
        'Constant',
        inputs=[],
        outputs=['B'],
        value=helper.make_tensor(
            name='B',
            data_type=onnx.TensorProto.FLOAT,
            dims=b_mat.shape,
            vals=b_mat.flatten(),
        ),
    )
    graph = helper.make_graph(
        nodes=[b_node, matmul_node],
        name='matmul',
        inputs=[
            helper.make_tensor_value_info(
                'A',
                onnx.TensorProto.FLOAT,
                [M, N],
            ),
        ],
        outputs=[
            helper.make_tensor_value_info(
                'C',
                onnx.TensorProto.FLOAT,
                [M, K],
            ),
        ],
    )

    model = helper.make_model(graph, producer_name='onnx-matmul')
    onnx.checker.check_model(model)
    model = version_converter.convert_version(model, 14)
    return model
```

I can now load and run the model in RKNN after the files have been generated. My installation uses LAPACK for BLAS, which is fairly slow, according to `np.show_config()`. 

Two Cortex-A73 and six Cortex-A53 CPUs are present in the RK3588. I also tried using TinyGrad when it supported OpenCL. Nevertheless, it appears that TinyGrad lacks the necessary optimization to outperform PyTorch (using the CPU).

![](/assets/img/rknn-vs-numpy-benchmark-matmul.png)

Image: Bar graph comparing RKNN and NumPy matrix multiplication performance

According to the result, RKNN is indeed the fastest in the bunch. However, there's a big caveat, RKNN internally runs at FP16 even though the original ONNX graph is FP32. 

Yet non of the other libraries does matrix multiplication at FP16. Thus this result is more like comparing oranges to tangerines. They are similar, but not quite the same. 

Also, according to the graph, the NPU only starts winning PyTorch and TinyGrad somewhere between `N = 2048` and `4096`. I have to assume this is some overhead in RKNN. Which I really hope they can resolve by fixing the low-level API.

Another question I'm asking myself, *What is the NPU's scalability with matrix size?* I created a number of ONNX models with various matrix sizes in order to respond to this. 

Beginning at 128 and increasing by 128 all the way up to 4096. The measured time for matrix multiplication on the NPU is represented by the blue line. Furthermore, since matrix multiplication is approximately `O(n^3)`, the red line is just the blue line fitted with an exponential curve.

![](assets/img/rknn-matmul-time-curve.png)

Image: Line graph showing the time it takes to run matrix multiplication on the NPU

And the fit log from ROOT:

```bash
****************************************
Minimizer is Minuit / Migrad
Chi2                      =    0.0840229
NDf                       =           29
Edm                       =  5.32403e-09
NCalls                    =           68
Constant                  =     -2.98728   +/-   0.0811982   
Slope                     =  0.000918918   +/-   2.29814e-05
```

For me, this graph shows 2 properties that I'm very intrigued with.
- There's no sudden jump in time. The NPU doesn't seem to cache anything. Likely it does chunked fetches from memory. And a local buffer holds it as long as needed.
- However, there is a few steps in the curve. Maybe they are using different strategies for different matrix sizes.
- For piratical sizes, the NPU takes really little time. Good news for me trying to run RWKV.
- Matrix multiplication doesn't scale well. We will run into issues if we try to widen language models.
  
That's everything I care enough to test. 
