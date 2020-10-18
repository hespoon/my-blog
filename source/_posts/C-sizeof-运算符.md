---
title: C++ sizeof 运算符
date: 2020-10-14 23:14:39
tags:
---



# 功能

计算对象或类型的大小，单位为字节 byte。

被计算大小的对象或类型的大小必须是明确的，不能是不可知的。

# 语法

```cpp
sizeof(type);
sizeof expression;
```

在不同的计算机体系结构下，一个字节占用的 bit 数是不确定的，可能是 8bit 或者更多，具体数值由宏 CHAR_BIT 定义。但是，无论一个 byte 占用多少个 bit，下列表达式的值都是 1。

```cpp
sizeof(char)==sizeof(char8_t)==sizeof(signed char)==sizeof(unsigned char)==sizeof(std::byte)==1
```

sizeof 不能计算函数类型，不完整类型和 glvalues 的大小，强行计算会报错。

当用 sizeof 计算引用的大小时，实际上计算的是被引用的类型的大小。

当用 sizeof 计算一个类的大小时，返回值是一个该类的对象的大小加上将一个该类的对象放入数组中时所要填充的空间的大小。也就是会返回该类的对象在按照字节对齐后的大小。

sizeof 的结果永远不会是 0，即使计算一个空类的大小也不是 0。

当使用 `sizeof expression` 时，sizeof 不会判断 expression 是否是有效的。