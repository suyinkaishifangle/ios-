# UITextField 和 UITextView

> 环境：<i class="fas fa-laptop "></i> Xcode 10.0 &nbsp;&nbsp;<i class="fas fa-mobile-alt"></i> iOS 12.0

## UITextField

UITextField 继承于 UIControl，用于展示一个可输入文本的文本控件，但是它只能输入一行内容，不会换行。

当用户点击 UITextField 时，它会自动成为第一响应者。当 UITextField 的对象成为第一响应者时，系统会自动显示键盘并将其输入绑定到此 UITextField 的对象。还可以通过调用 UITextField 的 `becomeFirstResponder` 方法来强制 UITextField 成为第一响应者。

### UITextField 文本

- **text 属性**

    输入的文本，NSString 类型。
    
- **attributedText 属性**

    输入的文本（富文本），NSAttributedString 类型。
    
- **placeholder 属性**

    没有输入的文本时显示的字符串（即占位符字符串），NSString 类型，默认为 nil。
    
    UITextField 的占位符字符串的字体和字号与 UITextField 的 font 属性中定义的字体和字号相同，但是颜色是系统默认的浅灰色，无法修改。要自定义的话需要使用 attributedPlaceholder 属性。
    
- **attributedPlaceholder 属性**

   占位符字符串（富文本）， NSAttributedString 类型，默认为 nil。

- **defaultTextAttributes 属性** ⚠️

- **font 属性**

    输入本文的字体样式，用来设置字体和字号。UIFont 类型。默认值是系统字体的样式。
    
- **textColor 属性**

    输入本文的字体颜色。UIColor 类型。默认值是黑色，属性的值只能设置为非 nil 值，将此属性设置为 nil 会引发异常。
    
- **textAlignment 属性**
    
    输入本文和占位符字符串的对齐方式。NSTextAlignment 类型。默认值是NSLeftTextAlignment。
    
- **typingAttributes 属性** ⚠️
 
### UITextField 外观

- **borderStyle 属性**
 
    UITextField 使用的边框样式。UITextBorderStyle 类型，默认值是 UITextBorderStyleNone。
 
    如果值设置为 UITextBorderStyleRoundedRect 样式，则与 UITextField 关联的自定义背景图像将被忽略。
 
    UITextBorderStyle 是一个枚举类型，具体值如下：
 
 
    | UITextBorderStyle 枚举值 | 说明 | 效果 |
    | :-: | :-: | :-: |
    | UITextBorderStyleNone | 不显示边框，默认 | 无 |
    | UITextBorderStyleLine | 显示细长矩形的边框 | ![UITextBorderStyleLine](media/15380541769828/UITextBorderStyleLine.png) |
    | UITextBorderStyleBezel | 显示 bezel-style 边框<br>（这种样式通常用于标准数据输入字段） | ![UITextBorderStyleBeze](media/15380541769828/UITextBorderStyleBezel.png) |
    | UITextBorderStyleRoundedRect | 显示圆角样式边框 | ![UITextBorderStyleRoundedRect](media/15380541769828/UITextBorderStyleRoundedRect.png) |

    <p style="text-align:center;color:#999999;font-size:14px"> UITextBorderStyle 枚举值 </p>
    
    使用此属性会自动调整 UITextField 中的输入区域，如下图中两个 UITextField 的尺寸设置为相同的，但上方的 UITextField 使用了 UITextBorderStyleRoundedRect 边框样式，下面的 UITextField 没有使用此属性。
    
    ![UITextField 使用边框样式和不使用边框样式对比](media/15380541769828/UITextField%20%E4%BD%BF%E7%94%A8%E8%BE%B9%E6%A1%86%E6%A0%B7%E5%BC%8F%E5%92%8C%E4%B8%8D%E4%BD%BF%E7%94%A8%E8%BE%B9%E6%A1%86%E6%A0%B7%E5%BC%8F%E5%AF%B9%E6%AF%94.png)

<p style="text-align:center;color:#999999;font-size:14px"> UITextField 使用边框样式和不使用边框样式对比 </p>

