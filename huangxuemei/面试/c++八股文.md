### 内存分布情况
- 栈：由编辑器管理分配和回收，存放函数参数和局部变量，空间较小
- 堆：由程序员自己管理，主要通过malloc、new、delete、free进行分配和回收，空间较大
- 全局/静态区：分为未初始化和初始化两个相邻的区域，存储初始化和未初始化的[全局变量](全局变量)和[静态变量](静态变量)
- 常量区：存储常量，一般不允许修改
- 代码区：用于存放程序的二进制代码

### 堆和栈的区别
- 栈：由编辑器管理分配和回收，存放函数参数和局部变量，空间较小，连续的存储空间
- 堆：由程序员自己管理分配，空间较大，不连续的空间

### 申请内存的new/delete和malloc/free的区别
- new和delete是c++关键字，malloc和free是c库函数，new是基于malloc实现的。
- new调用时，先申请内存，再调用构造函数，delete调用时，先调用析构函数，再释放内存，malloc和free只申请和释放内存，所以new比malloc安全。
- new不需要计算申请内存的大小，也不需要强制类型转换，因为new的返回值就是申请内存类型的指针，malloc需要手动计算申请内存的大小，并且进行强制类型转换，因为malloc的返回值是void*（空指针）
- new申请内存之后会进行初始化，malloc不会
- new如果申请内存失败返回NULL，malloc申请内存失败返回异常

### 为什么要设计new/delete
c++是面向对象的，而malloc是c的库函数，不能满足c++动态生成对象的要求，对象在生成和销毁的时候会自动调用构造函数和析构函数，而malloc是库函数不是运算符，不在编辑器的控制权限内，所以不能把构造和析构的任务交给malloc，所以设计了new/delete

### C++中指针参数的传递和引用参数的传递
- 指针参数的传递：形参是一个指向实参的指针，在栈中开辟的内存里存放的是指向实参的指针，当形参的指向没有变化时候，对形参指向的操作就是对实参进行操作。
- 引用参数的传递：形参是实参的一个[引用](指针和引用)，在栈中开辟的内存中存放的是实参的地址，每次对形参的操作，会通过间接寻址，找到实参进行操作。
- 二者的区别：都可以实现在函数中对形参的操作能影响到实参，但是引用参数的传递一旦函数被调用，在函数中形参就一直会影响实参，而不会被改变，因为引用一旦被初始化就不会再被改变，但是指针参数的传递，指针的指向是可以修改的，如果在函数中修改指针的指向，那么就不能再对实参产生影响

### C/C++的编译过程
- 预处理（展开宏定义和[[内联函数]]）
- 编译（编译成汇编语言）
- 汇编（汇编成二进制文件）
- 链接（链接其他所包含的库文件）

### 宏定义和const常量的区别
- 宏定义只是字符串的替换，没有数据类型的区别，也就没有类型安全性检查，const有
- 宏定义是在预处理的阶段进行展开，而const是编译的时候进行处理
- 宏定义是直接替换，没有内存分配，而const有内存分配
- 宏定义是全局定义，而const是局部的

### struct和class的区别
struct的默认[[访问权限]]和[[继承权限]]都是public，class的是private

### 重载、重写、重定义的区别
- 重载：对函数或者运算符进行重载，体现在**函数名和返回值相同**，但是参数不同（**参数的类型、参数的个数、参数的顺序**）
- 重写：在派生类中重新定义父类的[[虚函数]]，其访问权限可以由程序员自己定义
- 重定义：在派生类中对父类的非虚函数进行重新定义，将父类的同名函数（**返回值和参数列表都可以不同**）进行覆盖隐藏，如果想要访问父类的同名函数需要加上父类的定义域。

### [[构造函数]]

### [[析构函数]]

### [[构造函数、析构函数的调用顺序]]
有一个本类C，虚继承A，正常继承B
- 构造函数：A-B-C
- 析构函数：C-B-A

### 面向对象的三大特性
- 封装
- 继承
- 多态：重载，重写，重定义
	- 静态多态：重载
	- 动态多态重写：重写（虚函数）

### 头文件引用中""和<>的区别
- “”引用的是自定义头文件的，<>引用的是系统头文件
- 查找路径不同：
	- <>：编辑器设置的头文件路径->系统变量
	- ""：项目文件的头文件路径->编辑器设置的头文件路径->系统变量

### [[虚函数]]

### 接口和抽象类的区别
- 接口不能为普通方法提供方法体，在接口中的普通方法会默认为抽象方法
- 抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是 public static final 类型的，并且必须赋值，否则通不过编译。
- 接口不能包含构造器，抽象类可以包含构造器。
- 一个类只能继承一个抽象类，而一个类却可以实现多个接口。
**什么时候使用抽象类和接口**
- 如果你拥有一些方法并且想让他们中的一些有默认实现，使用抽象类吧。
- 如果你想实现多重继承，那么你必须使用接口。由于Java不支持多继承，子类不能够继承多个类，但可以实现多个接口。因此你就可以使用接口来解决它。 

