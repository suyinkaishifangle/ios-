# NSDateFormatter ✅
    
NSDateFormatter 继承自 NSFormatter，称为「日期格式器」，用于完成 NSDate 和 NSString 之间的转换。
    
使用 NSDateFormatter 完成 NSDate 和 NSString 之间的转换步骤如下：

1. 创建 NSDateFormatter 对象（使用 alloc/init 方法）。
2. 设置要转换的格式字符串：
    - 方式一：使用 `dateStyle` 和 `timeStyle` 属性设置格式化日期、时间的样式。
    - 方式二：使用 `dateFormat` 属性自定义格式字符串的样式。
3. NSDate 与 NSString 相互转换：
    - 如果将 NSDate 转化为 NSString，调用 NSDateFormatter 的 `stringFormDate:` 方法执行格式化。
    - 如果将 NSString 转化为 NSDate，调用 NSDateFormatter 的 `dateFormString:` 方法执行格式化。

> ⚠️ **注意**
> 
> 如果在处理 ISO8601 格式的日期表示时，使用 NSISO8601DateFormatter 类，NSDateFormatter 用于处理 RFC 3339 格式日期，目前常用后者表示时间格式。

## NSDateFormatter 属性和方法

### 管理显示样式

- **dateStyle** 属性

    NSDateFormatterStyle 类型，日期样式。默认值为 `NSDateFormatterNoStyle`。

- **timeStyle** 属性 

    NSDateFormatterStyle 类型，时间样式。默认值为 `NSDateFormatterNoStyle`。
    
- **NSDateFormatterStyle** 枚举类型

    用于指定日期和时间的预定义格式的不同样式。
    
    | NSDateFormatterStyle 枚举值 | 说明 | 日期样式示例（括号中是农历） | 时间样式示例 |
    | :-: | :-: | :-: | :-: |
    | NSDateFormatterNoStyle | 没有样式 | 不显示任何日期 | 不显示任何时间 |
    | NSDateFormatterShortStyle | 短样式 | 19/4/15<br>（2019-4-15） | 3:30 PM |
    | NSDateFormatterMediumStyle | 中等样式 | 2019年5月19日<br>（2019年四月十五）| 3:30:32 PM |
    | NSDateFormatterLongStyle | 长样式 | 2019年5月19日<br>（2019己亥年四月十五）| 太平洋标准时间下午3:30:32 |
    | NSDateFormatterFullStyle | 完整样式 | 2019年5月19日星期日、<br>（2019己亥年四月十五星期日）| 太平洋标准时间下午3:30:42|
    
    > ⚠️ **注意**
    >
    > 这些枚举值的样式不固定，因为它们取决于用户区域设置，用户首选项设置和操作系统版本。如果需要自定义显示的样式，使用 `dateFormat` 属性来自定义格式字符串的样式。 
    
