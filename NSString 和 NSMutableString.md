# NSString 和 NSMutableString

## NSString



## NSMutableString

NSMutableString 继承自 NSString，它是动态纯文本 Unicode[^Unicode] 字符串对象，也就是可变的字符串对象。NSMutableString 类声明了一个对象的编程接口，该对象管理一个可变字符串——即一个可以编辑其内容的字符串（在概念上表示一个 Unicode 字符数组）。

[^Unicode]: [Unicode](https://baike.baidu.com/item/Unicode) 包含了世界上所有文字的编码，甚至是一些符号表情，如😊，😂等。这些符号实际上也是 Unicode 编码，而不是图片。

NSMutableString 类将一个基本方法 `replaceCharactersInRange:withString:` 添加到从 NSString 继承的基本字符串处理行为。修改字符串的所有其他方法都可以通过此方法工作。例如，`insertString:atIndex:` 只是替换 `0` 长度范围内的字符，而 `delet/eCharactersInRange:` 替换给定范围内没有字符的字符。

### NSMutableString 属性和方法

#### 修改字符串

- **appendFormat:** 方法

    将字符串添加到接收者。
    
    - 参数一：NSStirng 类型，格式字符串，该参数不能为 `nil`，否则引发异常。

- **appendString:** 方法

    在接收者的末尾添加指定的字符串。
    
    - 参数一：NSString 类型，要添加的字符串，不能为 `nil`。
    
- **deleteCharactersInRange:** 方法

    从接收者中删除指定范围内的字符。
    
    - 参数一：NSRange 类型，要删除的字符范围。不能超过接收者的字符串范围。

- **insertString:atIndex:** 方法

    在接收者的指定位置插入给定的字符串。

    - 参数一：NSSting 类型，要添加的字符串，不能为 `nil`。
    - 参数二：NSUInteger 类型，插入的位置。

- **replaceCharactersInRange:withString:** 方法

    将指定范围内的字符串替换为给定的字符串。
    
    - 参数一：NSRange 类型，要替换的范围。
    - 参数二：NSString 类型，替换的字符串。

- **replaceOccurrencesOfString:withString:options:range:** 方法

    用给定的另一个字符串替换指定范围内的所有相同的字符串，返回替换次数。
    
    - 参数一：NSString 类型，原来的字符串。
    - 参数二：NSString 类型，替换的字符串。
    - 参数三：NSStringCompareOptions 类型，指定搜索选项的掩码。
    - 参数四：NSRange 类型，要替换的字符串的范围。
    - 返回值：NSUInteger 类型，替换的次数。

- **setString:** 方法

    用给定字符串替换接收者的字符串。
    
    - 参数一：NSString 类型，给定的字符串，不能为 `nil`。
    

    