- **background 属性**

    UITextField 显示的背景外观图像。UIImage 类型，此属性默认设置为 nil。
    
    在设置了这个属性的值的前提下，并且设置了 borderStyle 属性的值为 UITextBorderStyleBezel，那么这个背景图片会替换 UITextBorderStyleBezel 边框样式，并且这个 UITextField 相比只使用 background 属性设置的背景图片的 UITextField 还是有区别，如下图：
    
    ![同时使用UITextBorderStyleBezel 边框样式和背景图片的对比](media/15380541769828/%E5%90%8C%E6%97%B6%E4%BD%BF%E7%94%A8UITextBorderStyleBezel%20%E8%BE%B9%E6%A1%86%E6%A0%B7%E5%BC%8F%E5%92%8C%E8%83%8C%E6%99%AF%E5%9B%BE%E7%89%87%E7%9A%84%E5%AF%B9%E6%AF%94.png)
    
    <p style="text-align:center;color:#999999;font-size:14px"> UITextField 同时使用 UITextBorderStyleBezel <br>边框样式和背景图片与只使用背景图片的对比 </p>
    
    背景图像绘制在 UITextField 的边框矩形部分（如上图中下方的 UITextField）。用于 UITextField 背景的图像应该能够拉伸以适应需要。
    
- **disabledBackground 属性**

    禁用 UITextField 时显示的背景外观图像。UIImage 类型，此属性默认设置为 nil。
    
    类比 background 属性，这个属性仅仅是在禁用 UITextField 时生效。

### UITextField 叠加视图

叠加视图（Overlay views）是显示在 UITextField 可编辑区域的左边和右边的小视图。例如 UITextField 右边的清除内容的按钮。通常，叠加视图是一个按钮。例如，您可以使用叠加视图来显示或隐藏密码。

在配置叠加视图时，考虑是否希望文本字段显示内置的clear按钮。clear按钮为用户提供了一种删除所有文本字段文本的方便方法。此按钮显示在正确的叠加位置，但如果您提供了自定义的右叠加视图，则使用rightViewMode和clearButtonMode属性来定义何时应该显示自定义叠加，何时应该显示clear按钮。

通过 UITextField 的下列属性和方法来设置叠加视图：

- **clearButtonMode 属性**

    控制内置的清除按钮何时出现在 UITextField 中。UITextFieldViewMode 类型，默认值为 UITextFieldViewModeNever。
    
    UITextFieldViewMode 是枚举类型，它的具体值如下表：
     
    | UITextFieldViewMode 枚举值 | 说明 |
    | :-: | :-: |
    | UITextFieldViewModeNever | 叠加视图永远不会显示 |
    | UITextFieldViewModeWhileEditing | 仅在编辑时才显示叠加视图<br>(如果是内置清除按钮，则 UITextField 有内容才会出现) |
    | UITextFieldViewModeUnlessEditing | 仅在未编辑时才显示叠加视图 |
    | UITextFieldViewModeAlways | 如果有文字，则始终显示叠加视图 |

    <p style="text-align:center;color:#999999;font-size:14px"> UITextFieldViewMode 枚举值 </p>
    
    > <i class="fas fa-exclamation-circle"></i> 其中 UITextFieldViewModeWhileEditing 枚举值是指当 UITextField 成为第一响应者时显示叠加视图。但是要注意，虽然对 clearButtonMode 属性赋值 UITextFieldViewModeWhileEditing，但是内置的清除按钮并不是当 UITextField 成为第一响应者时就马上出现，而是要 UITextField 中存在文字时才会出现。
    
    当 UITextField 中存在文字时，内置的清除按钮显示在 UITextField 的右侧，为用户提供快速删除文本的方式。内置的清除按钮根据这个属性的值确定显示的时机。内置清除按钮实际样式如下图：
    
    ![内置清除按钮](media/15380541769828/%E5%86%85%E7%BD%AE%E6%B8%85%E9%99%A4%E6%8C%89%E9%92%AE.png)
    
    <p style="text-align:center;color:#999999;font-size:14px"> UITextField 的内置清除按钮 </p>
    
    内置清除按钮的 UIImageView 的尺寸是 14\*14 pt，而放置这个 UIImageView 的 UIButton 尺寸是 19\*19 pt。
    
