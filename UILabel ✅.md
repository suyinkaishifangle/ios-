# UILabel ✅

UILabel 继承自 UIView，用于显示一行或多行文本。Label 翻译过来为「标签」，所以一般说标签，就是指 UILabel。

## UILabel 属性和方法

UILabel 的 `userInteractionEnabled` 属性默认值为 `NO`，`clipsToBounds` 属性默认值为 `YES`，关于这两个属性的详细作用参考 UIView。

### 文本和外观

- **text** 属性

    NSString 类型，标签当前显示的文本内容，默认值为 `nil`。

- **attributedText** 属性

    NSAttributedString 类型，标签当前显示的富文本内容，默认值为 `nil`。

    要在标签上启用自动字距调整，请将特征字符串的 NSKernAttributeName 设置为 `nil`。

- **font** 属性

    UIFont 类型，标签当前显示的文本的字体。此属性的默认值是大小为 17pt 的系统字体。

- **textColor** 属性

    UIColor 类型，标签当前显示的文本的颜色。
  
    此属性的默认值是黑色（`blackColor`），将此属性设置为 `nil` 会将其重置为默认值。

- **textAlignment** 属性

    NSTextAlignment 类型，标签当前显示的文本的对齐方式。

    在 iOS 9 及更高版本中，此属性的默认值为 `NSTextAlignmentNatural`；在 iOS 9 之前，默认值为 `NSTextAlignmentLeft`。`NSTextAlignmentNatural` 表示使用与应用程序当前本地化相关联的默认对齐方式。

- **shadowColor** 属性

    UIColor 类型，标签文本的阴影颜色。此属性的默认值为 `nil`，表示不绘制阴影。

- **shadowOffset** 属性

    CGSize 类型，文本的阴影偏移（以 pt 为单位），默认偏移大小为 `(0,-1)`，表示文本上方一点的阴影。

    shadowColor 属性值必须为非 `nil`，才能使此属性产生效果。使用指定的偏移和颜色绘制文本阴影，不会出现模糊。
    
### 可用和高亮状态
    
- **enabled** 属性

    BOOL 类型，此属性确定标签的可用状态。此属性的默认值为 `YES`。

    如果此属性为 `NO`，则文本会稍微变暗以表示它未处于活动状态。
  
- **highlighted** 属性

    BOOL 类型，指示标签是否处于高亮状态，默认值为为 `NO`。

    设置此属性会使标签重绘为高亮状态。为了绘制高亮显示，highlightTextColor 属性必须包含非 `nil` 值。

- **highlightedTextColor**

    UIColor 类型，标签文本高亮状态时的颜色，默认值为 `nil`。

    只要 `highlighted` 属性设置为 `YES`，标签文本的颜色就会自动改变为此属性的值。
    
    > 🔥 **提示**
    >
    > 高亮属性也就是另一种状态的标签，除了颜色和非高亮状态不同外，其他没有什么区别。

### 行数和换行

- **numberOfLines** 属性

    NSInteger 类型，标签显示文本的最大行数，默认值为 `1`。

    如果要根据标签的宽度展示，不设置标签的固定行数，则将此属性设置为 `0`。

    使用 `sizeToFit` 方法调整标签大小时，会考虑此属性中的值。例如，此属性设置为 `3`，则 `sizeToFit` 方法会调整标签的大小，使其足够显示三行文本。

