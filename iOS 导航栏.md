# iOS 导航栏

> 环境：<i class="fas fa-laptop "></i> Xcode 9.4.1 &nbsp;&nbsp;<i class="fas fa-mobile-alt"></i> iOS 11.4

-------

## 导航栏结构

iOS 中的导航栏的主要由一个导航栏控制器（UINavigationController）和一个导航栏（UINavigationBar）构成，且导航栏由导航栏控制器管理。

- UINavigationController 继承自 UIViewController，负责管理子控制器和导航栏。

- UINavigationBar 继承自 UIView，负责显示一些视图控件。

## UINavigationController

UINavigationController 是一个容器视图控制器，基于堆栈的方案来控制导航分层内容。iOS 应用使用导航界面来模拟由应用管理的分层数据的组织。

下图显示iOS系统中「设置」提供的导航界面示例。第一个屏幕向用户显示包含首选项的应用程序列表。选择应用程序会显示该应用程序的各个设置和设置组。选择一个组会产生更多的设置等等。除了根视图以外的所有视图，导航控制器都会提供一个返回按钮，以允许用户将层次结构向后移动。

![「设置」导航界面](media/15293848568054/%E3%80%8C%E8%AE%BE%E7%BD%AE%E3%80%8D%E5%AF%BC%E8%88%AA%E7%95%8C%E9%9D%A2.png)

<p style="text-align:center;color:#999999;font-size:14px"> 系统应用「设置」的导航界面结构 </p>

UINavigationController 使用称为「导航栈」的有序数组来管理其子视图控制器。这个有序数组保存在一个栈中，数组中的第一个视图控制器是根视图控制器，代表了栈的底部。数组中的最后一个视图控制器是栈中最顶端的项目，并表示当前正在显示的视图控制器。可以使用此类的方法从栈中添加和删除视图控制器。用户还可以使用导航栏中的返回按钮或使用左侧滑动手势来移除最顶层的视图控制器。

初始化 UINavigationController 对象时，需要传入一个 UIViewController 对象。这个 UIViewController 对象将成为 UINavigationController 对象的根视图控制器。根视图控制器将永远位于栈的底部（虽然 UIViewController 被叫做 UINavigationController 的根视图控制器，但是UINavigationController 并没有直接指向根视图控制器的属性）。

应用程序可以在运行时向 UINavigationController 的栈压入更多的视图控制器，这些视图控制器会加在视图控制器数组的末尾，同时会加在栈的顶部。UINavigationController 的 `topViewController` 属性会一直指向栈顶所在的视图控制器。

### UINavigationController 子视图

UINavigationController 是 UIViewController 的子类，所以 UINavigationController 对象也有自己的 view ，但是这个 view 一般不会显示。UINavigationController 对象的 view 有两个子对象：

- 一个是 UINavigationBar 对象
- 另一个是 topViewController 的视图

如下图：

![UINavigationController 对象的视图](media/15293848568054/UINavigationController%20%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%A7%86%E5%9B%BE.png)

<p style="text-align:center;color:#999999;font-size:14px"> UINavigationController 对象的视图 </p>

### UINavigationController 属性和方法

- `initWithRootViewController:` 使用这个方法初始化一个导航栏控制器，传入的参数是根视图控制器。

    > <i class="fas fa-exclamation-circle"></i> 注意
    > 
    > 使用 initWithRootViewController: 方法为 UINavigationController 初始化根视图控制器（必须要有），但是这个根视图控制器不能是 UITabBarController 类型。

- `pushViewController:animated:` 入栈方法，视图控制器通过水平切换方式过渡，如果视图控制器已在栈中，则无效。第一个参数传入的是需要入栈的视图控制器，第二个参数表示是否展示过渡动画。

    > <i class="fas fa-exclamation-circle"></i> 提醒
    >
    > `animated:` 这个参数在很多方法中都有，表示的意思都相同，即表示是否展示动画。
    
- `popViewControllerAnimated:` 出栈方法，出栈一个视图控制器，如果已经是根视图控制器，则无效。

- `popToViewController:animated:` 出栈方法，出栈到指定视图控制器。第一个参数表示指定的视图控制器。

- `popToRootViewControllerAnimated:` 出栈方法，出栈到根视图控制器。

    > <i class="fas fa-exclamation-circle"></i> 提醒
    >
    > 上面三个方法都是视图控制器从导航栈中出栈的方法。但是导航栈中的栈底，即根视图是无法出栈的。

- `topViewController` 属性，表示当前导航栈中栈顶视图控制器