- **clearButtonRectForBounds: 方法**

    返回内置清除按钮的绘制矩形。
    
    - 第一个参数：边界矩形。
    - 返回值：用于绘制清除按钮的矩形。

    不应该直接调用这个方法。如果想要将内置清除按钮放在其他位置，可以重写此方法并返回新矩形。重写的方法应该调用父类的实现并只且修改返回的矩形的 origin。如果不重写这个方法，那么内置清楚按钮默认在 UITextField 的左边 
    
    > <i class="fas fa-exclamation-circle"></i> 更改内置清除按钮的大小可能会导致按钮图像失真。
    
- **leftView 属性**

    显示在 UITextField 左侧的叠加视图。UIView 类型。要使用 frame 属性为其设置大小，不能使用自动布局。
    
    左侧覆盖视图被放置在 UITextField 的 `leftViewRectForBounds:` 方法返回的矩形中。
    
- **leftViewMode 属性**

    控制左侧叠加视图何时出现在 UITextField 中。UITextFieldViewMode 类型，默认值为 UITextFieldViewModeNever。
    
- **leftViewRectForBounds: 方法**

    返回左侧叠加视图的绘制矩形。
    
    - 第一个参数：边界矩形。
    - 返回值：用于绘制左侧叠加视图的矩形。
   
    不应该直接调用此方法。如果想要将左侧叠加视图放在其他位置，可以重写此方法并返回新矩形。请注意，绘制矩形在从右到左的用户界面中保持不变。
    
- **rightView 属性**

    显示在 UITextField 右侧的叠加视图。UIView 类型，要使用 frame 属性为其设置大小，不能使用自动布局。
    
    右侧叠加视图被放置在 UITextField 的 `rightViewRectForBounds:` 方法返回的矩形中。
    
    假如要设置一个在密码框后用来显示或隐藏密码的按钮，为了更好的用户体验。个人在开发中是将这个按钮大小尽量与账号框的内置清除按钮对齐，所以就涉及到如何设置这个密码框的 rightView 中按钮的大小。经过测试，发现把右侧叠加视图的 UIButton 尺寸设置为 **28\*19**，UIButton 中添加的按钮图片尺寸是为 **14\*14**（如果按钮图片为长方形，以短的一边为 14）时最为合适，如下图：

    ![内置清除按钮和右侧叠加视图对齐](media/15380541769828/%E5%86%85%E7%BD%AE%E6%B8%85%E9%99%A4%E6%8C%89%E9%92%AE%E5%92%8C%E5%8F%B3%E4%BE%A7%E5%8F%A0%E5%8A%A0%E8%A7%86%E5%9B%BE%E5%AF%B9%E9%BD%90.png)

    <p style="text-align:center;color:#999999;font-size:14px"> 内置清除按钮和右侧叠加视图对齐 </p>

    其实从图中还是能看出稍微有点偏差，但是从用户的视觉效果来看是对齐的。解释一下为什么使用 28*19 尺寸的 UIButton。首先，通过打印内置清除按钮的尺寸，发现其尺寸是 19\*19，如果将叠加视图也设置为 19\*19，通过界面看发现并不是对齐了。因为内置的清除按钮并不是直接使用的叠加视图实现的，从上图中的 UITextField 输入框可以看出来。叠加视图的输入框会自动调整为紧贴叠加视图，而内置清除按钮的并不是！其次，在官方文档中给出的示例代码是为 UITextField 添加一个书签按钮，设置的尺寸是 28\*28。于是折中一下，将叠加视图的按钮尺寸设置为 28\*19，宽度 28 刚好和内置清除按钮对齐，高度 19 和内置清除按钮高度一致。（当然其实可以自定义一个清除按钮，这里就不提具体方法了）

    > <i class="fas fa-exclamation-circle"></i> 注意，虽然按钮的尺寸已经确定了，但实际上在 UITextField 中叠加视图（包括内置清除按钮）的点击范围比按钮的范围要大，暂时不知道如何设置，但是这个点击范围默认是合适的。
    
        