- **dateFormat** 属性 

    NSString 类型，日期格式字符串模版。
    
    日期格式字符串模版在格式化时用日历中的日期和时间数据替换，或者用于在解析时生成日历的数据。
    
    > 🔥 **提示**
    >
    > 如果同时使用此属性和 `dateStyle` 及 `timeStyle` 属性，那么相当于此属性无效。
    
    常用的格式字符串所代表的含义如下表，也可以通过 [nsdateformatter.com](https://nsdateformatter.com/) 来查询。
    
    | 格式 | 说明 | 中文环境示例 | 英文环境示例 |
    | :-: | :-: | :-: | :-: |
    | G | 时代 | 公元 | AD |
    | yy | 年份的后两位 | 19 | 19 |
    | yyyy | 完整的年份 | 2019 | 2019 |
    | M | 月份 | 5、10 | 5、10 |
    | MM | 月份 | 05、10 | 05、10 |
    | MMM | 月份（简写） | 5月、10月 | May、Oct |
    | MMMM | 月份（全写） | 五月、十二月 | May、October |
    | d | 日 | 4、18 | 4、18 |
    | dd | 日 | 04、18 | 04、18 |
    | EEE | 星期 | 周六 | Sat |
    | EEEE | 星期 | 星期六 | Saturday |
    | aa | 上下午 | 上午、下午 | AM、PM |
    | H | 小时（24小时值）| 0、19 | 0、19 |
    | K | 小时（12小时值）| 0、7 | 0、7 |
    | m | 分钟 | 0、21 | 0、21 |
    | mm | 分钟 | 00、21 | 00、21 |
    | s | 秒钟 | 0、34 | 0、34  |
    | ss | 秒钟 | 00、34 | 0、34  |
    | S | 毫秒 | 7、15 | 7、15 |
    | Z | 格林威治时间（GMT）| +0800 | +0800 |

- **formattingContext** 属性  

    NSFormattingContext 类型，格式化日期时使用的大小写格式上下文。
    
    此属性允许日期格式器应用适当的大小写，具体取决于如何使用字符串，以及地区是否区分大小写。
    
    NSFormattingContext 是枚举类型，具体如下表：
    
    | NSFormattingContext 枚举值 | 说明 |
    | :-: | :-: |
    | NSFormattingContextUnknown | 未知的格式化上下文，默认值。|
    | NSFormattingContextDynamic | 运行时自动确定格式化上下文。|
    | NSFormattingContextStandalone | 独立使用的格式化上下文。|
    | NSFormattingContextListItem | 列表或菜单项的格式上下文。|
    | NSFormattingContextBeginningOfSentence | 句子开头的格式化上下文。|
    | NSFormattingContextMiddleOfSentence | 句子中间的格式化上下文。|

- **setLocalizedDateFormatFromTemplate:** 方法

    使用指定的区域设置从给定的日期格式模版设置日期格式。
    
    - 参数一：NSString 类型，包含日期格式模版的字符串（例如「MM」或「h」）。

    调用此方法相当于（但不一定实现为）将 `dateFormatFromTemplate:options:locale:method` 方法的返回值设置给 `dateFormat` 属性，并且不传递任何选项和 `locale` 属性值。
    
    只有在设置 NSDateFormatter 的语言环境后才应调用此方法。
    
- **dateFormatFromTemplate:options:locale:** 类方法

    返回一个本地化的日期格式字符串，表示为指定的语言环境适当排列的给定日期格式组件。
    
    - 参数一：NSString 类型，包含日期格式模版的字符串（例如「MM」或「h」）。
    - 参数二：NSUInteger 类型，目前没有定义任何选项。
    - 参数三：NSLocale 类型，语言环境。
    - 返回值：NSString 类型，日期格式字符串。

### 转换对象

- **dateFromString:** 方法

    将 NSSting 对象解析为 NSDate 对象。
    
    - 参数一：NSString 类型，要解析的字符串。
    - 返回值：NSDate 类型，解析后的日期对象，如果无法解析参数一的字符串，则返回 `nil`。

- **stringFromDate:** 方法

    将 NSDate 对象转换为 NSString 对象。   
    
    - 参数一：NSDate 类型，要转换的日期。
    - 返回值：NSString 类型，格式化后的日期字符串。

- **localizedStringFromDate:dateStyle:timeStyle:** 类方法

    使用指定的日期样式和时间样式以及当前语言环境来返回给定日期的字符串表示形式。
    
    - 参数一：NSDate 类型，给定的日期。
    - 参数二：NSDateFormatterStyle，日期样式。
    - 参数三：NSDateFormatterStyle，时间样式。
    - 返回值：NSString 类型，格式化后日期字符串。

### 管理属性

- **calendar** 属性

    NSCalendar 类型，日历。默认使用当前用户设置的日历（iOS 中即 ⚙️ **设置 ➤ 通用 ➤ 语言与地区 ➤ 日历** 选项所指示的日历类型）。
    
    如果想要日期格式器转换相关的日期是其他的日历历法，则将日历对象赋值给此属性。 
      
- **defaultDate** 属性

    NSDate 类型，默认日期。
    
    默认情况下，此属性为 `nil`。
    
- **locale** 属性

    NSLocale 类型，语言环境。
    
    如果未指定，则使用系统的语言环境。
    
- **timeZone** 属性

    NSTimeZone 类型，时区。
    
    如果未指定，则使用系统时区。
    
- **gregorianStartDate** 属性

    NSDate 类型，公历的开始日期。
    
### 管理自然语言支持

- **lenient** 属性

    BOOL 类型，指示日期格式器在解析字符串时是否使用启发式方法。
    
    如果日期格式器已设置为在解析字符串时使用启发式以预测日期，则为 `YES`，否则为 `NO`。如果此属性为 `YES`，它可能会导致结果日期错误。
    
- **doesRelativeDateFormatting** 属性

    BOOL 类型，指示日期格式器是否对日期组件使用诸如「今天」和「明天」之类的短语。
    
    如果此属性为 `YES`，则在可能的情况下，它会使用短语（例如「今天」或「明天」）替换其输出的日期字符串。可用的短语取决于日期格式化程序的区域设置。然而，对于将来的日期，英语可能只允许「明天」，而法语却可能允许「后天的后天」。

