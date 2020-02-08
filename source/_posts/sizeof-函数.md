---
title: sizeof() 函数
date: 2019-11-02 18:09:33
tags:
---
```C++
int c[10];
int *a = (int *)malloc(10 * sizeof(int));
cout << sizeof(a) << "  " << sizeof(c) << endl;
```
输出为 4  40
int 指针 a 的大小为 4 字节，虽然它指向了一个包含 40 字节的数组！！
数组名 c 的大小为 40 字节！！