- `visibleViewController` 属性，表示当前的模态视图控制器（如果存在），否则表示栈顶视图控制器。

- `viewControllers` 属性，表示栈中所有的视图控制器所在的数组，索引 0 即根视图控制器，索引 n-1 表示栈顶视图控制器，其中 n 为数组中视图控制器的个数。

- `navigationBarHidden` 属性，BOOL 类型，指示导航栏是否隐藏。

- `navigationBar` 属性，被导航栏控制器管理的导航栏视图。

## UINavigationBar

### UINavigationBar 结构

UINavigationBar 是条形一个视图，它是 UIView 的子类，通常显示在窗口的顶部，包含用于在屏幕层次结构内进行导航的按钮。主要组件是左侧（后退）按钮，中央标题和可选右侧按钮，如下图：

![navigation Bar 结构](media/15293848568054/navigation%20Bar%20%E7%BB%93%E6%9E%84.png)

<p style="text-align:center;color:#999999;font-size:14px"> navigationBar 结构 </p>

### UINavigationBar 外观

#### UINavigationBar 外观样式

首先是导航栏的外观样式，导航栏的外观样式通过 `barStyle` 属性定义，这个属性是 UIBarStyle 类型，是一个枚举类型，它只有两个值可用：

- UIBarStyleDefault：默认值，白色背景，黑色文字
- UIBarStyleBlack：黑色背景，白色文字

所以，默认的导航栏都是白色背景，黑色的文字。

#### UINavigationBar 底部阴影线条

在导航栏的下方默认有一条高度为 0.5 pt（plus 系列和 X 系列上高度是 1/3 pt）的浅灰色的分隔线，它是一个 UIImageView 对象。根据需求的不同，有的项目可能需要这个分隔线，而有的项目又需要隐藏这个分隔线。隐藏这个分隔线的方法有很多，但是有一个问题，不是所有的方法都能适配不同的系统，例如这个方法可能在 iOS 11 上有效，但是在 iOS 10 上却无效了，所以对苹果目前提供支持的系统进行了实验，总结出下列方法。

- 方法一：置空阴影图片（⚠️ **iOS 11~12 有效**）
    
    使用 UINavigationBar 的 `shadowImage` 属性，为其赋值一个空的 UIImage 对象来将分隔线置空。
    
    示例代码：
    
    ```Objective-C
    self.navigationBar.shadowImage = [[UIImage alloc]init];
    ```
    
- 方法二：设置导航栏背景图片并置空阴影图片（⚠️ **iOS 8~12 上有效**）

   查看了官方文档中关于 shadowImage 属性的说明。文档中的意思是：shadowImage 属性默认值为 nil，对应使用默认阴影图像（就是看到的一条浅灰色的分隔线）。非 nil 时，此属性表示要显示的自定义阴影图像。要显示自定义阴影图像，还必须使用 `setBackgroundImage:forBarMetrics:` 方法设置自定义导航栏背景图像。如果使用默认背景图像，则无论此属性的值如何，都将使用默认阴影图像。
    
    原来如此，这就是为什么在 iOS 11 以前的系统中使用 shadowImage 属性置空分隔线图片也无法隐藏分隔线的原因。是还需要使用 setBackgroundImage:forBarMetrics: 方法为导航栏设置背景图片。
    
    示例代码：
    
    ```Objective-C
    self.navigationBar.shadowImage = [[UIImage alloc]init];
    UIImage *image = [UIImage imageNamed:@"image"];
    [self.navigationBar setBackgroundImage:image forBarMetrics:UIBarMetricsDefault];
    ```
    
    就像官方文档中说的只要不使用上面设置背景图片的方法，那么对 shadowImage 属性的设置就无效。如果需求是要求使用纯色填充导航栏，那又该怎么办呢？
    
    > 这个时候只需要导入一张很小分辨率的纯色图片，将其设置为导航栏背景图片就行了。虽然导入的纯色图片小，但是使用它作为导航栏背景图片，那么就会自动将其拉伸到导航栏的大小，而且图片是纯色，无需担心拉伸模糊的问题。
    
