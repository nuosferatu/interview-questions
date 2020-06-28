![奇怪的知识增加了](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/奇怪的知识增加了.webp)

# TODO

- Lambda 表达式（C++11）

  语法、特点和优点、为什么用 Lambda 表达式？

- const static 和 static const 的区别

- const 和 volatile

- 移动语义和右值引用（C++11）

- RTTI 机制

- 类型转换运算符（C++11）

- 虚拟内存

- 委托构造函数和继承构造函数（C++11）


# 关键字

数据类型：

![Snipaste_2019-12-19_16-33-29](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/Snipaste_2019-12-19_16-33-29.png)

![Snipaste_2019-12-19_16-33-45](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/Snipaste_2019-12-19_16-33-45.png)

C++ 类型转换有哪些？

（4 种）



## static

static 的作用

【可见性】全局变量对整个文件，以及其它包含了该文件的文件，都可见；而 static 只在局部可见，如果定义成了全局的，那也只在当前文件中可见。

【存储方式】静态区

什么时候用：

- 当需要一个变量为整个类而不是某一个具体对象服务的时候，成员定义为 static，所有变量共享，还不破坏封装性，对外不可见；访问不需要对象名也可以。

- 函数内定义一个 static 变量，可以保留在下一次调用继续使用，而不是在函数结束时释放，比如一个计数器的功能。

## const

const 必须初始化，因为后续不可以更改（赋值）。

函数传参时，为了避免复制，所以形参为引用，但是又为了防止实参被篡改，所以用 const。

返回一个 const 可以防止这个值被用作左值并进行修改。

一个类的常成员函数，可以访问、但不可以修改成员变量；一个常量对象，可以访问到成员变量，但是不可以修改，也不可以访问到非常成员函数（因为这个函数可能会修改成员变量，这显然是 const 不允许的）。

对于指针变量来说，本身可以是一个 const，也可以指向一个 const 变量，也可以兼备，一句话就是 const 离谁近，谁就不可更改。（顶层 const 和底层 const 的区别）

对于引用来说，引用本身就是顶层 const，即引用一经初始化，不可以更改引用指向，所以引用前面加 const 势必是底层 const。

在拷贝/赋值操作中，要确保不会出现一个非常量指向/引用一个常量（顶层 const）的情况，否则错误。

## class 和 struct

默认成员访问控制符，class 是 private，struct 是 public；

默认继承访问权限，class 是 private，struct 是 public。

## union 和 struct



## 指针和引用

指针和引用的区别



## 数组

**关于 sizeof 和数组**

数字前面有额外信息记录数组大小，sizeof 一个数组名是可以求出数组的长度的，但要注意一定是一个数组名，如果一个指针指向了该数组，sizeof 指针名是求不出数组长度的，结果是一个元素的长度，同样的，数组名作为参数传递时，会自动退化成同类型指针，即便这个函数的形式参数被定义成数组形式。

# C++11 新特性

智能指针

shrink_to_fit

默认的方法和禁用的方法（= default 和 = delete）

管理虚方法（override 和 final）

移动语义和右值引用

构造函数的初始化列表

列表初始化器

