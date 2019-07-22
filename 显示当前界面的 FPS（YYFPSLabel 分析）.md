# 显示当前界面的 FPS（YYFPSLabel 分析）

## YYFPSLabel 

[YYFPSLabel](https://github.com/yehot/YYFPSLabel/blob/master/YYFPSLabel/YYFPSLabel/YYFPSLabel.m) 是大神 👨🏻‍💻 **[ibireme](https://blog.ibireme.com/author/ibireme/)** 开发的 🛠 **[YYKit](https://github.com/ibireme/YYKit)** 库中的一个用来显示当前界面的 FPS 值的工具。

### 实现原理

YYFPSLabel 主要是使用了 CADisplayLink 这个类，它是一个能够以和屏幕刷新率相同的频率将内容绘制到屏幕上的定时器。并且它能够绑定一个目标对象，在每一次运行循环时调用目标对象的指定方法。并且 CADisplayLink 对象的 `timestamp` 属性能够保存当前最后一帧的时间值。

YYFPSLabel 的原理是：`刷新次数 / 刷新时间 = 刷新频率`。

### 核心代码分析

```Objective-C
- (void)tick:(CADisplayLink *)link {
// 第一次调用方法时，记录显示第一帧时的时间值。
    if (_lastTime == 0) {           
        _lastTime = link.timestamp;     
        return;
    }

    _count++;   //每过一帧（即调用一次方法）计数加一。
    NSTimeInterval delta = link.timestamp - _lastTime;  // 第 n 帧的时间值减去第一帧的时间值。
    if (delta < 1) return;      // 如果时间小于 1s，则返回。

    _lastTime = link.timestamp;     // 记录当前帧的时间值。
    CGFloat fps = _count / delta;   // fps = 帧数 / 时间差
    _count = 0;
    
    CGFloat progress = fps / 60.0;
    UIColor *color = [UIColor colorWithHue:0.27 * (progress - 0.2) saturation:1 brightness:0.9 alpha:1];
}
```

### 扩展延伸

在 YYFPSLabel 中的为 CADisplayLink 绑定目标对象和选择器方法的时候，使用时是如下代码：

```Objective-C
_link = [CADisplayLink displayLinkWithTarget:[YYWeakProxy proxyWithTarget:self] selector:@selector(tick:)];
```

参数一中传递的是：`[YYWeakProxy proxyWithTarget:self]` 而不是直接传递的 `self` 或 `weakSelf`，因为 CADisplayLink 的 `displayLinkWithTarget:selector:` 方法会强引用参数一的对象，而后两者都不能解决循环引用的问题。

当 CADisplayLink 被添加到 NSRunLoop 中时，CADisplayLink 对象会被 NSRunLoop 强引用，而目标对象（target）也会被 CADisplayLink 强引用。由于 NSRunLoop 是始终存在的，所以 CADisplayLink 对象也会一直存在，进而导致目标对象也不会被释放。虽然 CADisplayLink 中的 `invalidate` 方法能够将其自身从 NSRunLoop 中删除，但是在 YYFPSLabel 中没有合适的时机能够调用此方法。

所以只能够在调用 `displayLinkWithTarget:selector:` 方法的时候使用弱引用。在 YYFPSLabel 中，使用的是 YYWeakProxy。

## YYWeakProxy