- **rightViewMode 属性**

    控制右侧叠加视图何时出现在 UITextField 中。UITextFieldViewMode 类型，默认值为 UITextFieldViewModeNever。

- **rightViewRectForBounds: 方法**

    返回右侧叠加视图的绘制矩形。
    
    - 第一个参数：边界矩形。
    - 返回值：用于绘制右侧叠加视图的矩形。
    
    不应该直接调用此方法。如果想要将右侧叠加视图放在其他位置，可以重写此方法并返回新矩形。请注意，绘制矩形在从右到左的用户界面中保持不变。
    
下面在实际项目中为密码输入框添加密码显示或隐藏按钮的示例代码（部分）：

```Objective-C
- (void)viewDidLoad {
   // ... 其他控件
   
	// 密码输入框
	_passwordTextField = [[UITextField alloc] init];
	_passwordTextField.textColor = mineShaftColor;
	_passwordTextField.font = _accountTextField.font;
	_passwordTextField.secureTextEntry = YES;
	_passwordTextField.delegate = self;
	_passwordTextField.rightViewMode = UITextFieldViewModeWhileEditing;
	_passwordTextField.attributedPlaceholder = [[NSAttributedString alloc]
												initWithString:NSLocalizedString(@"请输入密码", nil)
												attributes:@{
															 NSForegroundColorAttributeName:paleSlateColor,
															 NSFontAttributeName:_accountTextField.font
															 }];
	[self.view addSubview:_passwordTextField];
    	
   // ... 其他控件
}


#pragma mark - 交互事件

/**
 切换密码框显示或隐藏密码

 @param sender 显示或隐藏密码的按钮
 */
- (void)clickHideOrShowPasswordButton:(UIButton *)sender {
	
	NSString *passwordString = _passwordTextField.text;
	_passwordTextField.text = nil;
	
	if (_passwordTextField.secureTextEntry) {
		[_hideOrShowPasswordButton setImage:[UIImage imageNamed:@"showPassword"]
								   forState:UIControlStateNormal];
		[_hideOrShowPasswordButton setImage:[UIImage imageNamed:@"showPassword"]
								   forState:UIControlStateHighlighted];
		_passwordTextField.secureTextEntry = NO;
	} else {
		[_hideOrShowPasswordButton setImage:[UIImage imageNamed:@"hidePassword"]
								   forState:UIControlStateNormal];
		[_hideOrShowPasswordButton setImage:[UIImage imageNamed:@"hidePassword"]
								   forState:UIControlStateHighlighted];
		_passwordTextField.secureTextEntry = YES;
	}
	
	_passwordTextField.text = passwordString;
}


#pragma mark - UITextField 代理

- (void)textFieldDidBeginEditing:(UITextField *)textField {
	if (textField == _passwordTextField) {
		[self hideOrShowPasswordButton];
	} else {
		
	}
}


#pragma mark - 懒加载

- (UIButton *)hideOrShowPasswordButton {
	if (!_hideOrShowPasswordButton) {
		_hideOrShowPasswordButton = [UIButton buttonWithType:UIButtonTypeCustom];
		_hideOrShowPasswordButton.frame = CGRectMake(0, 0, 28, 19);
		[_hideOrShowPasswordButton setImage:[UIImage imageNamed:@"hidePassword"]
								   forState:UIControlStateNormal];
		[_hideOrShowPasswordButton setImage:[UIImage imageNamed:@"hidePassword"]
								   forState:UIControlStateHighlighted];
		_passwordTextField.rightView = _hideOrShowPasswordButton;
		
		[_hideOrShowPasswordButton addTarget:self
									  action:@selector(clickHideOrShowPasswordButton:)
							forControlEvents:UIControlEventTouchUpInside];
	}
	return _hideOrShowPasswordButton;
}
```

解释一下部分代码，这里的显示或隐藏按钮设置为懒加载的方式，并且在 UITextField 的代理方法中（这里用的是 UITextField 成为第一响应者时会调用方法）调用懒加载，还有一点就是将显示或隐藏按钮的普通和高亮状态都设置了图片，是因为按钮的类型是 `UIButtonTypeCustom`，如果只设置普通状态下的图片，那么在点击时，会默认高亮状态下加深颜色（系统自动加深的颜色），这样把两个状态下的按钮图片设置为相同，那么点击时按钮就不会变化，而点击事件结束后按钮图片又切换为另一种。

