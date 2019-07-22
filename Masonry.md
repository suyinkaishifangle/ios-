# Masonry

Masonry 是一个轻量级的 Objective-C 布局第三方框架，它使用更好的语法来包装自动布局。Masonry 有自己的布局 DSL [^DSL]，它提供了链式语法来描述 NSLayoutConstraint，这使布局代码更简洁，可读性更好，并且 Masonry 支持 iOS 和 macOS 双平台。

[^DSL]: 领域特定语言（英语：domain-specific language、DSL）。指的是专注于某个应用程序领域的计算机语言。又译作领域专用语言。不同于普通的跨领域通用计算机语言（GPL），领域特定语言只用在某些特定的领域。 比如用来显示网页的 HTML，以及 Emacs 所使用的 Emac LISP 语言。

Masonry 是基于 NSLayoutConstraint 的封装，本质上是使用 NSLayoutConstraint。Masonry 将 NSLayoutConstraint 封装，然后使用简洁的语句来声明视图的约束，从而达到自动布局的效果。

## Masonry 使用示例

首先需要包含 Masonry 库的头文件 `#import "Masonry.h"`。

例如，这里要实现一个视图在 self.view 中的自动布局，原型图如下：

![示例原型图](media/15379294985166/%E7%A4%BA%E4%BE%8B%E5%8E%9F%E5%9E%8B%E5%9B%BE.png)

<p style="text-align:center;color:#999999;font-size:14px"> 示例原型图 </p>

根据原型图的示例代码：

```Objective-C
UIView *blueView = [[UIView alloc] init];
blueView.backgroundColor = [UIColor blueColor];
[self.view addSubview:blueView];

blueView.translatesAutoresizingMaskIntoConstraints = NO;

[blueView mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.equalTo(self.view.mas_top).with.offset(122);
    make.left.equalTo(self.view.mas_left).with.offset(100);
    make.bottom.equalTo(self.view.mas_bottom).with.offset(-145);
    make.right.equalTo(self.view.mas_right).with.offset(-100);
}];
```

上面是一个最简单的使用示例，如果是使用苹果提供的 NSLayoutConstraint 类来创建一个视图的约束，那么代码量将是上面代码的大约 4 倍，这还仅仅是一个视图的约束。而使用 Masonry 通过链式语法能很简洁的一条语句就声明一个约束。通过一个代码块就能将一个视图的四个约束全部解决，既节约开发时间、减少了代码量，又让代码很清晰、简洁，便于后期维护。

> <i class="fas fa-exclamation-circle"></i>  注意
> 
> 1. 使用 Masonry 约束需要先将视图添加到父视图上（ 调用 `addSubview:` 方法）。
> 2. 必须将视图的 `translatesAutoresizingMaskIntoConstraints` 属性设置为 `NO`。

## Masonry 具体语法

### 约束语句

Masonry 的约束语句并不固定，可根据情况省略部分内容。这里以下图中这一句代码为例解释这个语句的各个部分。

![约束语句](media/15379294985166/%E7%BA%A6%E6%9D%9F%E8%AF%AD%E5%8F%A5.png)

<p style="text-align:center;color:#999999;font-size:14px"> 约束语句 </p>

- **make** 

    MASConstraintMaker 类型的对象，可以理解为即将添加约束的视图对象。

