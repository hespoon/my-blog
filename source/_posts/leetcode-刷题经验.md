---
title: leetcode 刷题经验
date: 2020-02-01 16:20:40
tags:
---
# C++ 不要用局部静态变量。在多个测试用例的调用过程中，局部静态变量只会被初始化一次，导致测试用例之间的结果相互影响，出现 bug。
# 想要所有的递归函数使用同一个变量有三种方法，全局变量，局部静态变量，参数传引用。
# string 转 int
1. 使用 C++ 11 中的全局函数 std::to_string
```C++
#include <string>
string to_string(int);
string to_string(long);
string to_string(long long);
string to_string(unsigned);
string to_string(unsigned long);
string to_string(unsigned long long);
string to_string(float);
string to_string(double);
string to_string(long double);
```
2. 使用 sstream 中定义的字符串流对象来实现
```C++
ostringstream os; // 构造一个输出字符串流，流的内容为空
int i=12;
os<<i; // 向输出字符串流中输入 int 型整数 i 的内容
cout<<os.str()<<endl;
```
# C++ stringstream 使用
C++ 的输入输出分为三种
1. 基于控制台的 I/O

|头文件|类型|
|---|---|
|iostream|istream 从流中读取、ostream 写到流中去、iostream 对流进行读写|

2. 基于文件的 I/O

|头文件|类型|
|---|---|
|fstream|ifstream 从文件中读取、ofstream 写到文件中去、fstream 对文件进行读写|

3. 基于字符串的 I/O

|头文件|类型|
|---|---|
|sstream|istringstream 从 string 对象中读取、ostringstream 写到 string 对象中去、stringstream 对 string 对象进行读写|

ostringstream、istringstream、stringstream 这三个类包含在 sstream.h 文件中。
istringstream 类用于执行 C++ 风格的串流的输入操作。
ostringstream 类用于执行 C++ 风格的串流的输出操作。
stringstream 类同时支持 C++ 风格的串流的输入输出操作。
* istringstream 类
从字符串中提取数据，支持 >> 操作。
```C++
istringstream::istringstream(string str); // 构造函数原型
istringstream istr("1 56.3"); // 初始化一个 istringstream 对象
istr.str("1100 2.3"); // 把字符串写入 istr 中。可以使用分界点获取不同的数据，完成字符串到其他数据类型的转换。
istr.str(); // 使 istringstream 返回一个字符串
// 举例 把字符串转换为其他数据类型
#include <iostream>   
#include <sstream>   
using namespace std;  
int main()  
{  
    istringstream istr("1 56.7");  
  
    cout<<istr.str()<<endl;//直接输出字符串的数据 "1 56.7"   
      
    string str = istr.str();//函数str()返回一个字符串   
    cout<<str<<endl;  
      
    int n;  
    double d;  
  
    //以空格为界，把istringstream中数据取出，应进行类型转换   
    istr>>n;//第一个数为整型数据，输出1   
    istr>>d;//第二个数位浮点数，输出56.7   
  
    //假设换下存储类型   
    istr>>d;//istringstream第一个数要自动变成浮点型，输出仍为1   
    istr>>n;//istringstream第二个数要自动变成整型，有数字的阶段，输出为56   
  
    //测试输出   
    cout<<d<<endl;  
    cout<<n<<endl;  
    system("pause");  
    return 0;  
}  
```

