# UIScrollView

UIScrollView 继承自 UIView，称为「滚动视图」。它能够滚动和缩放所包含视图。

UIScrollView 是几个 UIKit 类的父类，包括 UITableView 和 UITextView。UIScrollView 对象包含一个内容视图，内容视图包含的内容是整个滚动视图的内容。而滚动视图实际在屏幕上显示的内容只是内容视图的一部分。当滚动视图滚动时，滚动视图的框架矩形的原点就在内容视图上调整位置，以此来显示内容视图不同的内容。

滚动视图本身除了显示垂直和水平滚动指示器外不进行绘图。滚动视图必须知道内容视图的大小，以便知道何时停止滚动。默认情况下，当滚动超过内容的边界时，它会「弹回」。

## UIScrollView 处理触摸事件原理

当用户在滚动视图的一个子视图上触摸时，滚动视图并不知道用户是想要滑动内容视图还是点击对应子视图，所以在触摸的瞬间，事件 UIEvent 从 UIApplication 传递到 UIScrollView，其会先将该事件拦截而不会立即传递给对应的子视图，同时启动一个 150ms 的计时器，通过启动计时器暂时拦截触碰事件，并监听用户接下来的行为。

当计时器倒计时结束前，如果用户的手指发生了移动，则直接滚动内容视图，而不会将该事件传递给对应的子视图；当计时器倒计时结束时，如果用户的手指位置没有改变，则调用自身的 `touchesShouldBegin:withEvent:inContentView:` 方法询问是否将事件传递给对应的子视图（如果返回 `NO`，则该事件不会传递给对应的子视图；如果返回 `YES`，则该事件会传递给对应的子视图。默认为 `YES`。)

当事件被传递给子视图后，如果手指位置又发生了移动，则调用自身的 `touchesShouldCancelInContentView:` 方法询问是否取消已经传递给子视图的事件。

## UIScrollView 属性和方法

### 管理内容大小和偏移量

- **contentSize** 属性

    CGSize 类型，内容视图的大小。默认值为 `CGSizeZero`。
    
    **必须设置此属性**，否则显示不正确。
    
    > ⚠️ **注意**
    >
    > 此属性的值必须大于滚动视图的 frame 值，否则不会滚动。例如：此属性的宽度小于等于滚动视图本身的宽度，但是高度大于滚动视图本身的高度，则只能够垂直滚动，无法水平滚动。

- **contentOffset** 属性

    CGPoint 类型，内容视图的原点从滚动视图的原点处偏移后的点。默认值为 `CGPointZero`。
    
- **setContentOffset:animated:** 方法

    设置内容视图原点相对于滚动视图原点的偏移量。
    
    - 参数一：CGPoint 类型，从内容视图的原点偏移的点（以点表示）。
    - 参数二：BOOL 类型，是否显示过渡动画。
    
### 管理内容内嵌行为

- **contentInset** 属性

    UIEdgeInsets 类型，内容视图从安全区域或滚动视图边缘内嵌的自定义距离。默认值为 `UIEdgeInsetsZero`。
    
    使用此属性可扩展滚动视图与内容视图边缘之间的间距。默认情况下，UIKit 会自动调整内容插入以计算重叠条。可以使用此属性进一步扩展该距离，以适应自定义内容。使用`adjustContentInset` 属性获取总调整值（即安全区域加上自定义的内嵌值）。要更改安全区域的应用方式，需要修改 `contentInsetAdjustmentBehavior` 属性。 
    
- **adjustedContentInset** 属性

    UIEdgeInsets 类型，只读。从内容嵌入和滚动视图的安全区域派生的内嵌值。
    
    使用此属性可获得用于绘制内容的调整区域。`contentInsetAdjustmentBehavior` 属性确定是否将安全区域的内嵌值包含在调整中。然后将安全区域内嵌值添加到 `contentInset` 属性中的值以获取此属性的最终值。
    
