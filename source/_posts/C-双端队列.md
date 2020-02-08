---
title: C++双端队列
date: 2019-10-03 11:12:48
tags:
---
* 头文件 
* #include <deque\>
* 常用方法
* a.push_front(0); // 在头部加入数据0
* a.push_back(11); // 在尾部加入数据11
* a.pop_front(); // 在头部删除数据
* a.pop_back(); // 在尾部删除数据
* a.resize(num); // 重新指定队列的长度
* a.size(); // 返回容器中实际数据的个数
* a.max_size(); // 返回容器中最大数据的数量
```C++
#include <iostream>
#include <algorithm>
#include <vector>
#include <deque>
using namespace std;
int main()
{
    deque<int> a(10); // 创建一个有10个元素的双端队列a，初始值都为0

    // 赋值
    for (int i = 0; i < a.size(); i++)
    {
        a[i] = i;
    }

    // 按下标访问
    for (int i = 0; i < a.size(); i++)
    {
        cout << a[i] << " ";
    }

    cout << endl;
    // 在队头插入新元素
    cout << "在队头插入新元素100\n";
    a.push_front(100);

    for (int i = 0; i < a.size(); i++)
    {
        cout << a[i] << " ";
    }

    cout << endl;
    // 在队尾插入新元素
    cout << "在队尾插入新元素11\n";
    a.push_back(11);

    for (int i = 0; i < a.size(); i++)
    {
        cout << a[i] << " ";
    }

    cout << endl;
    // 删除队头元素
    cout << "删除队头元素\n";
    a.pop_front();

    for (int i = 0; i < a.size(); i++)
    {
        cout << a[i] << " ";
    }

    cout << endl;
    // 删除队尾元素
    cout << "删除队尾元素\n";
    a.pop_back();

    for (int i = 0; i < a.size(); i++)
    {
        cout << a[i] << " ";
    }

    cout << endl;
    // 更改队列大小
    cout << "更改队列大小\n";
    a.resize(11);

    for (int i = 0; i < a.size(); i++)
    {
        cout << a[i] << " ";
    }

    cout << endl;
    // 获取容器中最大数据的数量
    cout << "a.max_size() = " << a.max_size() << endl;
    getchar();
    return 0;
}

```