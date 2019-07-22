# NSDateComponents ✅

NSDateComponents 继承自 NSObject，称之为「日期组件」，表示一个日期信息的容器（信息包括：年，月，日，月中的某天，年中的某周，或者是否是闰月等等)。NSDateComponents 以可扩展的面向对象方式封装了日期的组件。通过 NSDateComponents 可以快速而简单地获取某个时间点对应的「年、月、日、时、分、秒」等信息。

NSDateComponents 对象本身没有意义，需要知道它是根据什么日历解释的，因此 NSDateComponents 必须和 NSCalendar 一起使用，默认为公历。

日、周、工作日、月和年等的序数通常是从 `1` 开始，而不是从 `0` 开始，不过可能存在某些类型的日历有例外情况。有些日历必须将它们的基本单位概念映射到年/月/周/日/……命名法中。该单位的特定值由每个日历定义，不一定与另一个日历中该单位的值一致。

还可以使用 NSDateComponents 来表示时间段，例如，5个小时16分钟、两周、三个月等。不需要 NSDateComponents 对象来定义所有组件字段。时间段用于日历的计算，例如：获取在公历历法下，现在这个时间点的三个月前的日期和时间。

NSDateComponents 对象还可以用来计算相对日期。使用 NSCalendar 的 `dateByAddingComponents:toDate:options:` 方法来确定昨天，下周或者 5 小时 30 分钟之后的日期。

```Objective-C
NSCalendar *calendar = [NSCalendar currentCalendar];
NSDate *date = [NSDate date];

NSDateComponents *components = [[NSDateComponents alloc] init];
[components setWeek:1];
[components setHour:12];

NSLog(@"1 week and twelve hours from now: %@", [calendar dateByAddingComponents:components toDate:date options:0]);
```

> 🔥 **提示**
> 
> NSDateComponents 类能够被手动初始化（使用 alloc/init 方法），但是在大多数时候，会使用 NSCalendar 类的 `components:fromDate:` 方法来获取某个日期的日期组件对象。

## 用 NSDateComponents 来创建日期

NSDateComponents 类最强大的特性就是能够通过组件反向创建 NSDate 对象。NSCalendar 类的 `dateFromComponents:` 方法就是用来实现这个目的。

```Objective-C
NSCalendar *calendar = [NSCalendar currentCalendar];

NSDateComponents *components = [[NSDateComponents alloc] init];
[components setYear:1987];
[components setMonth:3];
[components setDay:17];
[components setHour:14];
[components setMinute:20];
[components setSecond:0];

NSLog(@"date: %@", [calendar dateFromComponents:components]);
```

这个方法除了正常的月/日/年方式之外，也可以用某些信息来确定一个日期。只要提供的信息能够唯一确定一个日期，就会得到一个结果。例如：指定 2013 年的第 316 天，那么就会返回一个 2013 年 12 月 11 日 0 点 0 分 0 秒的 NSDate 对象。

## NSDateComponents 属性和方法

### 验证日期

- **validDate** 属性

    BOOL 类型，只读。指示当前的属性组合是否能够表示当前日历中存在的日期。
    
- **isValidDateInCalendar:** 方法

    返回一个布尔值，指示当前属性组合是否能够表示指定日历中存在的日期。
    
    - 参数一：NSCalendar 类型，要在计算中使用的日历。
    - 返回值：BOOL 类型，如果日期有效且存在于给定日历中，则为 `YES`，否则为 `NO`。

- **date** 属性

    NSDate 类型，只读。使用存储的日历从当前组件计算的日期。
    
    如果接收者的日历属性值为 `nil` 或无法将接收者转换为 NSDate 对象，则返回 `nil`。

### 设置日历和时区

- **calendar** 属性

    NSCalendar 类型，用于解释日期组件的日历。
    
- **timeZone** 属性

    NSTimeZone 类型，用于解释日期组件的时区。

### 访问年月日

- **era** 属性

    NSInteger 类型，年代（纪元）。

- **year** 属性

    NSInteger 类型，年份。
    
    > ⚠️ **注意**
    >
    > 对于农历来说，此属性的值为 `1-60` 中的一个数，并不是对应的年份，因为农历中的干支纪年有 60 个，对应这 60 个数字。

- **quarter** 属性

    NSInteger 类型，季度。
    
- **month** 属性

    NSInteger 类型，月份。

- **leapMonth** 属性

    BOOL 类型，指示月份是否为闰月。 
    
    如果月份是闰月，则为 `YES`，否则为 ` NO`。
    
- **day** 属性

    NSInteger 类型，日。
    
    例如 6 月 2 日，此属性值为 2。

- **weekday** 属性

    NSInteger 类型，星期。
    
    星期的单位是数字 `1` 到 `n`，其中 `n` 是一周中的天数。例如，在公历中，`n` 为 `7`，`1` 表示星期日。某些日历可能 `1` 并不表示星期日。
    
### 访问时分秒

