# UIButton ✅

## UIButton 简介

[UIButton](https://developer.apple.com/documentation/uikit/uibutton?language=objc) 类是用于响应用户交互执行自定义代码的控件，称为「按钮」。它是 UIControl 的子类。

按钮使用目标-动作（Target-Action）设计模式在用户点击时调用相关操作方法。使用按钮可以不用直接处理触摸事件，而是将操作方法分配给按钮，并指定哪些事件触发方法的调用。

使用 `addTarget:action:forControlEvents:` 方法将按钮连接到操作方法，操作方法采用以下三种形式之一：

```Objective-C
- (void)doSomething;
- (void)doSomething:(id)sender;
- (void)doSomething:(id)sender forEvent:(UIEvent*)event;
```

## UIButton 样式

UIButton 具有分为不同类型，UIButtonType 是一个枚举类型，包含 UIButton 类型的枚举常量值。具体的 UIButton 类型如下表：

| UIButtonType 枚举值 | 说明 |
| :-: | :-: |
| UIButtonTypeCustom | 自定义样式 |
| UIButtonTypeSystem | 系统样式按钮 |
| UIButtonTypeDetailDisclosure | 详细信息按钮 |
| UIButtonTypeInfoLight | 具有浅色背景的信息按钮 |
| UIButtonTypeInfoDark| 具有深色背景的信息按钮 |
| UIButtonTypeContactAdd| 添加内容的按钮 |

> 🔥 **提示**
>
> 虽然按钮的样式很多，但是最常用的是 UIButtonTypeCustom 样式，其次偶尔会用到 UIButtonTypeSystem 样式实现一些需求。

**UIButtonTypeSystem 和 UIButtonTypeCustom 的区别**

新手开发者只知道一般使用 UIButtonTypeCustom 样式，但是对于 UIButtonTypeSystem 类型具体的作用什么，可能没有了解和深入探索。如果对这两种样式做个对比，会发现在实际开发中，有时使用 UIButtonTypeSystem 会省去一些工作量。他们的主要区别在于**高亮时的状态**（二者在普通状态时相同），下面三个表中通过对按钮常用的设置方式来对两个类型的按钮做一个对比
（因为是对比两种按钮的高亮状态，所以下列结果是在都设置了对应的普通状态下的图像或标题颜色之后得到的结果）。

- **对比背景图像**

  | 高亮状态下的背景图像 | UIButtonTypeSystem | UIButtonTypeCustom |
  | :-: | :-: | :-: |
  | 不设置 | 高亮状态时，背景图像颜色变**浅** | 高亮状态时，背景图像颜色变**深** |
  | 设置 | 高亮状态时，背景图像颜色变**浅** | 高亮状态时，显示高亮状态的背景图像 |

  **总结：**

   - UIButtonTypeSystem 类型的按钮的高亮的状态时的样式是系统确定好的，无论开发者有没有设置高亮时的背景图像，按钮高亮状态时背景图像颜色都会变浅。
   - UIButtonTypeCustom 类型的按钮如果不设置高亮状态时的背景图像，那么系统会自动将其背景图像颜色变深；如果设置了高亮状态时的背景图像，那么按钮高亮状态时就显示设置的背景图像。

- **对比按钮图像**

  | 高亮状态下的按钮图像 | UIButtonTypeSystem | UIButtonTypeCustom |
  | :-: | :-: | :-: |
  | 不设置 | 高亮状态时，按钮图像颜色变**浅** | 高亮状态时，按钮图像颜色变**深** |
  | 设置 | 高亮状态时，按钮图像颜色变**浅** | 高亮状态时，显示高亮状态的按钮图像 |

  **总结：**

  按钮图像的效果和背景图像相同。但是，**UIButtonTypeSystem 类型的按钮图像，普通状态下的颜色会被自动变成蓝色，无论原图像是什么颜色，所以高亮状态就是变成浅蓝色**（UIButtonTypeSystem 类型的**背景图像**不会被自动变色）。

- **对比按钮标题颜色**

  | 高亮状态下的按钮标题颜色 | UIButtonTypeSystem | UIButtonTypeCustom |
  | :-: | :-: | :-: |
  | 不设置 | 高亮状态时，标题颜色变**浅** | 高亮状态时，标题颜色不变 |
  | 设置 | 高亮状态时，标题颜色变**浅** | 高亮状态时，标题颜色改变为设置的颜色 |

  要注意，这里对比时是将普通状态和高亮状态的标题颜色设置为相同，比如：普通状态是是红色，那么高亮状态时也是红色。如果将普通状态和高亮状态时的标题颜色设置的不一致，那么标题颜色肯定是会变化的，但是 UIButtonTypeSystem 类型的按钮还会在高亮状态的标题颜色的基础上变浅。例如：UIButtonTypeSystem 类型的按钮普通状态时标题颜色为红色，高亮状态时设置颜色为绿色，那么当其高亮时，就是浅绿色。

## UIButton 创建

创建一个按钮对象，一般使用 `buttonWithType:` 类方法。此方法是一种便捷创建对象的方法，用于创建具有特定样式的按钮对象。

- **buttonWithType:** 类方法

  创建一个指定样式的按钮对象。

  - 参数一：UIButtonType 类型，按钮样式。
  - 返回值：UIButton 对象。

  > ⚠️ **注意**
  >
  > 如果是继承 UIButton 的子类按钮，则必须使用 `alloc/init` 初始化按钮，不能使用该方法，否则不会返回子类的对象。

除了使用 `buttonWithType:` 类方法便捷创建指定样式的 UIButton 之外，还是可以使用 `alloc/init` 创建 UIButton 的对象，不过使用 `alloc/init` 创建 UIButton 对象只会是 `UIButtonTypeCustom` 的样式。

## UIButton 外观

### 标题

使用 `setTitle:forState:` 方法和 `setAttributedTitle:forState:` 方法能为按钮指定不同状态下的标题内容，使用 `setTitleColor:forState:` 方法能为按钮指定不同状态下的标题颜色，但是要修改标题文字的大小，那么只能使用其 `titleLabel` 属性，但是该属性不能用来设置标题颜色和标题内容。

- **currentTitle** 属性

    NSString 类型，只读。按钮上显示的当前标题。
    
    每当按钮状态更改时，将自动设置此属性的值。此属性用于获取按钮的标题。

- **titleLabel** 属性

    UILabel 类型，只读。用于显示按钮的 `currentTitle` 属性的值。

    虽然此属性是只读的，但它自己的属性是可读可写的。主要使用此属性的 `font` 属性和 `lineBreakMode` 属性来配置按钮的文本。

    - `titleLabel.font` UIFont 类型，设置按钮上标题的字体。UIButton 上的标题文字大小默认值是 `18pt`。
    - `titleLabel.lineBreakMode` NSLineBreakMode 类型，设置文本的换行，具体参考 UILabel。

  不能使用该属性来设置文本颜色或阴影颜色，即使设置了也无效。
  
- **setTitle:forState:** 方法
    
    设置指定状态下按钮的标题内容。

      - 参数一：NSString 类型，按钮上显示的标题。
      - 参数二：UIControlState 类型，按钮状态。

    按钮标题文字的对齐方式默认是居中对齐。如果只设置了普通状态下了按钮标题内容，没有设置高亮状态下的按钮标题内容，那么按钮在高亮状态时依旧显示的时普通状态下的标题内容，`UIButtonTypeSystem` 样式和 `UIButtonTypeCustom` 样式的按钮都是如此。

- **setAttributedTitle:forState:** 方法

    设置指定状态下按钮的标题内容（文字为富文本）。
 
      - 参数一：NSAttributedString 类型，按钮上显示的富文本标题。
      - 参数二：UIControlState 类型，按钮状态。

- **setTitleColor:forState:** 方法
    
    设置指定状态下的按钮标题的颜色。
    
    - 参数一：UIColor 类型，按钮上显示的文字的颜色。
    - 参数二：UIControlState 类型，按钮状态。

    `UIButtonTypeCustom` 样式的按钮的标题颜色默认是白色。`UIButtonTypeSystem` 样式的按钮的标题颜色默认是蓝色（因为受 tintColor 属性的影响）。
  
- **setTitleShadowColor:forState:** 方法

    设置指定状态下按钮标题的阴影颜色。
    
    - 参数一：UIColor 类型，按钮上显示的标题的阴影颜色。
    - 参数二：UIControlState 类型，按钮状态。

### 图像

按钮的图像分为两种，一种是按钮的背景图像，另一种是按钮上展示的图像，称为「按钮图像」。

- **imageView** 属性

    UIImageView 类型，只读。按钮的图像视图。
    
    虽然此属性是只读的，但它自己的属性是可读可写的，使用这些属性可以配置按钮视图的外观和行为。
    
    即使尚未显示按钮，`imageView` 属性也会返回一个值。系统按钮的属性值为 nil。

- **setBackgroundImage:forState:** 方法 

    设置指定状态下的按钮背景图像。
    
    - 参数一：UIImage 类型，按钮的背景图像。
    - 参数二：UIControlState 类型，按钮状态。
    
    按钮的背景图像会填满整个按钮的矩形区域。假如图像尺寸为 40\*40，按钮尺寸为 80\*80，那么会将图像拉伸为 80\*80 的大小。
    
- **currentBackgroundImage** 属性

     UIImage 类型，只读。按钮上当前显示的背景图像。

- **setImage:forState:** 方法

    设置指定状态下的按钮图像。
    
    - 参数一：UIImage 类型，按钮上展示的图像。
    - 参数二：UIControlState 类型，按钮状态。

    如果按钮范围小于图像尺寸，那么图像会随着按钮缩小，并且是充满按钮。假如如图像大小是 40\*40，按钮大小是 20\*20，那么图像也会变为 20\*20；如果按钮范围大于图像尺寸，那么图像保持原来的尺寸不会变化。假如图像大小是 40\*40，按钮大小为 80\*80，那么图像还是 40\*40 的大小。
    
- **currentImage** 属性

    UIImage 类型，只读。按钮上当前显示的图像。

- **tintColor** 属性

    UIColor 类型，用于修改 `UIButtonTypeSystem` 样式按钮的标题颜色和按钮图像颜色。

    注意不能修改背景图像的颜色，背景图像原来是什么颜色就显示什么颜色。

    对于 `UIButtonTypeSystem` 样式的按钮，如果不设置此属性，那么按钮的标题和按钮图像都默认为蓝色。如果使用了 `setTitleColor:forState:` 方法，那么标题的颜色由该方法控制。

   > ⚠️ **注意**
   >
   > 此属性对于 `UIButtonTypeCustom` 样式的按钮无效。该样式的按钮只能使用 `setTitleColor:forState:` 方法来设置按钮标题的颜色，并且该样式的按钮图像颜色就是该图像本来的颜色。

## UIButton 内部位置

如果 UIButton 只设置其矩形原点坐标，而不设置其大小时，调用其 `sizeToFit` 方法，UIButton 能够根据自身的内容（图像、标题）的大小来设置自己的 frame 值的大小（使用 Masonry 布局框架时不约束 UIButton 的大小时也会自动适应，也是底层调用了 `sizeToFit`）。

### 使用 UIEdgeInsets 

如果一个按钮既设置了图像，又设置了标题，那么在按钮中默认图像在左侧，标题在右侧，并且两者是相互紧挨在一起，两者形成的整体相对于按钮居中。可以通过 `contentEdgeInsets`、`titleEdgeInsets` 和 `imageEdgeInsets`三个属性分别用来调整 UIButton 的内容（包含标题和图像的整体）、标题和图像的矩形的位置。

- **imageEdgeInsets** 属性

    UIEdgeInsets 类型，按钮图像矩形的内嵌位置。默认值为 `UIEdgeInsetsZero`。
    
    仅适用于布局期间定位标题的位置。
    
    此属性可以修改按钮图像，也可以缩小按钮图像，但是不能够将其放大。
    
    - UIEdgeInsets 中的一边的值为正数，且对应边的值为非负时。表示将图像向中心移动对应的值。此时没有修改的边不会发生任何变化。

        -  例如 `_button.imageEdgeInsets = UIEdgeInsetsMake(0, 10, 0, 0);` 表示将按钮图像的左边缘向右移动 10pt，此时图像的其他边都不会变化，所以这样看到的效果是图像的左边缩小变形，如下图：左边按钮图像为 `UIEdgeInsetsZero `时的图像。
        
        ![按钮的图像内嵌值为正数](media/15504596268502/%E6%8C%89%E9%92%AE%E7%9A%84%E5%9B%BE%E5%83%8F%E5%86%85%E5%B5%8C%E5%80%BC%E4%B8%BA%E6%AD%A3%E6%95%B0.png)
        
        - 例如 `_button.imageEdgeInsets = UIEdgeInsetsMake(0, 10, 0, 10);` 表示将按钮图像的左边缘向右移动 10pt，右边缘向左移动 10pt。如下图：左边按钮图像为 `UIEdgeInsetsZero `时的图像。

        ![按钮的图像内嵌值为正2](media/15504596268502/%E6%8C%89%E9%92%AE%E7%9A%84%E5%9B%BE%E5%83%8F%E5%86%85%E5%B5%8C%E5%80%BC%E4%B8%BA%E6%AD%A32.png)
    
    - UIEdgeInsets 中的值如果一边为负数，无论另一边为何值。都将计算与对应边的值的**差**再除以 2 后移动。

        - 例如 `_button.imageEdgeInsets = UIEdgeInsetsMake(-10, 0, 0, 0);` 表示将图像上边缘向下移动 (-10-0)/2pt，将图像下边缘向上移动 (0-(-10))/2pt。如下图：左边按钮图像为 `UIEdgeInsetsZero `时的图像。

        ![按钮的图像内嵌值为负1](media/15504596268502/%E6%8C%89%E9%92%AE%E7%9A%84%E5%9B%BE%E5%83%8F%E5%86%85%E5%B5%8C%E5%80%BC%E4%B8%BA%E8%B4%9F1.png)

        - 例如 `_button.imageEdgeInsets = UIEdgeInsetsMake(8, 0, -10, 0);` 表示将图像上边缘向下移动 (8-(-10))/2pt，将图像下边缘向上移动 (-10-(8))/2pt。如下图：左边按钮图像为 `UIEdgeInsetsZero `时的图像。

        ![按钮的图像内嵌值为负2](media/15504596268502/%E6%8C%89%E9%92%AE%E7%9A%84%E5%9B%BE%E5%83%8F%E5%86%85%E5%B5%8C%E5%80%BC%E4%B8%BA%E8%B4%9F2.png)
        
- **titleEdgeInsets** 属性

    UIEdgeInsets 类型，按钮标题矩形的内嵌位置。默认值为 `UIEdgeInsetsZero`。
  
    此属性可调整按钮标题的有效绘制矩形的大小和位置。仅适用于布局期间定位标题的位置。
    
    具体作用参考 `imageEdgeInsets` 属性。

- **contentEdgeInsets** 属性

    UIEdgeInsets 类型，按钮内容矩形的内嵌位置。默认值为 `UIEdgeInsetsZero`。
    
    `contentEdgeInsets` 和 `titleEdgeInsets`、`imageEdgeInsets` 不一祥。使用此属性的实际效果相当于直接修改整个按钮的大小。例如：
    
    - 使用 UIEdgeInsets 值为 `(0, 12, 0, 12)`，表示将按钮大小向左向右都扩大 12pt；
    - 使用 UIEdgeInsets 值为 `(0, 24, 0, 0)`，表示将按钮向左扩大 24pt，不过要注意，只设置一边扩大时，内部内容会稍许移动，但是只要将对称的边都扩大相同时，就不会有这种效果。
        
### 重写子类

在 UIButton 子类中，以下方法的默认实现是返回参数一中的值，表示按钮绘制其标准的内部位置。可以在子类中重写这些方法，返回自定义的矩形，以此设置标题、图像、内容以及背景的相对于整个按钮的位置。

- **backgroundRectForBounds:** 方法

    返回按钮背景的绘制矩形，以定位按钮背景的位置。
    
    - 参数一：CGRect 类型，按钮背景默认的矩形。
    - 返回值：CGRect 类型，用于绘制按钮背景位置的矩形。

    只有为按钮设置了 BackgroundImage 之后才会调用此方法。
    
- **contentRectForBounds:** 方法

    返回按钮内容的绘制矩形，以定位按钮内容的位置。
    
    - 参数一：CGRect 类型，按钮内容默认的矩形。
    - 返回值：CGRect 类型，用于绘制按钮内容位置的矩形。

    内容矩形是显示整个图像和标题所需的区域，包括任何填充和对齐和其他设置的调整。
    
- **titleRectForContentRect:** 方法

    返回按钮标题的绘制矩形，以定位按钮标题的位置。
    
    - 参数一：CGRect 类型，按钮标题默认的矩形。
    - 返回值：CGRect 类型，用于绘制按钮标题位置的矩形。

- **imageRectForContentRect:** 方法

    返回按钮图像的绘制矩形，以定位按钮图像的位置。
    
    - 参数一：CGRect 类型，按钮图像默认的矩形。
    - 返回值：CGRect 类型，用于绘制按钮图像位置的矩形。
    
    
    