### UITextField 行为

- **editing 属性**

    指示 UITextField 当前是否处于编辑模式。BOOL 类型，只读。
    
    当用户开始在这个 UITextField 中编辑文本时，这个属性被设置为 YES；当编辑结束时，它又被设置为NO。文本字段在编辑开始和结束时通知其代理。
    
- **clearsOnBeginEditing 属性**

    开始编辑时是否清空 UITextField 中的旧文本。BOOL 类型。默认值为 NO。
    
    > <i class="fas fa-exclamation-circle"></i> 即使将该属性设置为 YES，UITextField 代理也可以通过从其`textFieldShouldClear:` 方法返回 NO 来覆盖此属性的效果。
    
- **clearsOnInsertion 属性**

    插入文本是否选中先前的内容。BOOL 类型，默认值为 NO。
    
    当该属性的值为 YES 且 UITextField 处于编辑模式时，旧文本（如果存在旧文本）将被全部选中。
    
### UITextField 键盘

当 UITextField 成为第一响应者时，系统会自动显示键盘并将其输入绑定到 UITextField。当用户点击 UITextField 时，它会自动成为第一响应者。也可以通过调用 UITextField 的 `becomeFirstResponder` 方法来强制 UITextField 成为第一响应者。

- **keyboardType 属性** 

    设置 UITextField 处于第一响应者是弹出的系统键盘的类型。UIKeyboardType 类型，默认为 UIKeyboardTypeDefault。参考下文「键盘类型」。

- **keyboardAppearance 属性**

    设置编辑 UITextField 是出现的键盘的外观样式。UIKeyboardAppearance 类型，默认为 UIKeyboardAppearanceDefault。参考下文「键盘外观」。
    
- **returnKeyType 属性**

    设置键盘上返回键的样式。UIReturnKeyType 类型，默认值为 UIReturnKeyDefault。参考下文「键盘返回键」。
    
    返回键默认情况下处于禁用状态。要响应返回键的点击，需要实现 `textFieldShouldReturn:` 代理方法。
    
- **inputView 属性** ⚠️
- **inputAccessoryView 属性** ⚠️

#### 键盘类型

UIKeyboardType 枚举值对应的键盘样式和效果如下表：

> <i class="fas fa-exclamation-circle"></i> 以下键盘样式均是在 iPhone 6s 上英文输入法状态下键盘刚打开时的状态。

