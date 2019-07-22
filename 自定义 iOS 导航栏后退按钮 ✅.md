# 自定义 iOS 导航栏后退按钮 ✅

## 方法一：使用 navigationItem 的 backBarButtonItem 属性

此方法适用于 iOS 11 及以上。

1. 在基类视图控制器中，可以自定义一个 UIBarButtonItem 的对象，将这个对象分配给视图控制器导航项的 `backBarButtonItem` 属性，这样就使用自定义的按钮替代了原来系统默认的后退按钮。
2. 在导航控制器中将导航栏的 `backIndicatorImage` 属性和 `backIndicatorTransitionMaskImage` 属性设置为空的 UIImage 对象，这是为了去掉系统默认的后退按钮的图标。

> ⚠️ **注意**
>
> 1. 步骤 2 不能少，否则会出现自定义的返回按钮中存在系统默认的图标。
> 
> 2. 这个方法在 iOS 11 以下也是能够使用的，不过会出现自定义的返回按钮位置很奇怪，试过很多修改的方法都无效。所以在 iOS 11 以下不推荐使用这个方法。

示例代码：

```Objective-C
// 基类视图控制器 .m 文件
- (void)viewDidLoad {
    [super viewDidLoad];
    UIBarButtonItem *backBarButtonItem = [[UIBarButtonItem alloc] 
                                         initWithImage:[UIImage imageNamed:@"backButton"]
                                         style:UIBarButtonItemStyleDone
                                         target:nil
                                         action:nil];
    self.navigationItem.backBarButtonItem = backBarButtonItem;
}

// 导航控制器 .m 文件
- (void)viewDidLoad {
    [super viewDidLoad];
    UIImage *nilImage = [[UIImage alloc] init];
    self.navigationBar.backIndicatorImage = nilImage;
    self.navigationBar.backIndicatorTransitionMaskImage = nilImage;
}
```

## 方法二：使用 navigationItem 的 leftBarButtonItem 属性 

此方法适合全部 iOS 版本。

思路就是自定义按钮项作为基类控制器的左导航项按钮，将这个导航项用作后退按钮，绑定视图控制器的出栈方法。并且把系统默认的后退按钮给隐藏掉。

示例代码：

```Objective-C
// 基类视图控制器 .h 文件
/**
 是否隐藏导航栏后退按钮，默认显示
 */
@property (nonatomic, assign) BOOL hideBackButton;

// 基类视图控制器 .m 文件
- (void)viewDidLoad {
    [super viewDidLoad];
    // 隐藏默认的后退按钮
    self.navigationItem.hidesBackButton = YES;
}

- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:YES];
    // 如果是根视图控制器或者主动隐藏则不显示后退按钮
    if (self.navigationController.viewControllers.count > 1 && !_hideBackButton) {
    UIBarButtonItem *backBarButtonItem = [[UIBarButtonItem alloc] 
                                         initWithImage:[UIImage imageNamed:@"backButton"]
                                         style:UIBarButtonItemStyleDone
                                         target:self
                                         action:@selector(clickBackBarButtonItem)];
    self.navigationItem.leftBarButtonItem = backBarButtonItem;
    }
}

- (void)clickBackBarButtonItem {
    [self.navigationController popViewControllerAnimated:YES];
}
```

> ⚠️ **注意**
>
> 要注意，设置左导航项按钮的代码一定要写在基类控制器的 viewWillAppear: 方法中，如果写在 viewDidLoad 方法中的话，加载继承基类的子类视图控制器时，调用 viewDidLoad 方法时其并没有放入导航栈，所以 if 判断中的语句永远不会执行。

