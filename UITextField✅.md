# UITextField✅

UITextField 继承于 UIControl，用于展示一个可输入文本的控件，这个控件称为「文本框」。文本框只能输入一行内容，**不能够换行输入**。

当用户点击文本框时，它会自动成为第一响应者。当文本框对象成为第一响应者时，系统会自动显示键盘并将其输入绑定到此文本框对象，当然也可以通过调用 `becomeFirstResponder` 方法来强制文本框成为第一响应者。

除了基本的编辑功能外，还可以为文本框指定「叠加视图」，叠加视图添加在文本框最左侧或最右侧，用于显示其他信息或其他可执行控件，UIKit 提供了一些叠加视图，文本框也可以自定义叠加视图。

文本框还可以像 UIButton 一样，可以使用目标-动作（Target-Action）为其添加指定的目标和动作。使用 `addTarget:action:forControlEvents:` 方法为文本框对象添加指定的触发事件和动作方法。对于文本框，常用的关于编辑的 UIControlEvents 事件有以下几种：

| UIControlEvents 部分枚举值 | 说明 |
| :-: | :-- |
| UIControlEventEditingDidBegin | 当文本控件中编辑开始时的事件。 |
| UIControlEventEditingChanged | 当文本控件中的文本被改变的事件。 |
| UIControlEventEditingDidEnd | 当文本控件中编辑结束时的事件。 |
| UIControlEventEditingDidEndOnExit | 当文本控件内通过按下回车键（或等价行为）结束编辑的事件。 |
| UIControlEventAllEditingEvents | 所有关于文本编辑的事件。 |

上面列出的只是开发中常用与文本框有关的事件，查看全部事件参考 UIControl 中的事件。

## UITextField 文本

文本框中的文本有两个，一个是用于显示当前文本框内容的「内容字符串」，另一个是用于当文本框没有内容时，用来提醒用户而显示的「占位符字符串」。

使用下列属性和方法来配置文本框中与文本有关的内容。

- **text** 属性

    NSString 类型，文本框中展示的内容字符串，默认值为空字符串。
  
    这个属性的值表示的是文本框中的内容，当用户在文本框中输入字符时，该属性的值随之变化。

- **attributedText** 属性

    NSAttributedString 类型，文本框中展示的内容字符串（此字符串为特征字符串），默认值为 `nil`。
  
- **placeholder** 属性

    NSString 类型，文本框中没有内容字符串时，显示的占位符字符串，默认值为 `nil`。

    该属性的占位符字符串的颜色不能更改，使用系统定义的颜色（根据文本框父视图的颜色自动设置）。

- **attributedPlaceholder** 属性

    NSAttributedString 类型，文本框中没有内容字符串时，显示的占位符字符串（此字符串为特征字符串），默认值为 `nil`。

- **defaultTextAttributes** 属性

    存储 NSAttributedStringKey 的 NSDictionary 类型，应用于文本框内容字符串的默认特征。

    这个属性的作用是设置文本框中内容字符串特征，如果没有设置某些键，那么其保持默认值，参考 NSAttributedString 类。如果修改了文本框的 textColor 属性等与内容字符串外观有关的属性，那么这个 defaultTextAttributes 属性的值也会对应修改。

- **font** 属性

    UIFont 类型，文本框中内容字符串的字体和占位符字符串的字体，默认值是系统字体样式。

    这个属性的值会应用于整个内容字符串的字体和整个占位符字符串的字体，修改这个属性，两者的字体都会随之更改。

- **textColor** 属性

    UIColor 类型，文本框中内容字符串的颜色，默认值为 `blackColor`。此值不能为 `nil`，否则会引发异常。

    这个属性的值会应用于整个内容字符串。

- **textAlignment** 属性

    NSTextAlignment 类型，内容字符串和占位符字符串的对齐方式。默认值为 NSLeftTextAlignment。

> ⚠️ **注意**
>
>  为 `font` 属性、`textColor` 属性和 `textAlignment` 属性设置新的值时，那么之前的内容字符串和占位符字符串的特征字符串中对应的键也会修改，并且应用于整个字符串。例如：将占位符字符串设置为前两个字符为红色，后面的字符都是黑色，如果此时再为 `textColor` 属性设置新的值，那么整个占位符字符串都将改变为新的值。

- **typingAttributes** 属性

    存储 NSAttributedStringKey 的 NSDictionary 类型，应用于新输入的内容字符串的特征。

    此字典包含要应用于新输入的内容的特征键（和相应的值）。当文本框的选择发生更改时，将自动清除该字典的内容。

    如果文本框未处于编辑模式，则此属性值为 `nil`。同样，除非文本框当前处于编辑模式，否则无法为此属性指定值。

