---
layout: post
title: Cpp limits
date: 2018-3-28
categories: blog
tags: [Cpp,limits]
description: Cpp （numeric_limits
---
Cpp （numeric_limits

标签（空格分隔）： limits

---

## #include <limits>
1. numeric_limits是什么？

>（A）《C++标准程序库》：
```c
一般来说，数值型别的极值是一个与平台相关的特性。C++标准程序库通过template numeric_limits提供这些极值，取代传统C语言，所采用的预处理常数。新的极值概念有两个优点，第一是提供更好的型别安全性，第二是程序员可借此写出一些template以核定这些极值。  
```

>（B）MSDN

```c
The template class describes arithmetic properties of built-in numerical types.  

The header defines explicit specializations for the types wchar_t, bool, char, signed char, unsigned char, short, unsigned short, int, unsigned int, long, unsigned long, float, double, and long double. For these explicit specializations, the member numeric_limits::is_specialized is true, and all relevant members have meaningful values. The program can supply additional explicit specializations. Most member functions of the class describe or test possible implementations of float.  

  For an arbitrary specialization, no members have meaningful values. A member object that does not have a meaningful value stores zero (or false) and a member function that does not return a meaningful value returns Type(0).  
```
  
  
  
上面的意思是说：  
  
这个模板类描述了内建类型的数值属性。  
  
C++标准库显式地为wchar_t, bool, char, signed char, unsigned char, short, unsigned short, int, unsigned int, long, unsigned long, float, double, and long double这些类型提供了特化。对于这些类型来说，is_specialized为true，并且所有的相关的成员（成员变量或成员函数）是有意义的。这个模板也提供其他的特化。大部分的成员函数可以用float型别来描述或测试。  
  
对于一个任意的特化，相关的成员是没有意义的。一个没有意义的对象一般用0（或者false）来表示，一个没有意义的成员函数会返回0.  

说白了，它是一个模板类，它主要是把C++当中的一些内建型别进行了封装，比如说numeric_limits<int>是一个特化后的类，从这个类的成员变量与成员函数中，我们可以了解到int的很多特性：可以表示的最大值，最小值，是否是精确的，是否是有符号等等。如果用其他任意（非内建类型）来特化这个模板类，比如string,string怎么可能有最大值？我们从MSDN上可以了解到，这对string，成员变量与成员函数是没有意义的，要么返回0要么为false。  

```c++
#include <limits>  
#include <iostream>  
using namespace std;  
  
int main() {  
    cout << boolalpha;  
  
    cout << "max(short): " << numeric_limits<short>::max() << endl;  
    cout << "min(short): " << numeric_limits<short>::min() << endl;  
  
    cout << "max(int): " << numeric_limits<int>::max() << endl;  
    cout << "min(int): " << numeric_limits<int>::min() << endl;  
  
    cout << "max(long): " << numeric_limits<long>::max() << endl;  
    cout << "min(long): " << numeric_limits<long>::min() << endl;  
  
    cout << endl;  
  
    cout << "max(float): " << numeric_limits<float>::max() << endl;  
    cout << "min(float): " << numeric_limits<float>::min() << endl;  
  
    cout << "max(double): " << numeric_limits<double>::max() << endl;  
    cout << "min(double): " << numeric_limits<double>::min() << endl;  
  
    cout << "max(long double): " << numeric_limits<long double>::max() << endl;  
    cout << "min(long double): " << numeric_limits<long double>::min() << endl;  
  
    cout << endl;  
  
    cout << "is_signed(char): "  
        << numeric_limits<char>::is_signed << endl;  
    cout << "is_specialized(string): "   
        << numeric_limits<string>::is_specialized << endl;  
}  
```

输出为：

```c
max(short): 32767
min(short): -32768
max(int): 2147483647
min(int): -2147483648
max(long): 2147483647
min(long): -2147483648

max(float): 3.40282e+038
min(float): 1.17549e-038
max(double): 1.79769e+308
min(double): 2.22507e-308
max(long double): 1.18973e+4932
min(long double): 3.3621e-4932
```

关于为什么float的最小值竟然是正的？我也存在疑问，从结果中，我们看出，min返回的是float型别可以表示的最小的正值，
而不是最小的float数。
从这个例子中，我们差不多了解到numeric_limits的基本用法。

```c
#include <limits>  
#include <iostream>  
using namespace std;  
  
int main() {  
    cout << boolalpha;  
    // 可以表示的最大值  
    cout << "max(float): " << numeric_limits<float>::max() << endl;  
    // 可以表示的大于0的最小值，其他类型的实现或与此不同  
    cout << "min(float): " << numeric_limits<float>::min() << endl;  
    // 标准库是否为其实现了特化  
    cout << "is_specialized(float): " << numeric_limits<float>::is_specialized << endl;  
    // 是否是有符号的，即可以表示正负值  
    cout << "is_signed(float): " << numeric_limits<float>::is_signed << endl;  
    // 不否是整形的  
    cout << "is_integer(float): " << numeric_limits<float>::is_integer << endl;  
    // 是否是精确表示的  
    cout << "is_exact(float): " << numeric_limits<float>::is_exact << endl;  
    // 是否存在大小界限  
    cout << "is_bounded(float): " << numeric_limits<float>::is_bounded << endl;  
    // 两个比较大的数相加而不会溢出，生成一个较小的值  
    cout << "is_modulo(float): " << numeric_limits<float>::is_modulo << endl;  
    // 是否符合某某标准  
    cout << "is_iec559(float): " << numeric_limits<float>::is_iec559 << endl;  
    // 不加+-号可以表示的位数  
    cout << "digits(float): " << numeric_limits<float>::digits << endl;  
    // 十进制数的个数  
    cout << "digits10(float): " << numeric_limits<float>::digits10 << endl;  
    // 一般基数为2  
    cout << "radix(float): " << numeric_limits<float>::radix << endl;  
    // 以2为基数的最小指数  
    cout << "min_exponent(float): " << numeric_limits<float>::min_exponent << endl;  
    // 以2为基数的最大指数  
    cout << "max_exponent(float): " << numeric_limits<float>::max_exponent << endl;  
    // 以10为基数的最小指数  
    cout << "min_exponent10(float): " << numeric_limits<float>::min_exponent10 << endl;  
    // 以10为基数的最大指数  
    cout << "max_exponent10(float): " << numeric_limits<float>::max_exponent10 << endl;  
    // 1值和最接近1值的差距  
    cout << "epsilon(float): " << numeric_limits<float>::epsilon() << endl;  
    // 舍入方式  
    cout << "round_style(float): " << numeric_limits<float>::round_style << endl;  
}  
```


