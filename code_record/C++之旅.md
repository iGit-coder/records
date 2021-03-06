### C++之旅

------

#### 一、开发环境搭建

1. 系统：Windows10

2. IDE: Visual Studio 2019, 下载地址：社区版即可

   ```shell
   https://visualstudio.microsoft.com/zh-hans/
   ```

#### 二、数据类型、运算符和表达式

1. 语言基本元素：`常量`、`变量`、`类型`、`运算符`、`表达式`  等

2. 术语讲解：

   `STL(Standard Template Library) `  ：标准模板库，包括容器、算法等

#### 三、命名空间

1. 声明/使用

   ```c++
   // 声明
   namespace firstname
   {
       void radius(){
           //...
       }
       
   }
   //使用
   firstname::radius();
   ```

2. 头文件防卫式声明

   ```c++
   #ifndef __HEAD__
   #define __HEAD__
   //...
   #endif
   ```

   

3. 内存泄漏： `程序中已动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。`

4. 继承

   > 子类中书写与父类同名函数(返回值/参数可以不同)，父类方法就会被遮蔽。想要调用父类同名方法：
   >
   > ```c++
   > //方式1：在子类同名方法中使用
   > void Men::samenamefunc(){
   >     Human::samenamefunc();
   > }
   > //方式2：使用子类对象名
   > men.Human::samenamefunc();
   > ```

5. 多态

   > 使用父类引用调用子类方法(重写)
   >
   > ```c++
   > //父类中
   > virtual void eat();
   > //子类中,override主要是为了在重写时，子类保持与父类中的方法完全一样
   > virtual void eat() override;
   > //父类不允许子类重写
   > virtual void eat() final;
   > ```
   >
   > virtual 修改的函数为虚函数，虚函数必须同时有声明和定义（其他普通成员函数可以只有声明), 因为虚函数是在程序调用时才知道具体使用哪个虚函数，所以，必须要有声明和定义，以备编译器随时使用随时存在。
   >
   > 多态下的析构函数调用
   >
   > ```c++
   > //父类指针指向子类对象
   > Parent *p = new Son();
   > delete p;//子类析构函数没有执行
   > //解决方法，将父类中的析构函数定义为虚函数即可。多态的含义就是父类调用子类的方法，而只有虚函数才有多态这么一说。
   > ```

6. 友元函数、类

   > 友元函数本身是一个函数，通过将其声明为某个类的友元函数，他就可以访问这个类中的所有成员(包括成员变量、成员函数)。
   >
   > ```c++
   > friend void func(参数列表);//在类中做这样的声明
   > ```
   >
   > 友元类同样，友元类的成员函数是可以访问类中的所有成员。友元类的关系是由自己的类控制的，例如定义B类是A类的友元类
   >
   > ```c++
   > class B{
   >     void func(A &a){
   >         a.data = 0;
   >     }
   > }
   > class A{
   >   friend class B;//声明B是A的友元类，即使B没有定义也不会报错
   > private:
   >     int data;//执行B的成员方法，可以访问到data
   > }
   > ```
   >
   > 