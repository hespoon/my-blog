---
title: C++ vector 用法
date: 2019-12-08 16:11:14
tags:
---
1. 声明以及初始化
```C++
vector<int> a; // 声明一个int型的向量a
vector<int> a(10); // 声明一个初始大小为10的向量
int n=10;
vector<int> a(n); // 声明一个初始大小为n的向量
vector<int> a(n,1); // 声明一个初始大小为n且初值都为1的向量
vector<int> b(a); // 声明并用向量a初始化向量b
vector<int> b(a.begin(),a.begin()+3); // 将向量a中的第0个到第2个作为向量b的初始值
int m[]={1,2,3,4,5};
vector<int> a(n,n+5); // 将数组n的前5个元素作为向量a的初始值
vector<int> a(&n[1],&n[4]); // 将n[1]~n[4]范围内的元素作为向量a的初始值
vector<vector<int> > b(10,vector<int>(5)); // 创建一个10*5的int型二维向量 
```
2. size 与 capacity 的区别
size 是指当前向量中所包含的元素的个数，取决于你向向量中添加了多少元素。resize() 函数可以改变 size 的大小。
capacity 是指向量当前使用的空间量，表示当前分配给了该向量多少空间，它总是大于或等于 size。reserve() 函数可以改变 capacity 的大小。