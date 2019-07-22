# iOS应用程序的生命周期

> 环境：<i class="fas fa-laptop"></i> Xcode 10.0 &nbsp;&nbsp;<i class="fas fa-mobile-alt"></i> iOS 12.0

---

## UIApplication 和 UIApplicationDelegate 

### UIApplication 简介

UIApplication 是 UIKit 框架中的一个单例类，它是 iOS 中应用程序运行时控制和协调的集中点。

UIApplication 是应用程序的核心，每一个程序在运行期间有且仅有一个 UIApplication（或子类）的一个对象。该对象接受事件，并把所有用户事件放入事件队列，逐个处理，并把部分事件转交给代理对象。通过调用 `sharedApplication` 类属性来访问该对象。

1. UIApplication 对象是应用程序的象征，一个 UIApplication 对象就代表一个应用程序。
2. 每一个应用都有自己的 UIApplication 对象，而且是单例的。如果试图在程序中新建一个 UIApplication 对象，那么将报错提示。
3. 通过 `sharedApplication` 类属性可以获得这个单例对象。
4. 一个iOS程序启动后创建的第一个对象就是 UIApplication 对象。
5. 利用 UIApplication 对象，能进行一些应用级别的操作，比如设置应用右上角的角标、调用其他应用程序。

大多数应用程序不需要子类化 UIApplication。只需要使用应用程序代理（遵守 UIApplicationDelegate 的对象）来管理系统和应用程序之间的交互。

### UIApplicationDelegate 简介

UIApplicationDelegate 是 UIKit 框架中的协议，是单例 UIApplication 的代理对象遵守的协议，负责管理应用程序的生命周期，接受相应的系统事件。UIApplicationDelegate 代理对象实际上是应用程序的根对象。像 UIApplication 对象本身一样，UIApplicationDelegate 代理对象也是一个单例对象，并且在运行时始终存在。

### UIApplicationDelegate 的代理方法

每次新建完项目，都有个带一个叫 `AppDelegate` 的类，它就是 UIApplication 的代理类，AppDelegate 类默认已经遵守了 UIApplicationDelegate 协议，已经是 UIApplication 的代理。

UIApplicationDelegate 也用来管理应用的状态转换，对于发生的每个状态更改（参考下文 iOS 应用程序的五种状态），系统会调用应用程序代理的适当方法。状态转换发生时调用以下方法。下面是代理方法：

> 表1：UIApplicationDelegate 的代理方法

| 调用时间 | 调用方法 | 作用 |
| :--: | :-- | :-- | 
| 应用启动时 | application:<br>willFinishLaunchingWithOptions: | 告诉代理：启动过程完成<br>应用程序可以运行。 |
| 应用启动时 | application:<br>didFinishLaunchingWithOptions: | 告诉代理：应用的启动过程已经开始<br>但还没有状态复位。 |  
| 应用即将进入前台时 | applicationWillEnterForeground: | 告诉代理：应用即将进入前台。 |
| 应用已经转换到前台时 | applicationDidBecomeActive: | 告诉代理：应用程序已启动。 |
| 应用即将从活动状态<br />转换到非活动状态时 | applicationWillResignActive: | 告诉代理：应用即将变为非活动状态 |
| 应用已经转换到后台时 | applicationDidEnterBackground: | 告诉代理：应用程序现在在后台。 |
| 应用即将被销毁的时 | applicationWillTerminate: |  告诉代理：应用即将终止<br>（结束运行，销毁。不是挂起）。 |

> **注意**
> 
> 1. `applicationDidBecomeActive:` 方法在程序第一次启动或者程序恢复到前台状态时，都会先执行这个方法。
> 2. `applicationWillTerminate:` 方法仅在应用运行时（即没有进入挂起/暂停状态时）调用。如果应用已经被挂起，则不会调用此方法

## UIApplication 的应用级别的操作

### 设置应用图标右上角的角标（红色提醒数字）

设置 UIApplication 的 `applicationIconBadgeNumber` 属性可以改变应用图标的角标显示数字。

```ObjectiveC
@property(nonatomic) NSInteger applicationIconBadgeNumber; // 此属性的默认值为 0。设置为 0 表示隐藏角标显示。
```

代码实现及效果：

```ObjectiveC

```

