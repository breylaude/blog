---
layout: post
title: C++ Challenge - Programming without If/Else/For/While and so on...
subtitle: 
thumbnail-img: /assets/img/challenge-accepted.png
share-img: /assets/img/challenge-accepted.png
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [rant, politics]
comments: true
---

Someone recently challenged me to develop a C++ game without utilizing any branching or looping expressions. It's really not that difficult. I could write a game in C, compile it using the `M/o/Vfuscator`, and then deconstruct it and insert all of the assembly instructions into my C++ code. 

{: .box-note}
Bob is your uncle, and the challenge is complete.

I intend to implement the forbidden keywords in such a way that they appear to be the original term.

To build, the following code requires a compiler that supports **C++14**. 

I'm running **GCC 6.3.1** and **clang 3.9.1** (both of which support **C++17**). If you're using another compiler, utilize Google to find out what compilation flags you'll need and whether your compiler supports **C++14**.

### If
Let’s start with implementing `if`. The closest syntax using a function to mimic if would be passing a bool value and 2 lambdas. One for the first branch and the other for the other branch.

```c
If(me.isHot(),[]()
{
    cout << "True story." << endl;
},[]()
{
    cout << "No, I am hot." << endl;
})
```

The only way I could came up to implement it is by using a jump table. But you can’t make an array of lambdas in C++ since every lambda have a different type event they have the same signature(returns the same type and takes in the same types of parameters).

The solution is `std::function` . It can take in any callable and turn it into a function.

So, the implementation ends up looking like this

```c
template<typename TFunc, typename FFunc>void If(bool condition, TFunc tFunc,
FFunc fFunc)
{
    std::function<void()> jumpTable[2] = {fFunc,tFunc};
    jumpTable[static_cast<int>(condition)]();
};
 
template<typename TFunc>void If(bool condition, TFunc tFunc)
{
    If(condition, tFunc, [](){});
};
```

### While
To do almost everything, we need a loop. And the while loop is the most basic. Starting with the implementation. 

It is simple to implement using `If` from above and recursion.

```c
template<typename ConditionFunc, typename Func>
void While(ConditionFunc conditionFunc, Func func)
{
    auto compFunc = [&]() -> bool {return conditionFunc();};
    If(compFunc(),
    [&](){
        func();
        While(conditionFunc, func);
    });
}
```

This gives up the ability to do a basic loop. Such as

```c
int i;
While([&](){return i<10;},[&]()
{
    cout << i << endl;
    i++;
});
```

### For
Looking a the for loop’s [documentation](http://en.cppreference.com/w/cpp/language/for). This is the loop we use a lot in C++. 

Here is the implementation I came up with

```c
template<typename InitFunc, typename ConditionFunc , typename ItFunc, typename Func>
void For(InitFunc initFunc, ConditionFunc conditionFunc
, ItFunc itFunc, Func func)
{
    initFunc();
    While(conditionFunc, [&]()
    {
        func();
    itFunc();
    });
}
```
{: .box-warning}
Yeah...Using 4 lambdas as parameters for a function is bad. It look trash. This is how the for loop looks like. I'm seeing that I’ll end up using the while loop a bit more in the game.

```c
int i;
For([&](){i=0;},[&](){return i<10;},[&](){i++;},[&]()
{
    cout << i << endl;
});
```

[Here](https://gist.github.com/breylaude/6649277ea332b200dbfc04e062f801c1) is the implementation of all the functions above and demo.

Unfortunately, I don’t have time to write a game out of that. I might write it in the future.

Edit:

Someone points out that I don’t have switch implemented! I totally forget about it.
The implementation isn’t difficult. Variadic templates does the trick.

```c
template 
void Switch(const VType& val)
{
}

template 
void Switch(const VType& val, Func func)
{
    func();
}

template 
void Switch(const VType& val, const VType& arg, Func func , Args ... args)
{
    If(val == arg,[&](){
        func();
    },[&](){
        Switch(val, args ...);
    });
}
```

Ex.

```c
int main()
{
    int num = 42;
    Switch(num,
        42, [](){
            std::cout << "Meaning of life" << std::endl;
        },
        60, [](){
            std::cout << "How many seconds in a minute" << std::endl;
        },
        [](){
            std::cout << "nothing" << std::endl;
        }
    );
}
```

The first parameter is the switch statement's input value. Then there are value and callable pairs. When the value and the input value are the same. The associated callable is invoked. 

A callable without a value can also be added at the end of the parameter list. It will then behave as if it were a `default statement`. It is called when none of the preceding values match.
