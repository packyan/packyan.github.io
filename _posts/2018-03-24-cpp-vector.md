---
layout: post
title: Cpp STL vector
date: 2018-3-24
categories: blog
tags: [Cpp,vector]
description: Cpp STL vector
---
标准模板库 vector使用方法

标签（空格分隔）： vector c++

---
## 1. 初始化

```c
vector<int>::iterator int_ite;
vector<string>::iterator string_ite;

//vector<T> v(n,i)形式，v包含n 个值为 i 的元素
vector<int> v(10,i);

//vector<T> v(v1)形式，v是v1 的一个副本
 vector<int> ivec1(ivec);
 
 //vector<T> v(n)形式，v包含n 个值初始化的元素
 vector<int> ivec2(10);

 //vector<T> v(n)形式，v包含n 个值初始化的元素
 vector<string> svec(10);

 //数组初始化vector
 int iarray[]={1,2,3,4,5,6,7,8,9,0};
 //count： iarray数组个数
 size_t count=sizeof(iarray)/sizeof(int);
 //int数组初始化 ivec3
 vector<int> ivec3(iarray,iarray+count);


 //string数组初始化 svec1
 string word[]={"ab","bc","cd","de","ef","fe"};
 //s_count： word数组个数
 size_t s_count=sizeof(word)/sizeof(string);
 //string数组初始化 svec1
 vector<string> svec1(word,word+s_count);

//用 back_inserter 函数
 vector<int> ivec4; //空对象
 fill_n(back_inserter(ivec4),10,3); //10个3 填充ivec4.
}
```

## 2. 基本操作
1. 头文件
`#include<vector>`
2. 创建vector对象
`vector<int> vec;`
3. 尾部插入元素
`vec.push_back(number);`
4. 使用下标访问元素（类似数组）
`vec[0]`
5. 使用迭代器 iterator
`for(vector<int>::iterator it = vec.begin(), it_end = vec.end(); it != it_end; ++it)
`
6. 插入元素
`vec.insert(vec.begin() + i,a);
//在i+1个元素前面插入a`
7. 删除元素
`vec.erase(vec.begin()+2);`
8. vector大小：`:vec.size();`
9. 清空vector：`vec.clear();`


---
## 3.Algorithm.h 算法
使用`#include<algorithm>`可以使用vector自带算法。
- 元素翻转：`reverse(vec.begin(),vec.end());`
- 元素排序：`sort(vec.begin(),vec.end());`
默认是从小到大排序，但是可以用自定义比较函数的方法排序
```c++
bool Comp(const int &a,const int &b)
{
    return a>b;
}
sort(vec.begin(),vec.end(),Comp);
```