| UIKeyboardType 枚举值 | 说明 | 效果 |
| :-: | :-: | :-: |
| UIKeyboardTypeDefault  | 当前输入方法的默认键盘 | ![UIKeyboardTypeDefault](media/15332026405298/UIKeyboardTypeDefault.png) |
| UIKeyboardTypeASCIICapable | 显示标准ASCII字符的键盘 | ![UIKeyboardTypeASCIICapable](media/15332026405298/UIKeyboardTypeASCIICapable.png) |
| UIKeyboardTypeNumbersAndPunctuation | 数字和标点符号键盘 | ![UIKeyboardTypeNumbersAndPunctuation](media/15332026405298/UIKeyboardTypeNumbersAndPunctuation.png) |
| UIKeyboardTypeURL | 针对 URL 条目优化的键盘<br>突出显示 `.`、`/` 和 `.com` 字符串 | ![UIKeyboardTypeUR](media/15332026405298/UIKeyboardTypeURL.png) |
| UIKeyboardTypeNumberPad | 专为PIN输入设计的数字小键盘<br>突出显示数字 0 到 9 <br>不支持自动大写 | ![UIKeyboardTypeNumberPad](media/15332026405298/UIKeyboardTypeNumberPad.png) |
| UIKeyboardTypePhonePad | 用于输入电话号码的键盘 <br>突出显示数字 0 到 9 <br>以及 `*` 和 `#` 字符<br>不支持自动大写 | ![UIKeyboardTypePhonePad](media/15332026405298/UIKeyboardTypePhonePad.png) |
| UIKeyboardTypeNamePhonePad | 用于输入人名或电话号码的键盘<br>不支持自动大写 | ![UIKeyboardTypeNamePhonePad](media/15332026405298/UIKeyboardTypeNamePhonePad.png) |
| UIKeyboardTypeEmailAddress | 用于输入电子邮件地址的键盘<br>突出显示 `@`、`.` 和空格字符 | ![UIKeyboardTypeEmailAddress](media/15332026405298/UIKeyboardTypeEmailAddress.png) |
| UIKeyboardTypeDecimalPad | 带数字和小数点的键盘 | ![UIKeyboardTypeDecimalPad](media/15332026405298/UIKeyboardTypeDecimalPad.png) |
| UIKeyboardTypeTwitter | 针对 Twitter 文本条目的键盘<br>突出显示 `@` 和 `#` 字符 | ![UIKeyboardTypeTwitter](media/15332026405298/UIKeyboardTypeTwitter.png) |
| UIKeyboardTypeWebSearch | 针对 Web 搜索和 URL 条目的键盘<br>突出显示空格和 `.` 字符 | ![UIKeyboardTypeWebSearch](media/15332026405298/UIKeyboardTypeWebSearch.png) |
| UIKeyboardTypeASCIICapableNumberPad | 指定仅输出ASCII数字的数字键盘<br>**仅在 iOS 10 上适用** | ![UIKeyboardTypeASCIICapableNumberPad](media/15332026405298/UIKeyboardTypeASCIICapableNumberPad.png) |
| UIKeyboardTypeAlphabet | 指定针对字母输入优化的键盘 | ![UIKeyboardTypeAlphabet](media/15332026405298/UIKeyboardTypeAlphabet.png) |

<p style="text-align:center;color:#999999;font-size:14px"> UIKeyboardType 枚举值 </p>    

#### 键盘外观

UIKeyboardAppearance 枚举值对应的键盘外观如下表：

| UIKeyboardAppearance 枚举值  | 说明 |
| :-: | :-: | 
| UIKeyboardAppearanceDefault | 输入法的默认键盘外观<br>等于 UIKeyboardAppearanceLight |
| UIKeyboardAppearanceLight | 适合浅色 UI 外观的键盘外观，键盘是白色外观 | 
| UIKeyboardAppearanceDark | 适合深色 UI 外观的键盘外观，键盘时深灰色外观 | 

<p style="text-align:center;color:#999999;font-size:14px"> UIKeyboardAppearance 枚举值 </p> 

#### 键盘返回键

UIReturnKeyType 枚举值对应键盘的返回键显示效果如下表：

| UIReturnKeyType 枚举值 | 中文状态下 | 英文状态下 |
| :-: | :-: | :-: |
| UIReturnKeyDefault | ![UIReturnKeyDefault_中](media/15380541769828/UIReturnKeyDefault_%E4%B8%AD.png) | ![UIReturnKeyDefault_英](media/15380541769828/UIReturnKeyDefault_%E8%8B%B1.png) |
| UIReturnKeyGo | ![UIReturnKeyGo_中](media/15380541769828/UIReturnKeyGo_%E4%B8%AD.png) | ![UIReturnKeyGo_英](media/15380541769828/UIReturnKeyGo_%E8%8B%B1.png) |
| UIReturnKeyJoin | ![UIReturnKeyJoin_中](media/15380541769828/UIReturnKeyJoin_%E4%B8%AD.png) | ![UIReturnKeyJoin_英](media/15380541769828/UIReturnKeyJoin_%E8%8B%B1.png) |
| UIReturnKeyNext | ![UIReturnKeyNext_中](media/15380541769828/UIReturnKeyNext_%E4%B8%AD.png) | ![UIReturnKeyNext_英](media/15380541769828/UIReturnKeyNext_%E8%8B%B1.png) |
| UIReturnKeyRoute | ![UIReturnKeyRoute_中](media/15380541769828/UIReturnKeyRoute_%E4%B8%AD.png) | ![UIReturnKeyRoute_英](media/15380541769828/UIReturnKeyRoute_%E8%8B%B1.png) |
| UIReturnKeySearch | ![UIReturnKeySearch_中](media/15380541769828/UIReturnKeySearch_%E4%B8%AD.png) | ![UIReturnKeySearch_英](media/15380541769828/UIReturnKeySearch_%E8%8B%B1.png) |
| UIReturnKeySend | ![UIReturnKeySend_中](media/15380541769828/UIReturnKeySend_%E4%B8%AD.png) | ![UIReturnKeySend_英](media/15380541769828/UIReturnKeySend_%E8%8B%B1.png) |
| UIReturnKeyDone | ![UIReturnKeyDone_中](media/15380541769828/UIReturnKeyDone_%E4%B8%AD.png) | ![UIReturnKeyDone_英](media/15380541769828/UIReturnKeyDone_%E8%8B%B1.png) |
| UIReturnKeyContinue<br> （仅支持 iOS 9 以上） | ![UIReturnKeyContinue_中](media/15380541769828/UIReturnKeyContinue_%E4%B8%AD.png) | ![UIReturnKeyContinue_英](media/15380541769828/UIReturnKeyContinue_%E8%8B%B1.png) |

