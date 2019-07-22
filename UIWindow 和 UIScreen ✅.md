# UIWindow 和 UIScreen ✅

## UIWindow

UIWindow 也称为「窗口」，它是应用程序用户界面（用户可见的最底层）的背景，并且它也将事件分派给视图。它是 UIView 的子类，但是它没有任何视觉外观，只是作为一个容器。一个 iOS 程序之所以能显示到屏幕上，完全是因为它有 UIWindow。如果没有 UIWindow，就看不见任何 UI 界面（实际效果就是整个屏幕都是黑的）。

UIWindow 是创建的第一个视图，只有当需要做以下事情时才创建它：

- 提供一个主窗口来显示应用程序的内容。
- 创建额外的窗口来显示额外的内容。

UIWindow 对象的作用有以下几点：

- 它包含应用程序的可见内容。
- 它在向视图和其他应用程序对象传递触摸事件中起着关键作用。
- 它在应用程序的视图控制器方向改变时发挥作用。

通常，在 Xcode 中创建新的 iOS 项目自动提供应用程序的主窗口。新的 iOS 项目使用故事板来定义应用程序的视图。 故事板需要在 UIApplicationDelegate 的代理对象（新 iOS 项目默认是 AppDelegate.h 和 AppDelegate. m 文件）上存在窗口属性，Xcode 模板会自动提供该属性。 如果应用程序不使用故事板，则必须自己创建此窗口对象。

iOS 程序启动完毕后，创建窗口对象，接着创建窗口的 rootViewController 属性中的视图控制器和视图，最后将视图控制器的视图添加到窗口上，于是视图控制器的视图就显示在屏幕上了。窗口本身没有任何视觉外观，窗口托管一个或多个视图，这些视图由窗口的根视图控制器管理。

> 🔥 **提示**
> 
> 虽然窗口本身没有任何视觉外观，但是本质上窗口是一个 UIView 类型，所以还是有 background 等属性可以为其设置外观。不过没必要这样做，因为显示在屏幕上的是最终是 UIWindow 的根视图控制器的视图。

