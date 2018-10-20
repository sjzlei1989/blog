---
title: C++ 11中的lambda表达式解析
tags: [c++,cpp,c++11,lambda]
categories: [c++]
date: 2018-06-20 11:02:40
---
C++11中的lambda表达式规范如下：
```cpp
[capture](params)mutable exception attribute->ret {body}
[capture](params)->ret {body}
[capture](params) {body}
[capture] {body}
```
<!--more-->
其中

(1) 是完整的 lambda 表达式形式，

(2) const 类型的 lambda 表达式，该类型的表达式不能改捕获(“capture”)列表中的值。

(3)省略了返回值类型的 lambda 表达式，但是该 lambda 表达式的返回类型可以按照下列规则推演出来：如果 lambda 代码块中包含了 return 语句，则该 lambda 表达式的返回类型由 return 语句的返回类型确定。如果没有 return 语句，则类似 void f(…) 函数。

(4)省略了参数列表，类似于无参函数 f()。

mutable 修饰符说明 lambda 表达式体内的代码可以修改被捕获的变量，并且可以访问被捕获对象的 non-const 方法。

exception 说明 lambda 表达式是否抛出异常(noexcept)，以及抛出何种异常，类似于void f() throw(X, Y)。

attribute 用来声明属性。

另外，capture 指定了在可见域范围内 lambda 表达式的代码内可见得外部变量的列表，具体解释如下：

1、[a,&b] a变量以值的方式被捕获，b以引用的方式被捕获。

2、[this] 以值的方式捕获 this 指针。

3、[&] 以引用的方式捕获所有的外部自动变量。

4、[=] 以值的方式捕获所有的外部自动变量。

5、[] 不捕获外部的任何变量。

此外，params 指定 lambda 表达式的参数。

一个具体的 C++11 lambda 表达式例子：
```cpp
#include <vector>
#include <iostream>
#include <algorithm>
#include <functional>

int main()
{
    std::vector<int> c { 1,2,3,4,5,6,7 };
    int x = 5;
    c.erase(std::remove_if(c.begin(), c.end(), [x](int n) { return n < x; } ), c.end());

    std::cout << "c: ";
    for (auto i: c) {
        std::cout << i << ' ';
    }
    std::cout << '\n';

    // the type of a closure cannot be named, but can be inferred with auto
    auto func1 = [](int i) { return i+4; };
    std::cout << "func1: " << func1(6) << '\n'; 

    // like all callable objects, closures can be captured in std::function
    // (this may incur unnecessary overhead)
    std::function<int(int)> func2 = [](int i) { return i+4; };
    std::cout << "func2: " << func2(6) << '\n'; 
}
```