- **top**

    MASConstraint 类型，指视图对象的属性，相当于 NSLayoutAttribute 枚举，参考下表。
    
    MASConstraint 类型和下文将提到的 MASViewAttribute 类型，都是基于 NSLayoutAttribute 类型的封装。在 Masonry 中约束对象的属性是 MASConstraint 类型，参考对象的属性是 MASViewAttribute 类型，但是他们的作用和 NSLayoutAttribute 类型中的枚举值是一一对应的。
    
    | MASConstraint | MASViewAttribute  | NSLayoutAttribute 枚举值 | 说明 |
    | :-: | :-: | :-: | :-: |
    | left | view.mas_left | NSLayoutAttributeLeft | 左边界 |
    | right | view.mas_right | NSLayoutAttributeRight | 右边界 |
    | top | view.mas_top	| NSLayoutAttributeTop | 顶边界 |
    | bottom | view.mas_bottom	| NSLayoutAttributeBottom | 底边界 |
    | leading | view.mas_leading | NSLayoutAttributeLeading | 前沿 |
    | trailing | view.mas_trailin | NSLayoutAttributeTrailing | 后缘 |
    | width | view.mas_width | NSLayoutAttributeWidth | 宽度 |
    | height | view.mas_height	| NSLayoutAttributeHeight | 高度 |
    | centerX | view.mas_centerX | NSLayoutAttributeCenterX | X 轴中心 |
    | centerY | view.mas_centerY | NSLayoutAttributeCenterY | Y 轴中心 |
    | baseline | view.mas_baseline | NSLayoutAttributeBaseline | 基线 |

    <p style="text-align:center;color:#999999;font-size:14px"> Masonry 视图属性和 NSLayoutAttribute 对比  </p>
    
    > <i class="fas fa-exclamation-circle"></i> 如果不想使用这些属性的 `mas_` 前缀。在项目的 PCH 文件中添加 `#define MAS_SHORTHAND` 语句。

- **equalTo()**

    表示约束关系，共三种约束关系。Masonry 中表示约束关系的方法等等价于 NSLayoutRelation 中的枚举值。
    
    他们关系对比如下表：

    | Masonry 约束关系 | NSLayoutRelation 枚举值 | 效果说明 |
    | :-: | :-: | :-: |
    | equalTo()| NSLayoutRelationEqual | 都表示「等于」 |
    | lessThanOrEqualTo() | NSLayoutRelationLessThanOrEqual | 都表示「小于等于」 |
    | greaterThanOrEqualTo() | NSLayoutRelationGreaterThanOrEqual | 都表示「大于等于」 |
    
    <p style="text-align:center;color:#999999;font-size:14px"> Masonry 约束关系和 NSLayoutRelation 对比  </p>
    
    除了上面表中的三个方法，还有分别添加了前缀 `mas_` 的三个方法：`mas_equalTo()`、`mas_lessThanOrEqualTo()` 和 `mas_greaterThanOrEqualTo()`。他们的区别是没有添加前缀的方法只能传入对象类型，而不能传入基础数据类型（例如：CGFloat），而添加了前缀的方法可以既能传入对象类型也能传入非对象类型。
    
    > <i class="fas fa-exclamation-circle"></i> 如果不想使用这些方法的 `mas_` 前缀。在项目的 PCH 文件中添加 `#define MAS_SHORTHAND_GLOBALS` 语句。


- **self.view.mas_top**

   MASViewAttribute 类型，是指约束参考的视图对象的属性，相当于 NSLayoutAttribute 枚举，参考上文「Masonry 视图属性和 NSLayoutAttribute 对比」表。
   
   这里可以替换为非对象类型的值。例如：`make.width.mas_equalTo(100)`，表示约束视图的宽度为 100 pt。
   
   > <i class="fas fa-exclamation-circle"></i> 实际开发中一般来说只有约束视图的 `width` 和 `height` 属性可以直接传值来约束。

- **with**

    和前面的 **and** 一样，为了代码使用和阅读更容易理解，无实际作用，可不写，例如下面代码：
    
     ```Objective-C
    // 使用 with 和不使用 with 效果相同
    make.left.equalTo(self.view.mas_left).with.offset(100);
    make.left.equalTo(self.view.mas_left).offset(100);

    // 使用 and 和不使用 and 效果相同
    make.left.and.right.equalTo(self.view.mas_left).with.offset(100);
    make.left.right.equalTo(self.view.mas_left).with.offset(100);
    ```

- **offset()**

    偏移量方法，如果不写表示不偏移。除了 `offset()`，还有其他的方法，如下表：
    
    | 方法名 | 说明 |
    | :-: | :-: |
    | offset() | 偏移量 |
    | multipliedBy()| 约束比例 |

    Masonry 框架中的偏移量是有正负之分的，这里的正负表示方向。以参考对象的属性（比如上面示例原型图的右边界）为原点，向右偏移为正，向左偏移为负；同理向下偏移为正，向上偏移为负。
 
    还有一些不常用的方法，没有列出。
    
 
### 约束优先级
 
 
### 创建约束

Masonry 使用 `mas_makeConstraints` 对象方法创建对于视图的约束。在其代码块中填写对于视图的约束语句。



<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">