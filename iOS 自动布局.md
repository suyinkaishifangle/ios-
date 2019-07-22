# iOS 自动布局

https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW1  未完成。

----

「**自动布局**」是根据对视图的约束，动态计算视图层次结构中所有视图的大小和位置。

例如，约束一个按钮，使其与图像视图水平居中，并使按钮的上边缘始终保持在图像底部下方 8 pt。如果图像视图的大小或位置发生变化，则按钮的位置会自动调整以匹配。

这种基于约束的设计方法允许开发者构建动态响应内部和外部变化的用户界面。

- 外部变化

    父视图的大小或形状发生变化时会发生外部变化。每次变化时，都必须更新视图层次结构的布局，以便最好地利用可用空间。常见的是外部变化比如：
        
    - 设备旋转
    - 不同大小的屏幕的设备

- 内部变化

    当用户界面中的视图或控件的大小发生变化时，会发生内部变化。常见的外部变化比如：
    
    - 应用程序显示的内容会发生变化
    - 应用程序支持国际化（不同的文字等长短不同）
    - 应用程序支持动态类型

## 苹果提供的布局方式及问题

苹果提供了几种布局方式，但是对于开发者来说这些方式目前都不够友好。

布局用户界面有三种主要方法:

- 使用视图的 frame（编程方式）
- 使用自动调整遮罩（autoresizing masks）
- 使用自动布局（Auto Layout）

### 使用视图的 frame（编程方式）

frame 属性是视图的属性，用来设置视图相对于父视图的位置和视图自身的大小。要布置用户界面，必须计算视图层次结构中每个视图的 frame 值。但是随着 iOS 设备屏幕尺寸越来越多，使用视图的 frame 属性来布局已经不能满足开发需求。

因为 frame 属性是以屏幕为坐标系来确定视图的点。如果屏幕的大小不同，那么这个坐标系的大小也不同。所以用 frmae 属性布局的视图在不同的屏幕的设备上的位置是不同的。

### 使用自动布局

> 使用自动布局时，某些控件不需要为其制定宽度和高度。控件会自动匹配，例如 UILabel，会自动匹配自身的宽度为字符串宽度，但是高度会比字号值大一些。

自动布局需要的是确定视图层次结构的约束关系，比如下图中 Red 视图的右边界距离 Blue 视图的左边界的距离是 8 pt。iOS 中使用 `NSLayoutConstraint `类来定义视图与视图之间的约束。

![视图约束](media/15379323804993/%E8%A7%86%E5%9B%BE%E7%BA%A6%E6%9D%9F.png)

<p style="text-align:center;color:#999999;font-size:14px"> 视图层次结构布局的约束 </p>

视图层次结构布局的约束为一系列线性方程式。每个约束代表一个方程，如上图中的方程。开发者的目标就是声明一系列具有唯一可能解决方案的方程式，从而确定视图的位置。

上述方程表示「Red 视图的右边界距离 Blue 视图的左边界的距离是 8 pt」，下面是它每一项的含义：

- `Item 1`：被约束的视图。这里指 Red 视图。
- `Attribute 1`：要约束在第一个项目上的属性。通常，这包括四个边界（视图的顶、底、左和右边界）、高度、宽度、垂直和水平中心（文本项还具有一个或多个基线属性）。这里指 Red 视图的前沿。
- `Relationship`：左右两侧的关系。该关系可以具有三个值之一：等于，大于或等于，或小于或等于。这里指等于。
- `Multiplier`：Attribute 2 的值乘以此浮点数。这里乘数是 1.0。
- `Item 2`：约束依赖的视图，可以为空。这里是 Blue 视图。
- `Attribute 2`：要约束在第一个项目上的属性。如果 Item 2 为空，则必须为 `Not an Attribute`。这里是  Blue 视图的后缘。
- `Constant`：常数，表示约束的距离，这里是 8 pt。

大多数约束定义了用户界面中两个视图之间的关系。约束还可以定义单个视图的两个不同属性之间的关系，例如，在视图的高度和宽度之间设置宽高比。还可以为项目的高度或宽度指定常量值。使用常量值时，Item 2 为空，Attribute 2 设置为 Not An Attribute，并且乘数设置为 0.0。

#### 左右两侧关系

上述方程中左右两侧的关系使用 NSLayoutRelation 这个枚举值来表示，具体枚举值如下：

| NSLayoutRelation 枚举值 | 说明 |
| :-: | :-: |
| NSLayoutRelationLessThanOrEqual | 约束要求第一个属性小于或等于修改后的第二个属性 |
| NSLayoutRelationEqual | 约束要求第一个属性与修改后的第二个属性完全相等 |
| NSLayoutRelationGreaterThanOrEqual | 约束要求第一个属性大于或等于修改后的第二个属性 |

<p style="text-align:center;color:#999999;font-size:14px"> NSLayoutRelation 枚举值 </p>

#### 自动布局的属性

上面方程中使用的属性，包含在 NSLayoutAttribute 这个枚举中，具体枚举值如下：

| NSLayoutAttribute 枚举值 | 说明 |
| :-: | :-: |
| NSLayoutAttributeLeft | 对象的矩形的左边界 | 
| NSLayoutAttributeRight | 对象的矩形的右边界 |  
| NSLayoutAttributeTop | 对象的矩形的顶边界 |  
| NSLayoutAttributeBottom | 对象的矩形的底边界 |  
| NSLayoutAttributeLeading | 对象的矩形的前沿 |  
| NSLayoutAttributeTrailing | 对象的矩形的后缘 |  
| NSLayoutAttributeWidth | 对象的矩形的宽度 |  
| NSLayoutAttributeHeight | 对象的矩形的高度 |  
| NSLayoutAttributeCenterX | 对象的矩形的 X 轴中心 |  
| NSLayoutAttributeCenterY | 对象的矩形的 Y 轴中心 |  
| NSLayoutAttributeBaseline | 对象的基线 |  
| ... | ... | 

<p style="text-align:center;color:#999999;font-size:14px"> NSLayoutAttribute 枚举值 </p>

> <i class="fas fa-exclamation-circle"></i> 这里没有列举完所有 NSLayoutAttribute 的枚举值，因为没有列举的值，可能「永远」都不会用上，如果需要去官方文档查阅即可。


下图是自动布局中视图的属性：

![自动布局的属性](media/15379323804993/%E8%87%AA%E5%8A%A8%E5%B8%83%E5%B1%80%E7%9A%84%E5%B1%9E%E6%80%A7.png)

<p style="text-align:center;color:#999999;font-size:14px"> 自动布局中视图的属性图示 </p>

<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">