- **adjustsFontSizeToFitWidth** 属性

    BOOL 类型，指示是否应缩小字体的大小来使内容字符串适应文本框的边界矩形，默认值为 `NO`。
    
    通常，文本框的内容字体使用在 `font` 属性中指定的字体绘制的。但是，如果此属性设置为 `YES`，并且当文本框中的内容超出文本框的边界矩形时，则文本框的内容将沿着基线开始缩小字体大小，直到达到最小字体的大小。

    如果将其更改为`YES`，则还应通过修改 `minimumFontSize` 属性来设置适当的最小字体的大小。

- **minimumFontSize** 属性

    CGFloat 类型，用于设置文本框内容的最小允许字体的大小，默认值为 `0.0`。

    在使用 `adjustsFontSizeToFitWidth` 属性时，可以使用此属性来防止文本框将字体大小减小的过多导致字体不清晰。

## UITextField 背景

因为 UITextField 也是继承自 UIView 类，所以可以使用 UIView 的 `backgroundColor` 属性来为其设置背景颜色。

- **borderStyle** 属性

    UITextBorderStyle 类型，文本框的边框样式，默认值为 `UITextBorderStyleNone`。

    如果该值设置为 `UITextBorderStyleRoundedRect` 样式，则文本框的 `background` 属性设置的自定义背景图像无效。
    
    UITextBorderStyle 是一个枚举类型，具体值如下：
 
    | UITextBorderStyle 枚举值 | 说明 | 效果 |
    | :-: | :-: | :-: |
    | UITextBorderStyleNone | 不显示边框，默认。 | 无边框 |
    | UITextBorderStyleLine | 显示细长矩形的边框。 | ![UITextBorderStyleLine](media/15525423234610/UITextBorderStyleLine.png) |
    | UITextBorderStyleBezel | 显示底座样式的边框。<br>（通常用于标准数据输入字段） | ![UITextBorderStyleBeze](media/15525423234610/UITextBorderStyleBezel.png) |
    | UITextBorderStyleRoundedRect | 显示圆角样式边框。 | ![UITextBorderStyleRoundedRect](media/15525423234610/UITextBorderStyleRoundedRect.png) |

    > 🔥 **提示**
    >
    > 将这个属性的值更改为 `UITextBorderStyleNone` 以外的值时，文本框的范围会自动调整，例如：两个大小相同的文本框，使用 `UITextBorderStyleRoundedRect` 值的文本框的可输入范围相对于使用 UITextBorderStyleNone 值的文本框要小一些。

- **background** 属性

  UIImage 类型，文本框显示的背景外观图片，默认值为 `nil`。

  当文本框的边框为 `UITextBorderStyleRoundedRect` 类型时，此属性无效。

- **disabledBackground** 属性

  UIImage 类型，文本框被**禁用时**显示的背景外观图片，默认值为 `nil`。

  如果文本框的 `background` 属性为 `nil`，则此属性不会生效。

## UITextField 键盘

- **keyboardAppearance** 属性

    UIKeyboardAppearance 类型，与文本框关联的键盘外观样式，默认值为 `UIKeyboardAppearanceDefault`。

    UIKeyboardAppearance 枚举值对应的值如下表：

    | UIKeyboardAppearance 枚举值 | 说明 |
    | :-: | :-: | 
    | UIKeyboardAppearanceDefault | 等同于 UIKeyboardAppearanceLight，默认外观。 |
    | UIKeyboardAppearanceLight | 适合浅色界面的白色键盘外观。 | 
    | UIKeyboardAppearanceDark | 适合深色界面的深色键盘外观。 | 