<p style="text-align:center;color:#999999;font-size:14px"> UIReturnKeyType 枚举值 </p> 

### UITextFieldDelegate 协议（⚠️ 表示方法没有整理完）

包含一组可选方法，用于管理 UITextField 对象中文本的编辑和验证等，这些都是可选方法。

- **textFieldShouldBeginEditing: 方法**
    
    询问代理是否应在指定的 UITextField 中开始编辑。
    
    - 第一个参数：UITextField 类型，指定的 UITextField。
    - 返回值：BOOL 类型，YES 表示允许编辑，NO 表示不允许编辑（不允许编辑时不会有任何交互效果）。

    当用户点击 UITextField，UITextField 将成为第一响应者。在马上成为第一响应者之前，UITextField 调用这个方法。使用该方法允许或不允许编辑 UITextField 的内容。
    
    如果代理未实现此方法，则 UITextField 的行为就像此方法返回 YES 一样。
    
- **textFieldDidBeginEditing: 方法**

    告诉代理在指定的 UITextField 中开始编辑。
    
    - 第一个参数：UITextField 类型，指定的 UITextField。

    此方法通知代理指定的 UITextField 刚刚成为第一响应者。使用此方法更新状态信息或执行其他任务。例如，可以使用此方法显示仅在编辑时可见的叠加视图。
    
- **textFieldShouldEndEditing: 方法**

    询问代理是否应在指定的 UITextField 中停止编辑。
    
    - 第一个参数：UITextField 类型，指定的 UITextField。
    - 返回值：BOOL 类型，YES 表示停止编辑，NO 表示继续编辑。

    ⚠️
    
- **textFieldDidEndEditing:reason: 方法**

    告诉代理指定的 UITextField 已停止编辑。
    
    - 第一个参数：UITextField 类型，指定的 UITextField。
    - 第二个参数：UITextFieldDidEndEditingReason 类型，编辑结束的原因。

        使用此字段可确定是合并文本编辑更改还是放弃它们。
        
    ⚠️
    
- **textFieldDidEndEditing: 方法**

    告诉代理指定的 UITextField 已停止编辑。
    
    - 第一个参数：UITextField 类型，指定的 UITextField。

    ⚠️
    
- **textFieldShouldReturn: 方法**

    询问代理是否应处理指定 UITextField 的返回按钮按下的事件。
    
    - 第一个参数：UITextField 类型，指定的 UITextField。
    - 返回值：BOOL 类型，返回 YES 和 NO 无影响（⚠️没发现不同）。

    这个方法的返回值是 YES 或者是 NO，当用户点击返回按钮时，都会调用这个代理方法，并且会执行方法中的语句。可以用这个方法来关闭键盘等。
    
