---
layout: post
title: Strange Thing About Clang/GCC
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: []
comments: true
---

Consider the following code

```c
#include <iostream>
#include <type_traits>
using namespace std;
 
// base template
template <typename T>
struct what_type
{
  void operator()()
  {
    cout << "T" << endl;
  }
};
 
// specialization 1
template <typename T, size_t N>
struct what_type<T[N]>
{
  void operator()()
  {
    cout << "T[" << N << "]" << endl;
  }
};
 
// specialization 2
template <>
struct what_type<int[0]>
{
  void operator()()
  {
    cout << "int[0]" << endl;
  }
};
int main(void)
{
  int x[] = {};
  what_type<std::remove_reference_t<decltype(x)>>()();
 
  return 0;
}
```

I compiled this with GCC (g++ (Ubuntu 4.9.1-16ubuntu6) 4.9.1) and I get:

```bash
root@machine:~/dev/scratch$ g++ -std=c++1y main.cpp 
root@machine:~/dev/scratch$ ./a.out
T
```

When I use Clang (Ubuntu clang version 3.5.0-4ubuntu2 (tags/RELEASE_350/final) (based on LLVM 3.5.0)) I get:

```bash
root@machine:~/dev/scratch$ clang++ -std=c++1y main.cpp 
root@machine:~/dev/scratch$ ./a.out
int[0]
```

It looks like GCC is wrong here; specialization 2 should have been chosen. Now, when I remove specialization 2, clang produces `T`, which is the same output as GCC.

If I put an assert in the base template member function, both Clang and GCC think that `T = int[0]` and give almost identical messages

```bash
a.out: main.cpp:53: void what_type<int [0]>::operator()() [T = int [0]]: Assertion false’ failed`.
```

So why isn’t specialization 1 selected? (And it makes no difference either if I have a specialization for `T[]` as well as/instead of `T[N]`.)

{: .box-note}
Edit : Zero-sized arrays are illegal in C++, although GCC and Clang both compile them by default, presumably for historical reasons. If I turn on pedantic, both give me a warning.

From *n4296 8.5.1 section 4*:

> An empty initializer list {} shall not be used as the initializer-clause for an array of unknown bound. (note 105) 105) The syntax provides for empty initializer-lists, but nonetheless C++ does not have zero length arrays.

So if GCC/Clang are going to allow zero-length arrays, I think they should be consistent about it and do specialization “correctly”. 

A friend reports that Clang/LLVM 3.3 does what GCC does here and uses the base template. Clang/LLVM 3.5 uses specialization 2, which is “more” correct. I have yet to try with Clang/LLVM 3.6+.

{: .box-note}
Edit - Clang 3.7 produces the same output as clang 3.5.
