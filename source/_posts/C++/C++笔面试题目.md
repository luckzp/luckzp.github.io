---
layout: post
title: "C++笔面试题目"
date: 2017-09-18 16:42
comments: true
reward: true
tags: 
	- C++
---

### 1.函数指针

a.获取函数地址

```c++
process(think);//passes address of think() to process()
thought(think);//passes return value of think to thought()
```

b.声明函数指针

```C++
double pam(int);//prototype
double (*pf)(int);//pf points to a function that takes
				//one int argument and that 
				//return type double
pf = pam;//pf now points to the pam() function
```

为提供正确的运算符优先级，必须在声明中使用括号将* pf括起。括号的优先级比*运算符高。

```c++
double (*pf)(int);//pf points to a function that returns double
double *pf(int); //pf() a function that return a pointer-to-double
```

注意，pam()的参数列表和返回类型必须与pf相同。如果不相同，编译器将拒绝这种赋值：

```c++
double ned(double);
int ted(int);
double (*pf)(int);
pf = ned;//invalid -- mismatched signature
pf = ted;//invalid -- mismatched return type
```

c.使用指针来调用函数

```c++
double pam(int);//prototype
double (*pf)(int);//pf points to a function that takes
				//one int argument and that 
				//return type double
pf = pam;//pf now points to the pam() function
double x = pam(4);//call pam() using the function name
double y = (*pf)(5);//call pam() using the pointer pf
double y = pf(5);//also call pam() using the pointer pf
```

### 2.虚函数工作原理：

编译器处理虚函数的方法是：给每个对象添加一个隐藏成员。隐藏成员中保存了一个指向函数地址数组的指针。这种数组称为虚函数表(virtual function table，vtbl)。**虚函数表中存储了为类对象进行声明的虚函数地址。**

![](http://ovuyz1070.bkt.clouddn.com/17-9-19/47767513.jpg)

### 3.**结构体内存对齐**

#### **1.什么是内存对齐**

#### 假设我们同时声明两个变量：char a;short b;用&（取地址符号）观察变量a，b的地址的话，我们会发现（以16位CPU为例）：如果a的地址是0x0000，那么b的地址将会是0x0002或者是0x0004。那么就出现这样一个问题：0x0001这个地址没有被使用，那它干什么去了？ 答案就是它确实没被使用。 因为CPU每次都是从以2字节（16位CPU）或是4字节（32位CPU）的整数倍的内存地址中读进数据的。如果变量b的地址是0x0001的话，那么CPU就需要先从0x0000中读取一个short，取它的高8位放入b的低8位，然后再从0x0002中读取下一个short，取它的低8位放入b的高8位中，这样的话，为了获得b的值，CPU需要进行了两次读操作。

但是如果b的地址为0x0002，那么CPU只需一次读操作就可以获得b的值了。**所以编译器为了优化代码，往往会根据变量的大小，将其指定到合适的位置，即称为内存对齐（对变量b做内存对齐，a、b之间的内存被浪费，a并未多占内存）。**

#### **2.结构体内存对齐规则（请记住三条内存规则(在没有#pragam pack宏的情况下）**

结构体所占用的内存与其成员在结构体中的声明顺序有关，其成员的内存对齐规则如下：

（1）每个成员分别按自己的对齐字节数和PPB（指定的对齐字节数，32位机默认为4）两个字节数最小的那个对齐，这样可以最小化长度。如在32bit的机器上，int的大小为4，因此int存储的位置都是4的整数倍的位置开始存储。

（2）复杂类型(如结构)的默认对齐方式是它最长的成员的对齐方式，这样在成员是复杂类型时，结构体数组的时候，可以最小化长度。

（3）结构体对齐后的长度必须是成员中最大的对齐参数（PPB）的整数倍，这样在处理数组时可以保证每一项都边界对齐。 

（4）结构体作为数据成员的对齐规则：在一个struct中包含另一个struct，内部struct应该以它的最大数据成员大小的整数倍开始存储。如 struct A 中包含 struct B, struct B 中包含数据成员 char, int, double，则 struct B 应该以sizeof(double)=8的整数倍为起始地址。

#### **3.实例演示：**

```c++
struct A
{
char a;　　　//内存位置:  [0]
double b;　  // 内存位置: [8]...[15]
int c;　　　　// 内存位置: [16]...[19]　　----　　规则1
};　　　　　　　 // 内存大小：sizeof(A) = (1+7) + 8 + (4+4) = 24, 补齐[20]...[23]　　----　　规则3

struct B
{
int a,　　　　// 内存位置: [0]...[3]
A b,　    　　// 内存位置: [8]...[31]　　----　　规则2
char c,　　　// 内存位置: [32]
};　　　　　　　  // 内存大小：sizeof(B) = (4+4) + 24 + (1+7) = 40, 补齐[33]...[39]
```

*注释：(1+7)表示该数据成员大小为1，补齐7位；(4+4)同理。