- 方法三：遍历导航栏的子视图，找到分隔线将其隐藏（⚠️ **iOS 8~11 有效**）

    iOS 8～iOS 11，导航栏的子视图都在变化，那么就可以通过遍历出导航栏的所有子视图，再根据分隔线的特征（高度，对象类型）来隐藏掉它。具体代码如下：
    
    示例代码：
    
    ```Objective-C
    // 去掉 navigationBar 下方的分隔线线
    if ([self.navigationBar respondsToSelector:@selector(setBackgroundImage:forBarMetrics:)]) {
        NSArray *viewArray_1 = self.navigationBar.subviews;
        for (id obj in viewArray_1) {
            if ([obj isKindOfClass:[UIView class]]) {
                UIView *view = (UIView *)obj;
                NSArray *viewArray_2 = view.subviews;
                for (id obj in viewArray_2) {
                    if ([obj isKindOfClass:[UIImageView class]]) {
                        UIImageView *imageView = (UIImageView *)obj;
                        imageView.hidden = imageView.bounds.size.height <= 1;
                    }
                }
            }
        }
    }
    ```

#### UINavigationBar 透明度

UINavigationBar 中的 `translucent` 属性用来设置 UINavigationBar 的半透明状态，它是 BOOL 类型，默认值是 YES。并且它还会影响子控制器中视图的位置。例如：

- 当 translucent 属性值默认为 YES 时，子控制器的视图从屏幕顶端开始。红色视图出现在导航栏层次的下面，并且半透明的状态。

![translucent 属性值 为 YES](media/15293848568054/translucent%20%E5%B1%9E%E6%80%A7%E5%80%BC%20%E4%B8%BA%20YES.png)

<p style="text-align:center;color:#999999;font-size:14px"> translucent 属性值默认为 YES </p>

- 当 translucent 设置为 NO 时，子控制器的视图从导航栏底端开始。红色视图出现在导航栏下方。

![translucent 属性值 为 NO](media/15293848568054/translucent%20%E5%B1%9E%E6%80%A7%E5%80%BC%20%E4%B8%BA%20NO.png)

<p style="text-align:center;color:#999999;font-size:14px"> translucent 属性值默认为 NO </p>

**使用 setBackgroundImage:forBarMetrics: 设置图片做为导航栏背景时，translucent 的默认值会发生变化**。具体变化要根据方法的第二个参数的值来确定，一般该方法使用第二个参数使用 UIBarMetricsDefault，此时 translucent 的默认值变为 NO。

#### UINavigationBar 阴影

UINavigationBar 阴影就是导航栏最下面的那一条横线，这是 UINavigationBar 的 `shadowImage` 属性，它是 UIImage 类型，如果想要去掉这个线条，只需要给这个属性设置一个空的 UIImage 对象即可。例如：

```Objective-C
self.navigationBar.shadowImage = [[UIImage alloc]init];
```

#### UINavigationBar 背景


##### 使用颜色作为背景

> 下面的例子都是将 translucent 属性设置为 NO 之后的演示，如果不这样，就会出现导航栏看起来不清晰。

UIView 设置背景颜色都是使用 backgroundColor 属性。虽然 UINavigationBar 是 UIView 的子类，但是设置 UINavigationBar 的背景颜色却不能直接使用 backgroundColor 这个属性。

因为 UINavigationBar 还有一个叫 _UIBarBackground 的子视图，这个子视图就是用来为 UINavigationBar 设置背景颜色的，让用户看起来是导航栏的背景，但其实不是 UINavigationBar 视图的颜色。苹果并没有直接为这个子视图提供接口，而需要使用 UINavigationBar 提供的属性 `barTintColor` 为导航栏设置颜色。并且 _UIBarBackground 视图的高度是导航栏的高度加上状态栏的高度，所以使用 barTintColor 设置颜色，从屏幕上看起来也为状态栏的背景也设置了颜色（个人认为在开发中，一般不会单独为状态栏背景设置颜色，都是状态栏和导航栏的颜色相同，所以苹果才会这样做）。

如下图，为 UINavigationBar 的 backgroundColor 属性设置了颜色，没有为 UINavigationBar 的 barTintColor 属性设置颜色，前面的 _UIBarBackground 默认是白色，就会挡住后面的 UINavigationBar 的视图颜色，所以在屏幕上是看不到红色的。

![UINavigationBar 的 backgroundColor 属性设置为红色](media/15293848568054/navigationBar%20%E7%9A%84%20backgroundColor%20%E5%B1%9E%E6%80%A7%E8%AE%BE%E7%BD%AE%E4%B8%BA%20%E7%BA%A2%E8%89%B2.png)

<p style="text-align:center;color:#999999;font-size:14px"> UINavigationBar 的 backgroundColor 属性设置为红色 </p>

使用 Xcode 的工具「Debug View Hierarchy」可以看到如下图，将 barTintColor 属性设置为红色。选中的蓝色是 UINavigationBar，红色的视图是 _UIBarBackground。