- **lineBreakMode** 属性

    NSLineBreakMode 类型，标签当前显示的文本的换行模式，默认值为 `NSLineBreakByTruncatingTail`。
    
    如果未使用富文本文本，则此属性将应用于 `text` 属性中的整个字符串。如果使用富文本，则为此属性指定新值会使换行模式应用于 `attributedText` 属性中的整个字符串。

    NSLineBreakMode 是枚举常量，用于指定当容器的行太长时该如何变化，如下表：

    | NSLineBreakMode 枚举类型 | 说明 | 效果 | 
    | :-: | :-: | :-: | 
    | NSLineBreakByWordWrapping | 以单词为显示单位显示，<br>显示不完的部分截断。 | ![NSLineBreakByWordWrapping](media/15506729931528/NSLineBreakByWordWrapping.png) |
    | NSLineBreakByCharWrapping | 以字符为显示单位显示，<br>显示不完的部分截断。 | ![NSLineBreakByCharWrapping](media/15506729931528/NSLineBreakByCharWrapping.png) |
    | NSLineBreakByClipping | 剪切文本宽度的内容长度，<br>显示不完的部分截断。 | ![NSLineBreakByClipping](media/15506729931528/NSLineBreakByClipping.png) |
    | NSLineBreakByTruncatingHead | 开头部分省略，<br>显示尾部文字内容。 | ![NSLineBreakByTruncatingHead](media/15506729931528/NSLineBreakByTruncatingHead.png) |
    | NSLineBreakByTruncatingTail | 结尾部分省略，<br>显示头的文字内容。 | ![NSLineBreakByTruncatingMiddle](media/15506729931528/NSLineBreakByTruncatingMiddle.png) |
    | NSLineBreakByTruncatingMiddle | 中间部分省略，<br>显示头尾的文字内容。 | ![NSLineBreakByTruncatingTai](media/15506729931528/NSLineBreakByTruncatingTail.png) |
    
    > ⚠️ **注意**
    >
    > 1. 前三个常量与换行时截断的位置有关。如果只有一行文本时，它们不会截断文本，超出范围的部分无法看见。
    > 2. 后三个常量与省略文本有关，省略都发生在最后一行。省略号的样式（中文或英文）会自动适应文本的内容。
    > 3. 截断文本的常量和省略文本的常量可以同时使用，使用 `|` 符号来使用多个常量。
    
- **adjustsFontSizeToFitWidth** 属性

    BOOL 类型，指示是否应缩小字体大小以使标签的文本字符串适合标签的边界矩形，默认值为 `NO`。

    通常，标签文本使用其 `font` 属性中指定的字体。如果此属性设置为 `YES`，并且标签的文本内容超出了标签的边界矩形，则标签会缩小其字体大小，直到文本内容适合或达到最小字体。
    
- **allowsDefaultTighteningForTruncation** 属性

    BOOL 类型，指示标签在截断之前是否收紧文本，默认值为 `NO`。

    当此属性设置为 `YES` 时，标签会在允许任何截断发生之前收紧其文本的字符间距。标签根据字体，当前行宽，换行模式和其他相关信息自动确定最大收紧量。

- **minimumScaleFactor** 属性

    CGFloat 类型，标签文本支持的最小比例因子，默认值为 `0`。

    如果 `adjustsFontSizeToFitWidth` 属性设置为 `YES`，则使用此属性指定当前字体大小的最小字体。如果为此属性指定值 `0`，则使用当前字体大小作为最小字体大小（即不限制最小字体大小）。

