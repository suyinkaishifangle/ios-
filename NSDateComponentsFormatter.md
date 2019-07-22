# NSDateComponentsFormatter

NSDateComponentsFormatter 继承自 NSFormatter，称为「日期组件格式器」，用于完成 NSDateComponents 和 NSString 之间的转换。

NSDateComponentsFormatter 对象占用大量时间并将它们格式化为用户可读的字符串。使用日期组件格式器为应用程序界面创建字符串，并且格式器在生成字符串时会考虑当前用户的语言环境和语言。

要使用此类，需要创建实例，配置其属性，并调用其中一个方法以生成适当的字符串。通过此类的属性，可以配置日历并指定要在结果字符串中显示的日期和时间单位。 清单1显示了如何配置格式化程序以创建字符串“剩余约5分钟”。

可以从应用程序的任何线程安全地调用此类的方法。从多个线程共享此类的单个实例也是安全的，不过需要注意的是，当另一个线程使用它来生成字符串时，不应更改对象的配置。

## NSDateComponentsFormatter 属性和方法

### 配置格式化选项

- **calendar** 属性

    NSCalendar 类型，格式化日期组件时使用的默认日历。
    
    格式器在格式化没有固有日历的值时使用此属性中的日历。例如，格式器在格式化 NSTimeInterval 值时使用此日历。

    此属性的默认值是 NSCalendar 的 `autoupdatingCurrentCalendar` 方法返回的日历。将此属性设置为 `nil` 会使格式器使用带有 en_US_POSIX 语言环境的公历。

- **allowedUnits** 属性

    NSCalendarUnit 类型，日历单位的位掩码，如日期和月份，包含在输出字符串中。
    
    此属性允许的日历单位是一下部分，如果将任何其他日历单位分配给此属性会导致异常。

    - NSCalendarUnitYear
    - NSCalendarUnitMonth
    - NSCalendarUnitWeekOfMonth
    - NSCalendarUnitDay
    - NSCalendarUnitHour
    - NSCalendarUnitMinute
    - NSCalendarUnitSecond

- **allowsFractionalUnits** 属性

    BOOL 类型，指示是否可以将非整数单位用于值。默认值为 `NO`。
    
    当使用可用单位无法准确表示值时，可以使用小数单位。例如，值 `1h 30m` 可以格式化为 `1.5h`。
    
- **collapsesLargestUnit** 属性

    BOOL 类型，指示在满足特定阈值时是否将最大单元折叠为较小单元。默认值为 `NO`。
    
    此属性适用使用的时机例如表达 63 秒这个时间。当此属性设置为 `YES` 时，格式化后的值将为 `63s`；当此属性设置为 `NO` 时，格式化后的值将为 `1m 3s`。
    
- **includesApproximationPhrase** 属性

    BOOL 类型，指示结果短语是否反映了不精确的时间值。默认值为 `NO`。
    
    将此属性的值设置为 `YES` 会在输出字符串中添加措辞，以反映给定时间值是近似值而不是精确值。使用此属性会产生更准确的措辞，而不是简单地将字符串「About」添加到输出字符串。

- **includesTimeRemainingPhrase** 属性

    BOOL 类型，指示输出字符串是否反映剩余的时间量。默认值为 `NO`。
    
    将此属性设置为 `YES` 会使输出字符串如「剩余 30 分钟」。
    
- **maximumUnitCount** 属性

    NSInteger 类型，要包含在输出字符串中的最大时间单位数。默认值为 `0`，这不会消除任何单位。
    
    使用此属性可限制结果字符串中显示的单位数。例如，将此属性设置为 2，而不是 `1h 10m 30s`，结果字符串将为 `1h 10m`。当受限于空间或想要将值向上舍入到最近的大单位时，使用此属性。
    
- **unitsStyle** 属性

    NSDateComponentsFormatterUnitsStyle 类型，单位名称的格式样式。此属性的默认值为 `NSDateComponentsFormatterUnitsStylePositional`。
    
    为单位名称（如天，小时，分钟和秒）配置要使用的字符串（如果有）。使用此属性指定是否需要缩写或缩短版本的单元名称 - 例如，`hrs` 而不是 `hours`。
    
    根据当前用户的语言首选项对所有日期和时间值进行本地化和格式化。
    
    NSDateComponentsFormatterUnitsStyle 枚举类型用于指定如何表示时间量的常量，的具体值如下表，下表显示了每种样式在美国英语区域设置中显示的 9 小时，41 分钟和 30 秒。
    
    | NSDateComponentsFormatterUnitsStyle 枚举值 | 说明 |
    | :-: | :-: |
    | NSDateComponentsFormatterUnitsStyleSpellOut ||
    | NSDateComponentsFormatterUnitsStyleFull ||
    | NSDateComponentsFormatterUnitsStyleShort ||
    | NSDateComponentsFormatterUnitsStyleBrief ||
    | NSDateComponentsFormatterUnitsStyleAbbreviated ||
    | NSDateComponentsFormatterUnitsStylePositional ||
    
- **zeroFormattingBehavior** 属性

    NSDateComponentsFormatterZeroFormattingBehavior 类型，值为0的单位的格式样式。

### 格式化值

- **stringFromDateComponents:** 方法

    根据指定的日期组件信息返回格式化的字符串。
    
    - 参数一：NSDateComponents 类型，日期组件对象。此参数不能为 `nil`。
    - 返回值：NSString 类型，格式化字符串。

    `allowedUnits` 属性确定实际用于生成字符串的日期组件。所有其他日期组件都将被忽略。使用此方法格式化已分解为日期和时间组件值的日期信息。
    
- **stringForObjectValue:** 方法

    根据指定对象中的日期信息返回格式化字符串。
    
    - 参数一：id 类型，包含要格式化的日期和时间信息的对象。此参数中的对象必须是 NSDateComponents 对象，如果不是，则该方法引发异常。此参数不能为 `nil`。
    - 返回值：NSString 类型，格式化字符串。

    此方法与 `stringFromDateComponents:` 方法具有相同的行为。
    
- **stringFromDate:toDate:** 方法

    根据两个日期之间的时差返回格式化字符串。
    
    - 参数一：NSDate 类型，开始日期。此参数不得为 `nil`。
    - 参数二：NSDate 类型，结束日期。此参数不得为 `nil`。
    - 返回值：NSString 类型，格式化字符串。

    此方法计算开始日期和结束日期值之间的经过时间，并使用该信息生成字符串。例如，如果开始日期和结束日期之间只有一小时十分钟的差异，则生成缩写字符串为 `1h 10m`。

- **stringFromTimeInterval:** 方法

    根据指定的秒数返回格式化字符串。
    
    - 参数一：NSTimeInterval 类型，时间间隔，以秒为单位。创建字符串时，负数被视为正数。
    - 返回值：NSString 类型，格式化字符串。

    此方法将指定的秒数格式化为适当的单位。例如，如果格式化器允许显示分钟和秒，则为值 70 秒创建缩写字符串会产生字符串 `1m 10s`。
    
- **localizedStringFromDateComponents:unitsStyle:** 类方法

    根据指定的日期组件和样式选项返回本地化字符串。
    
    - 参数一：NSDateComponents 类型，日期组件对象。
    - 参数二：NSDateComponentsFormatterUnitsStyle 类型，结果单位的风格。
    