![navigationBar 部分层次结构图](media/15293848568054/navigationBar%20%E9%83%A8%E5%88%86%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE.png)

<p style="text-align:center;color:#999999;font-size:14px"> UINavigationBar 的 barTintColor 属性设置为红色 </p>

如果没有设置 barTintColor 属性的值，那么这个 _UIBarBackground 的颜色会根据这个 UINavigationBar 的 barStyle 属性的值来推断，例如 barStyle 的值是 UIBarStyleBlack，那么 _UIBarBackground 的颜色就是黑色，如下图，同理那么默认值 UIBarStyleDefault 时， _UIBarBackground 的颜色就是白色。

![navigationBar 部分层次结构图（2）](media/15293848568054/navigationBar%20%E9%83%A8%E5%88%86%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE%EF%BC%882%EF%BC%89.png)

<p style="text-align:center;color:#999999;font-size:14px"> 不设置 UINavigationBar 的 barTintColor 属性 </p>

##### 使用图片作为背景

使用图片作为背景需要使用 `setBackgroundImage:forBarMetrics:` 方法。

- 第一个参数是 UIImageView 类型，指要设置为导航栏背景的图片
- 第二个参数是 UIBarMetrics 类型，一般使用 UIBarMetricsDefault

#### UINavigationBar 按钮项颜色

使用 UINavigationBar 对象的 tintColor 属性可以修改导航栏的按钮项的颜色，但是对于使用 initWithCustomView 方法初始化的导航栏按钮项则无法修改。需要注意，tintColor 属性无法修改导航栏的标题颜色。

如果没有设置 tintColor 属性，那么按钮项的默认颜色是蓝色，并且点击会有变淡的效果。

示例代码：

```Objective-C
self.navigationBar.tintColor = [UIColor redColor];
```

## UINavigationItem

UINavigationItem 是用于导航栏显示的项目。UINavigationItem 的对象包含要在导航栏中显示的按钮和视图。

每个 UIViewController 都有一个 navigationItem 属性，这个属性是 UINavigationItem 类型的，用于表示父级导航栏中视图控制器的导航项。UINavigationItem 不是 UIView 的子类，所以不能直接显示在屏幕上，只是 UINavigationItem 为 UINavigationBar 上的导航项的显示提供内容。

当一个 UIViewController 出现在 UINavigationController 的栈顶时，UINavigationBar 使用 UIViewController 的 navigationItem 属性来配置显示信息。关系如下图：

![NavigationItem关系图](media/15293848568054/NavigationItem%E5%85%B3%E7%B3%BB%E5%9B%BE.png)

<p style="text-align:center;color:#999999;font-size:14px"> UINavigationItem 使用关系图 </p>

每个 UINavigationItem 有三个可以自定义的区域：`leftBarButtonItem`、`rightBarButtonItem` 和 `titleView`。leftBarButtonItem 和 rightBarButtonItem 是 UIBarButtonItem 对象，UIBarButtonItem 是专门用于放置在工具栏或选项卡栏上的按钮类型，它不是 UIView 的子类，它是 UIBarItem 的子类，它包含了 UINavigationBar 上的按钮的显示信息。另一个自定义区域是 titleView，可以使用字符串作为标题，也可以自定义一个 UIView 放在 UINavigationItem 中间。

### UINavigationItem 的常用方式

#### 设置导航栏的标题

通过 UINavigationItem 的 title 和 titleView 两个属性都可以为导航栏设置标题，它们的具体介绍和使用方式如下。

##### 使用 title 属性设置标题

title 属性用于显示导航栏上中间位置的标题，它是 NSString 类型，默认为值为 nil。
    
设置了这个属性的 UINavigationItem 所属的视图控制器如果位于 topViewController 的下面（即在导航栈中位于 topViewController - 1 的位置），并且 topViewController 上显示了系统的返回按钮，那么返回按钮显示的文字就是 topViewController - 1 的视图控制器的 navigationItem 的 title 值；如果没有设置这个值，那么返回按钮默认显示文字「返回」。
    
下图分别是设置了 title 的值和没有设置 title 值的效果图：
    
![title 属性设置为 「Sixi」](media/15293848568054/title%20%E5%B1%9E%E6%80%A7%E8%AE%BE%E7%BD%AE%E4%B8%BA%20%E3%80%8CSixi%E3%80%8D.png)

<p style="text-align:center;color:#999999;font-size:14px"> title 属性设置为「Sixi」 </p>
    
