# MBProgressHUD

> 🗜 版本：1.1.0  🔗 官网链接：https://github.com/jdg/MBProgressHUD

-------

## MBProgressHUD 简介

是一个 iOS 上用来显示 HUD 的第三方库，它用来显示带有指示器和文字的半透明 HUD，HUD 就是透明指示层，它的作者是 👨🏻‍💻 [Matej Bukovinski](https://www.bukovinski.com/)。MBProgressHUD 基于 Foundation、UIKit 和 CoreGraphics三个官方库，并且基于 ARC 构建。

## MBProgressHUD 使用方法

MBProgressHUD 可以分为两种方式：

- 使用类方法
- 使用对象方法

使用这两种方式首先都要导入头文件 `MBProgressHUD.h`。

### 使用类方法展示 HUD

使用类方法非常简单，就只有两个步骤，需要显示的时候显示出来，需要隐藏的时候隐藏起来。

步骤：

1. 使用 `showHUDAddedTo:animated:` 类方法显示 HUD。
2. 使用 `hideHUDForView:animated:` 类方法隐藏 HUD。

但是这种方式不能自定义 HUD，只能是默认的样式（默认样式为旋转指示器，没有文字和按钮）。如果需要自定义 HUD 来满足需求，那么就只能使用对象方法来实现。

### 使用对象方法展示 HUD

使用对象方法的需要写的代码量比使用类方法的代码量稍多，因为要根据不同项目的需求来自定义 HUD。

步骤：

1. 创建 MBProgressHUD 对象。
2. 对 MBProgressHUD 对象进行自定义设置（设置相关属性的值）。
3. 将 MBProgressHUD 对象添加到需要显示的视图上。
4. 显示 HUD。
5. 隐藏 HUD。

> 如果需要实现代理方法（目前 MBProgressHUD 库只有一个代理方法 `hudWasHidden:`），那么还需要声明代理协议 `MBProgressHUDDelegate`。

示例代码：

```Objective-C
// 创建 MBProgressHUD 对象
MBProgressHUD *HUD = [[MBProgressHUD alloc]initWithView:self.view];

// 对 HUD 对象进行自定义设置
HUD.mode = MBProgressHUDModeDeterminate; 
HUD.label.text = @"正在加载...";
...

//将 MBProgressHUD 对象添加到需要显示的视图上
[self.view addSubview:HUD];

// 显示 HUD
[HUD showAnimated:YES];
// 隐藏 HUD
[HUD hideAnimated:YES];
```

## MBProgressHUD 视图结构

这里将 HUD 的背景颜色设置为 grayColor（灰色），便于观察。

![HUD 视图结构](media/15359649010731/HUD%20%E8%A7%86%E5%9B%BE%E7%BB%93%E6%9E%84.png)

<p style="text-align:center;color:#999999;font-size:14px"> HUD 视图结构 </p>

MBProgressHUD 中的 HUD 视图如上图，用户可见的图层只有两个：`bezelView` 和 `backgroundView`。它们都是 `MBBackgroundView` 类型，可以通过 MBProgressHUD 对象的属性自定义它们的颜色、样式等。`bezelView` 中包含了指示器（即用户看到的图像）、文字和按钮，backgroundView 就是整个背景。

下图是通过 Xcode 的视图层次结构工具查看的 HUD 的视图层次结构：

![HUD 视图层次结构](media/15359649010731/HUD%20%E8%A7%86%E5%9B%BE%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84.png)

<p style="text-align:center;color:#999999;font-size:14px"> HUD 视图层次结构 </p>

图中 ❷ 和 ❸ 分别是 bezelView 和 backgroundView 类的对象，还有未标注的两个 UILabel 和一个 UIButton 对象，分别是指示器中的文字和按钮。

## MBProgressHUD 多线程

当程序在处理**耗时任务**时（例如网络请求），UI 界面不会有任何变化和交互，给用户的感觉就像应用程序卡死了。使用 MBProgressHUD 给用户一种正在等待程序运行的直观感觉，能够很好的提升用户体验。

处理耗时操作就会涉及到 UI 处理的问题。因为只有主线程才能更新 UI，所以 MBProgressHUD 任务只能在主线程中执行。那么也就是耗时任务是在主线程中处理还是在子线程中处理的区别：

- 在主线程中处理耗时任务
    
    在执行耗时操作前，在主线程中先处理 HUD 任务，然后处理耗时操作，在耗时操作完成之后，马上结束 HUD 任务。

- 在子线程中处理耗时任务（建议）

    在要执行耗时任务时，将耗时任务移到子线程中处理，主线中只执行 HUD 任务，这样就能够及时更新 UI，在耗时任务执行完毕之后再关闭子线程。这是作者建议的使用方式。
    
## MBProgressHUD 源码

MBProgressHUD 库文件很简单，只包含两个文件：一个 MBProgressHUD.h 头文件和一个 MBProgressHUD.m 实现文件。

### MBProgressHUD 类

MBProgressHUD 类继承自 UIView，用于显示类似于 UIKit 中私有的 UIProgressHUD 类的进度界面视图。视图中包含进度指示器和两个短消息可选标签。

MBProgressHUD 捕获所有用户在此区域上输入，从而阻止用户对视图下方的组件进行操作（即 HUD 视图显示在最上面时，下面的组件无法和用户交互）。MBProgressHUD 是一个 UI 类，因此只能在主线程上访问。

如果要允许触摸事件通过 HUD 视图，可以设置 `hud.userInteractionEnabled = NO`。

#### MBProgressHUD 方法

- **showHUDAddedTo:animated:** 类方法

    创建一个新的 HUD，将它添加到提供的视图中并显示出来。
    
    - 参数一：UIView 类型，要添加 HUD 的视图。
    - 参数二：BOOL 类型，是否显示动画。
    - 返回值：MBProgressHUD 对象。
    
    其中，参数二如果设置为 `YES`，则将使用 `animationType` 属性指定的值显示 HUD。
    
    此方法设置 `removeFromSuperViewOnHide` 为 `YES`。
    
- **hideHUDForView:animated:** 类方法
    
    查找尚未完成的最顶层 HUD 子视图并将其隐藏。
        
    - 参数一：UIView 类型，要查找 HUD 子视图的视图。
    - 参数二：BOOL 类型，是否显示动画。
    - 返回值：BOOL 类型，如果找到并移除了 HUD，返回 `YES`，否则为 `NO`。
    
    其中，参数二如果设置为 `YES`，则将使用 `animationType` 属性指定的值隐藏 HUD。 
    
    此方法设置 `removeFromSuperViewOnHide` 为 `YES`。在隐藏 HUD 后，HUD 将自动从视图层次结构中删除。

- **HUDForView:** 类方法

    搜索一个视图中尚未完成的最顶层 HUD 子视图并将其返回。
    
    - 参数一：UIView 类型，要搜索的视图。
    - 返回值：MBProgressHUD 类型，尚未完成的最顶层 HUD 对象。

    如果参数一的视图没有尚未完成的 HUD 子视图，那么返回 `nil`。

- **initWithView:** 方法

    使用指定的视图来创建 HUD 子视图对象。
    
    - 参数一：UIView 类型，即将添加HUD的视图。

    此方法用传入视图的边界初始化 HUD 的大小。即使用传入的视图的 view.bounds 作为 HUD 视图的 view.bounds。要注意，如果传入的视图大小很小（比如 10*10），并不会影响 HUD 中的部件（比如旋转的齿轮）的大小变小，影响的是 HUD 的真实视图大小。
    
- **showAnimated:** 方法

    显示 HUD。
    
    - 参数一：BOOL 类型，是否显示动画。

- **hideAnimated:** 方法

    隐藏 HUD。
    
    - 参数一：BOOL 类型，是否显示动画。
    
    若设置了代理，那么会调用 hudWasHidden: 代理方法。这是 `showAnimated:` 方法的对应部分。在任务完成时使用此方法来隐藏 HUD。
    
- **hideAnimated:afterDelay:** 方法

    延迟后隐藏HUD。
    
    - 参数一：BOOL 类型，是否显示动画。
    - 参数二：NSTimeInterval 类型，隐藏 HUD 之前的延迟时间，单位秒。
    
    若设置了代理，那么会调用 hudWasHidden: 代理方法。这是 `showAnimated:` 方法的对应部分。在任务完成时使用此方法来隐藏 HUD。
    
> ⚠️ **注意**
> 
> 使用对象方法隐藏 HUD 之后，HUD 视图并没有从视图层次结构中删除。可以通过其 `removeFromSuperViewOnHide` 属性为 `YES`，在隐藏 HUD 对象之后从视图层次结构中删除 HUD 对象。（类方法隐藏 HUD 视图后自动从层次结构中删除）。

#### MBProgressHUD 属性

- **delegate** 属性

    HUD 代理对象，接收 HUD 状态通知。遵守 MBProgressHUDDelegate 协议。

- **completionBlock** 属性

    MBProgressHUDCompletionBlock 类型，在 HUD 被隐藏之后调用。

- **graceTime** 属性

    NSTimeInterval 类型，延迟显示 HUD 的时间。默认值为 `0`（无延迟）。

    此属性用来设置不显示 HUD 的情况下运行的时间（以秒为单位）。可以用于防止非常短的耗时任务时显示 HUD。例如，在网络请求时。网络好的时候，可能请求时间很短，短到不用显示 HUD；但是当网络不好的时候，可能请求时间就比较长，这个时候就需要显示 HUD。所以这个属性能够很好的解决这个问题。

- **minShowTime** 属性

    NSTimeInterval 类型，表示显示 HUD 的最小时间（以秒为单位），默认为 `0`（没有最小显示时间）。

    此属性避免了显示 HUD 之后立即隐藏的问题。

- **removeFromSuperViewOnHide** 属性

    BOOL 类型，是否在隐藏 HUD 时从其父视图中删除 HUD。默认为 `NO`。

- **mode** 属性

    MBProgressHUDMode 类型，表示 HUD 的显示模式。默认值为 `MBProgressHUDModeIndeterminate`。
    
    MBProgressHUDMode 是枚举类型，表示的是 MBProgressHUD 不同的显示模式。具体枚举值如下：

    > 这里除了 MBProgressHUDModeText 模式添加了文字，其他的模式都没有添加文字。能显示进度的模式，将显示的进度都设置为 75%，其他的设置（比如颜色等）都是默认的。

    | MBProgressHUDMode 枚举值 | 说明 | 效果 |
    | :-: | :-: | :-: |
    | MBProgressHUDModeIndeterminate | UIActivityIndicatorView | ![MBProgressHUDModeIndeterminate](media/15359649010731/MBProgressHUDModeIndeterminate.png) |
    | MBProgressHUDModeDeterminate | 圆形，饼图，进度视图 | ![MBProgressHUDModeDeterminate](media/15359649010731/MBProgressHUDModeDeterminate.png) |
    | MBProgressHUDModeDeterminateHorizontalBar | 水平进度条 | ![MBProgressHUDModeDeterminateHorizontalBa](media/15359649010731/MBProgressHUDModeDeterminateHorizontalBar.png) |
    | MBProgressHUDModeAnnularDeterminate | 环形进展视图 | ![MBProgressHUDModeAnnularDeterminate](media/15359649010731/MBProgressHUDModeAnnularDeterminate.png) |
    | MBProgressHUDModeCustomView | 显示自定义视图 | ![MBProgressHUDModeCustomVie](media/15359649010731/MBProgressHUDModeCustomView.png) |
    | MBProgressHUDModeText | 只显示文字 | ![MBProgressHUDModeText](media/15359649010731/MBProgressHUDModeText.png) |

- **contentColor** 属性

    UIColor 类型，所有标签和指示符的颜色。iOS 7 及更高版本默认为半透明黑色。
    
    在 iOS 7+ 上的自定义视图还可以通过其 `tintColor` 属性来设置颜色。
    
- **animationType** 属性

    MBProgressHUDAnimation 类型，显示和隐藏 HUD 时使用的动画类型。
    
    MBProgressHUDAnimation 是枚举类型，表示的是 MBProgressHUD 不同的动画。具体枚举值如下：

    | MBProgressHUDAnimation 枚举值 | 说明 |
    | :-: | :-: | 
    | MBProgressHUDAnimationFade | 不透明动画 |
    | MBProgressHUDAnimationZoom | 不透明 + 缩放动画<br>（在消失时出现缩小时放大） |
    | MBProgressHUDAnimationZoomOut | 不透明度 + 缩放动画（缩小样式） |
    | MBProgressHUDAnimationZoomIn | 不透明度 + 缩放动画（放大样式） |

- **offset** 属性

    CGPoint 类型，bezel 视图（即中心的圆角方框视图）相对于 HUD 视图中心的偏移量。

    可以使用 `MBProgressMaxOffset` 和 `-MBProgressMaxOffset` 在每个方向将bezel 视图移动到 HUD 视图边的缘。例如 CGPointMake(0.f, MBProgressMaxOffset) 将把 HUD 放在屏幕底部的中间。

- **margin** 属性

    CGFloat 类型，在 HUD 边缘和 HUD 元素(文字、指示器或自定义视图)之间的空间量。默认值为 20。

    这也表示到 HUD 视图边缘的最小边框距离。

- **minSize** 属性

    CGSize 类型，bezel 视图的最小尺寸。默认为 CGSizeZero（无最小尺寸）。

- **square** 属性

    BOOL 类型。如果可以，强制 HUD 尺寸相等。

- **defaultMotionEffectsEnabled** 属性

    BOOL 类型。当启用时，边框中心受到设备加速度计数据的轻微影响。默认值为 YES。

- **progress** 属性

    float 类型。进度指示器的进度，从 0.0 到 1.0。默认为 0.0。

- **progressObject** 属性

    NSProgress 类型。NSProgress 对象将进度信息提供给进度指示器。

- **bezelView** 属性

    MBBackgroundView 类型，只读。用于设置文字和指示器的背景样式（即 HUD 视图中的圆角方形视图）。

- **backgroundView** 属性

    MBBackgroundView 类型，只读。视图覆盖整个 HUD 区域，位于 bezelView 视图后面。

- **customView** 属性

    UIView 类型。当 HUD 在 MBProgressHUDModeCustomView 模式时显示的视图。

    视图应该实现 `intrinsicContentSize` 来进行适当的大小调整。为了获得最佳效果，大约使用 37×37 px。

- **label** 属性

    UILabel 类型，只读。一个文本标签，将显示在活动指示器下面。HUD 会自动调整大小以适应需要。

- **detailsLabel** 属性

    UILabel 类型，只读。用于保存标签文本消息下面显示的可选详细信息。详细信息文本可以跨越多行。

- **button** 属性

    UIButton 类型，只读。放置在文字下方的按钮。仅在其 target/action 被添加时可见。

### MBRoundProgressView 类

MBRoundProgressView 是通过填充圆形(饼状图)来显示确定进度的进度视图，继承自 UIView。

#### MBRoundProgressView 属性

- **progress** 属性

    float 类型。指示器的进度，从 0.0 到 1.0。默认为 0.0。

- **progressTintColor** 属性

    UIColor 类型。指示器进度的颜色。默认为 whiteColor（白色）。

- **backgroundTintColor** 属性

    UIColor 类型。指示器背景（非进度）颜色。默认为半透明白色（alpha 0.1）。

- **annular** 属性

    BOOL 类型。显示样式，NO = 圆形；YES = 环形。默认为 `YES`。

### MBBarProgressView 类

一个扁平的进度条视图，继承自 UIView。

#### MBBarProgressView 属性

- **progress** 属性

    float 类型。进度指示器的进度，从 0.0 到 1.0。默认为 0.0。

- **lineColor** 属性

    UIColor 类型。进度条边框线颜色。默认为 whiteColor（白色）。

- **progressRemainingColor** 属性

    UIColor 类型。进度条背景颜色。默认为 clearColor。

- **progressColor** 属性

    UIColor 类型。进度条的进度颜色。默认为 whiteColor（白色）。

### MBBackgroundView 类

MBBackgroundView 继承自 UIView。

#### MBBackgroundView 类中属性

- **style** 属性

    MBProgressHUDBackgroundStyle 类型，背景风格。默认值为 `MBProgressHUDBackgroundStyleSolidColor`。
    
    MBProgressHUDBackgroundStyle 枚举类型表示的是 MBProgressHUD 背景的风格。具体枚举值如下：

    | MBProgressHUDBackgroundStyle 枚举值 | 说明 |
    | :-: | :-: |
    | MBProgressHUDBackgroundStyleSolidColor | 纯色背景 |
    | MBProgressHUDBackgroundStyleBlur | UIVisualEffectView 或 <br>UIToolbar.layer 背景视图 | |

- **blurEffectStyle** 属性

    UIBlurEffectStyle 类型。`style` 属性值为 `MBProgressHUDBackgroundStyleBlur` 时的模糊效果样式。默认为 `UIBlurEffectStyleLight` 。

    这个属性只有在 iOS 8.0 以上或者 tvOS 才会生效。

- **color** 属性

    UIColor 类型。背景颜色或模糊色调颜色。

### MBProgressHUDDelegate 协议

- **hudWasHidden:** 方法

    在指定的 HUD 完全隐藏后调用。

    - 参数一：MBProgressHUD 类型，指定的 HUD。