- **hour** 属性

    NSInteger 类型，小时。
    
- **minute** 属性

    NSInteger 类型，分钟。

- **second** 属性

    NSInteger 类型，秒钟。

- **nanosecond** 属性

    NSInteger 类型，纳秒。
    
### 访问其他

- **yearForWeekOfYear** 属性

    NSInteger 类型，*ISO 8601* 标准的年份（即周编号年）。

    公历定义一周有 7 天，一年有 365 天（闰年 366 天）。但是，365 或 366 都不能平均分配 7 天（周），所以通常情况是一年的最后一周在下一年的某一天结束，而一年的第一周从前一年开始。为了协调这一点，*ISO 8601* 标准定义了一个**周编号年**，一年包括 52 或 53 整周（364 或 371 天），这样一年的第一周被指定为包含一年中第一个星期四的一周。对于给定日期，`weekOfYear` 属性指示日期所在的周序号，`yearForWeekOfYear` 提供相应的周编号年份。
    
    > ⚠️ **注意**
    >
    > `weekOfYear` 和 `yearForWeekOfYear` 的值取决于它们使用的日历。例如，*ISO 8601* 日历在星期一开始一周，并使用第一个星期四来确定一年中的第一周。但是，北美使用的公历在星期日开始，并使用年份的第一个星期六确定第一周。
    > 
    > 🔥 **提示**
    >
    > 因为在农历的日期组件中中，`year` 属性不能够准确的定位是哪一年（原因参考该属性），所以实际需要时使用此属性来确定农历的年份。但是在农历中，此属性的值是实际的年份加上 `2637`。例如，在农历的 2020 年，此属性的值为 `4657`。
    
- **weekOfYear** 属性

    NSInteger 类型，*ISO 8601* 标准的周序号。
    
    参考 `yearForWeekOfYear` 属性。                              
    
- **weekOfMonth** 属性

    NSInteger 类型，当前日期在本月的周的序号。
    
- **weekdayOrdinal** 属性

    NSInteger 类型，星期序号（周序号）。
    
    星期序号一般配合星期使用，表示该月的第几个星期几。例如，在公历中，3 个星期单位和 2 个星期序号单位表示「这个月的第二个星期二」。所以可以简单的理解此属性表示月份的周序号，即该周处于该月的第几周。
    
### 将组件作为日历单位访问

- **NSCalendarUnit** 枚举类型

    NSCalendarUnit 枚举用于表示日历的单位。如年，月，日和小时等。日历单位可以用作位掩码来指定单位的组合。具体枚举值如下表：
    
    | NSCalendarUnit 枚举值 | 说明 |
    | :-: | :-: | 
    | NSCalendarUnitEra | 时代单位 |
    | NSCalendarUnitYear | 年份单位 |
    | NSCalendarUnitQuarter | 季度单位 |
    | NSCalendarUnitMonth | 月份单位 |
    | NSCalendarUnitDay | 日单位 |
    | NSCalendarUnitWeekday | 星期单位 |
    | NSCalendarUnitWeekdayOrdinal | 星期序号单位 |
    | NSCalendarUnitHour | 小时单位 |
    | NSCalendarUnitMinute | 分钟单位 |
    | NSCalendarUnitSecond | 秒钟单位 |
    | NSCalendarUnitNanosecond | 纳秒单位 |
    | NSCalendarUnitYearForWeekOfYear | 周编号年份单位 |
    | NSCalendarUnitWeekOfYear | 一年中的周单位 |
    | NSCalendarUnitWeekOfMonth | 一个月中的周单位 |
    | NSCalendarUnitCalendar | 日期组件对象的日历 |
    | NSCalendarUnitTimeZone | 日期组件对象的时区 |
    
    > ⚠️ **注意**
    >
    > 1. 官方文档中提到 NSCalendarUnitQuarter 的大部分未实现，不建议使用。
    > 2. 星期单位是数字 `1` 到 `N`（其中公历 N = 7，1 是星期日）。星期序号单位描述对应星期单位在月份单位内的序号位置。例如，在公历中，3 个星期单位和 2 个星期序号单位表示「这个月的第二个星期二」。

- **valueForComponent:** 方法

    返回给定日历单位的值。
    
    - 参数一：NSCalendarUnit 类型，要检索其值的日历单位。不要传递 `NSCalendarUnitCalendar` 或`NSCalendarUnitTimeZone`。
    - 返回值：NSInteger 类型，给定日历单位的值。

    此方法允许为 `NSCalendarUnit` 值检索组件值。
    
- **setValue:forComponent:** 方法

    设置给定日历单位的值。
    
    - 参数一：NSInteger 类型，要为单位组件设置的值。
    - 参数二：NSCalendarUnit 类型，要为其设置值的日历单位。不要传递 `NSCalendarUnitCalendar` 或`NSCalendarUnitTimeZone`。
    
    此方法允许为 `NSCalendarUnit` 值检索组件值。