- **baselineAdjustment** 属性

    UIBaselineAdjustment 类型。在文本需要缩小以适合标签时控制其如何调整文本基线，默认值为`UIBaselineAdjustmentAlignBaselines`。

    如果 `adjustsFontSizeToFitWidth` 属性设置为`YES`，则此属性控制在需要调整字体大小的情况下文本基线的行为。仅当 `numberOfLines` 属性设置为 1 时，此属性才有效。
  
    > 🔥 **提示**
    >
    > 如果有这样的需求：标签中的文本随着文字内容变化而改变字体大小使其填满标签的矩形区域，参考[这个问题](https://stackoverflow.com/questions/17972036/center-vertically-in-uilabel-with-autoshrink)。
    
### 绘图和重写定位

- **textRectForBounds:limitedToNumberOfLines:** 方法

    返回标签文本的绘图矩形。

    - 参数一：CGRect 类型，接收者的边界矩形。
    - 参数二：NSInteger 类型，用于标签的最大行数。`0` 表示没有最大行数。
    - 返回值：CGRect 类型，标签的计算绘图矩形。

    在子类中重写此方法，该子类要求在系统执行其他文本布局计算之前更改标签的边界矩形。使用 `numberOfLines` 参数中的值将返回矩形的高度限制为指定的文本行数。

  如果先前调用 `sizeToFit` 或 `sizeThatFits:` 方法，则系统可以调用此方法。请注意，UITableViewCell 对象中的标签的大小基于单元格尺寸，而不是请求的大小。

- **drawTextInRect:** 方法

    在指定的矩形中绘制标签的文本（或其阴影）。

    - 参数一：用于绘制文本的矩形。

    不要直接调用此方法。如果需要修改标签文本的默认绘图行为，则重写此方法。

    调用此方法时，已使用默认环境和文本颜色配置当前图形上下文以进行绘制。在重写的方法中，可以进一步配置当前上下文，然后调用 super 来执行实际绘图，或者可以自己进行绘图。如果自己绘图，则不要调用 super。

- **preferredMaxLayoutWidth** 属性

  CGFloat 类型，多行标签的首选最大宽度（以 pt 为单位）。

  应用布局约束时，此属性会影响标签的大小。在布局期间，如果标签文本超出此属性指定的宽度，则超出宽度的文本将排列到一个或多个新的行。例如：本来文本的高度为 3 行，如果超出宽度的文本排列到新行，那么整个标签的高度就是 6 行文本的高度。

## UILabel 布局

在开发中对 UILabel 布局时可以使用 frame，也可以使用第三方布局框架（以 Masonry 为例）。

### 使用 frame 布局

如果使用 frame 进行布局，那么就需要确定 UILabel 的宽度和高度。但是在开发中，有些情况不能确定 UILabel 的宽度或高度，因为首先设计图不一定会很详细的给出文本的宽度，即使设计图给出了宽度也有在可能实际开发中并不准确，而且如果遇到需要语言本地化的项目，文字的宽度也会变化，并且实际文本的宽度受到文字字体和字号的影响；其次高度不能直接设置为字号的大小，因为某些字体中字母 `g` 和 `j` 等类似的字符的在显示时会稍微靠下。还有最常见的当文字的内容不确定时，行数的不同造相同类型的 UILabel 之间高度不同。

综上，其实本质原因在于无法知道具体的文字的宽度和高度。解决方法就是调用 `sizeToFit` 方法，无论将 UILabel 的高度值设置为多少，只要调用此方法，就会自动计算 UILabel 最合适的高度。

### 使用自动布局

如果使用 Masonry 布局框架通过约束来进行布局，则不需要对 UILabel 的高进行设置，其会自动适应最合适的高度（猜测应该是在底层也调用了 `sizeToFit` 方法），对于宽度也是同样的道理。

Masonry 是基于约束的自动布局方式，使用约束进行自动布局时，某些控件的内容部分（即宽度或高度）不需要设置。参考《[ Views with Intrinsic Content Size ](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ViewswithIntrinsicContentSize.html#//apple_ref/doc/uid/TP40010853-CH13-SW1)》。

- 单行标签

    对于单行文本，也分为两种情况：如果不需要确定最大宽度，则无需在约束中设置标签的宽度；如果需要确定标签最大的宽度，则在约束中设置标签的宽度。例如用于显示「用户名」的标签：每个用户的用户名长短不同，那么在界面上显示用户名的标签的宽度肯定有个最大的宽度，超过宽度的文本自动省略。

    > 🔥 **提示**
    >
    > 使用 Masonry 框架时，如果要确定一个标签的最大宽度，可以使用 **equalTo**，也可以使用 **lessThanOrEqualTo**。前者无论标签的文本多长，标签的长度都设置为指定的最大宽度值。后者则是根据标签中文本的长度来自动设置其宽度，但是这个宽度不会超过指定的最大宽度值。
    
    > ⚠️ **注意**
    >
    > 如果不为标签设置宽度，那么标签会根据标签的内容自适应宽度。不过有些时候标签的自适应宽度会出现一些问题，比如比标签实际的内容宽或者窄。那么此时需要使用下面两个语句来调整。
    
    ```Objective-C
    // 宽度不够时
    [label setContentCompressionResistancePriority:UILayoutPriorityRequired
                                           forAxis:UILayoutConstraintAxisHorizontal];

    // 宽度足够时
    [label setContentHuggingPriority:UILayoutPriorityRequired
                             forAxis:UILayoutConstraintAxisHorizontal];
    ```

- 指定行标签

    对于指定行的标签，其宽度一般是确定的， 并且行数也是确定的，**那么此时无需再设置高度**，使用约束会自动计算标签此时的高度值。
  
    如果此时也设置了高度值，那么标签会使用设置的高度值，如果设置的高度值不足以展示指定行数的文本，那么会自动减少展示的文本的行数，直到能够被标签展示的行数。例如：标签设置为显示 5 行，假设自适应显示 5 行的高度为 100 pt，此时却在标签的约束中设置了高度为 50 pt，那么最终标签只会有 50 pt 的高度，并且只显示 2 行（不会显示为 2.5 行，最终结果都是行数的整数倍）。

- 不确定行标签

    最后一种是不指定行数，但是一定会指定宽度。类似于「微信朋友圈」的动态文字，如果文字内容多，那么标签的高度就大，如果文字内容少，那么标签的高度就小，也就是实际的标签高度根据内容来确定，不指定行数。

    这时只需要将标签的 `numberOfLines` 属性设置为 0，并且设置一个宽度，不需要为其设置高度。

    > ⚠️ **注意**
    >
    > 一般在实际开发中，这种自适应高度的 UILabel 大多都是在表视图的单元格中使用，而使用表视图单元格需要提前计算好高度，但是 Masonry 并没有提供计算高度的功能，只能在局部期间自适应高度。常用解决思路是首先使用 NSString 的计算高度的方法将文本的高度计算出来存入模型中，在需要展示时使用模型中的高度来设置表视图单元格的高度。


### 获取指定字符串的宽度或高度

UILabel 的高度可以通过自适应来确定，但是有时候需要提前知道它的高度，例如上面提到的不确定行标签中，表视图中的每一个单元行的高度根据文字的多少来确定，但是这个高度值需要提前告知给表视图的代理方法，这种情况只能依靠下面的方法来计算出高度，将其传递给表视图的代理方法。

**NSString**

- **boundingRectWithSize:options:attributes:context:** 方法

    计算并返回在当前图形上下文中指定的矩形内，使用指定选项和显示特征绘制的字符串的边界矩形。

    - 参数一：CGSize 类型，要绘制的字符串的边界矩形的大小。

        - 如果要固定宽度，计算高度，则 `width` 设置为 `CGFLOAT_MAX`；固定高度，计算宽度同理。
        - 如果要计算单行文字的宽度和高度，则 `width` 和 `height` 都设置为 `CGFLOAT_MAX`。

    - 参数二：NSStringDrawingOptions 类型，字符串绘制选项。
    - 参数三：NSDictionary 类型，包含特征字符串的 NSAttributedStringKey 键的字典。
        
        这个字典中的属性和特征字符串的属性相同，不过这些属性用于 NSString 对象，用于的是整个字符串，而不是像 NSAttributedString 的范围。
        
    - 参数四：NSStringDrawingContext 类型，用于字符串绘制的上下文，可以为 `nil`。
        
        该上下文中包含包括一些信息，例如如何调整字间距以及缩放，。
        
    - 返回值：CGRect 类型，字符串的矩形，矩形原点是第一个字符的原点。

    此方法返回字符串中符号的实际边界。一些符号(例如空格)允许与传入的大小指定的布局约束重叠，因此在某些情况下，返回的 CGRect 的 `size` 的宽度值可能超过传入的参数一的宽度值。

    NSStringDrawingOptions 是一个枚举类型，是绘制时字符串的显示选项的常量。具体值如下：

    | NSStringDrawingOptions 枚举值 | 说明 |
    | :-: | :-: |
    | NSStringDrawingUsesLineFragmentOrigin | 指定的原点是线段原点，而不是基线原点。 |
    | NSStringDrawingUsesFontLeading | 计算行高时使用行距。（字体大小 + 行间距 = 行距） |
    | NSStringDrawingUsesDeviceMetrics | 使用图像符号边界代替印刷边界。 |
    | NSStringDrawingTruncatesLastVisibleLine | 如果文本内容超出指定的矩形边界，<br>文本将被截断并将省略号字符添加到最后一个可见行。<br>如果没有指定 `NSStringDrawingUsesLineFragmentOrigin` 选项，<br>则该选项被忽略。 |
    
    > 🔥 **提示**
    >
    > 1.  一般这个方法都用来计算固定宽度时的字符串高度，在参数二中传递 `NSStringDrawingUsesLineFragmentOrigin` 就可以了，其他的选项不用详细了解。
    >
    > 2. 因为计算得出的文本的宽度和高度一般都带有小数，为了准确，计算完成之后可以使用 `ceil()` 函数将宽度和高度换成整数。该函数的作用是：返回大于或者等于指定表达式的最小整数。例如宽度为 `15.1201pt`，使用该函数后宽度变为 `16pt`。

**NSAttributedString**

- **boundingRectWithSize:options:context:** 方法

    计算并返回在当前图形上下文中指定的矩形内，使用指定选项绘制的特征字符串的边界矩形。
    
    - 参数一：CGSize 类型，要绘制的字符串的边界矩形的大小。
    - 参数二：NSStringDrawingOptions 类型，字符串绘制选项。
    - 参数三：NSStringDrawingContext 类型，用于字符串绘制的上下文，可以为 `nil`。
    - 返回值：CGRect 类型，字符串的矩形。




