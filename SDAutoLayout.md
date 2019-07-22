# SDAutoLayout

## SDAutoLayout 简介

**[SDAutoLayout](https://github.com/gsdios/SDAutoLayout)** 库是通过代码来「自动布局」的第三方库，它的作者是 <i class="fas fa-user-circle"></i> [gsdios](https://github.com/gsdios)。SDAutoLayout 使用 Objective-C 编写，并且它使用**链式编程[^链式编程]**思想实现。

[^链式编程]: 将多个操作（多行代码）通过点语法（.）链接在一起成为一句代码，使代码可读性提高。

### 链式编程

链式编程的特点在于所有方法的调用均可通过一连串的方法调用来实现（即使用点语法调用），可以说只要显示器够宽，就可以在一行内完成对一个控件的所有约束布局。

实现链式编程的关键点在于每个调用的方法的返回值都是一个 Block（代码块），Block 的返回值是这个对象的本身。这样才能保证每次调用完一个方法之后，仍然可以用点语法来继续调用方法。

编程思想的好处不言而喻，代码整洁、增强可读性、减少重复代码。

## SDAutoLayout 使用方法

### SpaceToView 方法

方法名中带有「**SpaceToView**」的方法表示当前视图到某个参照视图的间距，需要传递 **2** 个参数：（UIView）参照 view 和 （CGFloat）间距数值。

> 注意
> 
> 布局当前视图时，若传递的参数是当前视图的父视图，那么参考的就是父视图对应的边的距离，而不是到父视图的间距。

这样的方法，共有4个，分别是：

- **topSpaceToView**
- **bottomSpaceToView**
- **leftSpaceToView**
- **rightSpaceToView**

如下示意图，分别表示当前视图参照不同方向的视图（不是父视图）。

![自动布局用法简析](media/15312732897440/%E8%87%AA%E5%8A%A8%E5%B8%83%E5%B1%80%E7%94%A8%E6%B3%95%E7%AE%80%E6%9E%90.png)

<p style="text-align:center;color:#999999;font-size:14px"> SpaceToView 方法示意图 </p>

### EqualToView 方法

方法名中带有「**EqualToView**」的方法表示当前视图和参照视图的水平对齐或垂直对齐方式，需要传递 **1** 个参数：（UIView）参照视图。

这样的方法共有 6 个，分别是：

- **topEqualToView**
- **bottomEqualToView**
- **leftEqualToView**
- **rightEqualToView**
- **centerXEqualToView**
- **centerYEqualToView**

如下示意图 (1) 中的四个方法分别表示当前视图的边和参照视图边水平对齐或者垂直对齐。

![EqualToView 方法示意图 -1-](media/15312732897440/EqualToView%20%E6%96%B9%E6%B3%95%E7%A4%BA%E6%84%8F%E5%9B%BE%20-1-.png)

<p style="text-align:center;color:#999999;font-size:14px"> EqualToView 方法示意图 (1) </p>

如下示意图 (2) 中的两个方法则是表示当前视图的中心点 X 坐标（中心点 Y 坐标）和参照视图的中心点 X 坐标（中心点 Y 坐标）对齐。

![EqualToView 方法示意图 -2-](media/15312732897440/EqualToView%20%E6%96%B9%E6%B3%95%E7%A4%BA%E6%84%8F%E5%9B%BE%20-2-.png)

<p style="text-align:center;color:#999999;font-size:14px"> EqualToView 方法示意图 (2) </p>

### Is 方法



### RatioToView 方法

方法名中带有「RatioToView」的方法表示视图的宽度或者高度等属性相对于参照视图的对应属性值的比例，需要传递 2 个参数：（UIView）参照视图和（CGFloat）倍数。

<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

