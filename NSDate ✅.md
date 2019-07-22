# NSDate ✅

Cocoa 将日期和时间表示为 NSDate 对象。NSDate 是基本的 Cocoa 值对象之一。

NSDate 继承自 NSObject，称之为「日期」对象，NSDate 对象表示不变的时间点，独立于任何特定的日历系统或时区。NSDate 对象是不可变的。日期对象的标准时间单位是浮点值，NSTimeInterval 以秒为单位表示。这种类型可以实现广泛和细粒度的日期和时间值范围，相隔 10,000 年的日期精确到毫秒。

NSDate 计算相对于「绝对参考时间」的秒数：2001 年 1 月 1 日 00:00:00，格林威治标准时间（GMT）。之前的日期存储为负数，之后的日期存储为正数。NSDate 的 `timeIntervalSinceReferenceDate ` 方法为 NSDate 接口中的所有其他方法提供了基础。NSDate 将所有日期和时间表示转换为相对于「绝对参考时间」的 NSTimeInterval 值。

NSDate 类提供了用于比较日期、计算两个日期之间的时间间隔以及从相对于另一个日期的时间间隔创建新日期的方法。要获取日期的组成元素（例如星期几），需要将 NSDate 将 NSDateComponents 对象与 NSCalendar 对象结合使用。

## NSDate 属性和方法

### 创建和初始化日期对象

> 🔥 **提示**
> 
> 以下创建日期对象的类方法都有与之对应的实例方法。与 `date` 类方法对应的实例方法是 `init`。

- **date** 类方法

    创建并返回设置为当前日期和时间的新日期对象。
    
- **dateWithTimeIntervalSinceNow:** 类方法

    创建并返回一个距离当前日期和时间指定秒数间隔的新日期对象。
    
    - 参数一：NSTimeInterval 类型，从当前日期到新日期的时间的秒数。使用负值则指定当前日期之前的日期。

- **dateWithTimeInterval:sinceDate:** 类方法

    创建并返回一个距离指定日期对象的指定秒数间隔的新日期对象。
    
    - 参数一：NSTimeInterval 类型，指定秒数。使用负值则指定参数二日期对象之前的日期。
    - 参数二：NSDate 类型，指定日期对象。

- **dateWithTimeIntervalSinceReferenceDate:** 类方法

    创建并返回距离 2001 年 1 月 1 日 00:00:00 UTC 指定秒数间隔的新日期对象。
    
    - 参数一：NSTimeInterval 类型，指定秒数。使用负值则指定参数二日期对象之前的日期。

- **dateWithTimeIntervalSince1970:** 类方法

    创建并返回距离 1970 年 1 月 1 日 00:00:00 UTC 指定秒数间隔的新日期对象（将时间戳转换为 NSDate）。
    
    - 参数一：NSTimeInterval 类型，指定秒数。使用负值则指定参数二日期对象之前的日期。

### 获取时间边界

- **distantFuture** 类属性

    NSDate 类型，只读。表示遥远未来日期的日期对象。
    
    可以使用 distantFuture 返回的对象作为日期参数，以无限期地等待事件发生。
    
- **distantPast** 类属性

     NSDate 类型，只读。表示遥远过去日期的日期对象。
     
     可以将此对象用作控制日期，保证时间边界。
     
### 比较日期

要比较日期，可以使用 `isEqualToDate:`、`compare:`、`laterDate:` 和 `earlierDate:` 方法。这些方法执行精确比较，这意味着它们检测日期之间的亚秒差异。如果希望以较小的粒度比较日期。例如，如果它们彼此相距一分钟，可能需要考虑两个相等的日期。如果是这种情况，请使用 `timeIntervalSinceDate:` 比较两个日期。

- **isEqualToDate:** 方法

    返回一个布尔值，指示给定对象的日期是否与接收者的日期完全相等。
    
    - 参数一：NSDate 类型，与接收者比较的日期对象。
    - 返回值：BOOL 类型，如果两个日期对象完全相等，则为 `YES`，否则为 `NO`。

    此方法检测日期之间的亚秒级（sub-second）差异。如果要比较细粒度较小的日期，使用`timeIntervalSinceDate:` 方法来比较两个日期。
    
- **earlierDate:** 方法

    返回接收者和另一个给定的日期对象两者中日期较早的一个。
    
    - 参数一：NSDate 类型，给定的日期对象。
    - 返回值：NSDate 类型，两者中日期较早的对象。

    使用 `timeIntervalSinceDate` 确定接收者和参数一中较早的日期对象。如果接收者和参数一代表相同的日期，则返回接收者。
    
- **laterDate:** 方法

    返回接收者和另一个给定的日期对象两者中日期较晚的一个。
    
    - 参数一：NSDate 类型，给定的日期对象。
    - 返回值：NSDate 类型，两者中日期较晚的对象。

    使用 `timeIntervalSinceDate` 确定接收者和参数一中较晚的日期对象。如果接收者和参数一代表相同的日期，则返回接收者。
    
- **compare:** 方法

    返回接收者和另一个给定日期对象之间的时间顺序。
    
    - 参数一：NSDate 类型，给定日期对象。不能为 `nil`。
    - 返回值：[NSComparisonResult](https://developer.apple.com/documentation/foundation/nscomparisonresult?language=objc) 类型，表示排序顺序的常量。

    此方法检测日期之间的亚秒级差异。如果要比较细粒度较小的日期，使用`timeIntervalSinceDate:` 方法来比较两个日期。
    
### 获取时间间隔

- **timeIntervalSinceDate:** 方法

    返回接收者与另一个给定日期之间的间隔。
    
    - 参数一：NSDate 类型，给定日期对象。
    - 返回值：NSTimeInterval 类型，接收者和参数一之间的间隔，单位秒。如果接收者早于参数一的日期，则返回值为负。如果参数一为 `nil`，则结果未定义。

- **timeIntervalSinceNow** 属性

    NSTimeInterval 类型，只读。日期对象与当前日期和时间之间的间隔。
    
    如果日期对象早于当前日期和时间，则此属性的值为负。
    
- **timeIntervalSinceReferenceDate** 属性

    NSTimeInterval 类型，只读。日期对象与 2001 年 1 月 1 日 00:00:00 UTC 之间的间隔。

    如果日期对象早于系统的绝对参考日期（2001年1月1日00:00:00 UTC），则此属性的值为负。
    
- **timeIntervalSince1970** 属性

    NSTimeInterval 类型，只读。日期对象与 1970 年 1 月 1 日 00:00:00 UTC 之间的间隔（即时间戳）。
    
    如果日期对象早于 1970 年 1 月 1 日 00:00:00 UTC，则此属性的值为负。
    
- **NSTimeIntervalSince1970** 宏

    从 1970 年 1 月 1 日至参考日期 2001 年 1 月 1 日的秒数。
    
    1970 年 1 月 1 日是 Unix 的时代（或起点）。
    
### 添加时间间隔
    
- **dateByAddingTimeInterval:** 方法

    返回一个新的日期对象，该对象设置为相对于接收者的给定秒数。
    
    - 参数一：NSTimeInterval 类型，给定秒数。
    - 返回值：NSDate 类型，设置为相对于接收者的秒数。

### 描述日期

- **description** 方法

    NSString 类型，只读。日期对象的字符串表示形式。
    
    该表示仅对调试有用。
    
### 通知发布

- **NSSystemClockDidChangeNotification** 通知

    系统时钟更改时发布通知。
    
    通知对象为 `null`。 此通知不包含 userInfo 字典。
    
