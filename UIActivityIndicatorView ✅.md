# UIActivityIndicatorView ✅

UIActivityIndicatorView 继承自 UIView，它是活动指示器，用于显示任务正在进行的视图（类似于齿轮形状的动画）。

可以通过调用 `startAnimating` 和 `stopAnimating` 方法来控制活动指示器何时动画。要在动画停止时自动隐藏活动指示器，请将 `hidesWhenStopped` 属性设置为 `YES`。

可以使用 `color` 属性设置活动指示器的颜色。

## UIActivityIndicatorView 属性和方法

### 初始化活动指示器

- **initWithActivityIndicatorStyle:** 方法

    初始化并返回活动指示器实例。
    
    - 参数一：UIActivityIndicatorViewStyle 类型，指定活动指示器的样式。
    - 返回值：UIActivityIndicatorView 实例。

    UIActivityIndicatorView 根据指定的样式调整返回实例的大小。除了此初始化方法，还能够使用 `initWithFrame:` 初始化方法来创建活动指示器，其样式为 `UIActivityIndicatorViewStyleWhite`。

### 配置活动指示器外观

- **UIActivityIndicatorViewStyle** 枚举类型

     UIActivityIndicatorViewStyle 类型是枚举类型，用于指定活动指示器的视觉样式，具体如下表（其中前为了效果明显，前两个样式使用的是黑色背景，而第三个样式使用了白色背景）：
    
    | UIActivityIndicatorViewStyle 枚举类型 | 说明 ||
    | :-: | :-: | :-: |
    | UIActivityIndicatorViewStyleWhiteLarge | 较大的白色指示器。 | ![UIActivityIndicatorViewStyleWhiteLarge](media/15587568551860/UIActivityIndicatorViewStyleWhiteLarge.png) |
    | UIActivityIndicatorViewStyleWhite | 标准的白色指示器。默认值。 | ![UIActivityIndicatorViewStyleWhite](media/15587568551860/UIActivityIndicatorViewStyleWhite.png) |
    | UIActivityIndicatorViewStyleGray | 标准的灰色样式。 | ![UIActivityIndicatorViewStyleGray](media/15587568551860/UIActivityIndicatorViewStyleGray.png) |

- **activityIndicatorViewStyle** 属性

    UIActivityIndicatorViewStyle 类型，活动指示器的样式。
    
    默认值为 `UIActivityIndicatorViewStyleWhite`。
    
- **color** 属性

    UIColor 类型，活动指示器的颜色。
    
    如果为活动指示器设置颜色，它将覆盖 `activityIndicatorViewStyle` 属性提供的颜色。
    
### 管理活动指示器

- **startAnimating** 方法

    启动活动指示器的动画。

    当活动指示器被激活时，齿轮旋转以指示不确定的进度。在调用 `stopAnimating` 方法之前，该指示器一直处于动画状态。
    
- **stopAnimating** 方法

    停止活动指示器的动画。
    
    调用此方法可停止活动指示器的动画，该动画从调用 `startAnimating` 开始。当动画停止时，活动指示器是隐藏的，除 `hidesWhenStopped` 属性的为 `NO`。
    
- **hidesWhenStopped** 属性

    BOOL 类型，指示活动指示器动画停止时是否隐藏自身。默认值为 `YES`。
    
    如果此属性的值是 `YES`，当活动指示器没有动画时，其自身将隐藏。如果此属性的值为 `NO`，则在动画停止时接收器不会被隐藏。
    
- **animating** 属性

    BOOL 类型，只读。指示活动指示器当前是否正在运行其动画。
    
    
    


