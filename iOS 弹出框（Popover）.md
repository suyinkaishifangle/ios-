# iOS 弹出框（Popover）

> 环境：<i class="fas fa-laptop"></i> Xcode 9.4.1 &nbsp;&nbsp;<i class="fas fa-mobile-alt"></i> iOS 11.4

-------

## 弹出框简介
modalInPopover;

iOS 上的弹出框类似经常在书本上看到的对话框。下图是 Apple 官方文档中展示的弹出框在 iPad 上的系统应用「日历」中的应用。

![iPad「日历」应用中的弹出框](media/15366576680354/iPad%E3%80%8C%E6%97%A5%E5%8E%86%E3%80%8D%E5%BA%94%E7%94%A8%E4%B8%AD%E7%9A%84%E5%BC%B9%E5%87%BA%E6%A1%86.png)

<p style="text-align:center;color:#999999;font-size:14px"> iPad「日历」应用中的弹出框 </p>

在 iOS 9 以前，弹出框是通过 `UIPopoverController` 类来管理。在 iOS 9 及以后，UIPopoverController 被弃用，取而代之的是 `UIPopoverPresentationController` 类，但是 UIPopoverPresentationController 类却是 iOS 8 的时候出现的。

## UIPopoverPresentationController 类

使用弹出框并不需要自定义 UIPopoverPresentationController 类。几乎在所有情况下，都是按照原样使用它，并且**不需要创建它的实例**。把 UIViewController 以模态方式呈现，并且将其 `modalPresentationStyle` 属性（即模态样式）设置为 `UIModalPresentationPopover` 时，UIKit 会自动创建 UIPopoverPresentationController 类的对象。

UIViewController 中有一个 `popoverPresentationController` 属性，它是 UIPopoverPresentationController 类型，前面提到的自动创建的 UIPopoverPresentationController 对象就是通过这个属性获取。

> <i class="fas fa-exclamation-circle"></i> 重要
> 要实现弹出框效果必须要重写 `adaptivePresentationStyleForPresentationController:` 方法，使其返回值为 `UIModalPresentationNone`，如果不重写这个方法，那么弹出框无法正常显示，会跳转为空白页面。

### delegate 属性

遵守 UIPopoverPresentationControllerDelegate 协议，处理与弹出框相关的消息的委托。

### 弹出框尺寸

弹出框显示的视图本质上是一个 UIViewController 管理的 view。设置弹出框的大小，实质上就是设置 UIViewController 的大小。

设置 UIViewController 的 preferredContentSize 属性来确定弹出框的大小。

### 弹出框外观

如果想要设置弹出框以外的背景样式，例如蒙层效果等，需要使用 UIPopoverPresentationController 类的父类 UIPresentationController 类实现。具体参考 UIPresentationController 类的相关内容。

#### backgroundColor 属性

UIColor 类型，设置弹出框视图的背景颜色。

弹出框本质上是视图控制器管理的视图，所以通过视图控制视图的 backgroundColor 属性可以为这个视图设置颜色，但是设置设置后是下图的效果（这里设置视图控制器的视图背景颜色为 grayColor）。

![弹出框中视图控制器的视图背景颜色](media/15366576680354/%E5%BC%B9%E5%87%BA%E6%A1%86%E4%B8%AD%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8%E7%9A%84%E8%A7%86%E5%9B%BE%E8%83%8C%E6%99%AF%E9%A2%9C%E8%89%B2.png)

<p style="text-align:center;color:#999999;font-size:14px"> 弹出框中视图控制器的视图背景颜色 </p>

如果不设置视图控制器的背景颜色而只设置 UIPopoverPresentationController 的背景颜色，效果如下（同样设置为 grayColor）：

![弹出框对象的背景颜色](media/15366576680354/%E5%BC%B9%E5%87%BA%E6%A1%86%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%83%8C%E6%99%AF%E9%A2%9C%E8%89%B2.png)

<p style="text-align:center;color:#999999;font-size:14px"> 弹出框对象的背景颜色 </p>

为了加强对比，这次再把视图控制器的视图背景色设置为 grayColor，将 UIPopoverPresentationController 的背景颜色设置为 greenColor，效果如下：

![同时设置两种颜色](media/15366576680354/%E5%90%8C%E6%97%B6%E8%AE%BE%E7%BD%AE%E4%B8%A4%E7%A7%8D%E9%A2%9C%E8%89%B2.png)

<p style="text-align:center;color:#999999;font-size:14px"> 同时设置两种颜色 </p>

通过上面的结果，可以猜测出 UIPopoverPresentationController 对象是处于 UIViewController 对象的下方，所以当只为 UIPopoverPresentationController 对象设置颜色时，视图颜色和弹出框的箭头一致，而如果设置 UIViewController 对象视图的背景颜色后，就会覆盖 UIPopoverPresentationController 对象的视图背景颜色。

从 Xcode 的视图层级工具上看，基本符合猜测。

![弹出框的层级视图](media/15366576680354/%E5%BC%B9%E5%87%BA%E6%A1%86%E7%9A%84%E5%B1%82%E7%BA%A7%E8%A7%86%E5%9B%BE.png)