![不设置 title 属性](media/15293848568054/%E4%B8%8D%E8%AE%BE%E7%BD%AE%20title%20%E5%B1%9E%E6%80%A7.png)

<p style="text-align:center;color:#999999;font-size:14px"> 不设置 title 属性 </p>
    
在 iOS 11 中，在 UINavigationBar 类和 UINavigationItem 类中分别新增了 `prefersLargeTitles` 属性和 `largeTitleDisplayMode`，这两个属性用来展示 iOS 11 新增加的标题风格，这种风格如下：
    
![系统应用「设置」中的标题风格](media/15293848568054/%E7%B3%BB%E7%BB%9F%E5%BA%94%E7%94%A8%E3%80%8C%E8%AE%BE%E7%BD%AE%E3%80%8D%E4%B8%AD%E7%9A%84%E6%A0%87%E9%A2%98%E9%A3%8E%E6%A0%BC.png)

<p style="text-align:center;color:#999999;font-size:14px"> 系统应用「设置」中的标题风格 </p>
    
prefersLargeTitles 是 UINavigationBar 的属性，它是BOOL 类型，默认值为 NO，表示是否使用上图这种大的导航栏标题样式（标题居左是固定的，无法修改）。
    
largeTitleDisplayMode 是枚举类型，表示大的标题的显示模式，即在视图控制器中显示的情况，它有三个枚举值：
    
- UINavigationItemLargeTitleDisplayModeAutomatic：自动显示，跟随上一级视图控制器的标题样式
- UINavigationItemLargeTitleDisplayModeAlways：总显示
- UINavigationItemLargeTitleDisplayModeNever：不显示

> <i class="fas fa-exclamation-circle"></i> 注意 
>
> 1. 只要设置 prefersLargeTitles 的属性值为 YES，即使没有设置 largeTitleDisplayMode 的属性值，导航栏也会显示为大标题。
> 2. 如果将 prefersLargeTitles 的属性值设置为 YES，那么导航栏的高度会变化为 96 pt。并且因为它是 UINavigationBar 的属性，所以它影响的是 UINavigationBar 的显示，如果没有重新将这个属性值设置为 NO，那么 UINavigationBar 的高度都会是 96 pt，不会改变为 44 pt。
    
##### 使用 titleView 属性设置标题

titleView 属性是用于代替标题的自定义视图，并且它的视图大小是可以调整的。它是 UIView 类型的，默认为 nil，使用它可以在导航栏上放置自定的视图例如图片、按钮等。

一半不用设置 titleView 的位置和大小，因为它会根据它的子视图来调整。

如果 topViewController - 1 的视图控制器设置了 titleView 属性，但是没有设置 title 属性，并且 topViewController 上有返回按钮，那返回按钮的文本就是系统默认的「返回」。如果 title 属性和 titleView 属性同时被设置时，导航栏上只会显示 titleView 属性的内容，并且 topViewController 上有返回按钮，那么topViewController 的返回按钮的文字会显示为 title 属性的值。

> <i class="fas fa-exclamation-circle"></i> 在 iOS 11 之前，必须要设置 titleView 中视图的 frame，否则 titleView 不会显示出来。

#### 设置导航栏的按钮项

和导航栏上的标题类似，导航栏上按钮其实也是 UINavigationItem 的属性，但是它们不是 UIButton 类型，而是 UIBarButtonItem 类型。UIBarButtonItem 适用于导航栏、标签栏、工具栏等和栏（Bar）有关的控件上的用于显示按钮的类型。这里就不详细分析这个类了，主要是要知道如何自定义导航栏上的按钮。

##### 自定义左右按钮项

UINavigationItem 类中有两个 UIBarButtonItem 类型的属性：`leftBarButtonItem` 和 `rightBarButtonItem`，它们分别用来设置导航栏上的左边和右边的按钮项。

下面的几种方法都是创建 UIBarButtonItem 对象再将其赋值给 leftBarButtonItem 或 rightBarButtonItem。不同点是使用的初始化方法不同，创建的按钮项也会有些差别。

