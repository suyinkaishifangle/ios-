# NSAttributedString 和 NSMutableAttributedString

## NSAttributedString

NSAttributedString 继承自 NSObject，它是一个字符串对象，称为「属性字符串」，其文本具有关联的属性（如视觉样式，超链接或辅助功能数据等）。它用来显示一些带有特殊样式的文本（即富文本），比如说带有下划线、删除线、斜体、空心字体、背景色、阴影以及图文混排（一种文字中夹杂图片的显示效果）。NSAttributedString 对象管理应用于字符串中的单个字符或范围字符的关联的属性集（例如，字体和字距等）。NSAttributedString 和 NSMutableAttributedString 分别声明了只读属性字符串和可修改属性字符串的编程接口。

属性字符串按名称标识属性，使用 NSDictionary 对象在给定名称（即属性名）下存储值（即属性值），可以将任何属性的键值对分配给指定范围的字符。应用程序也可以解释自定义属性（参阅 [属性字符串编程指南](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/AttributedStrings/AttributedStrings.html#//apple_ref/doc/uid/10000036i) ）。 如果要将属性字符串与 Core Text 框架一起使用，则还可以使用该框架定义属性键。

可以将属性字符串与任何接受它们的 API 一起使用，例如 Core Text。 AppKit 和 UIKit 框架还提供NSMutableAttributedString 的子类，称为 NSTextStorage，为扩展文本处理系统提供存储。在 iOS 6 及更高版本中，可以使用属性字符串在文本视图，文本框和其他一些控件中显示富文本。AppKit 和 UIKit 都定义了基本属性字符串接口的扩展，允许在当前图形上下文中绘制其内容。

NSAttributedString 对象的默认字体是 `Helvetica 12-point`，它可能与平台的默认系统字体不同。还可以使用NSParagraphStyle 类及其子类 NSMutableParagraphStyle 来封装 NSAttributedString 类使用的段落或标尺属性。

注意，使用 `isEqual:` 方法比较 NSAttributedString 对象会查找完全相等。比较包括逐字符字符串相等性检查和所有属性的相等性检查。如果字符串具有许多属性（例如附件，列表和表），则此类比较不太可能产生匹配。

### NSAttributedStringKey

NSAttributedString 通过一个 NSString 和一个 NSDictionary 来决定字符串的显示效果。NSSting 即是字符串文本，NSDictionary 中的字典的键是 `NSAttributedStringKey` 别名，字典的值用来设置该字符串需要呈现的效果，具体的键值对参考下表。

| 键 | 值类型 | 用途 | 说明 |
| :-: | :-: | :-: | :-: |
| NSFontAttributeName | UIFont | 字体样式 | 默认：Helvetica(Neue) 12pt |
| NSParagraphStyleAttributeName | NSParagraphStyle | 文本字、行间距<br>对齐方式 | 默认：defaultParagraphStyle |
| NSForegroundColorAttributeName | UIColor | 字体颜色 | 默认：blackColor |
| NSBackgroundColorAttributeName | UIColor | 文字背景色 | 默认：nil (无背景色) |
| NSLigatureAttributeName |	NSNumber（整数） | 连字符 | 0 表示没有连字符<br>1 是默认的连字符 |
| NSKernAttributeName | NSNumber（浮点） | 字符间距 | 默认：0（禁用字符间距调整）|
| NSStrikethroughStyleAttributeName | NSNumber（整数） | 删除线 | 默认：0（无删除线）|
| NSStrikethroughColorAttributeName | UIColor | 删除线颜色 | 默认：nil（和字体颜色相同）|
| NSUnderlineStyleAttributeName | NSNumber（整数）| 下划线 | 默认：0（无下划线）|
| NSUnderlineColorAttributeName | UIColor | 下划线颜色 | 默认：nil（和字体颜色相同）|
| NSStrokeColorAttributeName | UIColor | 描边颜色 | 默认：nil（和字体颜色相同）|
| NSStrokeWidthAttributeName | NSNumber（浮点） | 描边宽度 | 正值空心描边<br>负值实心描边<br>默认：0（不描边）|
| NSShadowAttributeName | NSShadow | 文本阴影 | 默认：nil（没有阴影）|
| NSTextEffectAttributeName | NSString | 文字效果 | 默认：nil（没有文字效果）|
| NSAttachmentAttributeName | NSTextAttachment | 常用作图文混排 | 默认：nil（没有附件）|
| NSLinkAttributeName | NSURL（优先）<br>或 NSString | 链接 | 默认：nil（没有链接）|
| NSBaselineOffsetAttributeName | NSNumber（浮点）| 基础偏移量 | 正值向上偏移<br>负值向下偏移<br>默认：0（不偏移）|
| NSObliquenessAttributeName | NSNumber （浮点）| 字体倾斜 | 正值向右倾斜<br>负值向左倾斜<br>默认：0（不倾斜）|
| NSExpansionAttributeName | NSNumber（浮点） | 文本扁平化 | 正值横向拉伸<br>负值横向压缩<br>默认：0（不拉伸）|
| ⌛️NSWritingDirectionAttributeName |||
| NSVerticalGlyphFormAttributeName | NSNumber（整数）| 文本方向 | iOS 中只能用水平文本|

### NSAttributedString 属性和方法

#### 创建 NSAttributedString 对象

- **initWithString:** 方法

    使用给定字符串的初始化 NSAttributedString 对象，但该对象不包含任何属性信息。
    
    - 参数一：NSString 类型，给定的字符串。
    - 返回值：NSAttributedString 对象。

- **initWithString:attributes:** 方法

    使用给定字符串和属性初始化 NSAttributedString 对象。
    
    - 参数一：NSString 类型，给定的字符串。
    - 参数二：NSDictionary 类型，新属性字符串的属性字典。
    - 返回值：NSAttributedString 对象。

- **initWithAttributedString:** 方法

    使用另一个给定的属性字符串初始化 NSAttributedString 对象。
    
    - 参数一：NSAttributedString 类型，给定的属性字符串。
    - 返回值：NSAttributedString 对象。

- **attributedStringWithAttachment:** 类方法

    使用附件创建属性字符串。
    
    - 参数一：NSTextAttachment 类型，附件。
    - 返回值：NSAttributedString 对象。

    这是一种使用 NSAttachmentCharacter 作为基本字符创建包含附件的属性字符串的便捷方法。
    
#### 检索字符信息

- **string** 属性

    NSString 类型，只读。属性字符串的 NSString 对象的内容。

- **length** 属性

    NSUInteger 类型，只读。属性字符串的长度。
    
#### ⌛️检索属性信息
