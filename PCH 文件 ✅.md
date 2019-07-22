# PCH 文件 ✅

## PCH 文件简介

PCH 文件是一个标准的预编译头文件（Pre-Compiled Header），它的后缀是 PCH，所以也叫 PCH 文件。它的作用是将头文件事先编译成一种二进制的中间格式，在整个编译过程中，只编译一次，并且会有缓存，如预编译头所涉及的部分不发生改变的话，在随后的编译过程中此部分不会重新进行编译，从而大大提高编译速度。

总的来说，它的作用是**使编译更快**。

预编译头文件隐式包含在每个头文件的开头。例如，预编译头文件名是 PrefixHeader.pch，那就像在每个头文件中最前面加上了下面这个语句：

```Objective-C
#import "PrefixHeader.pch"
```

## PCH 的优缺点

- 优点：提高了编译的速度。
- 缺点：不利于代码移植，且容易被滥用。

在 Xcode 6 以前，新建的项目会自动生成一个 PCH 文件，但是在 Xcode 6 及以后，不会再生成 PCH 文件。不过 PCH 文件并没有到退出历史舞台的时候，有一些场景还是会使用到，比如每个文件都需要用到的头文件、自定义方法、全局宏定义等。

Xcode 6 以及后默认不创建 PCH 文件，可能是因为很多开发者习惯把大量的头文件和宏定义放到 PCH 文件中，这样就不用在头文件中添加**预处理命令**（即文件包含、宏定义和条件编译）。但是这种做法会导致 PCH 文件庞大臃肿，反而导致编译时间过长。

> ⭐️ **提示**
>
>  该不该把头文件、宏定义等放到 PCH 文件中，要根据项目而定，不要盲目的将第三方框架的头文件都放到 PCH 文件中。

### 滥用 PCH 文件导致的问题

滥用 PCH 文件主要会导致两个问题：

- **可移植性差**

    假如在项目 A 中将第三方库的头文件 M 放到了这个项目的 PCH 文件中，那么这个 A 项目的所有头文件都可以调用这个第三方库。然后在 A 项目中创建了一个头文件 H，H 使用了这个第三方库。这时，有个项目 B 也可以使用项目 A 中的头文件 H，于是将项目 A 中的头文件 H 复制到项目 B 中，却发现无法运行。因为头文件 H 调用了头文件 M，但是是通过 PCH 文件来调用的，项目 A 和项目 B 的 PCH 文件包含的内容不同，所以导致头文件 H 在项目 B 中无法调用头文件 M。如果项目很大头文件很多，这种问题就更加的突出。
    
    那么还可以说为什么不把项目 A 的 PCH 文件也直接复制到项目 B 中呢？项目 A 和项目 B 的头文件不可能一模一样，如果直接复制。那么会有更多的问题，比如项目 B 不需要用到的框架、宏定义等。

- **隐藏依赖关系**

    如果将大量的头文件放在 PCH 文件中，一个头文件调用了哪些头文件将无法知道，因为 PCH 文件将 PCH 文件中的内容隐式地导入头文件中。如果项目大文件多，很难知道这个头文件依赖了哪些另外的头文件。

## 在项目中创建 PCH 文件

在 Xcode 6 以前，新建项目默认新建 PCH 头文件。在 Xcode 6 及以后，需要开发者手动创建 PCH 文件，下面是创建 PCH 文件的步骤：

1. 新建 PCH 文件：⚙️ **New File** ➤ **iOS** ➤ **other** ➤ **PCH File.**

2. 在工程中设置：⚙️ 选中工程文件 ➤ **Building Settings** ➤ 搜索 **prefix header** ➤ **Apple Clang - Language** （Xcode 10 以下是 Apple LLVM X.X - Language）

在 Apple Clang - Language 选项下有 Precompile Prefix Header 和 Prefix Header 两个条目。

### Prefix Header

Prefix Header 的值默认为空。

需要在 Prefix Header 条目中添加刚才创建的 PCH 文件的路径。

比如使用这个语句 `$(SRCROOT)/$(PRODUCT_NAME)/PrefixHeader.pch`，可以直接获取到名为 PrefixHeader.pch 的 PCH 文件（前提是 PrefixHeader.pch 就在项目名文件夹的目录下，如果在其他位置，那么需要适当调整语句路径）。

其中：

- `$(SRCROOT)` 表示的是项目的路径。
- `$(PRODUCT_NAME)` 表示项目名称。
- `PrefixHeader.pch` 就是 PCH 文件的名字。

### Precompile Prefix Header

Precompile Prefix Header 的值默认为 NO。

- 当它的值为 **NO** 时，PCH 文件不会被预编译，而是在每一个用到它导入的框架类库的 .m 文件中编译一次。这样降低了编译速度。
- 当它的值为 **YES** 时，PCH 文件会被预编译，预编译后的 PCH 文件会被缓存起来，从而提高编译速度。

## 实际项目中常用宏定义

下面列举了一些常用的宏定义，虽然不一定是每一个项目都会用到，但是其中大部分都是很有用的。

### 常用宽高

```Objective-C
#define kScreenWidth      [UIScreen mainScreen].bounds.size.width
#define kScreenHeight     [UIScreen mainScreen].bounds.size.height
#define kStatusBarHeight  [UIApplication sharedApplication].statusBarFrame.size.height
#define kNaviBarHeight    self.navigationController.navigationBar.bounds.size.height
#define kContentHeight    (kScreenHeight - kStatusBarHeight - kNaviBarHeight)

// 适配 X、XR、XS、XSMAX 屏幕底部安全区域
#define AdaptationHeight(height) (kScreenHeight < 812 ? height : height + 34)
```

### 颜色处理

使用下面的宏定义可以方便的使用 RGB 颜色值。

```Objective-C
#define kRGBColor(r, g, b)      [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:1.0]
#define kRGBAColor(r, g, b, a)  [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:(a)]
#define kHexColor(hex)          [UIColor colorWithHex:hex alpha:1.0]
#define kHexAColor(hex, a)      [UIColor colorWithHex:hex alpha:(a)]
#define kHexStringColor(hexString)      [UIColor colorWithHexString:hexString alpha:1.0]
#define kHexAStringColor(hexString, a)  [UIColor colorWithHexString:hexString alpha:(a)]
```

> 🔥 **提醒**
> 
> 后面四个宏涉及的方法并不是 UIKit 的方法，而是自定义的 UIColor 的新增的分类中用于处理十六进制颜色的方法。