- 方法一：使用 `initWithCustomView` 方法

    UIBarButtonItem 类有一个 UIView 类型的属性 customView，所以可以初始化一个 UIButton 对象，再使用 initWithCustomView 方法初始化一个 customView 为 UIButton 对象的 UIBarButtonItem 对象。再将这个 UIBarButtonItem 对象赋值给 UINavigationItem 上的 leftBarButtonItem 属性或者 rightBarButtonItem 属性。

    例如设置导航栏左按钮的步骤：
    
    1. 初始化一个 UIButton 对象，并设置 UIButton 对象的相关属性如背景图片，点击事件等。
    2. 使用 initWithCustomView 方法初始化一个 customView 为 UIButton 对象的 UIBarButtonItem 对象。
    3. 将 UIBarButtonItem 对象赋值给视图控制器的 navigationItem 属性的 leftBarButtonItem 属性。
   
    > <i class="fas fa-exclamation-circle"></i> 在 iOS 11 中可以不为 UIButton 对象设置 frame，但是在 iOS 11 以下，必须为 UIButton 对象设置 frame，否则导航栏上不会有按钮，但是实际开发中都会兼容低版本系统，所以需要对 UIButton 对象设置 frame。

    示例代码：

    ```Objective-C
    UIButton *mineButton = [UIButton buttonWithType:UIButtonTypeSystem];
    mineButton.frame = CGRectMake(0, 0, 30, 44);
    [mineButton setImage:[UIImage imageNamed:@"nav_mine_normal"] forState:UIControlStateNormal];
    [mineButton addTarget:self action:@selector(clickMineButton) forControlEvents:UIControlEventTouchUpInside];
    
    UIBarButtonItem *mine = [[UIBarButtonItem alloc]initWithCustomView:mineButton;
    self.navigationItem.leftBarButtonItem = mine;
    ```
    
- 方法二：使用 `initWithImage:style:target:action:` 方法

    这个方法直接在初始化 UIBarButtonItem 的时候传一个 UIImage 对象作为按钮项的背景，并且设置按钮项的样式和点击事件。
    
    - 第一个参数是传入的 UIImage 对象。
    - 第二个参数是 UIBarButtonItemStyle 枚举值，只有两个值可用：
        - UIBarButtonItemStylePlain：轻点时发光（文档中如此解释的），默认按钮样式。
        - UIBarButtonItemStyleDone：完成按钮的样式。
    - 第三个参数是接收操作消息的对象。
    - 第四个参数是点击此按钮项时要发送到目标的操作。

    示例代码：
    
    ```Objective-C
    UIBarButtonItem *left = [[UIBarButtonItem alloc]initWithImage:[UIImage imageNamed:@"nav_mine_normal"] style:UIBarButtonItemStylePlain target:self action:nil];
    self.navigationItem.leftBarButtonItem = left;
    ```

- 方法三：使用 `initWithTitle:style:target:action:` 方法

    这个方法和方法二类似，只不过这个方法是使用一个 NSString 作为按钮项的标题。第一个即是传入的要作为按钮项显示的标题的 NSString 对象，其他参数参考方法二中。
    
    示例代码：
    
    ```Objective-C
    UIBarButtonItem *left = [[UIBarButtonItem alloc]initWithTitle:@"我的" style:UIBarButtonItemStylePlain target:self action:nil];
    self.navigationItem.leftBarButtonItem = left;
    ```
    
- 方法四：使用 `initWithBarButtonSystemItem:target:action:` 方法

    这个方法是直接使用 UIBarButtonSystemItem 的枚举值作为按钮，UIBarButtonSystemItem 是苹果为 Bar 上的按钮项定义提供的系统图像的枚举类型。一共有 23 可用，具体的查阅官方文档。
    
    示例代码：
    ```Objective-C
    UIBarButtonItem *left = [[UIBarButtonItem alloc]initWithBarButtonSystemItem:UIBarButtonSystemItemSearch target:self action:nil];
    self.navigationItem.leftBarButtonItem = left;
    ```
    
- 方法五：使用 `initWithImage:landscapeImagePhone:style:target:action:` 方法

    这个和方法二是相同的，但是这个方法的第二个参数也是传递一个 UIImage 对象，不过这个 UIImage 对象是在手机横屏的时候显示。也就是说，使用这个方法，手机在竖屏的时候显示第一个参数的 UIImage 对象，手机在横屏的时候显示第二个 UIImage 对象。其他参数作用都相同。
    
    示例代码：
    ```Objective-C
    UIBarButtonItem *left = [[UIBarButtonItem alloc]initWithImage:[UIImage imageNamed:@"nav_mine_normal"] landscapeImagePhone:[UIImage imageNamed:@"nav_back_normal"] style:UIBarButtonItemStylePlain target:self action:nil];
    self.navigationItem.leftBarButtonItem = left;
    ```