大多数 iOS 应用程序在其生命周期内仅创建和使用一个窗口（除开由系统创建的所有其他窗口，例如：系统创建的用于显示状态栏的窗口、用于显示键盘的窗口等）。如果应用程序支持使用外部显示器进行视频输出，则可以创建另一个窗口以在该外部显示器上显示内容。 参考 [Displaying Content on a Connected Screen](https://developer.apple.com/documentation/uikit/windows_and_screens/displaying_content_on_a_connected_screen?language=objc)。

关于窗口的更多介绍：[View Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/CreatingWindows/CreatingWindows.html#//apple_ref/doc/uid/TP40009503-CH4-SW1)。

**以编程方式创建窗口**

以编程方式创建应用程序的主窗口，在遵守 UIApplicationDelegate 协议的代理对象的 application:didFinishLaunchingWithOptions: 方法中创建主窗口。创建窗口时，应始终将窗口大小设置为屏幕的完整边界。不应该减小窗口的大小。示例代码如下：

```Objective-C
self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
```

### UIWindow 的 Z 轴位置

每个窗口对象都有一个可配置的 windowLevel 属性，用于确定该窗口相对于其他窗口的定位方式（即在 Z 轴上，级别高的窗口显示在级别低的窗口的上层）。在大多数情况下，不需要更属性的值。

新窗口在创建时自动分配到正常窗口级别。较高的窗口级别用于需要浮动在应用程序内容之上的信息，例如系统状态栏或弹窗警告消息。虽然开发者可以自己为窗口设置级别，但系统通常会在使用特定接口时自定设置窗口级别。例如，当显示或隐藏状态栏或显示警报视图时，系统会自动创建所需的窗口以显示这些项目。

- **windowLevel** 属性

  UIWindowLevel 类型，窗口在 Z 轴上的位置，默认值为 UIWindowLevelNormal。

  UIWindowLevel 提供窗口沿 Z 轴的相对分组，无法保证同一级别的窗口之间在 Z 轴上的顺序。

  UIWindowLevel 是 CGFloat 类型的别名，用户窗口之间的定位。以下三个常量都是 UIWindowLevel 类型：

  - UIWindowLevelNormal：默认级别，大多数内容使用此级别，包括应用程序的主窗口。
  - UIWindowLevelStatusBar：状态栏的级别，此级别的窗口显示在应用程序主窗口的上层。
  - UIWindowLevelAlert：警告视图级别，此级别的窗口显示在状态栏的上层。

  > 🔥 **提示**
  >
  > 实际上应用程序的状态是单独显示在一个窗口上的，使用的就是 UIWindowLevelStatusBar 级别。

### 监视 UIWindow 更改

如果要监视应用程序中窗口的显示或消失，可以使用与窗口可见性相关的通知。

- **UIWindowDidBecomeVisibleNotification**

    当窗口已经显示时发布通知。此通知的 object 是已经显示的窗口对象，并且不包含 userInfo 字典。

- **UIWindowDidBecomeHiddenNotification**

    当窗口已经隐藏时发布通知。此通知的 object 是已经隐藏的窗口对象，并且不包含 userInfo 字典。

应用程序之间的切换不会为窗口发布与可见性相关的通知。窗口可见性的变化反映了窗口隐藏属性的变化，并且只反映窗口在应用程序中的可见性。当应用程序进入后台执行状态时，也不会发布这些通知。即使应用程序在后台时屏幕上没有显示窗口，但在应用程序的上下文中仍然可以看到它。

- **UIWindowDidBecomeKeyNotification**

    当窗口已经成为主窗口时发布通知。此通知的 object 是已经成为主窗口的窗口对象，并且不包含 userInfo 字典。

- **UIWindowDidResignKeyNotification**

    当窗口已经不是主窗口时发布通知。此通知的 object 是刚放弃其主窗口状态的窗口对象，并且不包含 userInfo 字典。

这两个通知可帮助应用程序跟踪哪个窗口是主窗口 —— 即哪个窗口当前正在接收键盘事件和其他非触摸相关事件。触摸事件被传递到发生触摸的窗口，而没有相关坐标值的事件（即非触摸事件）将被传递到应用程序的主窗口。

### UIWindow 属性和方法

- **rootViewController** 属性

    UIViewController 类型，窗口的根视图控制器。默认值为 nil。

    根视图控制器提供窗口的内容视图。 将视图控制器分配给此属性会将视图控制器的视图加载为窗口的内容视图。 新内容视图的大小跟随窗口大小，随窗口大小的变化而变化。 如果改变了 rootViewController 的值，那么窗口上原来视图控制器的视图也将被移除。

- **screen** 属性

    UIScreen 类型，表示显示窗口的屏幕。

    默认情况下，此属性设置为主设备屏幕。如果将其他屏幕附加到设备，则可以指定其他屏幕对象以在该屏幕上显示该窗口。 窗口始终只显示在一个屏幕上。

    将窗口从一个屏幕移动到另一个屏幕是一项耗费性能的操作，不应该在性能敏感的代码中完成。建议在第一次显示窗口之前更改屏幕。

**主窗口**

- **keyWindow** 属性

    BOOL 类型，只读，指示窗口是否是应用程序的主窗口。当前窗口是主窗口，则值为 YES，否则为 NO。

    主窗口是接收键盘和其他非触摸相关事件的窗口。程序中每个时刻只能有一个 UIWindow 是 keyWindow。如果某个 UIWindow 内部的文本框不能输入文字，可能是因为这个 UIWindow 不是 keyWindow。

- **makeKeyAndVisible** 方法

    显示窗口并使其成为主窗口。

    用于显示当前窗口并将其放置在同一级别或更低级别的所有其他窗口的前面。

- **makeKeyWindow** 方法

    使接收者成为主窗口。
    
    使用此方法设置主窗口而不更改其可见性。主窗口接收键盘和其他非触摸相关事件。

- **becomeKeyWindow** 方法

    通知窗口它已成为主窗口。
  
    切勿直接调用此方法。系统调用此方法并发布 UIWindowDidBecomeKeyNotification 通知以便让窗口知道它何时成为主窗口。 此方法的默认实现不执行任何操作，但子类可以覆盖它并使用它来执行与成为主窗口相关的任务。

- **resignKeyWindow** 方法
    
    通知窗口它不再是主窗口。

    切勿直接调用此方法。系统调用此方法并发布 UIWindowDidResignKeyNotification 通知以便让窗口知道它何时不再是主窗口。此方法的默认实现不执行任何操作，但子类可以覆盖它并使用它来执行与不再是主窗口相关的任务。

**转换坐标**

⌛️

### 使用纯代码方式创建 UIWindow

在使用 Xcode 新建的项目中的 UIApplication 代理对象 —— AppDelegate 对象中默认提供了一个 UIWindow 类型的属性 window。这个对象就是用来显示在 iOS 设备屏幕上的窗口。

项目默认是使用名为 `Main.storyboard` 文件作为应用程序的窗口。如果不使用 storyboard 文件作为窗口，那么就需要用到 AppDelegate 对象中的 window 属性来创建一个 UIWindow 对象作为窗口。并且在 ⚙️ **Targets** ➤ **General** ➤ **Deployment Info** ➤ **Main Interface** 中删除 `Main.storyboard`，这样当应用程序启动时，会使用 AppDelegate 中的 window 作为显示在屏幕上的窗口。

## UIScreen

UIScreen 继承自 NSObject，其定义与基于硬件显示相关的属性对象。

iOS 设备具有一个主屏幕和零个或多个附加屏幕。使用此类可以获取连接到设备且显示的屏幕对象。每个屏幕对象定义关联显示的边界矩形和其他一些的属性，例如亮度等。在 iOS 8 及更高版本中，屏幕的边界属性将屏幕的界面方向考虑在内。这意味着纵向设备的边界可能与横向设备的边界不同。在 iOS 8 之前，屏幕边界矩形始终反映相对于纵向方向的屏幕尺寸。将设备旋转到横向或倒置方向不会改变边界。

因为 UIScreen 大部分内容涉及到屏幕相关的处理，而一般 iOS 应用无需处理外接屏幕，所以这里暂不详细介绍，关于 UIScreen 的更多介绍请参考官方文档 [UIScreen](https://developer.apple.com/documentation/uikit/uiscreen?language=objc)。

### UIScreen 属性和方法

#### 类方法

- **mainScreen** 类属性

    UIScreen 类型，只读。返回表示设备屏幕的主屏幕对象。

- **screens**  类属性

    NSArray 类型，只读。返回包含连接到设备的所有屏幕的数组。

    主屏幕在数组中的索引始终为 `0`。

#### 获取屏幕相关信息

- **bounds** 属性

    CGRect 类型，只读。屏幕的边界矩形，以点为单位。

    此矩形在当前坐标空间中指定，该空间考虑了对设备有效的任何界面旋转。 因此，当设备在纵向和横向之间旋转时，此属性的值可能会更改。

- **scale** 属性

    CGFloat 类型，只读。与屏幕相关的自然比例因子。

    此值反映了从默认逻辑坐标空间转换为此屏幕的设备坐标空间所需的比例因子。使用「点」测量默认逻辑坐标空间。对于 Retina 显示器，比例因子可以是 `2.0` 或 `3.0`，并且一个点可以分别由 `4` 个或 `9` 个像素表示。对于标准分辨率显示，比例因子为 `1.0`，一个点等于一个像素。

#### 设置显示器的亮度

- **brightness** 属性

    CGFloat 类型，屏幕的亮度级别。
  
    仅在主屏幕上支持此属性。此属性的值应为介于 `0.0` 和 `1.0` 之间的数字。

    无论应用程序是否关闭，应用程序所做的亮度更改将一直有效，直到设备被锁定为止。系统亮度（用户可以在「设置」或「控制中心」中设置）在下次打开显示屏时恢复。