- **keyboardType** 属性

    UIKeyboardType 类型，与文本框对象关联的键盘样式，默认为 `UIKeyboardTypeDefault`。

    UIKeyboardType 枚举值对应的键盘样式和效果如下表：

    >  🔥 **提示**
    >
    > 以下键盘样式均是在 iPhone 6s 上 iOS 12 系统下英文输入法状态下键盘刚打开时的状态。（除了 `UIKeyboardTypeASCIICapableNumberPad` 样式是 iOS 10）

  | UIKeyboardType 枚举值 | 说明 | 效果 |
  | :-: | :-- | :-: |
  | UIKeyboardTypeDefault  | 当前输入方法的默认键盘。 | ![UIKeyboardTypeDefault](media/15525423234610/UIKeyboardTypeDefault.png) |
   | UIKeyboardTypeASCIICapable | 显示标准 ASCII 字符的键盘。 | ![UIKeyboardTypeASCIICapable](media/15525423234610/UIKeyboardTypeASCIICapable.png) |
  | UIKeyboardTypeNumbers<br>AndPunctuation | 数字和标点符号键盘。 | ![UIKeyboardTypeNumbersAndPunctuation](media/15525423234610/UIKeyboardTypeNumbersAndPunctuation.png) |
  | UIKeyboardTypeURL | 针对 URL 条目优化的键盘<br>突出 `.`、`/` 和 `.com` 字符。 | ![UIKeyboardTypeUR](media/15525423234610/UIKeyboardTypeURL.png) |
  | UIKeyboardTypeNumberPad | 专为 PIN 输入的数字小键盘，<br>只显示数字 0 到 9。 | ![UIKeyboardTypeNumberPad](media/15525423234610/UIKeyboardTypeNumberPad.png) |
  | UIKeyboardTypePhonePad | 用于输入电话号码的键盘，<br>只显示数字 0 到 9，<br>以及 `*` 和 `#` 字符，<br>不支持自动大写。 | ![UIKeyboardTypePhonePad](media/15525423234610/UIKeyboardTypePhonePad.png) |
  | UIKeyboardTypeNamePhonePad | 用于输入人名或电话号码的键盘，<br>不支持自动大写。 | ![UIKeyboardTypeNamePhonePad](media/15525423234610/UIKeyboardTypeNamePhonePad.png) |
  | UIKeyboardTypeEmailAddress | 用于输入电子邮件地址的键盘，<br>突出 `@`、`.` 和空格字符。 | ![UIKeyboardTypeEmailAddress](media/15525423234610/UIKeyboardTypeEmailAddress.png) |
  | UIKeyboardTypeDecimalPad | 带数字和小数点的键盘。 | ![UIKeyboardTypeDecimalPad](media/15525423234610/UIKeyboardTypeDecimalPad.png) |
  | UIKeyboardTypeTwitter | 针对 Twitter 文本条目的键盘，<br>突出 `@` 和 `#` 字符。 | ![UIKeyboardTypeTwitte](media/15525423234610/UIKeyboardTypeTwitter.png) |
  | UIKeyboardTypeWebSearch | 针对 Web 搜索的键盘，<br>突出空格和 `.` 字符。 | ![UIKeyboardTypeWebSearch](media/15525423234610/UIKeyboardTypeWebSearch.png) |
  | UIKeyboardType<br>ASCIICapableNumberPad | 指定仅输出 ASCII 数字的键盘，<br>**仅在 iOS 10 上适用**。 | ![UIKeyboardTypeASCIICapableNumberPad](media/15525423234610/UIKeyboardTypeASCIICapableNumberPad.png) |
  | UIKeyboardTypeAlphabet | 指定针对字母输入优化的键盘。 | ![UIKeyboardTypeAlphabet](media/15525423234610/UIKeyboardTypeAlphabet.png) |

