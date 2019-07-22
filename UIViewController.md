# UIViewController

[官方文档](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1)

UIViewController 称为「视图控制器」，是管理 UIKit 应用程序视图层次结构的对象。UIViewController 是最基础的视图控制器，UIViewController 类定义了用于管理视图、处理事件、从一个视图控制器到另一个视图控制器的转换以及与应用程序的其他部分进行协调的方法和属性。

视图控制器的主要职责包括以下内容：

- 更新视图的内容，通常是为了响应基础数据的更改。
- 响应用户与视图的交互。
- 调整视图大小并管理整个视图的界面布局。
- 在应用程序中与其他对象（包括其他视图控制器）进行协调。

视图控制器与其管理的视图紧密绑定，并参与处理其视图层次结构中的事件。视图控制器很少单独使用，它都是与其他视图控制器一起使用。视图控制器中可以加载另外的视图控制器用来显示新的视图集，或者它可以充当其他视图控制器的内容和动画视图的容器。

**视图控制器的分类**

视图控制器按照用途可以分为两种类型：

- **内容视图控制器**

  管理应用程序内容的一个独立部分，是主要的视图控制器类型。如：UIViewController、UITableViewController。

- **容器视图控制器**
  
   从其他视图控制器（称为子视图控制器）收集信息，并以不同方式显示子视图控制器的内容。如：UINavigationController。

大多数应用程序都是两种类型的视图控制器的混合。

## 视图控制器管理根视图

视图控制器最重要的作用便是管理视图的层次结构。每个视图控制器都有一个包含视图控制器所有内容的根视图（即视图控制器对象的 view 属性所指向的视图）。视图控制器有对根视图的强引用，每个视图都有对子视图的强引用。

根视图主要充当视图层次结构中全部子视图的容器视图。根视图的大小和位置由管理它的视图控制器来决定。窗口所拥有的视图控制器（即 UIWindow 对象的 rootViewController 属性）是整个应用程序的根视图控制器，它的视图用来填充窗口。

内容视图控制器管理它的所有视图。容器视图控制器管理它自己的视图和一个或多个子视图控制器的**根视图**，根据容器的设计对其进行调整和放置，它并不管理子视图控制器中的子视图。

### 视图控制器的 view 属性

view 属性是视图控制器的一个属性。这个属性指向的是这个视图控制器的视图层次结构中的根视图。此属性的默认值为 nil。每个视图控制器都包含这个属性。

首次访问视图控制器的 view 属性时加载或创建视图控制器的根视图，这种优化叫做**懒加载**（lazy loading），它可以降低内存使用。

> ⚠️ **注意**
>
> 因为是使用懒加载的方式创建视图控制器的根视图，所以在重写视图控制器的初始化方法时，方法内部的实现要注意：使用 self.view 会马上调用 loadView 和 viewDidLoad 方法。这样就会导致可能还没有初始化完成视图控制器对象就执行 loadView 和 viewDidLoad 方法，可能会出现预期之外的效果。

UIViewController 中有一个 loadView 方法的必要方法。此方法加载或创建根视图并将其分配给视图控制器的 view 属性。可以重写此方法来为视图控制器指定根视图。但是根据具体情况的不同，loadView 方法也会根据实际情况选择合适的方式加载或创建根视图，具体情况如下：

UIViewController 在访问其 view 属性但属性值为 nil 时，

- 如果没有重写 loadView 方法，则控制器会使用默认的 loadView 方法，并且按下列顺序选择加载或创建方式：
    
    1. 首先查找具有关联的 storyboard 文件。如果找到，则此方法从 storyboard 文件加载根视图。
    2. 然后查找具有关联的 xib 文件。如果找到，则此方法从 xib 文件加载根视图。
    3. 其次查找没有关联的但是名字和视图控制器类名相同（或类名去掉 Controller 字段后相同）的 xib 文件。如果找到，则此方法从此 xib 文件加载根视图。
    4. 最后如果上述情况都不满足，则系统会默认创建一个背景颜色为 clearColor 的 UIView 对象作为根视图。
        
