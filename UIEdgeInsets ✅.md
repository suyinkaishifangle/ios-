# UIEdgeInsets ✅

## UIEdgeInsets 简介

UIEdgeInsets 是一个结构体类型，用于设置视图的边缘内嵌值（即调整视图矩形的位置）。它包含 4 个 CGFloat 类型，分别为：

- **top**：顶边内嵌值。
- **left**：左边内嵌值。
- **bottom**：底边内嵌值。
- **right**：右边内嵌值。

「边缘内嵌值」可以用来缩小或扩大矩形表示的区域。通常在视图布局期间使用其来修改视图的 frame。内嵌值改变的是矩形的边线到矩形中心的距离。**内嵌值为正值表示缩小边到中心的距离，内嵌值为负值表示增大边到中心的距离**。也可以看成边线都是向内移动，不过当值为负时，边线向内移动了负值，就等于向外移动。

![UIEdgeInsets](media/15504538028137/UIEdgeInsets.png)

> ⚠️ **注意**
> 
> 内嵌值改变的是改变某一条边的位置，而不是整个矩形的位置。但是在 UIButton 中经过了特殊的处理，具体参考 UIButton。

## UIEdgeInsets 函数

### 创建 UIEdgeInsets

使用 UIEdgeInsetsMake 函数创建一个 UIEdgeInsets 结构体。

- **UIEdgeInsetsMake** 函数

    为视图创建边缘内嵌值。**上左下右**。

    - 参数一：CGFloat 类型，顶边内嵌值。
    - 参数二：CGFloat 类型，左边内嵌值。
    - 参数三：CGFloat 类型，底边内嵌值。
    - 参数四：CGFloat 类型，右边内嵌值。
    - 返回值：UIEdgeInsets 类型，边缘内嵌值。
    
### 比较 UIEdgeInsets

- **UIEdgeInsetsEqualToEdgeInsets** 函数

    比较两个边缘内嵌值是否相同。

    - 参数一：UIEdgeInsets 类型。
    - 参数二：UIEdgeInsets 类型。
    - 返回值：BOOL 类型，两个边缘内嵌值相同则返回 YES ，否则返回 NO。

### 使用 UIEdgeInsets 调整 CGRect

使用 UIEdgeInsetsInsetRect 函数可以使用指定的 UIEdgeInsets 来调整 CGRect。

- **UIEdgeInsetsInsetRect** 函数

      使用指定的边缘内嵌值调整矩形。

    - 参数一：CGRect 类型，需要被调整的矩形。
    - 参数二：UIEdgeInsets 类型。
    - 返回值：CGRect 类型，被调整后的矩形。

### 与字符串之间的相互转换

因为 UIEdgeInsets 是一个结构体类型，不能够直接将其值打印出来，使用 NSStringFromUIEdgeInsets 函数可以将其转换为字符串之后再打印。当然也可以使用 UIEdgeInsetsFromString 函数将一些字符串转换为 UIEdgeInsets 类型。

- **NSStringFromUIEdgeInsets** 函数

    返回包含 UIEdgeInsets 结构中的数据的格式化字符串。
   
    - 参数一：UIEdgeInsets 类型。
    - 返回值：NSString 类型，与 UIEdgeInsets 对应的字符串。例如：`{20, 30, 100, 0}`。

- **UIEdgeInsetsFromString** 函数

    根据指定的字符串中的数据返回 UIEdgeInsets 结构。

    - 参数一：NSString 类型，其内容格式必须为 `{top, left, bottom, right}`，并且使用英文逗号分隔。
    - 返回值：UIEdgeInsets 类型。如果字符串格式不正确，则函数返回 UIEdgeInsetsZero。

## UIEdgeInsetsZero 常量

`UIEdgeInsetsZero` 是 UIKit 定义的全局常量，表示一个四个边内嵌值都是为 0 的边缘内嵌值。