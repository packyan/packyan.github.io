---
layout: post
title: C++ 中 char 与 string 转换
date: 2018-3-26
categories: blog
tags: [Cpp,tpyeid]
description: Cpp char 和 string 互相转换，由于一些老旧代码中字符串并没有使用STL中的string，而是const char * 所以在某些时候要进行两种字符串的转换。
---
Cpp char 和 string 互相转换，由于一些老旧代码中字符串并没有使用STL中的string，而是const char * 所以在某些时候要进行两种字符串的转换。

标签（空格分隔）： c++ char string

---
# char 转 string
char[] & char * 可以直接赋值给 string
eg:
```c++
string str;
const char *x = "das";
const char y[] = "asd";
str = x;
str = y;
```
# string 转 char[] || char *

用c++标准库中对字符串的操作
1. 调用string的data()函数 `string str = "asd"; char *p = str.data()`
2. 调用string的c_str()函数 同上
3. 调用string的copy()函数 

```c++
string str="hello"; 
char p[40]; 
str.copy(p,5,0); //这里5，代表复制几个字符，0代表复制的位置
*(p+5)='/0';     //要手动加上结束符
cout < <p;
```

```c++
语法: 
const char *c_str();
c_str()函数返回一个指向正规C字符串的指针, 内容与本string串相同. 
这是为了与c语言兼容，在c语言中没有string类型，故必须通过string类对象的成员函数c_str()把string 对象转换成c中的字符串样式。
注意：一定要使用strcpy()函数 等来操作方法c_str()返回的指针 
比如：最好不要这样: 
char* c; 
string s="1234"; 
c = s.c_str(); //c最后指向的内容是垃圾，因为s对象被析构，其内容被处理
 
应该这样用： 
char c[20]; 
string s="1234"; 
strcpy(c,s.c_str()); 
这样才不会出错，c_str()返回的是一个临时指针，不能对其进行操作
 
再举个例子
c_str() 以 char* 形式传回 string 内含字符串
如果一个函数要求char*参数，可以使用c_str()方法： 
string s = "Hello World!";
printf("%s", s.c_str()); //输出 "Hello World!"
```

1、如果要将string转换为char*，可以使用string提供的函数c_str() ，或是函数data()，data除了返回字符串内容外，不附加结束符'\0'，而c_str()返回一个以‘\0’结尾的字符数组。

2、const char *c_str();
c_str()函数返回一个指向正规C字符串的指针,内容与本string串相同.
这是为了与c语言兼容,在c语言中没有string类型,故必须通过string类对象的成员函数c_str()把string 对象转换成c中的字符串样式.
注意：一定要使用strcpy()函数 等来操作方法c_str()返回的指针
比如：最好不要这样:
char* c;
string s="1234";
c = s.c_str(); //c最后指向的内容是垃圾,因为s对象被析构,其内容被处理
应该这样用：
char c[20];
string s="1234";
strcpy(c,s.c_str());
这样才不会出错,c_str()返回的是一个临时指针,不能对其进行操作
再举个例子
c_str() 以 char* 形式传回 string 内含字符串
如果一个函数要求char*参数,可以使用c_str()方法：
string s = "Hello World!";
printf("%s",s.c_str()); //输出 "Hello World!"
 
 
 
 
 
1、首先必须了解，string可以被看成是以字符为元素的一种容器。字符构成序列（字符串）。有时候在字符序列中进行遍历，标准的string类提供了STL容器接口。具有一些成员函数比如begin()、end()，迭代器可以根据他们进行定位。

注意，与char*不同的是，string不一定以NULL('\0')结束。string长度可以根据length()得到，string可以根据下标访问。所以，不能将string直接赋值给char*。

2、string 转换成 char *

如果要将string直接转换成const char *类型。string有2个函数可以运用。

一个是.c_str()，一个是data成员函数。

例子如下：

```
string s1 = "abcdeg";
const char *k = s1.c_str();
const char *t = s1.data();
printf("%s%s",k,t);
cout<<k<<t<<endl;
```
如上，都可以输出。内容是一样的。但是只能转换成const char*，如果去掉const编译不能通过。

那么，如果要转换成char*，可以用string的一个成员函数copy实现。

```
string s1 = "abcdefg";
char *data;
int len = s1.length();
data = (char *)malloc((len+1)*sizeof(char));
s1.copy(data,len,0);
printf("%s",data);
cout<<data;
```
 

3、char *转换成string

可以直接赋值。

```
string s;
char *p = "adghrtyh";
s = p;
```
不过这个是会出现问题的。

有一种情况我要说明一下。当我们定义了一个string类型之后，用printf("%s",s1);输出是会出问题的。这是因为“%s”要求后面的对象的首地址。但是string不是这样的一个类型。所以肯定出错。

用cout输出是没有问题的，若一定要printf输出。那么可以这样：
printf("%s",s1.c_str())

 
 
 
 
 
 
 
备注：
string::c_str()、string::data()的区别：

const value_type *c_str( ) const;

const value_type *data( ) const;

 

data只是返回原始数据序列，没有保证会用traits::eos()，或者说'\0'来作字符串结束.   当然，可能多数实现都这样做了。   

c_str是标准的做法，返回的char*   一定指向一个合法的用'\0'终止的C兼容的字符串。   
所以，如果需要C兼容的字符串，c_str  是标准的做法，data  并不保证所有STL的实现的一致性。

 

你或许会问，c_str()的功能包含data()，那还需要data()函数干什么？看看源码：
const charT* c_str () const
{

   if  (length () == 0)

        return "";

   terminate ();

   return data ();

}
原来c_str()的流程是：先调用terminate()，然后在返回data()。因此如果你对效率要求比较高，而且你的处理又不一定需要以\0的方式结束，你最好选择data()。但是对于一般的C函数中，需要以const char*为输入参数，你就要使用c_str()函数。

对于c_str() data()函数，返回的数组都是由string本身拥有，千万不可修改其内容。其原因是许多string实现的时候采用了引用机制，也就是说，有可能几个string使用同一个字符存储空间。而且你不能使用sizeof(string)来查看其大小。详细的解释和实现查看Effective STL的条款15：小心string实现的多样性。
另外在你的程序中，只在需要时才使用c_str()或者data()得到字符串，每调用一次，下次再使用就会失效，如：

string strinfo("this is Winter");
...
//最好的方式是:
foo(strinfo.c_str());
//也可以这么用:
const char* pstr=strinfo.c_str();
foo(pstr);
//不要再使用了pstr了, 下面的操作已经使pstr无效了。
strinfo += " Hello!";
foo(pstr);//错误！
会遇到什么错误？当你幸运的时候pstr可能只是指向"this is Winter Hello!"的字符串，如果不幸运，就会导致程序出现其他问题，总会有一些不可遇见的错误。总之不会是你预期的那个结果。

 

 strcpy

C语言标准库函数strcpy，把从src地址开始且含有'\0'结束符的字符串复制  到  以dest开始的地址空间。