- 如果重写了 loadView 方法，则直接执行 loadView 方法中的代码。

  示例代码：
    
    ```Objective-C
    - (void)loadView {
        UITableView *tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, kScreenWidth, kScreenHeight) style:UITableViewStylePlain];
        self.view = tableView;
    }
    ```
    上面示例代码重写 loadView 方法，把视图控制器的的根视图设置为一个 UITableView 类型的对象。

> ⚠️ **注意**
>
> 1. 重写  loadView 方法不能够调用 super 方法，并且创建的视图应该是唯一的实例，不应与任何其他视图控制器对象共享。
> 2. 如果要对根视图执行任何其他的初始化操作，最好在 viewDidLoad 方法中执行。
> 3. 在重写的 loadView 方法中如果创建的视图的 frame 值无论为何值，都不会影响 self.view 的布局，但是如果打印 self.view 的 frame 值，就是创建时设定的值。

### 根视图生命周期

当根视图的可见性发生变化时，视图控制器会自动调用自己的某些方法，以进行一些需要的操作。下面是视图控制器在根视图生命周期中调用的部分方法：

| 方法名 | 调用时间 | 作用 |
| :--: | :--: | :--: |
| init 初始化方法 | 为视图控制器对象分配内存并初始化时调用。 | 分配视图控制器所需的关键数据结构。 |
| **viewDidLoad** | 在根视图加载到内存后调用。**根视图加载到内存后只调用一次**  | 分配或加载要显示在根视图中的数据。 |
| **viewWillAppear:** | 在根视图**即将**添加到视图层次结构中时调用。 | 通知视图控制器其根视图即将添加到视图层次结构中。 |
| **viewDidAppear:** | 在根视图**已经**添加到视图层次结构中时调用。 | 通知视图控制器其根视图已经添加到视图层次结构中。 |
| **viewWillDisappear:** | 在视图控制器的根视图**即将**从视图层次结构中移除时调用。 | 通知视图控制器其视图即将从视图层次结构中删除。 |
| **viewDidDisappear:**  | 在视图控制器的根视图**已经**从视图层次结构中移除时调用。 | 通知视图控制器其视图已经从视图层次结构中删除。 |
| **dealloc** | 视图控制器移除时调用，释放其占用的内存。 | 释放视图控制器所需的关键数据结构。 |