- **contentInsetAdjustmentBehavior** 属性

    [UIScrollViewContentInsetAdjustmentBehavior](https://developer.apple.com/documentation/uikit/uiscrollviewcontentinsetadjustmentbehavior?language=objc) 类型，用于确定调整后的内容偏移的行为。默认值为`UIScrollViewContentInsetAdjustmentAutomatic`。
    
    此属性指定如何使用安全区域内嵌值来修改滚动视图的内容区域。
    
- **adjustedContentInsetDidChange** 方法

    当滚动视图的内容内嵌值调整时调用。

    
### 配置滚动视图

- **scrollEnabled** 属性
    
    BOOL 类型，确定是否启用滚动。默认值为 `YES`。
    
    如果此属性的值为 `YES`，则启用滚动，如果为 `NO`，则禁用滚动。滚动禁用时，滚动视图不接受触摸事件，它将触摸事件转发给响应者链。
    
- **directionalLockEnabled** 属性

    BOOL 类型，确定是否在特定方向上禁用滚动。默认值为 `NO`。
    
    如果此属性为 `NO`，则允许在水平和垂直方向上滚动。如果此属性为 `YES` 并且用户开始沿一个大致方向（水平或垂直）拖动，则滚动视图将禁用向另一个方向滚动。如果拖动方向是对角线，则滚动将不会被锁定，并且用户可以向任何方向拖动，直到拖动完成。 
    
- **pagingEnabled** 属性

    BOOL 类型，确定是否为滚动视图启用了分页。默认值为 `NO`。
    
    如果此属性的值为 `YES`，则滚动视图会在用户滚动时停止滚动视图边界的倍数。
    
- **scrollsToTop** 属性

    BOOL 类型，控制是否启用滚动到顶部的手势。默认值为 `YES`。
    
    滚动到顶部的手势是点击状态栏。当用户进行此手势时，系统会要求最靠近状态栏的滚动视图滚动到顶部。如果此属性设置为`NO`，则其代理从 `scrollViewShouldScrollToTop:` 方法返回 `NO`，如果内容已经在顶部，没有任何反应。滚动视图滚动到内容视图的顶部后，它会向代理对象发送 `scrollViewDidScrollToTop:` 消息。
    
    > 🔥 **提示**
    >
    > 如果屏幕上有多个滚动视图，且每个都将其 `scrollsToTop` 属性设置为 `YES`，则滚动到顶部的手势无效。
    
- **bounces** 属性

    BOOL 类型，用于控制滚动视图是否跳过内容视图边缘并再次返回。默认值为 `YES`。
    
    如果此属性的值为 `YES`，则滚动视图在遇到内容边界时会弹回。视觉上弹跳表示滚动已到达内容的边缘。如果值为 `NO`，则滚动会立即停止在内容边界处而不会弹跳。
    
- **alwaysBounceVertical** 属性

    BOOL 类型，用于确定垂直滚动到达内容结尾时是否始终发生弹跳。默认值为 `NO`。
    
    如果此属性设置为 `YES` 并且 `bounces` 属性为 `YES`，则即使内容小于滚动视图的边界，也允许垂直拖动。
    
- **alwaysBounceHorizontal** 属性

    BOOL 类型，用于确定水平滚动到达内容视图末尾时是否始终发生弹跳。默认值为 `NO`。
    
    如果此属性设置为 `YES` 并且 `bounces` 属性为 `YES`，则即使内容小于滚动视图的边界，也允许水平拖动。
    
- **scrollRectToVisible:animated:** 方法

    滚动内容视图的特定区域，使其在滚动视图中可见。
    
    - 参数一：CGRect 类型，指定的矩形区域。
    - 参数二：BOOL 类型，是否显示过渡动画。

    此方法滚动内容视图，使传入的参数一的区域在滚动视图中可见。如果该区域已经可见，则该方法不执行任何操作。
    
### 获取滚动状态

- **tracking** 属性

    BOOL 类型，只读。用户是否触摸了内容来启动滚动。
    
    如果用户触摸了内容视图但可能尚未开始拖动它，则此属性的值为 `YES`。
    
- **dragging** 属性

    BOOL 类型，只读。用户是否已开始滚动内容。
    
    此属性所持有的值可能需要一些时间或滚动距离才能设置为 `YES`。
    
- **decelerating** 属性

    BOOL 类型，只读。用户抬起手指后内容是否在滚动视图中继续移动。
    
    如果用户未拖动内容但仍在进行滚动，则返回值为 `YES`。
    
- **decelerationRate** 属性

    UIScrollViewDecelerationRate 类型，指定确定用户抬起手指后的减速率。
    
    UIScrollViewDecelerationRate 是类型别名，其有两个具体常量：
    
    - `UIScrollViewDecelerationRateNormal`：滚动视图的默认减速率。
    - `UIScrollViewDecelerationRateFast`：滚动视图的快速减速率。
    
### 管理滚动指示器和刷新控件        

- **indicatorStyle** 属性

    UIScrollViewIndicatorStyle 类型，滚动指示器的样式。默认样式为 `UIScrollViewIndicatorStyleDefault`。
    
    UIScrollViewIndicatorStyle 是一个枚举类型，具体如下表：
    
    | UIScrollViewIndicatorStyle 枚举值| 说明 |
    | :-: | :-: |
    | UIScrollViewIndicatorStyleDefault | 滚动指示器的默认样式，灰色，带有白色边框。 |
    | UIScrollViewIndicatorStyleBlack | 深灰色且比默认样式窄。 |
    | UIScrollViewIndicatorStyleWhite | 白色且比默认样式窄。 |
    
- **scrollIndicatorInsets** 属性

    UIEdgeInsets 类型，滚动指示器从滚动视图边缘插入的距离。默认值为 `UIEdgeInsetsZero`。
    
- **showsHorizontalScrollIndicator** 属性

    BOOL 类型，确定水平滚动指示器是否可见。默认值为 `YES`。
    
    指示器在滚动视图进行滚动的时候会显示，停止滚动后则会慢慢消失。
    
- **showsVerticalScrollIndicator** 属性

    BOOL 类型，确定垂直滚动指示器是否可见。默认值为 `YES`。
    
    指示器在滚动视图进行滚动的时候会显示，停止滚动后则会慢慢消失。
    
- **refreshControl** 属性

    UIRefreshControl 类型，与滚动视图关联的刷新控件。
    
- **flashScrollIndicators** 方法 

    短暂地显示一下滚动指示器（一般用来提醒用户有滚动视图可以滚动）。
    
### 管理触摸
    
- **touchesShouldBegin:withEvent:inContentView:** 方法

    手指触摸显示的内容时调用，子类重写改方法来执行自定义操作。
    
    - 参数一：NSSet 类型，一组 UITouch 对象，表示事件起始阶段的触摸。
    - 参数二：UIEvent 类型，触摸中的触摸对象所属的事件对象。
    - 参数三：UIView 类型，内容视图中触摸按下手势发生的子视图。
    - 返回值：BOOL 类型，如果不需要滚动视图向视图发送事件消息，返回 `NO`。如果需要视图接收这些消息，请返回 `YES`。

    此方法默认实现返回 `YES`，即 UIScrollView 对象会默认调用触摸发生的子视图的 UIResponder 事件处理方法（即默认滚动视图的子视图会接受触摸事件）。
    
- **touchesShouldCancelInContentView:** 方法

    是否取消已经传递给子视图的事件。
    
    - 参数一：UIView 类型，正在触摸的内容子视图。
    - 返回值：BOOL 类型，如果参数一是 UIControl 对象，则默认返回值为 `NO`；否则返回 `YES`。

    滚动视图在开始向内容视图发送跟踪消息后立即调用此方法。如果从此方法收到 `NO`，则会停止拖动并将触摸事件发送到内容子视图；反之则不会将触摸事件发送到内容子视图。如果 `canCancelContentTouches` 属性的值为 `NO`，则滚动视图不会调用此方法。
    
- **canCancelContentTouches** 属性

    BOOL 类型，用于控制内容视图是否始终跟踪触摸。默认值为 `YES`。
    
    如果此属性的值为 `YES` 并且内容中的一个视图已开始跟踪触摸，如果此时用户的触摸足以让滚动视图滚动，则视图将接收 `touchesCancelled:withEvent:` 消息，而滚动视图将触摸作为滚动处理。如果此属性的值为 `NO`，则在内容视图开始跟踪触摸它的手指后，无论手指如何移动，滚动视图都不会滚动。
    
- **delaysContentTouches** 属性

    BOOL 类型，用于确定滚动视图是否会延迟触摸手势的处理。默认值为 `YES`。
    
    如果此属性的值为 `YES`，则滚动视图会延迟处理触摸手势，直到它可以确定滚动是否是目的。如果值为 `NO`，则滚动视图立即调用 `touchesShouldBegin:withEvent:inContentView:` 方法。
    
### ⌛️缩放和平移

### ⌛️管理键盘

### ⌛️管理索引

## UIScrollViewDelegate 协议

UIScrollViewDelegate 协议声明的方法允许采用代理对象响应来自 UIScrollView 类的消息，从而响应并影响滚动视图内容的滚动，缩放，减速和滚动动画等操作。滚动视图的 `delegate` 属性用来设置遵守 UIScrollViewDelegate 协议的代理对象。

https://developer.apple.com/documentation/uikit/uiscrollviewdelegate?language=objc

- **scrollViewDidScroll:** 方法

    当用户在滚动视图中滚动内容视图时告诉代理。
    
    - 参数一：UIScrollView 类型，滚动视图。
    
    代理通常实现此方法从滚动视图获取内容偏移的更改并绘制内容视图的受影响部分。
    
- **scrollViewWillBeginDragging:** 方法

    滚动视图即将开始滚动内容时告诉代理。
    
    - 参数一：UIScrollView 类型，滚动视图。

    在拖动一小段距离之前，代理可能不会收到此消息。
    
- **scrollViewWillEndDragging:withVelocity:targetContentOffset:** 方法

    当用户完成滚动内容时告诉委托。

    - 参数一：UIScrollView 类型，滚动视图。
    - 参数二：CGPoint 类型，触摸释放时滚动视图的速度（以点为单位）。
    - 参数三：CGPoint 类型，滚动动作减速到停止时的预期偏移量。

- **scrollViewDidEndDragging:willDecelerate:** 方法

    拖动在滚动视图中结束时告诉委托。
    
    - 参数一：UIScrollView 类型，滚动视图。