无序关联容器（[unordered_map](##unordered_map)，[unordered_set](##unordered_set)）

# 类

## C++ 多态的实现机制

虚函数、RTTI：动态绑定（动态的多态）

重载：（静态的多态）

## 重载、覆盖和隐藏

重载：

- 相同的范围，或同一个类中
- 函数名相同
- 参数不同
- virtual 可有可无

覆盖：

- 不同范围，指派生类和基类
- 函数名相同
- 参数相同
- 基类必须是 virtual

隐藏：

- 如果派生类中的函数和基类中的同名，不论参数相同还是不相同，派生类都会隐藏基类中的函数（并不重载哦，重载是同一个范围内的）
- 如果基类的函数是 virtual，那就不隐藏，而是覆盖

## this

显式调用 this 的场合有哪些？

隐式调用 this 的场合有哪些？

## 构造函数、拷贝构造函数

问题：哪些情况会调用拷贝构造函数？

问题：class 都有拷贝构造函数吗？

拷贝构造和赋值运算符的区别。

## 析构函数

问题：析构函数可以抛出异常吗？

不可以，除非再析构函数内部 catch 并处理。

问题：析构函数一般设置为虚函数，为什么？

动态绑定

## 拷贝控制

## 赋值运算符

基本版：

```cpp
A& A::operator=(const A& a) {
    // 1. 避免自身赋值
    if (this == &a)
        return *this;
    // 2. 释放原来的资源
    // 3. 赋值新的资源
    // 4. 返回自身引用 *this，以允许连续赋值
    return *this;
}
```

进阶版：

```cpp
A& A::operator=(const A& a) {
    // 避免自身赋值，如果是自身，直接跳过 if，返回 *this
    if (this != &a) {
        // 新建临时变量，如果分配失败，跳出 if，直接返回 *this
        // 释放原来的资源
        // 赋值新的资源
    }
    // 返回自身引用 *this，以允许连续赋值
    return *this;
}
```







# 内存

## new 和 malloc()

1. 【运算符 vs 函数】new 是运算符，malloc() 是函数
2. 【调用构造函数】new 会调用类型的构造函数（实际上内置类型也可以用 new 来初始化），malloc() 不会调用
3. 【返回值】new 返回一个实际类型的指针，malloc() 返回一个 void\* 指针
4. 【分配失败】malloc() 返回 NULL，new 抛出 bad_alloc 异常
5. 【内存区域】malloc() 分配的内存在堆中，必须用 free 释放，new 分配的内存在自由存储区，至于自由存储区，是一个 C++ 标准中的抽象概念，区别于不同编译器的实现方式，大多数编译器使用 malloc() 来实现 new，new 还可以重载，因此分配区域取决于实现方式。
6. 【覆盖】new 可以被覆盖，malloc() 不可以被覆盖
7. 【size】new 会根据类型自动计算内存的 size，malloc() 需要手动把内存的 size 作为参数传入
8. 【resize】malloc() 分配的内存允许使用 realloc() 函数来 resize，而 new 不能

## 堆、栈的理解

（此处复习一下内存区的结构）

内存分布，Linux 栈大小。

如何知道计算机位数

## \* 虚拟内存

如果使用 new 和 malloc() 分配很大的空间，会怎样？

这个到底和虚拟内存有没有关系？

虚拟内存与物理内存。

malloc 分配的是虚拟内存还是物理内存？

memset 操作 malloc 指针，是操作物理内存还是虚拟内存？



## 字节对齐

最小单位 1 字节，数据成员开始的偏移位置必须是自身大小的整数倍，比如 int 是 4 个字节，它在对象中的偏移只能是 0，4，8，12 等。

好处是什么？

## C++ 对象模型

C++ 类：

- 数据成员
  1. static
  2. non-static
- 成员函数
  1. static
  2. non-static
  3. virtual

简单对象模型：

对象维护一个包含成员指针的一个表，表中都是成员的地址，不区分函数和数据，每个成员占用相同大小的空间，连续。

表格对象模型：

相对于简单对象模型，加入一个间接层，分为数据表和函数表，数据表、函数表、对象形式类似简单对象模型。

C++ 对象模型：

基本存储单位：字节

单继承：

![单继承](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/单继承.jpeg)

class B 继承自 A，继承数据成员 field1，覆盖虚函数 A::m1 为 B::m1，继承 A::m2 和 A::m3，因此 class B 为：数据成员（field1，field2）函数（B::m1，A::m2，A::m3）；

class C 继承自 B，继承数据成员 field1 和 field2，覆盖虚函数 A::m2 为 C::m2，继承 B::m1 和 A::m3，因此 class C 为：数据成员（field1，field2，field3）函数（B::m1，C::m2，A::m3）；

class C 的对象 var1 存储了自上而下继承而来、以及自己的数据成员，这前面还有一个虚表指针，指向 class C 的虚函数表；虚函数表的开头是一个 type_info，后面依次是各个虚函数。

class C 的对象 var1 的其它非虚成员中，non-static 的函数在代码区，static 的数据成员和函数在静态/全局变量区。

多继承：

![Cpp多继承](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/Cpp多继承.jpg)

## \* RTTI 机制

虚表中，偏移为 -4 的位置还有一个 typeinfo*，指向该对象的 Typeinfo 对象，用于在运行时获取一个基类指针指向的对象的实际类型（typeid 运算符），还用于 dynamic_cast 指针类型转换。

为什么需要 RTTI 机制？哪些地方需要用到它？

RTTI 机制包含了哪些元素？

（见 *C++ Primer Plus*）

如图，typeinfo* 在虚表头部往前偏移 -4 的位置：

![impl-linux-gplusplus](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/impl-linux-gplusplus.png)

## \* 类型转换运算符

区别于 C 语言的通用类型转换。

- dynamic_cast

  运行时动态类型转换，用于类层次中的上下行转换（主要是 is-a 关系），不可以安全转换的时候，返回 0。

- const_cast

  用于去掉 const 限制，修改一个const 引用或底层 const 指针。

  > 注意：则要求该值本来就是变量，只是后来被加了 const 限制，如果本来就是 const，那可能修改不成功，结果未定义。

- static_cast

  不进行动态类型检查，因此基类指针和派生类指针之间正反都可以转换，但不一定安全。

- reinterpret_cast（比较危险）





函数的底层调用是如何实现的

一个对象如何确定它的成员函数的偏移量

字节流的体现，如何读取字节流

## 虚表结构

sizeof 一个有虚函数的类

## 虚继承

为了解决菱形继承的问题。

以单继承为例：

```c++
struct A {
    virtual foo();
    int X;
}
struct B : virtual public A {
    virtual foo();
    virtual fooB();
    int Y;
}
```

B 虚继承自 A，和普通继承不同的是，B 的对象模型中，先是 B 的内容，然后才是 A 的内容，具体如图：

![1520393159326](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/1520393159326.png)

其中，虚表指针 B::vptr 指向 B 类的虚表，一般在对象开始处（但是偏移并不是 0，而是 -4），vbptr 指向 B 的 virtual base table，只有虚继承一个类时才有该项，vbptr 的内容第一项是与本类（B）虚表的偏移（如果有虚函数偏移为 -4，如果没有虚函数偏移为 0），第二项开始是与本类各个虚基类的偏移，有几个虚基类就有几项（注意，非虚基类不在这里，非虚基类也就是普通继承，就在 B 的上面），这个例子中，B 只有一个虚基类 A，偏移是 8（注意并不是 12，这是因为虚表指针 B::vptr 在对象偏移为 -4 的位置，而不是 0）。

菱形继承：

```c++
struct A
{
    A(int v=100):X(v){};
    virtual void foo(void){};
    int X;
};
struct B :virtual public A
{
    B(int v=10):Y(v),A(100){};
    virtual void foo(void){};
    virtual void fooB(void){}
    int Y;
};
struct C : virtual public A
{
    C(int v=20):Z(v),A(100){};
    virtual void foo(void){};
    virtual void fooC(void){};
    int Z;
};
struct D : public B, public C
{
    D(int v =40):B(10),C(20),A(100),L(v){};
    virtual void foo(void){};
    virtual void fooD(void){};
    int L;
};
```

基类 B 和 C 都虚继承了 A，然后 D 再多继承了 B 和 C。

A 的内存布局很简单，一个虚表，如图：

![1520393228683](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/1520393228683.png)

C 和 B 的内存布局类似，前面有图。

D 的比较复杂，如图：

![1520393848842](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/1520393848842.png)

D 类多继承自 B 类和 C 类，先从 B 开始，复制 B 类的虚表（这也将是 D 类的主要虚表）和 B 类的数据成员，由于 B 虚继承自 A，所以 A 的子对象要放在后面，B 的对象有一个 vbptr，记录了虚基类 A 的偏移位置；接下来继承 C，和 B 类似，C 也虚继承自 A，A 也要放在后面，并且虚基类的子对象不可重复，因此是同一个 A 子对象；最后是 D 类自己的数据，D 类独有的虚函数放在主要虚表中，也就是 B 类虚表部分。这里会按照继承顺序发生一些覆盖。



# 高级特性

## 智能指针

问题：智能指针 shared_ptr 和 unique_ptr 生命周期有什么不同？

问题：智能指针 shared_ptr 和 unique_ptr 有什么区别？

问题：unique_ptr 的适用场景是什么？

（关于资源所属权转移的问题）

**问题：如何解决智能指针 shared_ptr 的循环引用问题？**

循环（交叉）引用是两个对象内部都有一个智能指针 shared_ptr 指向对方，这样导致两个对象的引用计数都是 2（一个是创建时的智能指针，一个是对方内部的一个引用），对象生命周期结束时，引用计数都 -1 为 1，并不能销毁，于是造成了内存泄漏。

如果对象 p1 生命周期结束（可以使用 weak_ptr 获取一个对象内部对另一个对象的 shared_ptr 引用），引用计数 -1 为 0，p1 销毁完成的前提是其内部引用的 p2 先销毁，于是 p2 引用计数 -1 为 0，于是 p2 也要销毁，p2 销毁完成的前提又是其内部引用的 p1 先销毁，p1 需要 p2 销毁了才能销毁，于是陷入死循环。

解决方法：

方法一：创建对象的时候使用强智能指针 shared_ptr 指向对象，而类内对其它对象的引用使用弱智能指针 weak_ptr。这样，两个互相引用的对象都只有一个引用计数（weak_ptr 只观察 shared_ptr 指向的内存，而不增加引用计数），对象生命周期结束时，引用计数 -1 为 0，均可正常销毁。

方法二：在引用计数为 1 时，手动打破循环。可以使用 weak_ptr 获取一个对象内部对另一个对象的 shared_ptr 引用，赋值给一个新的 shared_ptr，这时候引用计数 +1 为 2，然后这个对象对另一个对象内部的引用，引用计数 -1 为 1，这时候，循环就打破了，新的 shared_ptr 指向一个对象，该对象内部又指向另一个对象，使用新的 shared_ptr 销毁一个对象，该对象内部指向另一个对象的 shared_ptr 也会 -1，于是另一个对象的引用计数也 -1 为 0，率先销毁，此后，前面的对象也完成了销毁。

图：shared_ptr 和 weak_ptr 示意图

![智能指针的正确实践- 知乎](https://aliyun-oss-pic-bucket.oss-cn-beijing.aliyuncs.com/img/v2-c8f24db60b58fcb776f17f07940b421b_1440w.jpg)

## 异常处理



## \* 移动语义和右值引用

问题：右值引用是什么？它的设计目的是什么？

引用折叠，移动语义，完美转发

# STL

## vector

vector 底层数据结构、扩容机制、释放内存方式（全部、部分）

capacity 和 size，resize，shrink_to_fit

满了以后，扩大至 2 倍或 1.5 倍（MSVC）。

## map

map 快速清空方式（不用 remove 的话）

map 和 unordered_map 有什么不同？（从底层数据结构、有序性、插入和删除的时间复杂度来说）

## unordered_map



## unordered_set



## deque

deque 底层数据结构



## 迭代器

问题：各类容器的迭代器失效



# Boost

在内存池新建对象，重载 new

# a b c