<p style="text-align:center;color:#999999;font-size:14px"> 弹出框的层级视图 </p>

> <i class="fas fa-exclamation-circle"></i> 提醒
> 
> 在实际开发中，一般只需要设置 UIPopoverPresentationController 的背景颜色。

#### popoverBackgroundViewClass 属性

遵守 UIPopoverBackgroundViewMethods 协议的 Class 类型。用于显示弹出框背景内容的类。

此属性的默认值为 nil，表示控制器使用默认的弹出框背景。如果为其赋值，那么必须是 `UIPopoverBackgroundView` 的子类对象，表示控制器使用指定的 UIPopoverBackgroundView 的子类对象来绘制弹出框的背景内容。UIPopoverBackgroundView 类具体内容参考下文。

#### canOverlapSourceViewRect 属性

BOOL 类型，指示弹出框是否可以与其参照矩形重叠。

设置此属性为 YES 时，表示允许弹出框在**空间受约束时**与 sourceRect 属性（见后文）中矩形（参照矩形）重叠。此属性的默认值为 NO，这可防止弹出框与参照矩形重叠。

#### passthroughViews 属性

 NSArray 类型（存储 UIView）。可以在弹出框可见时与之交互的视图数组。
 
 当弹出框处于活动状态时，会禁用与其他视图的交互，直到弹出窗口被取消。将 UIView 对象数组分配给此属性会使 UIKit 继续将触摸事件分派给指定的视图。
 
 比如，当弹出框出现的时候，会禁止用户与其他视图的交互。发生在弹出框视图之外的触摸事件将会隐藏弹出框（即点击弹出框外边，隐藏弹出框）。如果把 UIView 对象数组赋值给这个属性，那么在弹出框出现时，这个数组中的 UIView 对象上的触摸事件将不会使弹出框隐藏。

#### popoverLayoutMargins 属性

UIEdgeInsets 类型，定义屏幕部分的边距，允许显示弹出窗口。

> <i class="fas fa-exclamation-circle"></i> 目前没有发现这个属性的实际作用，使用代码测试也没有效果。查阅官方文档和其他资料都没有详细说明。

### 弹出框箭头

#### sourceView 属性

UIView 类型，弹出框弹出时的参照视图（即从哪个视图弹出）。这个属性一般与 sourceRect 属性配合使用。

#### sourceRect 属性

CGRect 类型，弹出框弹出时参照的矩形。这个属性一般与 sourceView 属性配合使用。

CGRect 值中的 x 和 y 参数表示参考的矩形相对于参照视图（即 sourceView 属性的视图）起点坐标位置。width 和 height 参数表示矩形的宽和高。例如下面代码：

```Objective-C
button.frame = CGRectMake(100, 200, 200, 50);
vc.popoverPresentationController.sourceView = button;
vc.popoverPresentationController.sourceRect = CGRectMake(100, 25, 100, 25);
```

这里设置弹出框参照这个 button 弹出。把参照视图起点坐标看作是坐标原点，那么参照矩形的起点作为就是 (100,25)，并且参照矩形的款高分别是 100 和 25。示意图如下：

![弹出框参照示意图](media/15366576680354/%E5%BC%B9%E5%87%BA%E6%A1%86%E5%8F%82%E7%85%A7%E7%A4%BA%E6%84%8F%E5%9B%BE.png)

<p style="text-align:center;color:#999999;font-size:14px"> 弹出框参照示意图 </p>

上图中的深色的矩形就是 sourceRect 属性的所确定的矩形。**弹出框箭头的坐标永远是在参照矩形的底边的中心点**，所以这个属性需要确定一个参照矩形，来定义这个弹出框箭头的点。

#### barButtonItem 属性

UIBarButtonItem 类型，弹出框弹出时的参照按钮项。

和 sourceView 属性类似，但是使用 barButtonItem 属性，那么就不需要使用 sourceRect 属性（即使使用了也无效）。

使用此属性时，弹出框的箭头位置是不固定的，具体由按钮项的宽度决定，但是不是在按钮项底部的中间。

> <i class="fas fa-exclamation-circle"></i> 此属性的优先级高于 sourceView 属性。也就是说如果 sourceView 属性和 barButtonItem 属性都设置了，那么弹出框依旧是在 barButtonItem 属性设置的地方弹出。

#### permittedArrowDirections 属性

UIPopoverArrowDirection 类型，用于指定弹出框箭头方向。

是一个枚举类型，具体枚举值如下表：

| UIPopoverArrowDirection 枚举值 | 说明 |
| :-: | :-: |
| UIPopoverArrowDirectionUp | 箭头指向上方 | 
| UIPopoverArrowDirectionDown | 箭头指向下方 | 
| UIPopoverArrowDirectionLeft | 箭头指向左方 | 
| UIPopoverArrowDirectionRight | 箭头指向右方 | 
| UIPopoverArrowDirectionAny | 默认值，箭头指向任何方向 | 
| UIPopoverArrowDirectionUnknown | 箭头的状态目前未知 |