![视图控制器生命周期]($resource/%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

> ⚠️ **注意**
> 
> 1. viewDidLoad 不是指只调用一次，而是指控制器根视图创建或加载之后只调用一次。如果根视图已经从内存中释放（一般视图控制器和根视图一起释放），那么下次视图控制器依然会调用。
> 2. 「添加到视图层次结构中」并不等于「出现在屏幕上」，因为屏幕上显示的都是视图层次中最上面的视图控制器的视图。
> 3. dealloc 不用写在实现文件中，使用 ARC 时系统会自动调用并释放内存。

### 处理视图旋转

https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc  

## 视图控制器层次结构

### 根视图控制器

根视图控制器是视图控制器层次结构的基础。每个窗口都只有一个根视图控制器，根视图控制器的内容填充了窗口对象，根视图控制器定义用户看到的初始内容。下图显示了根视图控制器和窗口之间的关系。由于窗口本身没有可见内容，所以是视图控制器的视图提供了所有内容。

![根视图控制器和应用程序窗口]($resource/%E6%A0%B9%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8%E5%92%8C%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E7%AA%97%E5%8F%A3.png)

> ⭐️ **提醒**
>
> 根视图控制器可以从窗口的 rootViewController 属性访问，如果是纯代码创建的窗口，则必须自己设置根视图控制器。

### 容器视图控制器

使用容器视图控制器可以将多个视图控制器组合到单个用户界面中，容器视图控制器能够配合内容视图控制器更加方便容易地组装复杂的界面。例如，UINavigationController 对象显示来自子视图控制器的内容，以及导航栏（navigationBar）和可选工具栏（ToolBar），并且它们都由导航控制器管理。UIKit 包括几个容器视图控制器：UINavigationController，UISplitViewController 和 UIPageViewController。

在很大程度上，容器视图控制器和其他内容视图控制器一样，它管理根视图和一些内容。 不同之处在于容器视图控制器从内容视图控制器中获取其内容的一部分（仅限于获取内容视图控制器的根视图）。容器视图控制器把从内容视图控制器中获取的根视图嵌入在自己的视图层次结构中。并且容器视图控制器仅设置该根视图的大小和位置，但内容视图控制器仍然管理这些视图中的内容。

容器视图控制器的视图总是填充指定的空间。容器视图控制器通常以根视图控制器的形式加载在窗口中，如下图所示。但容器视图控制器也可以作为其他容器的子容器来显示。容器负责适当地定位它的子视图。在示意图中，容器将两个子视图并排放置。

![容器视图控制器充当根视图控制器]($resource/%E5%AE%B9%E5%99%A8%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8%E5%85%85%E5%BD%93%E6%A0%B9%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8.png)

**示例：导航控制器**

UINavigationController 对象支持通过分层数据集导航。导航界面一次显示一个子视图控制器。界面顶部的导航栏显示数据层次结构中的当前位置，并显示后退按钮用来向后移动一个级别。

视图控制器之间的切换由导航控制器及其子视图控制器共同管理。当用户与子视图控制器交互时，子视图控制器请求导航控制器将新的子视图控制器推入视图。子视图控制器处理新视图控制器内容的配置，但导航控制器管理转换动画。导航控制器还管理导航栏，导航栏用于显示最顶层视图控制器的关闭按钮和导航栏上的相关视图。

![导航视图控制器的界面结构]($resource/%E5%AF%BC%E8%88%AA%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8%E7%9A%84%E7%95%8C%E9%9D%A2%E7%BB%93%E6%9E%84.png)

#### 容器视图控制器相关属性和方法

- **childViewControllers** 属性

  NSArray 类型，只能存放 UIViewController 类型的对象，只读。用户存放当前视图控制器的子视图控制器。

  数组中子视图控制器的顺序是按照添加的先后顺序存放，最先添加的视图控制器的索引为 0。

- **addChildViewController:** 方法

  将指定的视图控制器添加为接收方的子控制器。

  - 参数一：UIViewController 类型。要被添加为子控制器的视图控制器。

  此方法会在接收方和参数一的对象之间创建父子关系。必须存在父子关系才能将子视图控制器的根视图添加到父视图控制器的视图层次结构中。

  此方法仅用于实现自定义容器视图控制器时来调用。 如果重写此方法，则必须在实现中调用 super。

- **removeFromParentViewController** 方法

  将自身从父视图控制器中移除。

  此方法仅用于实现自定义容器视图控制器时来调用。 如果重写此方法，则必须在实现中调用 super。

- **transitionFromViewController:toViewController:duration:options:animations:completion:** 方法

  容器视图控制器的两个子视图控制器之间转换。

  - 参数一：UIViewController 类型，视图控制器，其根视图当前在父视图层次结构中。
  
  - 参数二：UIViewController 类型，视图控制器，其视图当前不在视图层次结构中（也就是视图控制器不是容器视图控制器的子级）。
  
  - 参数三：NSTimeInterval 类型，动画的总持续时间，以秒为单位。 
  
  - 参数四：UIViewAnimationOptions 类型，动画的选项。 参考 [UIViewAnimationOptions 枚举类型](✅uiview-动画#uiviewanimationoptions-枚举类型)。
  
  - 参数五：代码块，设置动画结束时视图的属性值。
  
  - 参数六：代码块，接受一个参数，设置动画结束时要执行的内容。
        
    - 参数一：BOOL 类型，如果动画结束则为 YES；如果它被跳过则为 NO。

  此方法将参数二的视图控制器的根视图添加到视图层次结构中，然后执行动画块中的动画。动画完成后，将从视图层次结构中删除第一个视图控制器的根视图。

  此方法仅用于实现自定义容器视图控制器时调用。 如果重写此方法，则必须在实现中调用 super。

- **willMoveToParentViewController:** 方法

  在视图控制器即将添加到容器视图控制器或即将从容器视图控制器中删除时调用。

   - 参数一：UIViewController 类型，父视图控制器，传 nil 表示没有父视图控制器（即从原来的父视图控制器中移除）。

  当视图控制器需要在它即将添加加到容器视图控制器或即将从容器视图控制器移除时执行某些需要的操作，可以覆盖此方法.

  > ⚠️ **注意** 
  >
  > 1. 如果是自定义的容器视图控制器，调用 addChildViewController: 方法时系统会自动调用子视图控制器的willMoveToParentViewController: 方法，无需手动调用。
  > 2. 如果是自定义的容器视图控制器，则必须在调用 removeFromParentViewController 方法之前调用子视图控制器的willMoveToParentViewController: 方法，并传入值为 nil。

- **didMoveToParentViewController:** 方法

  在视图控制器已经添加到容器视图控制器或已经从容器视图控制器中删除时调用。

  - 参数一：UIViewController 类型，父视图控制器，传 nil 表示没有父视图控制器（即从原来的父视图控制器中移除）。

  当视图控制器需要在它已将添加加到容器视图控制器或已经从容器视图控制器移除时执行某些需要的操作，可以覆盖此方法。

  > ⚠️ **注意** 
  >
  > 1. 如果是自定义的容器视图控制器，调用 removeFromParentViewController 方法从容器视图控制器中移除子视图控制器后自动调用子视图控制器的 didMoveToParentViewController: 方法，无需手动调用。
  > 2. 如果是自定义容器视图控制器，则在调用 addChildViewController: 方法后手动调用子视图控制器的  didMoveToParentViewController: 方法。
则必须在转换到新控制器完成后调用子视图控制器的didMoveToParentViewController: 方法。

#### 创建容器视图控制器

有时候需要在项目中使用自定义的容器视图控制器来实现某些功能。在设计自己的容器视图控制器时，始终要了解容器和其包含的视图控制器之间的关系。 视图控制器的关系可以帮助开发者了解其内容应如何显示在屏幕上以及容器视图控制器如何在内部管理它们的子视图控制器。 在设计容器视图控制器的过程中，需要明确以下问题：

- 容器视图控制器的作用是什么以及它的子视图控制器扮演什么样的角色？
- 同时显示多少个子视图控制器？
- 兄弟视图控制器之间的关系是什么（如果有的话）？
- 何时在容器视图控制器中添加或删除子视图控制器？
- 子视图控制器的视图大小或位置可以改变吗？ 在什么条件下会发生变化？
- 容器视图控制器是否提供自己的视图（例如导航栏）？
- 容器视图控制器与其子视图控制器之间需要什么样的信息传递？
- 容器视图控制器的外观可以以不同的方式配置吗？ 如果可以，怎样配置？

在明确了上面的问题之后，实现一个容器视图控制器就变得相对简单。自定义容器视图控制器关键是要建立正确的父子关系，因为**必须存在父子关系才能将子视图控制器的根视图添加到父视图控制器的视图层次结构中**。 以纯代码方式创建容器视图控制器时，可以在容器视图控制器中添加或删除子视图控制器，**一般自定义的容器视图控制器继承自 UIViewController 类**。

下面是创建容器视图控制器的步骤：

1. 调用 addChildViewController: 方法将子控制器添加到容器视图控制器。

2. 设置子控制器根视图的大小和位置。

3. 调用 addSubview: 方法将子视图控制器的根视图添加到容器视图控制器的视图层次结构中。

4. 如果需要限制子视图控制器的根视图的大小和位置，则可以添加一些约束。

5. 调用子视图控制器的 didMoveToParentViewController: 方法。

> ⚠️ **注意**
>
> 因为 addChildViewController: 法自动调用子视图控制器的 willMoveToParentViewController: 方法，所以无需手动调用。但是在将子视图控制器的根视图嵌入到容器视图控制器的层次结构中之后，必须手动调用 didMoveToParentViewController: 方法，因为在将子视图控制器的根视图嵌入到容器视图控制器的视图层次结构中之前，无法调用该方法。

下面以初始化一个「抽屉」控制器为例，该「抽屉」控制器可以管理两个视图控制器，其中一个子视图控制器用来管理正常情况时的内容，另一个子视图控制器用来管理「用户打开抽屉」时的内容。示例代码：

```Objective-C
- (instancetype)initWithCenterViewController:(UIViewController *)centerViewController
                          leftViewController:(UIViewController *)leftViewController {
    if (self = [super init]) {
        _centerViewController = centerViewController;
        _leftViewController = leftViewController;
        
        // 将子控制器添加到容器视图控制器
        [self addChildViewController:_leftViewController];
        [self addChildViewController:_centerViewController];
        
        // 设置子控制器根视图的大小和位置
        _centerViewController.view.frame = self.view.bounds;
        _leftViewController.view.frame = CGRectMake(-200, 0, 200, kScreenHeight);

        // 将子控制器的根视图添加到容器视图控制器的视图层次结构中
        [self.view addSubview:_leftViewController.view];
        [self.view addSubview:_centerViewController.view];
        
        // 调用子视图控制器的 didMoveToParentViewController: 方法
        [_leftViewController didMoveToParentViewController:self];
        [_centerViewController didMoveToParentViewController:self];
 }
    return self;
}
```


#### 删除子视图控制器

要从自定义容器视图控制器中删除子视图控制器，执行下列步骤删除视图控制器之间的父子关系：

1. 调用子视图控制器的 willMoveToParentViewController: 方法并传参数为 nil。

2. 如果子视图控制器的根视图存在约束，删除所有为子视图控制器的根视图设置的约束。

3. 从容器视图控制器的视图层次结构中删除子视图控制器的根视图。

4. 调用子视图控制器的 removeFromParentViewController 方法来结束父子关系。

删除子视图控制器会永久性地切断父视图控制器和子视图控制器之间的关系。 所以仅在不再需要引用子视图控制器时才删除子视图控制器。 例如，当新的控制器被推入导航堆栈时，导航控制器不会移除其当前的子视图控制器。只有当子视图控制器从堆栈弹出时才会删除它们。

下面代码是从容器视图控制器中移除某个子视图控制器的方法，示例代码：

```language
- (void)removeSubViewController:(UIViewController *)subViewController {

    // 调用子视图控制器的 willMoveToParentViewController: 方法并传参数为 nil
    [subViewController willMoveToParentViewController:nil];

    // 从容器视图控制器的视图层次结构中删除子视图控制器的根视图
    [subViewController.view removeFromSuperview];

    // 调用子视图控制器的 removeFromParentViewController 方法
    [subViewController removeFromParentViewController];
}
```

#### 子视图控制器之间切换

如果要为一个子视图控制器替换另一个子视图控制器的过渡设置动画，需要将子视图控制器的添加和删除合并到过渡动画中。 在动画期间，将新的子视图控制器的根视图移动到指定位置并删除旧视图控制器的根视图。 动画完成后，将子视图控制器删除。

下面示例代码显示了如何使用过渡动画将一个子视图控制器替换为另一个子视图控制器的示例。动画完成后，完成块将从容器中删除子视图控制器。在此示例中，transitionFromViewController:toViewController:duration:options:animations:completion: 方法自动更新容器的视图层次结构，因此无需自己添加和删除两个子视图控制器的根视图。

```Objective-C
- (void)cycleFromViewController:(UIViewController *)oldVC
               toViewController:(UIViewController *)newVC {
    // 准备进行两个视图控制器的更改
    [oldVC willMoveToParentViewController:nil];
    [self addChildViewController:newVC];
    
    newVC.view.frame = [self newViewStartFrame];
    CGRect endFrame = [self oldViewEndFrame];

    // 过渡动画
    [self transitionFromViewController:oldVC 
                      toViewController:newVC
                              duration:0.25 
                               options:0
                            animations:^{
                               newVC.view.frame = oldVC.view.frame;
                               oldVC.view.frame = endFrame;
                            }
                            completion:^(BOOL finished) {
                               [oldVC removeFromParentViewController];
                               [newVC didMoveToParentViewController:self];
                            }];
}
```

#### 创建容器视图控制器的建议

虽然实现容器视图控制器可能比较简单，但是整个控制器的可能非常复杂，在实现自定义容器视图控制器时，考虑以下建议：

- **仅访问子视图控制器的根视图。**

  容器控制器应该只访问每个子视图控制器的根视图，它永远不应该访问子视图控制器的任何其他视图。

- **子视图控制器应该对其容器知之甚少。**

  子视图控制器不应该管理容器视图控制器的内容。如果容器控制器允许其行为受到子控制器的影响，则应使用代理设计模式来管理这些交互。

- **让子视图控制器确定状态栏样式。**

  状态栏外观应该由子视图控制来管理，覆盖容器视图控制器中的 childViewControllerForStatusBarStyle 和childViewControllerForStatusBarHidden 方法。

- **让子视图控制器指定自己的根视图大小。**

  如果子控制器的根视图不需要填满整个屏幕，使用子控制器的 preferredContentSize 属性来确定子控制器根视图的大小。

## 显示视图控制器的视图

视图控制器的显示是指将当前视图控制器的视图替换为新视图控制器的视图。通常当前视图控制器的视图会消失，然后出现新视图控制器的视图。有两种方式可以在屏幕上显示视图控制器：将其嵌入容器视图控制器或者将其**模态（Modal）[^模态]显示**。

[^模态]: 模态这个词来源于 Modal，参考[模态对话框](https://baike.baidu.com/item/%E6%A8%A1%E6%80%81%E5%AF%B9%E8%AF%9D%E6%A1%86/6449704)。

### 嵌入容器视图控制器显示视图

显示视图控制器的视图最常用的方式是将视图控制器嵌入容器视图控制器中，使用 showViewController:sender: 或 showDetailViewController:sender: 方法显示视图控制器。在自定义视图控制器中，可以重写这些方法的实现来更适合视图控制器的显示方法。例如：导航控制器的入栈方法就是覆盖此方法并使用 push 方法来将视图控制器推送到导航堆栈中，使视图控制器嵌入导航控制器进而显示其视图。

### 模态显示视图控制器的视图

模态显示一个视图控制器时，UIKit 会在当前的视图控制器和即将显示的视图控制器之间创建一个关系（**当前的视图控制器称为「 presenting 控制器」，即将显示的视图控制器称「presented 控制器」**），这个关系称为「呈现关系」，这些关系构成视图控制器层次结构的一部分。如图下图所示：

![模态显示视图控制器示意图]($resource/%E6%A8%A1%E6%80%81%E6%98%BE%E7%A4%BA%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8%E7%A4%BA%E6%84%8F%E5%9B%BE.png)

模态显示一个视图控制器的视图的步骤：

1. 创建要显示的视图控制器对象。
2. （可选）设置新视图控制器的 modalPresentationStyle 属性，即设置模态显示样式。
3. （可选）设置新视图控制器的 modalTransitionStyle 属性，即设置转场动画样式。
4. 调用当前视图控制器的 presentViewController:animated:completion: 方法来显示新的视图控制器的视图。

- **presentViewController:animated:completion:** 方法

  模态显示视图控制器的视图。

  - 参数一：UIViewController 类型，presented 控制器。
  - 参数二：BOOL 类型，是否展示转场动画。
  - 参数三：代码块，无返回值，不带参数。presented 控制器显示完成后执行的处理，可传 nil。

    presented 控制器调用 viewDidAppear: 方法之后才会调用完成处理的代码块。

#### 模态显示样式

视图控制器的模态显示样式控制其视图在屏幕上的外观。UIKit 定义了许多标准的显示样式，每种样式都具有特定的外观和目的，当然还可以自定义显示样式。在设计应用程序时，选择和用户要执行的操作最有意义的显示样式（这样能提高用户的体验）。

下图展示了一个全屏显示的模态视图控制器的层次结构示意图：

![全屏模态视图控制器层次结构]($resource/%E5%85%A8%E5%B1%8F%E6%A8%A1%E6%80%81%E8%A7%86%E5%9B%BE%E6%8E%A7%E5%88%B6%E5%99%A8%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84.png)

modalPresentationStyle 属性的值是 UIModalPresentationStyle 枚举类型，具体枚举值如下：

- **UIModalPresentationFullScreen**

  模态视图覆整个屏幕，视图控制器 modalPresentationStyle 属性的默认值。

  > ⚠️ **注意**
  > 
  > 使用此样式显示视图控制器时，UIKit 通常会在过渡动画完成后从视图层次结构中删除模态视图下面的视图。但可以通过指定为 UIModalPresentationOverFullScreen 样式来阻止删除这些视图。

- **UIModalPresentationPageSheet**

  模态视图覆盖屏幕的一部分。

  - 当设备纵向使用时
  
      此枚举值与 UIModalPresentationFullScreen 的行为基本相同。
  
  - 当设备横向使用时
  
    - 在常规环境中，模态视图的宽度是设备纵向时屏幕的宽度，模态视图的高度是设备横向时屏幕的高度。而其他没有被模态视图覆盖的区域都会变暗以防止用户与它们进行交互。
   
    - 在紧凑环境中，此样式与  UIModalPresentationFullScreen 的行为相同。

- **UIModalPresentationFormSheet**

  模态视图显示在屏幕的中心。

  - 当设备纵向使用时
  
      此枚举值与 UIModalPresentationFullScreen 的行为基本相同。

  - 当设备横向使用时
  
    - 在常规环境中，视图控制器的大小使其内容区域小于屏幕大小，并且在内容下方放置调光视图。 如果此时键盘可见，则向上调整模态视图的位置，以使视图保持可见。而其他没有被模态视图覆盖的区域都会变暗以防止用户与它们进行交互。
    
    - 在紧凑环境中，此样式与 UIModalPresentationFullScreen 的行为相同。

- **UIModalPresentationCurrentContext**

- **UIModalPresentationCustom**

  使用自定义的呈递样式。

- **UIModalPresentationOverCurrentContext**

- **UIModalPresentationOverFullScreen**

  模态视图覆整个屏幕。
  
  但是和 UIModalPresentationFullScreen 的区别是：使用 UIModalPresentationFullScreen 样式时，会从视图层次结构中删除模态视图下方的视图，而使用 UIModalPresentationOverFullScreen 则不会从视图层次结构中删除模态视图下方的视图。

  所以当模态视图的背景不是完全不透明时，使用此样式就可以看到下方的视图。而使用 UIModalPresentationFullScreen 样式时，如果模态视图是透明的，那么看到的是窗口，而不是下方的视图。

- **UIModalPresentationBlurOverFullScreen**

  仅在 tvOS 上可用。

- **UIModalPresentationPopover**

  模态视图以弹出框样式显示。

  - 当设备横向使用时
  
    - 在常规环境中，此样式在弹出视图中显示视图控制器。 
    
    - 在紧凑环境中，此样式与 UIModalPresentationFullScreen 的行为相同。

- **UIModalPresentationNone**

  不应进行调整的显示样式。**不能使用此样式来显示视图控制器的视图**。

#### 模态转场样式

模态转场样式用于确定显示模态视图的转场动画类型。

modalTransitionStyle 属性用于确定当使用 presentViewController:animated:completion: 方法显示模态视图控制器时的转场动画样式。modalTransitionStyle 属性值是 UIModalTransitionStyle 枚举类型。具体枚举值如下：

| UIModalTransitionStyle 枚举值 | 说明 |
| :--: | :-- |
| UIModalTransitionStyleCoverVertical | 默认的转场样式。显示视图控制器时，其视图从屏幕底部向上滑动；隐藏视图控制器时，视图向下滑动。 |
| UIModalTransitionStyleFlipHorizontal | 显示视图控制器时，当前视图从右向左启动水平 3D 翻转；在隐藏视图控制器时，翻转从左到右发生，返回到原始视图 |
| UIModalTransitionStyleCrossDissolve | 显示视图控制器时，新视图淡入，当前视图淡出；在隐藏视图控制器时，使用类似的交叉淡入淡出来返回原始视图 |
| UIModalTransitionStylePartialCurl | 显示视图控制器时，当前视图的一个角向上卷曲以新视图；在隐藏视图控制器时，蜷缩的页面会在显示的视图之上展开。使用此转换显示的视图控制器本身被阻止显示任何其他视图控制器。并且仅当父视图控制器使用 UIModalPresentationFullScreen 样式显示时，才能使用此动画样式 |

### 关闭模态显示的视图控制器

调用 presenting 控制器的 dismissViewControllerAnimated:completion: 方法来关闭模态显示的视图控制器。也可以在 presented 控制器上调用此方法来关闭自己。如果在 presented 控制器上调用该方法时，UIKit 会自动将请求发送给 presenting 控制器。

- **dismissViewControllerAnimated:completion:** 方法

  关闭模态显示的 presented 控制器。

  - 参数一：BOOL 类型，是否显示转场动画。
  - 参数二：代码块，无返回值，不带参数。 presented 控制器关闭完成后执行的处理，可传 nil。

    presented 控制器调用 viewDidDisappear: 方法之后调用完成处理的代码块。

关闭 presented 控制器会将其视图从视图层次结构中删除。如果该视图控制器没有其他的强引用，则会释放与其关联的内存。

### 自定义转场动画

转场动画提供有关应用界面变化的视觉反馈。UIKit 提供了一组标准转场样式，可在视图控制器转场时使用，当然也可以使用自定义转场动画。

具体请参考文章：⌛️

## 处理交互事件

应用程序的响应者对象处理传入事件并采取适当的动作。虽然视图控制器是 UIResponder 对象，但它们很少直接处理触摸事件，而是以下列方式处理事件：

- 视图控制器定义处理高级事件的动作方法。（[Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3) 方式）

  - 具体动作。控件和一些视图调用动作方法来报告特定的交互。
  - 手势识别器。手势识别器调用动作方法来报告手势的当前状态，使用视图控制器处理状态更改或响应手势。

- 视图控制器观察系统或其他对象发送的通知。

  通知报告变化，是视图控制器更新其状态的一种方式。

- 视图控制器充当另一个对象的数据源或代理方。

  视图控制器通常用于管理表和集合视图的数据。可以将视图控制器用作对象的代理方。

对事件的响应通常涉及到更新视图的内容，这需要对那些视图进行引用。所以视图控制器是定义以后需要修改的视图的好地方。

## 常用属性和方法

- **beingDismissed** 属性

  BOOL 类型，只读。指示视图控制器是否正在被解除。

- **beingPresented** 属性

  BOOL 类型，只读。指示视图控制器是否正在显示。

- **movingFromParentViewController** 属性

  BOOL 类型，只读。指示是否从父视图控制器中删除视图控制器。

- **movingToParentViewController** 属性

  BOOL 类型，只读。指示是否将视图控制器移动到父视图控制器。

### 获取其他视图控制器

- **presentingViewController** 属性

  UIViewController 类型，只读。提供此视图控制器呈递的视图控制器。

  当使用 presentViewController:animated:completion: 方法以模态方式呈递新视图控制器时，被呈递视图控制器的此属性设置为呈递它的视图控制器。如果视图控制器没有以模态方式呈递，但它的一个父级控制器是以模态方式呈递的，则此属性包含呈递该父级控制器的视图控制器。如果当前视图控制器或其任何父级控制器都不是以模态方式呈递的，则此属性中的值为 nil。

- **presentedViewController** 属性

  UIViewController 类型，只读。由此视图控制器或视图控制器层次结构中的一 flag: green
的视图控制器。

  当使用 presentViewController:animated:completion: 方法以模态方式呈现新视图控制器时，呈现视图控制器的此属性设置为被呈现的视图控制器。如果当前视图控制器没有以模态方式显示另一个视图控制器，则此属性中的值为 nil。

- **navigationController** 属性

  UINavigationController 类型，只读。视图控制器层次结构中最近的父级导航控制器。
  
  如果视图控制器的父级之一是导航控制器，则此属性指向该导航控制器。如果其父级有多个导航控制器，那么指向离该视图控制器最近的父级导航控制器。如果视图控制器未嵌入导航控制器内，则此属性为 nil。

- **splitViewController** 属性
 
 UISplitViewController 类型，只读。视图控制器层次结构中最近的父级拆分控制器。
  
  如果视图控制器的父级之一是拆分控制器，则此属性指向该拆分控制器。如果其父级有多个拆分控制器，那么指向离该视图控制器最近的父级拆分控制器。如果视图控制器未嵌入拆分控制器内，则此属性为 nil。

- **tabBarController** 属性

  UITabBarController类型，只读。视图控制器层次结构中最近的父级标签栏控制器。
  
  如果视图控制器的父级之一是标签栏控制器，则此属性指向该标签栏控制器。如果其父级有多个标签栏控制器，那么指向离该视图控制器最近的父级标签栏控制器。如果视图控制器未嵌入标签栏控制器内，则此属性为 nil。

  