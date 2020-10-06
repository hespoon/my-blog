---
title: C++ 风格的强制类型转换
date: 2020-10-04 18:25:10
tags:
---
# C++ 风格的强制类型转换

强制类型转换运算符是一种特殊的运算符，它把一种数据类型转换为另一种数据类型。

强制类型转换运算符是一元运算符，它的优先级与其他一元运算符相同。

**static_cast\<type>(expression) **

static_cast 可用于各种隐式转换，比如 double 转 int，int 转 unsigned int 等，效果与隐式转换一样，可能有各种奇怪的错误，需要用户保证转换是正确的。

static_cast 可用于基类指针（引用）与派生类指针（引用）之间的相互转换。与 dynamic_cast 不同，在将基类指针转换为派生类指针时，编译器不会检查该基类指针是否指向了一个派生类对象，会强行将基类指针转换为派生类指针，即使这种转换是错误的。因此，使用 static_cast 将基类指针转换为派生类指针时，要保证转换是可行的。

static_cast 可用于将具有显示构造函数（explicit 修饰的构造函数）或者显示函数的类转换为隐式构造函数的类。

**const_cast\<type>(expression)**

用于 const 与非 const、volatile 与非 volatile 之间的转换。

可以将一个非常指针（引用）转换为常指针（引用），也可以将一个常指针（引用）转换为非常指针（引用）。

***const_cast is evil!***

const_cast 可以破坏 const 关键字对参数的限定，使得函数可以修改 const 关键字修饰的参数。

例 1：

```c++
#include <iostream>
void fun(const int *a) // 接收常指针，不能通过指针修改指针指向的值
{
    /*
    * int *b = a; 将常指针赋给普通指针是不合法的，会报错无法编译
    */
    int *b = const_cast<int *>(a); // 将常指针转换为非常指针
    *b = 100; // 修改常指针 a 指向的值
}
int main()
{
    const int *a = new int(10);
    std::cout << "before: " << *a << std::endl;
    fun(a);
    std::cout << "after: " << *a << std::endl;
    return 0;
}
```

输出

```
before: 10
after: 100
```

常指针 a 指向的值被函数 fun 修改了。

例 2：

```c++
#include <iostream>
void fun(const int &a) // 一般情况下 const 关键字表明 fun 不会修改 a 的值
{
    /*
    * int *b = &a; 将常引用赋给普通指针是不合法的，会报错无法编译
    */
    int *b = const_cast<int *>(&a); // 将常引用转换为普通指针
    *b = 100; // 通过指针修改 a 的值
}
int main()
{
    int a = 10;
    std::cout << "before: " << a << std::endl;
    fun(a);
    std::cout << "after: " << a << std::endl;
    return 0;
}
```

输出

```
before: 10
after: 100
```

a 的值被函数 fun 修改了。

如果没有 const_cast，则函数 fun 的参数用 const 修饰后，表明函数 fun 不会也无法修改参数的值，而现在可以通过 const_cast 将常量转换为非常量，导致 const 关键字修饰的参数可以被修改了，使 const 关键字失去了应有的约束力。 

**reinterpret_cast\<type>(expression)**

reinterpret_cast 用于将一种类型的指针强行转换为另一种类型的指针，不管这两个类型是否有关系。

比如 reinterpret_cast 可以将一个指向苹果类的指针转换为指向潜水艇类的指针。明显苹果与潜水艇之间没有一点关系，转换后的指针根本不能用。

reinterpret_cast 也可以实现指针和整形之间相互转换。可以将一个整数转换为指针，也可以将指针转换为整数。指针本质就是一个无符号整数，是指针指向变量的地址。将指针转换为整数仅仅是想表达指针转换后的值是可以存储为 int 类型的数据。

它可以把一个指针转换为一个整数，也可以把一个整数转换为一个指针。

高度危险的转换，这种转换仅仅是对二进制位的重新解释，不会借助已有的转换规则对数据进行调整。

**dynamic_cast\<type>(expression)**

只能用于转换不同类型的指针或者转换不同类型的引用。

转换指针失败时返回空指针。转换引用失败时抛出 bad_cast 异常。

借助 RTTI Run-Time Type Information，用于类型安全的向下转型（Downcasting），有些编译器的 RTTI 默认是关闭的，在使用 dynamic_cast 是要保证编译器的 RTTI 是开启的。

将派生类指针或引用转换为基类指针或饮用时，dynamic_cast 保证转换总是成功的。

一般情况下，用 dynamic_cast 将基类指针（引用）转换为派生类指针（引用）是会报编译错误的，除非基类指针指向了一个派生类对象。

dynamic_cast 在运行时执行转换，转换时会验证有效性。

如果转换未执行，则转换失败，表达式 expression 被判定为 null。

dynamic_cast 执行动态转换时，type 必须是类的指针、类的引用或者 void*。

如果 type 是类指针，则 expression 也必须是类指针；如果 type 是一个引用，则 expression 也必须是一个引用。