# 基本的 C 语言特性

Objective-C 大部分特性来源于 C 语言。  
当为了优化要求使用底层方法，可能会使用 C 语言的内置数据结构，而不是 Objective-C 等内置框架的数据结构。

# 1 数组

gradle[5],读作“gradle sub 5”.

# 2 函数

默认情况下，函数是外部的。即对于函数默认的作用域来说，所有与该函数链接在一起的文件中的所有函数或方法都可以调用它。
关键字 static 可以限制函数的作用域。静态函数只能由和该函数定义位于同一文件的其他函数或方法调用。

```c
static int sum(int a, int b){
    return a + b;
}
```

数组作为函数形参时，传递的是一个指针，它表示数组所在的计算机内存地址。  
`int []` ->`int *`.

# 3 块（block）

- 块是 C 语言的一种扩展，非标准 ANSIC 定义，由 Apple 公司添加到语言中。
- 块看起来像函数：传递参数、返回值。
- 块与函数不同的是：  
  (1) 块定义在函数或方法内部；  
  (2) 能够访问在函数或方法范围内块之外的任何变量。  
  一般来说，这些变量能够访问但并不能够改变这些变量的值；  
  (3) 块可以访问在其范围内定义的值。变量的值同时作为块中定义的值。

- 块的其中一个优势在于能够让系统分配给其他处理器或应用的其他线程执行。  
  块以`^`开头为标识。
- 能够定义块是全局或局部的。

```c
void print_message(void){
    NSLog(@"Hi, This is a block example");
}
```

```c
// 下面的块同样能完成任务。
^(void){
    NSLog(@"Hi, This is a block example");
};
```

```c
// 将块付给n变量 print_message
// 等号左边表示：print_message指向一个没有参数和返回值的块指针。
// 赋值语句以分号结束。
void (^print_message)(void)=
  ^(void){
      NSLog(@"Hi, This is a block example");
  };
```

# 4 结构

结构 vs 类 如何选择？  
若只有数据，没有操作它们的方法，选结构。  
若有数据，还有操作它们的方法，选类。

# 5 指针

指针是间接寻址，它是间接引用。它间接地访问特定数据项的值。  
指针即内存地址。  
在 Objective-C 中，将指针设置指向一些值之前，指针的值是没有意义的。

- 指向
  `&`是地址运算符，用来取得变量的指针。目的是建立指针变量 intPtr 与变量 count 之间的间接引用。

```c
int count  = 10;

// 定义变量intPtr，它是int的指针类型，它可以间接访问一个或多个整型变量的值
int *intPtr;
intPtr = &count;
```

