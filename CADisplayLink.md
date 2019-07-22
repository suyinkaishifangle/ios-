# CADisplayLink

CADisplayLink 继承自 NSObject，称为「**显示链接（Display Link）**」。它是一个能够以和屏幕刷新率相同的频率将内容绘制到屏幕上的定时器。

应用程序会初始化一个显示链接，并提供一个目标对象和一个选择器，在屏幕刷新时调用。为了使显示循环与显示同步，应用程序会使用 `addToRunLoop:forMode:` 方法将其添加到「**运行循环（Run Loop）**」中。

一旦显示链接以特定的模式添加到运行循环之后，当需要更新屏幕内容时，运行循环就会调用显示链接绑定的目标对象上的选择器。目标对象可以读取显示链接的 `timestamp` 属性来检索显示最后一帧的时间。

`duration` 属性提供 `maximumFramesPerSecond` 属性（`maximumFramesPerSecond` 是 UIScreen 的属性，表示屏幕能够达到的每秒最大帧数）的每两帧之间的时间量。解释一下：在 iOS 上 `maximumFramesPerSecond` 属性的值通常是 `60`，而 `duration` 属性就表示这 60 帧中每两帧之间的时间间隔，也就是屏幕每次刷新之间的时间间隔。在目标对象的选择器一次都未被调用之前，持续时间的值是未定义的（为 `0`）。

使用 `targetTimestamp - timestamp` 计算实际帧持续时间。可以在应用程序中使用实际帧长来计算显示的帧率、下一帧显示的大致时间，并调整绘图行为，以便及时准备下一帧以便显示。

通过 `pause` 属性开控制显示链接的运行。此外，如果应用程序无法在提供的时间内提供帧，则可能需要选择较慢的帧速率。与跳过帧的应用程序相比，具有虽然慢但一致的帧速率的应用程序对用户来说会显得更平滑。可以通过设置 `preferredFramesPerSecond` 属性来定义每秒的帧数。当应用程序使用完显示链接后，它应调用 `invalidate` 方法将其从所有运行循环中删除并将其与目标取消关联。

iOS设备的屏幕刷新频率是固定的，CADisplayLink 在正常情况下会在每次刷新结束都被调用，精确度相当高。

> ⚠️ **注意**
> 
> CADisplayLink 不能够被继承。

## 方法和属性

- **displayLinkWithTarget:selector:** 类方法

    返回一个新的显示链接。
    
    - 参数一：id 类型，更新屏幕时要通知的对象。
    - 参数二：选择器，调用目标的方法。
    - 返回值：CADisplayLink 类型，一个新的显示链接。**新构建的显示链接会保留参数一对象**。

    要在目标上调用的选择器必须是具有以下命名的方法：
    
    ```Objective-C
    - (void)selector:(CADisplayLink *)sender;
    ```
    
    其中 `sender` 参数是此方法返回的显示链接。
    
- **addToRunLoop:forMode:** 方法

    使用指定的模式将显示链接添加到运行循环中。
    
    - 参数一：NSRunLoop 类型，与显示链接关联的运行循环。
    - 参数二：NSRunLoopMode 类型，将显示链接添加到运行循环的模式。

    可以将显示链接与多种输入模式相关联。当运行循环以指定的模式执行时，显示链接会在需要新帧时通知目标。
    
    运行循环保留显示链接。要从所有运行循环中删除显示链接，请向显示链接发送 `invalidate` 消息。
    
- **removeFromRunLoop:forMode:** 方法

    从指定模式的运行循环中删除显示链接。
    
- **invalidate** 方法

    从所有运行循环模式中删除显示链接。
    
    从所有运行循环模式中删除显示链接会使它被运行循环释放。显示链接也会释放目标对象。此方法是线程安全的，这意味着它可以从与运行显示链接的线程分开的线程调用。
    
- **duration** 属性

    CFTimeInterval 类型（即 double 类型），只读。屏幕刷新之间的时间间隔。
    
    这个值是固定的，并不会受性能影响。
    
- **timestamp** 属性

    CFTimeInterval 类型，只读。显示最后一帧的时间值。
    
    目标使用此属性的值来计算下一帧应该显示什么。例如：显示电影的应用程序可能会使用 `timestamp` 属性来计算下一个要显示的视频帧。自定义动画的应用程序可能使用 `timestamp` 来确定在即将到来的帧中显示对象的位置和方式。
    
- **targetTimestamp** 属性

    CFTimeInterval 类型，只读。显示下一帧的时间值。
    
    可以使用此属性来取消或暂停长时间运行的进程，这些进程可能会超出帧之间的可用时间，以保持一致的帧速率。
    []()