---
layout: post
title: Cpp Main函数
date: 2018-6-08
categories: blog
tags: [cpp,Main]
description: c++中有不同的main函数，main()是控制台程序用的，而GUI界面常用wmain()
---


# Cpp Main Function

c++中有不同的main函数，main()是控制台程序用的，而GUI界面常用wmain()

main() -  Console, ANSI;
wmain() -  Console, UNICODE;
WinMain() -  GUI, ANSI
wWinMain() - GUI, UNICODE




在C/C++编程中，最常见的就是main()函数，这是应用程序的入口，那么 _tmain(), wmain(), wWinMain(), _tWinMain()这些函数又是什么呢？
 
_tmain()是个宏，它需要头文件#include “stdafx.h”的支持，因为头文件stdafx.h中包含了头文件tchar.h，在头文件tchar.h里，微软做了以下宏定义
#ifdef  _UNICODE

#define _tmain      wmain

#define _tWinMain   wWinMain

 

也就是说，wmain()是UNICODE版本的main()， _tmain()是个宏，如果是UNICODE则它是wmain()，否则它是main()，wWinMain()与_tWinMain()的区别也相同；以下是msdn中关于这些函数的描述：

A special function named main is the starting point of execution for all C and C++ programs. If you are writing code that adheres to the Unicode programming model, you can use wmain, which is the wide-character version of main.

在所有C和C++程序中，以一个叫main的特殊函数作为应用程序的入口点，如果你希望在程序中使用Unicode 编码，那么你可以使用wmain，这是main函数的宽字符版本。

You can also use _tmain, which is defined in TCHAR.h. _tmain resolves to main unless _UNICODE is defined. In that case, _tmain resolves to wmain.

同时，我们可以使用_tmain，它在TCHAR.h头文件中定义，如果没有定义_UNICODE，那么_tmain会转换成mian，否则会被转换成wmain。


熟悉C编程的人都知道main函数带有2个参数：arc和argv，完整的main函数定义是：int main(int argc, char *argv[])。argc指示程序启动时命令行参数的个数，argv则包含具体的参数字符串。

　　如果有程序叫“hello.exe”，直接启动时，argc=1, argv[0]=hello.exe。

　　如果以“hello.exe readme.txt”的形式启动，argc=2, argv[0]=hello.exe, argv[1]=readme.txt

　　通过这两个参数，程序可以获知自身在启动时的命令行信息。

　　而在WinMain函数中，带有4个参数，分别是：hInstance, hPrevInstance, lpCmdLine, nShowCmd。今天就探讨hInstance的含义。

　　hInstance是程序的当前实例的句柄。在Windows这样的多任务操作系统中，一个程序可以同时运行多个实例。不同的实例间需要彼此区别，句柄就是干这个的。

hInstance是操作系统分配给实例的指针. 程序根据hInstance访问其相应的内存空间

　　hInstance是操作系统分配给程序自身实例的句柄.句柄是用来标识实例的,句柄是实例指针的索引. 通过句柄能找到实例的地址.

　　HINSTANCE hInstance;是应用程序的实例句柄

　　获取方法 HINSTANCE AfxGetInstanceHandle( );

　　或者AfxGetApp( );

　　得到一个CWINAPP类的指针,其中有该句柄的成员

　　nCmdShow,你有SDK经验就知道,它是主窗口的状态,也就是WinMain(...)的参数之一