![oc_pointer](https://yingvickycao.github.io/img/ios/oc_pointer.jpg)

- 取值
  `*`是间接寻址运算符。表示通过指针变量 intPtr 引用变量 count 的值。

## 指针和结构体

`->` 或 `(*).` 是结构指针运算符

```c
// 写法1
datePtr->day = 25;
// 写法2
(*datePtr).day = 30;
```

## 指针和方法

按一般方式将指针作为参数传递给方法或函数。也可以让函数或方法返回指针。alloc 和 init 方法就是返回指针。

## 数组和指针

数组名是指向数组的第一个元素的指针。

```c
int *ptr;
// array 和 ptr 是指向数组的第一个元素的指针
ptr = array;
// 等价于
ptr = &array[0];


// x[i] === *(x+i)
array[10] = 27;
// 等价于
*(array + 10) = 27; // array + 10 表示指向 array[10]中包含的值


// 移动数组的指针指向位置
ptr ++;
```

- 对于指向同一个数组的两个指针变量，可以做比较。
- 在函数中，将数组声明为“int 数组”类型，还是“指针”类型？  
  都可以正常工作。  
  If 使用索引来引用数组元素，则为数组。  
  If 作为指向数组的指针，则声明为指针类型。

# 6 联合体（union）

When 使用 ： 当多个数据要共享内存或多个数据每次只取其一时。

联合体是什么？  
(1)联合体是一个结构  
(2)它的所有成员相当于基地值的偏移量都是 0.  
(3)它的对齐方式要适合其中所有成员的自身对齐方式。  
(4)它的结构空间要大到足够容纳最宽的成员。

联合体的空间大小与什么相关？  
大小能容纳最宽成员；  
大小能被其包含的所有基本数据类型的大小所整除。

```c
// 联合体
union U{  // size = 16
    char s[9];  // size = 9
    int n;      // size = 4
    double d;   // size = 8
};

// x >= 9;
// x / 1 = 0;
// x / 4 = 0;
// x / 8 = 0;
// => size = x = 16;
```

# 7 它们不是对象

数组、结构、字符串和联合，它们不是对象：  
不能给它们传递消息；  
不能利用它们获得 Foundation 框架提供的内存分配策略之类的最大优势。

推荐使用 Foundation 带的类，而不是 C 语言中内置的类。只有当真正需要时才使用它。

# 8 其他语言特性

## 复合字面量

复合字面量是包含在括号内的类型名称，之后是一个初始化列表。  
它创建特定类型的未命名值，作用域限于创建它的块。  
通常用于赋值或函数参数传参。

```c
struct date{
int month,
int day,
int year
};
struct date theDate;
// 复合字面值用于赋值
theDate = (struct date){.month=7, .day = 2, .year = 2014};
// 复合字面值用于函数参数传参
setStartDate((struct date){.month=7, .day = 2, .year = 2014});

// 复合字面值用于赋值
int *ptr = (int[10]{[0] = 1},[2] = 50);
```

## goto 语句

goto 语句使得程序难以阅读，因此不推荐使用，不要滥用 goto。

## 空语句

## 逗号运算符

逗号运算符的值是最右边的表达式。

## sizeof 运算符

sizeof 确定数据类型 或 对象 的字节大小。  
sizeof 参数是变量、数组名、基本数据类型名称、对象、派生类数据类型名称 或 表达式。  
使用 sizeof 来避免在程序中计算和硬编码数据大小。

## 命令行参数

命令行参数有什么用？  
(1)让程序请求用户输入相关信息  
(2)在程序执行时为该程序提供信息。

main 函数，指明了程序开始执行的位置。  
main 是在程序开始执行时，由运行时系统调用。当 main 执行完毕时，控制权返回给运行的系统，这样系统就知道程序已经执行完毕了。  
当运行时系统调用 main 函数时，系统向该函数传递 2 个参数。

```c
// argc(argument count): 指明从命令行输入的参数个数

// argv(argument vector): TBD
// argv数组包含了argc+1个字符指针
// 第一个元素是执行程序的名称指针。若你的系统中没有程序名称，则是空串。
// 数组的其他项指向由启动程序执行的命令行所指定的值。
// 数组的最后一个指针argv[argc]规定为空
int main(int argc, const char* argv[]){
}
```

<!-- https://blog.csdn.net/dgreh/article/details/80985928 -->

# 9 工作原理

## 事实 1：实例变量存储在结构中

定义一个新类和它的实例变量时，这些实例变量实际上存放在一个结构中。  
对象实际上是结构，结构的成员是实例变量。

## 事实 2：对象变量实际上是指针

## 事实 3：方法是函数，而消息表达式是函数调用。

方法实际是函数。  
调用方法时，是在调用与接收者类相关的函数。传递给函数的参数是接收者 (self) 和方法的参数。因此，关于传递参数给函数、返回值以及自动和静态变量的规则是一样的。  
Objective-C 编译器通过类名称和方法名称的组合为每个函数产生一个唯一的名称。

## 事实 4：id 类型是通用指针类型

通过指针（即内存地址）来引用对象，因此可以随意将它们在 id 变量之间来回赋值。  
返回 id 类型值的方法只是返回指向内存中某对象的指针。然后可以将该值赋给任何对象变量。因此，即使将它存储在 id 类型的通用对象变量中，也总是可以确定它的类。