- **returnKeyType** 属性

    UIReturnKeyType 类型，键盘上返回键的样式，默认值为 `UIReturnKeyDefault`。

    **返回键上的文字根据当前系统的语言环境决定。**要响应返回键的点击，需要实现 `textFieldShouldReturn:` 代理方法（返回键常用于收起键盘）。

    UIKeyboardType 枚举值对应的键盘样式和效果如下表：

    | UIReturnKeyType 枚举值 | 中文状态下 | 英文状态下 |
    | :-: | :-: | :-: |
    | UIReturnKeyDefault | ![UIReturnKeyDefault_中](media/15525423234610/UIReturnKeyDefault_%E4%B8%AD.png) | ![UIReturnKeyDefault_英](media/15525423234610/UIReturnKeyDefault_%E8%8B%B1.png) |
    | UIReturnKeyGo | ![UIReturnKeyGo_中](media/15525423234610/UIReturnKeyGo_%E4%B8%AD.png) | ![UIReturnKeyGo_英](media/15525423234610/UIReturnKeyGo_%E8%8B%B1.png) |
    | UIReturnKeyJoin | ![UIReturnKeyJoin_中](media/15525423234610/UIReturnKeyJoin_%E4%B8%AD.png) | ![UIReturnKeyJoin_英](media/15525423234610/UIReturnKeyJoin_%E8%8B%B1.png) |
    | UIReturnKeyNext | ![UIReturnKeyNext_中](media/15525423234610/UIReturnKeyNext_%E4%B8%AD.png) | ![UIReturnKeyNext_英](media/15525423234610/UIReturnKeyNext_%E8%8B%B1.png) |
    | UIReturnKeyRoute | ![UIReturnKeyRoute_中](media/15525423234610/UIReturnKeyRoute_%E4%B8%AD.png) | ![UIReturnKeyRoute_英](media/15525423234610/UIReturnKeyRoute_%E8%8B%B1.png) |
    | UIReturnKeySearch | ![UIReturnKeySearch_中](media/15525423234610/UIReturnKeySearch_%E4%B8%AD.png) | ![UIReturnKeySearch_英](media/15525423234610/UIReturnKeySearch_%E8%8B%B1.png) |
    | UIReturnKeySend | ![UIReturnKeySend_中](media/15525423234610/UIReturnKeySend_%E4%B8%AD.png) | ![UIReturnKeySend_英](media/15525423234610/UIReturnKeySend_%E8%8B%B1.png) |
    | UIReturnKeyDone | ![UIReturnKeyDone_中](media/15525423234610/UIReturnKeyDone_%E4%B8%AD.png) | ![UIReturnKeyDone_英](media/15525423234610/UIReturnKeyDone_%E8%8B%B1.png) |
    | UIReturnKeyContinue<br>（仅支持 iOS 9 以上） | ![UIReturnKeyContinue_中](media/15525423234610/UIReturnKeyContinue_%E4%B8%AD.png) | ![UIReturnKeyContinue_英](media/15525423234610/UIReturnKeyContinue_%E8%8B%B1.png) |