### 设置网络活动指示器

网络活动指示器是指 iOS 系统状态栏上的一个旋转的指示器，用来指示当前的联网状态。不过现在大部分应用都没有设置这个指示器，只有些系统应用会出现这个指示器。

```ObjectiveC
@property(nonatomic, getter=isNetworkActivityIndicatorVisible) BOOL networkActivityIndicatorVisible; // 一个布尔值，用于打开或关闭网络活动的指示符。
```

代码实现及效果：

```ObjectiveC
// 获取 UIApplication 单例对象
UIApplication *application = [UIApplication sharedApplication];
/* 这里不能使用 [UIApplication *application = [UIApplication alloc]init] 
来初始化UIApplication 单例对象，因为一个应用只能存在一个 UIApplication 对象*/

// 设置网络活动指示器为显示状态
application.networkActivityIndicatorVisible = YES;
```

> 图1 网络活动指示器效果图

![网络活动指示器效果](https://blog-image-1256099768.cos.ap-chengdu.myqcloud.com/blog-image/%E7%BD%91%E7%BB%9C%E6%B4%(づ￣3￣)づ%E5%8A%A8%E6%8C%87%E7%A4%BA%E5%99%A8%E6%95%88%E6%9E%9C.png)

### 管理状态栏

UIApplication 对象还可以用来管理状态栏的显示。具体参考文章

### 使用 URL

UIApplication 有个的十分强大的方法：`openURL:options:completionHandler:` / `openURL:`

```ObjectiveC
- (BOOL)openURL:(NSURL *)url;  // 在指定的URL处打开资源 , iOS 10.0 弃用
- (void)openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options completionHandler:(void (^)(BOOL success))completion;  // 尝试异步打开指定URL处的资源
```

## iOS 应用程序的声明周期

### 生命周期的五种状态

要了解生命周期，必定要了解当前应用程序所处的状态。

>  表2：iOS 应用程序的五种状态

| 状态 | 描述 |
| :--: | :--: |
| Not Running（未运行状态） | 应用尚未启动或被用户或系统终止。 |
| Inactive（非活动状态）| 应用在前台运行，但没有收到事件（它可能正在执行其他代码）<br />应用通常只会在转换到其他状态时暂时保持此状态。 |
| Active（活动状态）| 应用正在前台运行并接收事件。这是前台应用程序的正常模式。 |
| Background（后台状态）| 应用正在执行代码，但在屏幕上不可见。<br />当用户退出应用时，系统会暂时将应用移至后台状态。 |
| Suspended（挂起/暂停状态）| 应用在内存中，但不执行代码。系统挂起后台应用，不执行任何操作。<br />系统可能随时清除挂起的应用，为其他应用腾出内存空间。 |

UIKit 应用程序总是处于五种状态之一（如 图2 所示）。应用程序开始不运行。**当用户点击应用图标启动应用程序（或者从后台状态恢复），应用程序首先短暂的进入非活动状态，然后再进入活动状态**（活动状态和非活动状态都被称为前台状态）。退出应用程序会将其移出屏幕并进入短暂的进去后台状态，然后应用进入挂起状态。最后系统可以自行决定终止已挂起/暂停的应用程序，并将其恢复到未运行状态。

> 图2：UIKit 应用程序的执行状态

![UIKit 应用程序的执行状态](https://blog-image-1256099768.cos.ap-chengdu.myqcloud.com/blog-image/UIKit%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E7%9A%84%E6%89%A7%E8%A1%8C%E7%8A%B6%E6%80%81.png)

> UIKit 框架
> 
> UIKit 框架为 iOS 或 tvOS 应用程序提供所需的基础架构。它提供了用于实现界面的窗口和视图体系结构，用于向应用程序提供多点触控和其他类型输入的事件处理基础结构，以及管理用户、系统和应用程序之间交互所需的主运行循环。该框架提供的其他功能包括动画支持，文档支持，绘图和打印支持，关于当前设备的信息，文本管理和显示，搜索支持，可访问性支持，应用程序扩展支持和资源管理。

### 管理生命周期事件 

UIKit 执行大部分启动应用程序并准备运行的工作。UIKit 创建用于管理应用程序的 UIApplication 对象，加载初始视图和视图控制器，并启动应用程序的主事件循环。UIKit 还会创建应用程序代理对象——一个符合 `UIApplicationDelegate` 协议的对象。

当应用程序从一种状态转换到另一种状态时，UIKit 会通知应用程序代理对象。使用应用程序代理来修改应用程序的行为以匹配其新状态。例如，从非运行状态转移到非活动状态时，启动时处理准备运行应用程序所需的任务。

> **注意**
> 
> 除非另有说明，否则只能从您的应用的主线程或主调度队列中使用 UIKit 类。此限制特别适用于从 UIResponder 派生的类或涉及以任何方式操纵应用程序用户界面的类。

### 应用程序启动时的执行顺序

启动一个应用需要一系列复杂的步骤，其中大部分步骤 UIKit 会自动处理。在启动顺序中，UIKit 会调用应用程序代理的方法，以便可以执行自定义任务。图3 和 图4 分别显示了从应用程序启动、初始化到进入前台和后台之间发生的一系列步骤。

#### 应用程序启动和初始化到进入前台的顺序

> 图3：应用程序启动和初始化到进入前台的顺序

![应用程序启动和初始化到进入前台的顺序](https://blog-image-1256099768.cos.ap-chengdu.myqcloud.com/blog-image/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E5%90%AF%E5%8A%A8%E5%92%8C%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%B0%E8%BF%9B%E5%85%A5%E5%89%8D%E5%8F%B0%E7%9A%84%E9%A1%BA%E5%BA%8F_2x.png)

1. 该应用程序是由用户显示启动的，或者由系统隐式启动的。

2. Xcode 提供的 `main` 函数调用 UIKit 的 `UIApplicationMain` 函数。

3. `UIApplicationMain` 函数创建 `UIApplication` 对象并设置 `UIApplicationDelegate` 代理。

4. UIKit 采用资源文件（如storyboard，xib等）加载应用程序的默认界面。

5. UIKit 调用应用程序代理的 `application:willFinishLaunchingWithOptions:` 方法初始化并设置资源文件。

6. UIKit 执行状态恢复，调用应用程序代理和视图控制器的其他方法。

7. UIKit 调用应用程序代理的 `application:didFinishLaunchingWithOptions:` 方法。

> **注意**
> 
> 如果没有在项目中采用资源文件（如storyboard，xib等）描述主界面的 UI 元素，则需要在`application:didFinishLaunchWithOptions:` 方法中，进行以下操作：
> 
> 1. 显示实例化 UIWindow 对象。
> 2. 设置 UIWindow 的根控制器。
> 3. 使 UIWindow 对象成为主窗口并显示（使用 `makeKeyAndVisible` 方法）。

#### 应用程序启动和初始化到进入后台的顺序

> 图4：应用程序启动和初始化到进入后台的顺序

![应用程序启动和初始化到进入后台的顺序](https://blog-image-1256099768.cos.ap-chengdu.myqcloud.com/blog-image/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E5%90%AF%E5%8A%A8%E5%92%8C%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%B0%E8%BF%9B%E5%85%A5%E5%90%8E%E5%8F%B0%E7%9A%84%E9%A1%BA%E5%BA%8F_2x.png)

因为大多数应用程序都是启动和初始化到进入到前台中，所以这里暂不讨论应用程序进入到后台的情况。

初始化完成后，系统会将应用程序移至活动状态（Active）或后台状态（Background）。当应用移至活动状态时，其窗口将显示在屏幕上，并开始响应用户交互。当您的应用移至后台状态时，其窗口将保持隐藏，并且可能会在挂起状态（Suspend）之前短暂运行。

无论应用程序是启动到前台还是后台，大部分应用程序启动时初始化代码都应该相同。例如，初始化应用程序的数据结构并设置应用程序的用户界面。UIKit 将移动到前台的应用程序的 `applicationState` 属性设置为 `UIApplicationStateInactive`，将移动到后台的应用程序的属性设置为 `UIApplicationStateBackground`。

#### 应用程序切换到后台

应用程序进入后台一般有两种方式:一是，点击“Home键”返回；二是，切换程序；

### main 函数 和 UIApplicationMain 函数

main 函数是程序启动的入口，在 iOS 应用程序中，main 函数的功能被最小化，它的主要工作都交给了UIKit 框架。在实际项目中，main 函数的代码如下：

```ObjectiveC
#import <UIKit/UIKit.h>
#import "AppDelegate.h"

int main(int argc, char * argv[]) {
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

- `main()` 函数中有两个参数 `argc` 和 `argv`，分别是 int 类型和 char 类型，都是 C 语言类型。C 系语言大多都是这个样子，argc 是命令行总的参数个数，argv 是参数的数组，值得一提的是 argv 中第一个参数为应用的路径 + 全名。

- `@autoreleasepool` 是自动释放池，这里暂不讨论，它的作用是管理内存。

> 注意
> 
> 上面代码中的例子，UIApplicationMain 函数的第四个参数使用 `NSStringFromClass` 函数将`[AppDelegate class]` 传递的类转换成字符串。如果将第四个实参改为 `@"AppDelegate"`，会是一样的结果。

然后来细说 `UIApplicationMain()` 函数，它有四个参数，虽然不用改变这些参数的值，但是还是要弄懂这些参数的意义。下面给出 UIApplicationMain() 这个函数的声明，从 Xcode 的声明来看，发现它是 `UIApplication.h` 头文件中的，这也说明 UIApplicationMain 这个函数的作用。

```ObjectiveC
// If nil is specified for principalClassName, the value for NSPrincipalClass from the Info.plist is used. If there is no
// NSPrincipalClass key specified, the UIApplication class is used. The delegate class will be instantiated using init.
UIKIT_EXTERN int UIApplicationMain(int argc, char * _Nonnull * _Null_unspecified argv, NSString * _Nullable principalClassName, NSString * _Nullable delegateClassName);
```

1. 首先 `UIKIT_EXTERN` 是一个宏，“`UIKIT_EXTERN` 就是将函数修饰为兼容以往 C 编译方式的、具有 extern 属性(文件外可见性)、public 修饰的方法或变量库外仍可见的属性。”（具体可以参考《[iOS 从main函数开始](https://www.cnblogs.com/liuguanlei/p/4827704.html)》这篇文章，大神讲解的非常详细，这里不再累述）。
    
2. 继续看整个 UIApplicationMain 函数声明，前面的两个参数来自 main 函数。argv 参数的修饰关键字是 `* _Nonnull * _Null_unspecified` ，表示不能为空，不确定是否为空（？？？此处疑问🤔️）。

3. UIApplicationMain 函数声明的第三个参数和第四个参数的修饰关键字都是 `NSString * _Nullable`，都表示是一个可以为空的字符串类型。   

说完了函数的声明，再看看在声明语句的前面还有两句注释，直译出来就是：“如果为 principalClassName 指定了 `nil`，则将使用 Info.plist 中的 NSPrincipalClass 值。如果没有指定 NSPrincipalClass 键，则使用 UIApplication 类。代理类将使用 init 实例化。”。
意思就是： NSPrincipalClass 参数指定为 nil，就使用项目配置文件 Info.plist 中的 NSPrincipalClass 键的值（Info.plist 文件中是键值对的数据），如果配置文件 Info.plist 也没有指定 NSPrincipalClass 键，那么就使用 UIApplication 类。

第四个参数 `delegateClassName` ，代理类。在 UIApplication 中有个 delegate 的变量，delegate 遵守 UIApplicationDelegate 协议，负责程序的生命周期。UIApplication 接收到所有的系统事件和生命周期事件时，都会把事件传递给 UIApplicationDelegate 进行处理，至于为什么不让UIApplication 自己去实现，涉及到了上帝类、框架类，过深，不讲。

这些在实际开发中没有什么用处，但是要成为大牛，需要的不只是会用，还要懂其中的原理。

综合来说，UIApplicationMain 函数主要负责三件事：

1. 根据传入的第三个参数初始化 UIApplication 或者子类对象的一个实例，如果给定的是 nil，那么系统会默认 UIApplication 类，也就主要是这个类来控制以及协调应用程序的运行。在后续的工作中，可以用静态方法 sharedApplication 来获取应用程序的对象。 

2. 根据传入的第四个参数初始化 UIApplicationDelegate 类，并使其成为 UIApplication 的代理类。

3. 启动主事件循环，并开始接收事件。



<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