<p style="text-align:center;color:#999999;font-size:14px"> UIPopoverArrowDirection 枚举值 </p> 

#### arrowDirection 属性

UIPopoverArrowDirection 类型，只读。表示弹出框箭头当前的箭头方向。

只有弹出框当前在屏幕上弹出时，这个值才会是具体指定的方法，如果弹出框还未弹出或者弹出后已经消失，那么这个值为 UIPopoverArrowDirectionUnknown。

## UIPopoverPresentationControllerDelegate 代理

### prepareForPopoverPresentation: 方法

通知代理对象即将呈现弹出窗口。无返回值。在调用此方法时，弹出框尚未显示在屏幕上。可以使用此方法修改弹出窗口控制器控件的配置或执行应用程序所需的任何其他操作。

- 第一个参数：UIPopoverPresentationController 类型，即将显示弹出框控制器。


### popoverPresentationControllerShouldDismissPopover: 方法

询问代理对象是否应该隐藏弹出框。当弹出框出现时，用户与弹出框视图之外的界面进行交互时自动调用这个方法。但是如果是与在 passthroughViews 属性中的视图对象交互，那么不会调用这个方法。

- 第一个参数：UIPopoverPresentationController 类型，弹出框控制器。
- 返回值：YES 表示隐藏弹出框，NO 表示不隐藏弹出框。

如果未在代理对象中实现此方法，则默认返回值为YES。

### popoverPresentationControllerDidDismissPopover: 方法

告诉代理对象弹出框已经被隐藏。无返回值。弹出框控制器在隐藏弹出框后自动调用此方法。

- 第一个参数：UIPopoverPresentationController 类型，弹出框控制器。

### popoverPresentationController:willRepositionPopoverToRect:inView: 方法

告诉代理对象，UIKit 需要重新定位弹出框的位置。无返回值。

- 第一个参数：UIPopoverPresentationController 类型，弹出框控制器。
- 第二个参数：在输入时，弹出框新的参照矩形。此弹出框位于第三个参数中视图的坐标空间中。如果要为弹出框提出不同的矩形，请将新值放在此参数中。
- 第三个参数：在输入时，包含弹出窗框的新视图对象。如果要为弹出框提出不同的视图，请将该视图对象放在此参数中。

需要重新布局弹出框时，弹出框控制器调用此方法。例如，当必须调整弹出框为键盘腾出空间时，UIKit 会调用此方法，并可选择更改弹出框的参照视图和参照矩形。

如果未在代理对象中实现此方法，UIKit 会将弹出窗口调整为指定的矩形，并更改（根据需要）其参照视图。

## ❎UIPopoverBackgroundView 类

弹出框的背景外观。

必须先对该类进行子类化，然后才能使用它。子类的实现负责为弹出框提供边框装饰和箭头。子类必须覆盖所有声明的属性和方法，以提供有关在何处布置相应的弹出框内容和箭头的信息。子类还必须为 UIPopoverBackgroundViewMethods 协议的所有方法提供实现。

子类负责提供弹出框的背景视觉样式，其中包括箭头和适当样式的边框。

## ❎UIPopoverBackgroundViewMethods 协议

UIPopoverBackgroundViewMethods 协议中是 UIPopoverBackgroundView 子类必须实现的一组方法。当弹出框出现时，此协议中的方法仅被调用一次，并且该协议的所有方法都必须实现。

## ❎示例代码

下面示例代码省略了不重要的部分。当点击按钮是调用 clickPopover: 方法。

```Objective-C
- (void)clickPopover:(UIButton *)sender {
    UIViewController *vc = [[UIViewController alloc]init];
    
    vc.modalPresentationStyle = UIModalPresentationPopover;
    vc.preferredContentSize = CGSizeMake(200, 200);
    vc.popoverPresentationController.backgroundColor = [UIColor grayColor];
    vc.popoverPresentationController.delegate = self;
    vc.popoverPresentationController.sourceView = sender;
    vc.popoverPresentationController.sourceRect = CGRectMake(100, 25, 100, 25);
    vc.popoverPresentationController.permittedArrowDirections = UIPopoverArrowDirectionAny;
    vc.popoverPresentationController.canOverlapSourceViewRect = YES;
    
    [self presentViewController:vc animated:YES completion:nil];
}

- (BOOL)popoverPresentationControllerShouldDismissPopover:(UIPopoverPresentationController *)popoverPresentationController{
    return YES;
}

- (UIModalPresentationStyle)adaptivePresentationStyleForPresentationController:(UIPresentationController *)controller{
    return UIModalPresentationNone;
}

- (void)popoverPresentationControllerDidDismissPopover:(UIPopoverPresentationController *)popoverPresentationController{
    NSLog(@"dismissed");
}

- (void)popoverPresentationController:(UIPopoverPresentationController *)popoverPresentationController
          willRepositionPopoverToRect:(inout CGRect *)rect
                               inView:(inout UIView *__autoreleasing  _Nonnull *)view {
    
}
```

<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">