- **textContentType** 属性

  UITextContentType 类型，指示文本输入区域预期的语义，默认值为 `nil`。**iOS 10 及以后可用**

  使用此属性为键盘和系统提供有关用户输入内容的预期语义的信息。例如，可以将填写电子邮件的文本框指定为 `UITextContentTypeEmailAddress`。这样系统可以在某些情况下自动选择适当的键盘并改进语法纠错等。
  
  UITextContentType 的具体值参考[官方文档](https://developer.apple.com/documentation/uikit/uitextcontenttype?language=objc)。
  
## UITextField 编辑

- **editing** 属性

  BOOL 类型，只读。指示文本框当前是否处于输入状态。

  当用户开始在这个文本框中编辑时，这个属性自动被设置为 `YES`，编辑结束时，它又自动被设置为 `NO`。

- **clearsOnBeginEditing** 属性

  BOOL 类型，指示文本框开始编辑时是否清空其中的旧内容，默认值为 `NO`。

  如果实现了 `textFieldShouldClear:` 代理方法，则此属性的值将无效。
 
- **clearsOnInsertion** 属性

  BOOL 类型，指示插入文本是否全部选中旧的内容，默认值为 `NO`。

  当该属性的值为 `YES` 且文本框处于编辑模式时，将选中文本框中全部内容的旧内容。

- **allowsEditingTextAttributes** 属性

  BOOL 类型，指示是否可以编辑文本框中的特征字符串，默认值为 `NO`。

  如果此属性设置为 `YES`，则用户可以编辑文本的样式信息。此外，将带样式信息的文本粘贴到文本框中会保留该文本的样式信息。如果此属性为 `NO`，则文本框禁止编辑样式信息并且从粘贴的文本中删除样式信息。

## UITextField 叠加视图

叠加视图（Overlay views）是显示在文本框可编辑区域的左边和右边的小视图。例如文本框右边的清除内容的按钮。通常，叠加视图是一个按钮。例如，可以使用叠加视图来显示或隐藏密码。

**文本框的清除按钮**

在配置叠加视图时，考虑是否希望文本框显示内置的「清除按钮」。清除按钮为用户提供了一种删除文本框所有内容的快捷方法，清除按钮默认显示在右侧，可以重写 `clearButtonRectForBounds:` 方法来设置其位置。

如果提供了自定义的右叠加视图，则使用 `rightViewMode` 和 `clearButtonMode` 属性来确定何时应该显示自定义叠加视图，何时应该显示清除按钮。不过推荐需要使用清楚按钮的地方，就不使用右侧叠加视图。

内置清除按钮的是一个 UIButton 对象，大小是 19\*19pt。而其中的按钮图像是一个 UIImageView 对象，大小是 14\*14pt，内置清除按钮的外观如下图：

![内置清除按钮](media/15525423234610/%E5%86%85%E7%BD%AE%E6%B8%85%E9%99%A4%E6%8C%89%E9%92%AE.png)

通过文本框的下列属性和方法来设置其叠加视图。

- **clearButtonMode** 属性

    UITextFieldViewMode 类型，控制内置的清除按钮何时显示，默认值为 `UITextFieldViewModeNever`。

- **leftView** 属性

    UIView 类型，显示在文本框左侧的叠加视图，默认值为 `nil`。

    要使用 `frame` 属性为其设置大小，不能使用自动布局。左侧叠加视图放在 `leftViewRectForBounds:` 方法返回的矩形区域中。

- **leftViewMode** 属性

    UITextFieldViewMode 类型，控制左侧叠加视图何时出现，默认值为 `UITextFieldViewModeNever`。

- **rightView** 属性

    UIView 类型，显示在文本框右侧的叠加视图，默认值为 `nil`。

    可以使用 `frame` 属性为其设置大小，不能使用自动布局。右侧叠加视图放在 `rightViewRectForBounds:` 方法返回的矩形区域中。

- **rightViewMode** 属性

    UITextFieldViewMode 类型，控制右侧叠加视图何时出现，默认值为 `UITextFieldViewModeNever`。 
    
- **UITextFieldViewMode** 枚举类型

    它的具体值如下表：
    
    | UITextFieldViewMode 枚举值 | 说明 |
    | :-: | :-: |
    | UITextFieldViewModeNever | 永不显示叠加视图。 |
    | UITextFieldViewModeWhileEditing | 仅在文本框处于编辑状态时才显示叠加视图。<br>（如果是清除按钮，文本框有内容并且处于编辑模式时才会显示） |
    | UITextFieldViewModeUnlessEditing | 如果文本框处于编辑状态，且有内容文字，<br>则不显示叠加视图，其余情况都会显示叠加视图。 |
    | UITextFieldViewModeAlways | 如果文本框有内容，则始终显示叠加视图。<br>（如果是清除按钮，文本框有内容并且处于编辑模式时才会显示） |
    
    > 🔥 **提示**
    >
    > 当设置清除按钮的 `clearButtonMode` 属性为 `UITextFieldViewModeWhileEditing` 或 `UITextFieldViewModeAlways` 时，对自定义的右侧叠加视图使用 `UITextFieldViewModeUnlessEditing` 值，可以让清除按钮的右侧叠加视图来回切换而不会覆盖彼此。
    
## UITextField 内部位置

在 UITextField 子类中，以下方法的默认实现是返回参数一中的值，表示文本框绘制其标准的内部位置。可以在子类中重写这些方法，返回自定义的矩形，以此设置左侧叠加视图、右侧叠加视图、清除按钮、输入框等部件相对于整个按钮的位置。注意不要主动调用这些方法。

- **clearButtonRectForBounds:** 方法

    返回清除按钮的绘制矩形，以定位清除按钮的位置。

    - 参数一：CGRect 类型，清除按钮默认的矩形。
    - 返回值：CGRect 类型，用于绘制清除按钮的矩形。
    
    更改清除按钮的大小可能会导致清除按钮图像模糊，所以可以修改清楚按钮的位置，但是不要修改其大小。
    
- **leftViewRectForBounds:** 方法

    返回左侧叠加视图的绘制矩形，以定位左侧叠加视图的位置。

    - 参数一：CGRect 类型，左侧叠加视图默认的矩形。
    - 返回值：CGRect 类型，用于绘制左侧叠加视图的矩形。

    默认实现的左侧视图的位置与文本框的左侧对齐，且左侧视图受自身的 frame 值影响。如果文本框的 `leftView` 属性为 `nil`，则不会调用此方法，如果 `leftViewMode` 属性为 `UITextFieldViewModeNever` 也不会调用此方法。
    
- **rightViewRectForBounds:** 方法

    返回右侧叠加视图的绘制矩形，以定位右侧叠加视图的位置。

    - 参数一：CGRect 类型，右侧叠加视图默认的矩形。
    - 返回值：CGRect 类型，用于绘制左侧叠加视图的矩形。

    默认实现的右侧视图的位置与文本框的右侧对齐，且右侧视图受自身的 frame 值影响。如果文本框的 `rightView` 属性为 `nil`，则不会调用此方法，如果 `rightViewMode` 属性为 `UITextFieldViewModeNever` 也不会调用此方法。
    
- **textRectForBounds:** 方法

    返回文本内容视图的绘制矩形，以定位文本内容视图的位置。

    - 参数一：CGRect 类型，文本内容视图默认的矩形。
    - 返回值：CGRect 类型，用于绘制文本内容视图的矩形。

    此方法设置的是文本框未在编辑状态下时的文本内容视图的位置。没有左侧视图时默认实现的位置与文本框的左侧对齐，有左侧视图时默认实现的位置与左侧视图的右侧对齐。
    
- **editingRectForBounds:** 方法

    返回编辑时文本内容视图的绘制矩形，以定位编辑时文本内容视图的位置。

    - 参数一：CGRect 类型，编辑时文本内容视图默认的矩形。
    - 返回值：CGRect 类型，用于绘制编辑时文本内容视图的矩形。

    此方法设置的是文本框在编辑状态下时的文本内容视图的位置。没有左侧视图时默认实现的位置与文本框的左侧对齐，有左侧视图时默认实现的位置与左侧视图的右侧对齐。

## UITextField 输入视图

当文本框成为第一响应者时弹出的视图称为**输入视图**。默认是系统自带的键盘作为输入视图。还可以为输入视图指定一个附件视图。

- **inputView** 属性

    UIView 类型，当文本框成为第一响应者时显示的视图，默认值为 `nil`。

    如果此属性的值为 `nil`，则当文本框成为第一响应者时将显示标准的系统键盘。
    
    该视图忽略宽度，无论视图的宽度是多少，最终显示都是屏幕的宽度。

- **inputAccessoryView** 属性

    UIView 类型，当文本框成为第一响应者时显示的附件视图，默认值为 `nil`。

    当文本框成为第一响应者时此属性指向的视图将显示在标准系统键盘上方（如果提供了自定义输入视图，则显示在自定义输入视图上方）。例如，可以使用此属性将自定义工具栏附加到键盘。

## UITextField 处理键盘通知

> 📚 官方文档：**[Text Programming Guide for iOS - Managing the Keyboard](https://developer.apple.com/library/archive/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1)**

系统管理键盘的显示和隐藏，因此当文本框进入编辑模式弹出键盘或退出编辑模式收起键盘整个过程会发布以下通知用来跟踪与键盘相关的变化。

- **UIKeyboardWillShowNotification**

    在键盘显示之前立即发布通知。

- **UIKeyboardDidShowNotification**

    在键盘显示之后立即发布通知。

- **UIKeyboardWillHideNotification**

    在隐藏键盘之前立即发布通知。

- **UIKeyboardDidHideNotification**

    在隐藏键盘之后立即发布通知。

- **UIKeyboardWillChangeFrameNotification**

    在键盘 frame 发生变化开始前立即发布通知。

- **UIKeyboardDidChangeFrameNotification**

    在键盘 frame 发生变化完成后立即发布通知。 

上述每个通知都包含一个 **userInfo** 字典，字典中包含有关键盘的信息，并且每个通知的 **object** 为 `nil`。使用「KeyboardNotification User Info Keys」中描述的键从 userInfo 字典中获取键盘的位置和大小。

由于键盘会遮挡部分界面，因此应该使用键盘的尺寸信息在屏幕上重新定位内容视图的位置，以至于不会被键盘遮挡。对于嵌入在滚动视图中的内容，可以将文本框滚动到可见的位置中，如下图所示。在其他情况下，可以调整主内容视图的大小，使其不被键盘覆盖。

![键盘弹出时的处理](media/15525423234610/%E9%94%AE%E7%9B%98%E5%BC%B9%E5%87%BA%E6%97%B6%E7%9A%84%E5%A4%84%E7%90%86.png)

「KeyboardNotification User Info Keys」是指用于从键盘通知的 userInfo 字典中获取值的键的统称，具体有下列键：

- **UIKeyboardAnimationCurveUserInfoKey**

    包含 UIViewAnimationCurve 常量的 NSNumber 对象的键，用于定义键盘在屏幕上出现和消失的动画方式。

- **UIKeyboardAnimationDurationUserInfoKey**

    包含 double 类型的 NSNumber 对象的键，包含键盘动画的持续时间（以秒为单位）。

- **UIKeyboardIsLocalUserInfoKey**

    包含布尔值的 NSNumber 对象的键，该布尔值标识键盘是否属于当前应用程序。通过 iPad 进行多任务处理，当键盘出现并消失时，会通知所有可见应用程序。对于使键盘出现的应用程序，此键的值为 `YES`，而对于其他应用程序，该值为 `NO`。

- **UIKeyboardFrameBeginUserInfoKey**

    包含 CGRect 的 NSValue 对象的键，用于标识屏幕坐标中键盘的起始 frame。

- **UIKeyboardFrameEndUserInfoKey**

  包含 CGRect 的 NSValue 对象的键，用于标识屏幕坐标中键盘的结束 frame。

## UITextFieldDelegate

UITextFieldDelegate 协议提供用于管理文本框对象中内容的编辑和验证的方法。UITextField 的代理对象必须遵守该协议，并且文本框的 `delegate` 属性指向该代理对象。

### 调用代理方法和发送通知的过程

文本框调用其代理的方法以响应更改。可以使用这些方法验证用户输入的字符串，响应与键盘的特定交互，以及控制整个编辑过程等。在文本框成为第一响应者并显示键盘（或自定义的输入视图）之后进入编辑状态。 整个过程的流程如下：

1. 在成为第一响应者之前，文本框调用其代理对象的 `textFieldShouldBeginEditing:` 方法。该方法允许或阻止编辑文本框的内容。

2. 文本框成为第一响应者。

    此时系统显示键盘（或自定义的输入视图）并根据需要发布 `UIKeyboardWillShowNotification` 和`UIKeyboardDidShowNotification` 通知。如果键盘（或自定义的输入视图）已经可见，则系统会发布 `UIKeyboardWillChangeFrameNotification` 和 `UIKeyboardDidChangeFrameNotification` 通知。

3. 文本框调用其代理对象的 `textFieldDidBeginEditing:` 方法并发布 `UITextFieldTextDidBeginEditingNotification` 通知。

4. 文本框在编辑期间会调用代理对象的下列方法：

    - 当文本框中的内容字符串发生更改时，它都会调用 `textField:shouldChangeCharactersInRange:replacementString:` 方法并发布 `UITextFieldTextDidChangeNotification` 通知。

    - 当用户点击文本框的内置清除按钮时，它会调用 `textFieldShouldClear:` 方法。

    - 当用户点击键盘的返回按钮时，它会调用 `textFieldShouldReturn:` 方法。

5. 在文本框将要取消第一响应者之前，文本框调用 `textFieldShouldEndEditing:` 方法。

6. 文本框取消第一响应者。

    此时系统根据需要隐藏或调整键盘。隐藏键盘时，系统会发布 `UIKeyboardWillHideNotification` 和`UIKeyboardDidHideNotification` 通知。

7. 文本框调用其代理对象的 `textFieldDidEndEditing:` 方法并发布 `UITextFieldTextDidEndEditingNotification` 通知。

### 代理方法

- **textFieldShouldBeginEditing:** 方法

    询问代理是否应在指定的文本框中开始编辑。

    - 参数一：UITextField 类型，文本框对象。
    - 返回值：BOOL 类型，`YES` 表示允许编辑，`NO` 表示不允许编辑（不允许编辑时不会有任何交互效果）。

    当用户点击文本框时，文本框将会成为第一响应者。在成为第一响应者之前，文本框调用这个方法。使用该方法允许或阻止编辑文本框的内容。

    如果代理未实现此方法，则此方法的默认实现返回 `YES`。

- **textFieldDidBeginEditing:** 方法

    告诉代理已经在指定的文本框中开始编辑。

    - 参数一：UITextField 类型，文本框对象。

    此方法通知代理对象指定的文本框刚刚成为第一响应者。使用此方法更新状态信息或执行其他任务。例如，可以使用此方法显示仅在编辑时可见的叠加视图。

- **textFieldShouldEndEditing:** 方法

    询问代理是否应在指定的文本框中停止编辑。

    - 参数一：UITextField 类型，文本框对象。
    - 返回值：BOOL 类型，`YES` 表示停止编辑，`NO` 表示继续编辑。

    如果代理未实现此方法，则此方法的默认实现返回 `YES`。

- **textFieldDidEndEditing:** 方法

    告诉代理指定的文本框已停止编辑。

    - 参数一：UITextField 类型，文本框对象。

    如果代理对象也实现了 `textFieldDidEndEditing:reason:` 方法，则 UIKit 会优先调用该方法。

- **textFieldDidEndEditing:reason:** 方法

    告诉代理指定的文本框已停止编辑。

    - 参数一：UITextField 类型，文本框对象。
    - 参数二：UITextFieldDidEndEditingReason 类型，编辑结束的原因。

- **textFieldShouldReturn:** 方法

    询问代理是否应处理指定文本框的返回按钮按下的事件。

    - 参数一：UITextField 类型，文本框对象。
    - 返回值：BOOL 类型，如果为 `YES`，则文本框应实现其返回按钮的默认行为；否则为 `NO`。

    当用户点击返回按钮时，都会调用这个代理方法，并且会执行方法中的语句。可以用这个方法来关闭键盘等。

- **textFieldShouldClear:** 方法

     询问代理是否应删除文本框的当前内容。

    - 参数一：UITextField 类型，文本框对象。
    - 返回值：BOOL 类型，如果为 `YES`，则清空文本框中的内容，否则为 `NO`。

    文本框调用此方法来响应用户按下清除按钮的事件，如果文本框的 `clearsOnBeginEditing` 属性设置为 `YES`，那么在编辑开始时也会调用此方法。如果代理未实现此方法，则此方法的默认实现返回 `YES`。

- **textField:shouldChangeCharactersInRange:replacementString:** 方法

    询问代理是否应该更改指定的字符串。**当用户操作使文本框中的内容字符串发生更改时（添加或删除），代理将调用此方法。**

    - 参数一：UITextField 类型，文本框对象。
    - 参数二：NSRange 类型，要替换的字符串的范围。
    - 参数三：NSString 类型，指在定范围内的要插入的字符串。
    - 返回值：BOOL 类型，`YES` 表示指定的文本范围应该被替换；`NO` 表示不替换，保留旧文本。

    > ⚠️ **注意**
    > 
    > 这个方法是在用户按下键盘时，但是内容还没替换到文本框中的时候调用，这个方法执行完之后，再根据方法的返回值确定是否把用户的输入替换到文本框中。所以不应该使用这个方法来判断与文本框中字符长度有关的问题。判断与文本框内容长度有关的问题，为文本框添加目标-动作方法，使用 `UIControlEventEditingChanged` 状态来监听文本框字符变化。

    最开始时，文本框中的内容为空字符串 `@""`，当用户点击文本框，输入第一个字符到文本框时，会调用这个方法，输入第二个字符时会调用这个方法......当用户删除一个字符时，也会调用这个方法。也就是说用户的操作在文本框中已存在的文本的基础上，只要发生变化就会调用这个方法。

    为了更好理解「参数二」和「参数三」的作用，这里举几个例子：
        
    - 假如现在文本框中的字符为「abcde」，现在用户输入「f」，调用此方法，那么参数二的 length 和 location 两个值分别为 0 和 5：表示从第 5 个位置开始（因为字符「e」的位置是 4），替换的字符长度是 0（此时没有字符要被替换，所以长度为 0）。而此时的第三个参数的值就是：f。
   
    - 假如现在文本框的字符为「abcde」，现在选中「cde」，然后输入「f」，调用此方法，那么方法的第二个参数的 length 和 location 两个值分别为 3 和 2：表示从第 2 个位置开始（因为字符「c」的位置是 2），替换的字符长度是 3（因为字符「cde」长度为 3）。这样把「cde」就替换成了「f」。并且此时的第三个参数的值是：f。
    
    - 假如现在文本框的字符为「abcde」，现在选中「cde」，复制，然后选中「bcde」，粘贴。调用此方法，那么方法的第二个参数的 length 和 location 两个值分别为 4 和 1：表示从第 1 个位置开始，替换的字符长度是 4。这样把「bcde」就替换成了「cde」。而此时的第三个参数的值就是：cde。

    - 假如现在文本框中的字符为「abcde」，现在用户按下回车键，调用此方法，那么参数二的 length 和 location 两个值分别为 0 和 4：表示从第 4 个位置开始，替换的字符长度是 0。这样把「e」就替换成了空字符。并且此时的第三个参数的值就是空字符。

    综合上面的例子能看出，其实方法第二个参数的值的是文本框中要改变的字符的范围，而第三个参数就是要添加到文本框中的内容（当删除时，这个字符串为空字符串）。

    要特别注意的是，当用户通过中文拼音输入的时候，比如输入「哈」这个字，首先按下「h」，这时「h」这个字母是出现在文本框中的，所以也会调用此方法，然后按下「a」，同样再次调用这个方法，最后用户在键盘上选择「哈」这个字，这时文本框中就只有「哈」这个字符，这时同样会调用此方法。此过程的参数变化如下图：

    ![参数变化](media/15525423234610/%E5%8F%82%E6%95%B0%E5%8F%98%E5%8C%96.png)

    这就是为什么在一个限制长度的文本框中，使用拼音输入最后几个字符时，当拼音长度超过了文本框限制的最大长度，就会无法完成拼音输入的原因。

    > 🔥 **提示**
    >
    > 当使用内置清除按钮清空文本框的内容时，并不会调用这个此方法。