- **textField:shouldChangeCharactersInRange:replacementString: 方法**

    询问代理是否应该更改指定的文本。当用户操作使 UITextField 中的文本发生更改时，代理将调用此方法。
    
    > <i class="fas fa-exclamation-circle"></i> 要注意，这个方法是在用户按下键盘时，但是内容还没替换到输入框中的时候调用，这个方法执行完之后，再根据方法的返回值确定是否把用户的输入替换到输入框中。所以最好不要使用这个方法来判断与输入框中字符长度有关的问题。判断与输入框内容长度有关的问题，使用输入框的 `UIControlEventEditingChanged` 状态来监听输入框字符变化。
    
    最开始时，输入框中的内容为空（或者为占位字符），当用户点击输入框，输入第一个字符到输入框时，会调用这个方法，输入第二个字符时会调用这个方法......当用户删除一个字符时，也会调用这个方法。也就是说用户的操作在输入框中已存在的文本的基础上，只要发生变化就会调用这个方法。
    
    - 第一个参数：UITextField 类型，指定的 UITextField。
    - 第二个参数：NSRange 类型，要更改的字符的范围。

        NSRange 类型是个结构体，有两个值：length 和 location。分别要被替换的字符的长度和位置。   
        
    - 第三个参数：NSString 类型，指定范围的替换字符串。

        在输入过程中，该参数通常只包含输入的单个新字符，但如果用户粘贴文本，则可能包含更多字符。当用户删除一个或多个字符时，替换字符串为空。
        
    - 返回值：BOOL 类型，YES 表示指定的文本范围应该被替换；NO 表示不替换，保留旧文本。

    为了好理解第二个参数和第三个参数的作用，这里举几个例子：
        
    - 假如现在输入框中的字符为「abcde」，现在用户输入「f」，调用此方法，那么方法的第二个参数的 length 和 location 两个值分别为 0 和 5：表示从第 5 个位置开始（因为字符「e」的位置是 4），替换的字符长度是 0（此时没有字符要被替换，所以长度为 0）。而此时的第三个参数的值就是：f。
    - 假如现在输入框的字符为「abcde」，现在选中「cde」，然后输入「f」，调用此方法，那么方法的第二个参数的 length 和 location 两个值分别为 3 和 2：表示从第 2 个位置开始（因为字符「c」的位置是 2），替换的字符长度是 3（因为字符「cde」长度为 3）。这样把「cde」就替换成了「f」。并且此时的第三个参数的值是：f。
    - 假如现在输入框的字符为「abcde」，现在选中「cde」，复制，然后选中「bcde」，粘贴。调用此方法，那么方法的第二个参数的 length 和 location 两个值分别为 4 和 1：表示从第 1 个位置开始，替换的字符长度是 4。这样把「bcde」就替换成了「cde」。而此时的第三个参数的值就是：cde。
    
    综合上面的例子能看出，其实方法第二个参数的值的是输入框中要改变的字符的范围，而第三个参数就是要添加到输入框中的内容（当删除时，这个字符串为空字符串）。
    
    要特别注意的是，当用户通过中文拼音输入的时候，比如输入「哈」这个字，首先按下「h」，这时「h」这个字母是出现在输入框中的，所以也会调用此方法，然后按下「a」，同样再次调用这个方法，最后用户在键盘上选择「哈」这个字，这时输入框中就只有「哈」这个字符，这时同样会调用此方法。此过程的参数变化如下图：
    
    ![参数变化](media/15380541769828/%E5%8F%82%E6%95%B0%E5%8F%98%E5%8C%96.png)

    这就是为什么在一个限制长度的输入框中，使用拼音输入最后几个字符时，当拼音长度超过了输入框限制的最大长度，就会无法完成拼音输入的原因。
    
    > <i class="fas fa-exclamation-circle"></i> 但是要注意，当使用内置清除按钮清空输入框的内容时，并不会调用这个代理方法。

### UITextField 实用需求

#### 监控输入框内容变化

实际开发中常用到通过监控输入框中的内容来触发某些操作。例如登录界面，通过监控输入框中的内容格式是否正确来确定登录按钮的可用状态。

如果需要监控输入框中的内容，最好通过监听 UITextField 的 `UIControlEventEditingChanged` 事件来达到相应的需求。尽量不要使用 UITextField 的代理方法 `textField:shouldChangeCharactersInRange:replacementString:` ，因为这个代理方法是用来判断 UITextField 能否改变内容的，而前者是通过监听内容是否改变。


    
    
    
    
    
    

<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">