> <i class="fas fa-pen-square"></i> 总结
> 
> 1. 虽然初始化一个 UIBarButtonItem 有上面的五种方式，但是最常用的仅仅是方法一和方法二，其他方法用的较少或者不用。
> 2. 方法一和其他的方法最大的区别是：使用方法一设置导航栏的按钮项之后，再设置 UINavigationBar 对象的 tintColor 属性来修改按钮项的颜色无效。因为方法一中的按钮项中是 UIButton 的类型，使用图片作为背景时颜色无法修改。相反其他的方法设置的按钮项的颜色是可以使用 UINavigationBar 对象的 tintColor 属性来修改的，并且点击之后会变暗，而方法一中需要设置 UIButton 的高亮状态的图片，才会出现点击效果。

##### 自定义多个左右按钮项

在实际开发中，会遇到这样的情况：在导航栏上需要显示多个按钮项。这种情况，当然在导航栏上添加 UIButton 来实现。但是苹果也提供了方法来实现这样的需求。

UINavigationItem 类中还有一个叫 leftBarButtonItems 的属性，它是 NSArray 的类型，并且这个数组只能存放 UIBarButtonItem 的类型对象。其实就这个属性就是「加强版」的 leftBarButtonItem，用来将多个按钮项显示在导航栏上。效果图如下：

![导航栏上显示两个按钮项](media/15293848568054/%E5%AF%BC%E8%88%AA%E6%A0%8F%E4%B8%8A%E6%98%BE%E7%A4%BA%E4%B8%A4%E4%B8%AA%E6%8C%89%E9%92%AE%E9%A1%B9.png)

<p style="text-align:center;color:#999999;font-size:14px"> 导航栏上显示两个按钮项 </p>

那么问题来了，这两个按钮看着太近了，如果想增加他们的间距该怎么办呢？可以通过设置 UIButton 的 frame 来控制按钮之前的间距。但是需要注意，将 UIButton 的宽度设置大了，那么按钮之前的间距看起来是变大了，其实真实的按钮距离并没有变化。通过这张图可以看出来（这里把右边两个按钮的宽度设置为 50pt，右边按钮的宽度为 30pt）：

![真实按钮距离](media/15293848568054/%E7%9C%9F%E5%AE%9E%E6%8C%89%E9%92%AE%E8%B7%9D%E7%A6%BB.png)

虽然按钮看起来间距大了，但是按钮真实的大小并不只是图标大小，这也涉及到用户点击按钮的范围等，开发的时候一定要注意这些细节问题。

示例代码：

```Objective-C
// 导航栏右侧按钮 1
UIButton *searchButton = [UIButton buttonWithType:UIButtonTypeSystem];
searchButton.frame = CGRectMake(0, 0, 30, 44);
[searchButton setImage:[UIImage imageNamed:@"nav_search_normal"] forState:UIControlStateNormal];
[searchButton setImage:[UIImage imageNamed:@"nav_search_highlighted"] forState:UIControlStateHighlighted];
[searchButton addTarget:self action:@selector(clickSearchButton) forControlEvents:UIControlEventTouchUpInside];

// 导航栏右侧按钮 2
_newsButton = [UIButton buttonWithType:UIButtonTypeSystem];
_newsButton.frame = CGRectMake(0, 0, 30, 44);
[_newsButton setImage:[UIImage imageNamed:@"nav_news_normal"] forState:UIControlStateNormal];
[_newsButton setImage:[UIImage imageNamed:@"nav_news_highlighted"] forState:UIControlStateHighlighted];
[_newsButton addTarget:self action:@selector(clickNewsButton) forControlEvents:UIControlEventTouchUpInside];
    
UIBarButtonItem *search = [[UIBarButtonItem alloc]initWithCustomView:[[UIView alloc]init]];
search.customView = searchButton;
UIBarButtonItem *news = [[UIBarButtonItem alloc]initWithCustomView:[[UIView alloc]init]];
news.customView = _newsButton;
    
self.navigationItem.rightBarButtonItems = @[news, search];
```

> <i class="fas fa-exclamation-circle"></i> 注意
> 
> 1. 按钮项数组如果赋值给 rightBarButtonItems，那么数组中的第一个元素会放在导航栏最右边。相反如果赋值给 leftBarButtonItems，那么数组中的第一个元素会放在导航栏的最左边。
> 2. 添加到左边/右边的 UIBarButtonItem 对象无法再被添加到右边/左边，先添加到哪一边依赖代码的执行顺序。

##### 自定义返回按钮项

