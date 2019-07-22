# Objective-C 代码块

---

## 代码块简介

代码块对象通常称为代码块（Block），也叫闭包（Closure）。其本质上和其他的变量类似。不同的是，代码块是存储在一个变量中，并且需要参数和声明的返回类型。使用代码块可以像调用其他标准函数一样，传入参数，并得到返回值。幂符号（\^）是代码块的语法标记。

苹果在 iOS 4 开始引入 Block 来对 C 语言扩展，用来实现匿名函数的特性，Block 可以看作是一种特殊的数据类型，其可以正常定义变量、作为参数、作为返回值。特殊地，代码块还可以保存一段代码（即存储函数体），在需要的时候调用，目前 Block 已经广泛应用于 iOS 开发中，常用于 GCD、动画、排序及各类**回调[^回调]**。

[^回调]: 回调，即回调函数。就是一个通过函数指针调用的函数。如果把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时，这就是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。

## 代码块声明和定义

代码块声明和函数指针类似，只是把函数指针的星号（*）换成了幂符号（\^），在代码块定义的开头位置也需要使用幂符号。和函数一样，代码块的代码要放在花括号（{}）中。如下代码。

```Objective-C
// 函数指针和代码块声明的区别
void (*myFunc)(void);    // 函数指针的声明
void (^myBlock)(void);   // 代码块的声明

// 代码块的声明和定义。注意两个幂符号的位置区别
int (^myBlock)(int num) = ^(int num) { return num + num }; 

int result = myBlock(4); // 代码块声明为变量，所以像函数一样使用，结果为：8
``` 

<p style="text-align:center;color:#999999;font-size:14px"> 代码块声明和定义 </p>

在语句 `int (^myBlock)(int num) = ^(int num) { return num + num; };` 中：

- 第一个 `int` 是块对象的返回类型
- 第二个 `int`表示参数的类型
- `(^myBlock)` 中的 `myBlock` 是块对象名称，和标识符命名规则相同
- `(int num)` 是参数列表，这里有一个整形的参数 `num`，如果是 `void` 表示没有参数
- `^(int num) { return num + num }` 这部分是定义块对象，就是赋给变量 `myBlock` 的值
- `^(int num)` 这也是参数列表，前面要加上幂符号
- `{ return num + num }` 这部分就是块对象的主体部分（可执行代码）

上面的语句看起来有点复杂，但是还可以简化。如果代码块没有参数，也可以省略，但是不能省略括号，所以代码块的结构还可以是这样：

```Objective-C
// 下面两种方式编译器会良性警告，提醒在前面的参数列表加上 void
int (^myBlock)() = ^{return 10;};
int (^myBlock)() = ^(){return 10;};
  
// 这两种是正常书写方式
int (^myBlock)(void) = ^{return 10;};
int (^myBlock)(void) = ^(){return 10;};

// 下面的代码块书写编译无法通过
// int (^myBlock) = ^{return 10;};
// int (^myBlock) = ^(){return 10;};
```
<p style="text-align:center;color:#999999;font-size:14px"> 简化的代码块 </p>

> <i class="fas fa-exclamation-circle"></i> 提醒
> 
> 1. 个人推荐还是不要省略 `void`，这样不仅方便阅读代码，而且不容易会出现错误。
> 2. 定义一个 Block 变量，就相当于定义一个函数。在定义 Block 的地方并不会执行 Block 内部的代码，而当 Block 变量被调用的时候才会调用 Block 内部的代码。
> 但是两者还是有一定区别，函数不能定义在其他函数内部，而 Block 变量可以在函数内部定义。
> 3. 定义在函数外部的 Block 变量其实就是文件级别的全局变量。

## 使用 typedef 关键字

很长的代码块定义语句看着有些复杂，可以使用 typedef 关键字来定义代码块变量。如下

```Objective-C
// typedef 返回值类型 (^代码块名称)(参数列表);
typedef NSString* (^myBlock)(NSString *str);

// 使用代码块变量
myBlock block_1 = ^(NSString *str) { return @"这是一个代码块变量";};
``` 

<p style="text-align:center;color:#999999;font-size:14px"> typedef 关键字定义代码块变量 </p>

## 直接使用代码块

在实际的开发中，使用代码块时通常不需要创建代码块变量，只需要在代码中内联代码块的内容。一般是将代码块作为参数的方法或者函数。

```Objective-C
NSArray *array = [NSArray arrayWithObjects:@"张三", @"李四", @"王五", nil];
NSArray *sortedArray = [array sortedArrayUsingComparator:^(NSString
    *object1, NSString *object2){ return [object1 compare:object2]; }];
```

<p style="text-align:center;color:#999999;font-size:14px"> 直接使用代码块 </p>

## 代码块和变量

代码块是OC的一个特性，除了可执行的代码外，还可能包含变量的自动绑定（使用栈中的内存），或托管绑定（使用堆的内存）。所以一个代码块维护一个状态集（数据），可以在任何时候执行。代码块用来作为回调特别有用。

在多线程，异步任务，集合遍历，接口回调等地方用得比较多。为了性能，代码块变量都是分配在栈上面的，它的作用域就是当前函数。

### 捕获外部变量

代码块非常重要的一个功能就是进行值捕获。代码块在被声明之后会捕获创建点时的状态。代码块可以捕获外部的变量，因为是进行的值传递，所以只能捕获变量的值，而不能修改它（即外部变量会被代码块作为常量获取）。

代码块中定义的内部变量，作用域就像函数中的内部变量。

```Objective-C
int num = 10;
// 可以在代码块内部访问到外部的变量，但是不能修改；    
void (^myBlock)(void) = ^(void){
    // num = 100;  // 如果修改外部变量，报错无法编译
    NSLog(@"num = %d", num);   
};  
myBlock();   // 输出为：num = 10
```

<p style="text-align:center;color:#999999;font-size:14px"> 代码块捕获外部变量 </p>

### __block 类型变量

因为被 Block 捕获的外部变量在代码块的内部是无法修改的，如果想要修改这些变量，那么必须在外部就将它们声明为可修改的。使用关键字 `__block` 声明一个变量是代码块内部可修改的。注意关键字 `__block` 前面是两个下划线。此时的代码块中的这个变量就变成了引用类型。

```Objective-C
__block int num = 10;
void(^myBlock)(void) = ^(void){
    num = 100; 
    NSLog(@"num = %d", num);   
};  
myBlock();   // 输出为：num = 100
```

<p style="text-align:center;color:#999999;font-size:14px"> 代码块修改 __block 类型变量 </p>

> <i class="fas fa-exclamation-circle"></i> 注意
> 
> 不是所有的变量都可以声明为 __block 类型，下面两种变量无法声明为 __block 类型：
> 
> - 没有长度的可变数组
> - 没有包含可变数组的结构体

## 开发中 Block 的使用

### 页面间传值



<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

