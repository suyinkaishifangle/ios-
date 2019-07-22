# UIVisualEffectView

UIVisualEffectView 用于实现一些复杂视觉效果，其继承自 UIView，称为「视觉效果视图」。

视觉效果视图是一个视图，但是其并不提供视图的具体内容，而是专注于视图的效果。其有一个 `contentView` 属性，用于添加原来没有视觉效果的视图。在创建视觉效果视图之后，将其添加到视图层次结构中，再将需要添加视觉效果的子视图添加到视觉效果视图的 `contentView` 属性指向的视图上，进而合成视觉效果。

视觉效果视图的视觉效果由 UIVisualEffect 类创建，不过 UIVisualEffect 类自身并不包含创建视觉效果的接口，而是由其两个子类来创建：UIBlurEffect 和 UIVibrancyEffect。在创建 UIVisualEffectView 对象时，必须要传入 UIVisualEffect 对象。

> 🔥 **提示**
> 
> 1. 不要将 UIVisualEffectView 对象的 alpha 值设置为小于 1。因为创建部分透明的视图会使 UIVisualEffectView 在渲染过程中组合视图和所有关联的子视图，这和想要得到的效果不一致。
> 2. 不要设置 UIVisualEffectView 的 `backgroundColor` 属性，而是使用 UIBlurEffect 对象的 UIBlurEffectStyle 值来设置不同样式的模糊，否则不会出现模糊效果。

## UIVisualEffectView 方法和属性

- **initWithEffect:** 方法

    使用指定的视觉效果创建视觉效果视图。
    
    - 参数一：UIVisualEffect 类型，视觉效果。
    - 返回值：UIVisualEffectView 对象。

- **contentView** 属性

    UIView 类型，只读。用于添加视觉效果视图的子视图的视图对象。
    
    必须将子视图添加到此属性指向的视图上，而不是直接添加到 UIVisualEffectView 的对象上。
    
- **effect** 属性

    UIVisualEffect 类型，视图提供的视觉效果。
    
# UIVisualEffect 及其子类

UIVisualEffect 继承自 NSObject，它是具体视觉效果的基类，它是一个抽象类，自身并不提供对外的任何接口。目前 UIKit 中包含其两个字类：UIBlurEffect 和 UIVibrancyEffect。

## UIBlurEffect

UIBlurEffect 用于将模糊效果应用于视觉效果视图。添加到视觉效果视图的 `contentView` 的视图不受模糊效果的影响。

其只有一个创建方法：

- **effectWithStyle:** 类方法

    使用指定的样式创建模糊效果对象。

    - 参数一：UIBlurEffectStyle 类型，模糊样式。
    - 返回值：UIBlurEffect 对象。

- **UIBlurEffectStyle** 枚举类型

    UIBlurEffectStyle 是一个枚举类型，包含用于 UIBlurEffect 对象的模糊样式，具体值如下：
    
    | UIBlurEffectStyle 枚举值 | 说明 |
    | :-: | :-: |
    | UIBlurEffectStyleExtraLight | 视图区域的色调比底层视图浅。|
    | UIBlurEffectStyleLight | 视图的区域与底层视图的近似色调相同。 |
    | UIBlurEffectStyleDark | 视图区域的色调比底层视图更暗。 |
    | UIBlurEffectStyleRegular | 适应用户界面风格的常规模糊样式。 |
    | UIBlurEffectStyleProminent | 用于使内容更加突出，以适应用户界面风格。 |
    
## UIVibrancyEffect

UIVibrancyEffect 用于放大和调整视觉效果视图后面内容层的颜色。

活力效果旨在用作已使用 UIBlurEffect 配置的 UIVisualEffectView 的子视图或分层。使用活力效果可以帮助放置在contentView 内部的内容变得更加生动。

活力效果取决于颜色。添加到 UIVisualEffectView 的 `contentView` 的任何子视图都必须实现 `tintColorDidChange` 方法并相应地更新自己。具有 `UIImageRenderingModeAlwaysTemplate` 渲染模式的图像的 UIImageView 对象以及 UILabel 对象将自动更新。

- **effectForBlurEffect:** 类方法

    为指定的模糊效果对象添加活力效果。
    
    - 参数一：UIBlurEffect 类型，模糊效果对象。
    - 返回值：UIVibrancyEffect 对象。

    