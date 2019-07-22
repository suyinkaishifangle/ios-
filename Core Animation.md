# Core Animation

> 📚 官方文档：**[Core Animation Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514)**

Core Animation 是一个框架，用于渲染，构图并动画视觉元素。

Core Animation 是一个可以在 iOS 和 macOS 上使用的图形渲染和动画基础框架。可以用它来动画应用程序的视图和其他视觉元素。而开发者所要做的就是配置一些动画参数（比如开始和结束点），并告诉 Core Animation 开始。Core Animation 完成了剩下的工作，将大部分工作交给专用图形硬件，来加速渲染。Core Animation 能够提供高帧率和流畅的动画，而不会给 CPU 带来负担和降低应用程序的速度。

Core Animation 位于 AppKit 和 UIKit 之下，并紧密集成到 Cocoa 和 Cocoa Touch 的视图工作流程中。当然，Core Animation 也有一些接口，可以扩展应用程序视图的功能，让开发者对应用程序的动画有更精细的控制。

![结构](media/15535001760861/%E7%BB%93%E6%9E%84.png)