> ⚠️ 在实际项目中，如果被出栈的视图控制器的导航栏颜色和将要显示的视图控制器导航栏颜色不一致，那么使用系统提供的返回按钮出栈视图控制器时，会发生导航栏的颜色切换不流畅（即切换时没有动画），自定义一个返回按钮，此情况不再复现。

UINavigationItem 类的 backBarButtonItem 属性是导航栏上需要返回按钮时使用的栏按钮项，它的默认值是 nil。iOS 中返回按钮项一般是在导航栏的左侧。返回按钮项不需要额外设置，它会自动显示。如果该视图控制器设置了左侧按钮项，那么返回按钮项就不会显示。

如果没有设置左侧按钮项，并且也不需要显示返回按钮项时，可以使用 UINavigationItem 类 hidesBackButton 属性来隐藏返回按钮项。

系统的返回按钮项中的返回按钮是由一个图标和 topViewController-1 视图控制器的标题构成。如果topViewController-1 视图控制器的中的 navigationItem 属性的 title 属性没有设置或者 title 的值过长，那么 topViewController 视图控制器的返回按钮的文字则会显示「返回」，并且要注意，只有通过 title 设置的标题文字才会显示在返回按钮项中显示，如果使用 titleView 设置的标题，那么返回按钮项的文字依旧是 「返回」。效果图如下：

![左边设置了标题 右边没有设置标题](media/15293848568054/%E5%B7%A6%E8%BE%B9%E8%AE%BE%E7%BD%AE%E4%BA%86%E6%A0%87%E9%A2%98%20%E5%8F%B3%E8%BE%B9%E6%B2%A1%E6%9C%89%E8%AE%BE%E7%BD%AE%E6%A0%87%E9%A2%98.png)

<p style="text-align:center;color:#999999;font-size:14px"> 左边设置了标题 右边没有设置标题 </p>

返回按钮项的属性是 backBarButtonItem，同样的它也是 UIBarButtonItem 的类型。但是 backBarButtonItem 这个属性并不能像 rightBarButtonItem 和 leftBarButtonItem 一样可以自定义按钮。如果对它赋值，运行界面展示的将还是系统自带的返回按钮项，但是返回按钮不会再显示文字（包括「返回」也不会显示）。在官方文档中解释道：「When configuring your bar button item, do not assign a custom view to it; the navigation item ignores custom views in the back bar button anyway.」可以理解为：配置 bar 上的返回按钮项时，不要为其指定自定义视图，无论如何，导航项忽略 bar 中返回按钮中的自定义视图。

因为上面说到的原因，包括根据个人实际开发的经验，一般不使用系统自带的返回按钮项，原因不仅是它的样式无法自定义，而且系统自带的返回按钮距离屏幕边缘的距离要小于使用自定义的按钮项到屏幕边缘的距离（它们的距离都无法更改），所以就可能造成左右间距不等，影响界面美观。

![左右间距不等](media/15293848568054/%E5%B7%A6%E5%8F%B3%E9%97%B4%E8%B7%9D%E4%B8%8D%E7%AD%89.png)

<p style="text-align:center;color:#999999;font-size:14px"> 左右间距不等 </p>

所以，推荐使用定义左侧按钮项为返回按钮，把按钮的图片设置为后退的样式，在设置按钮的点击事件为出栈视图控制器，这样就能完美解决上面的问题。下图是使用这个方法自定义的返回按钮效果：

![自定义返回按钮效果演示](media/15293848568054/%E8%87%AA%E5%AE%9A%E4%B9%89%E5%90%8E%E9%80%80%E6%8C%89%E9%92%AE%E6%95%88%E6%9E%9C%E6%BC%94%E7%A4%BA.gif)

<p style="text-align:center;color:#999999;font-size:14px"> 自定义返回按钮效果演示 </p>

示例代码：

```Objective-C
UIBarButtonItem *back = [[UIBarButtonItem alloc]initWithCustomView:[[UIView alloc]init]];
UIButton *backButton = [[UIButton alloc]init];
back.customView = backButton;
[backButton setImage:[UIImage imageNamed:@"nav_back_normal"] forState:UIControlStateNormal];
[backButton setImage:[UIImage imageNamed:@"nav_back_normal"] forState:UIControlStateHighlighted];
[backButton addTarget:self action:@selector(clickBackButton) forControlEvents:UIControlEventTouchUpInside];
self.navigationItem.leftBarButtonItem = back;

// 出栈函数
- (void)clickBackButton {
    [self.navigationController popViewControllerAnimated:YES];
}
```

<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">






