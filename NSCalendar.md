# NSCalendar

NSCalendar 继承自 NSObject，称为「日历」对象，它是定义日历单位（例如年份和星期几）与绝对时间点之间关系的对象，提供计算和比较日期的功能。

NSCalendar 对象封装了关于计算时间系统的信息，其中定义了一年的开始、长度和划分。可以使用日历对象在绝对时间（NSDate）和日期组件（NSDateComponents）之间进行转换，例如年，日或分钟。

NSCalendar 提供各种日历的实现。它提供了几种不同日历的数据，包括佛教历法，公历，希伯来历，伊斯兰历等（支持的日历取决于操作系统——可以通过 NSLocale 类确定给定版本支持哪些日历）。NSCalendar 与 NSDateComponents 类紧密相关，后者的实例描述了日历计算所需日期的组成元素。

## 不同的日历

大多数语言环境使用最广泛使用的民用日历，称为**公历**（Gregorian），但仍有例外。 例如：

- 在沙特阿拉伯，一些地区主要使用伊斯兰教的 Umm al-Qura 日历。
- 在埃塞俄比亚，一些地区主要使用埃塞俄比亚日历。
- 在伊朗和阿富汗，一些地区主要使用波斯日历。
- 在泰国，一些地区主要使用佛教日历。

另外还有其他国家或地区同时使用公历和另一个日历。例如：

- 印度还使用印度国家日历。
- 以色列还使用希伯来日历。
- 中国和其他地区也使用中国历（农历），主要用于计算天文日期和中国传统节日。
- 日本使用日历（Japanese Calendar），主要用于添加年份名称。

## 公历的背景

公历于 1582 年首次引入，作为儒略历（Julian Calendar）的替代品。根据儒略历法，任何年份的 2 月份都要加上闰日，闰日的数字可以被 4 整除，这就导致了每年 11 分钟的差距，即每 128 年有一天的差距。公历修订了闰日的计算规则，跳过了任何能被 100 整除的年份的闰日，除非那个年份的数字也能被 400 整除（即闰年），导致每年的差距只有26秒，即 3323 年才有一天的差距。

为了从儒略历过渡到公历，从公历中删除了 10 天（10 月 5 日至 14 日）。引入公历后，许多国家仍继续使用儒略历，土耳其是 1926 年采用公历的最后一个国家。由于采用错开采纳，各国在采用时的过渡期有不同的开始日期和不同的跳过天数，以解释与闰日计算的额外差异。

NSCalendar 模拟了一个公历（由 *ISO 8601:2004* 定义）的行为，它从引入日期开始向后延伸公历。

## 日历算术

要执行日历算法，可以将 NSDate 对象与 NSCalendar 对象结合使用。例如，要在一个日历和另一个日历中的分解日期之间进行转换，必须首先使用第一个日历将分解的元素转换为日期，然后使用第二个日历对其进行分解。NSDate 提供日期和时间的绝对比例和纪元（参考点），然后可以将其渲染到特定日历中，用于日历计算或用户显示。

两个返回日期对象的 NSCalendar 方法：`dateFromComponents:` 和 `dateByAddingComponents:toDate:options:`，将 NSDateComponents 对象作为参数，该对象描述计算所需的日历组件。可以根据需要提供任意数量的组件（或选择）。

## NSCalendar 属性和方法

### 创建和初始化日历

- **NSCalendarIdentifier** 别名

    NSString 类型的别名，支持的日历类型。标识符指定日历的类型如下表：
    
    | 标识符 | 说明 |
    | :-: | :-: |
    | NSCalendarIdentifierGregorian | 公历 |
    | NSCalendarIdentifierISO8601 | ISO8601 日历 |
    | NSCalendarIdentifierBuddhist | 佛教历法 |
    | NSCalendarIdentifierChinese | 中国日历（农历）|
    | NSCalendarIdentifierCoptic | 科普特日历 |
    | NSCalendarIdentifierEthiopicAmeteAlem | Ethiopic（Amete Alem）日历 |
    | NSCalendarIdentifierEthiopicAmeteMihret | Ethiopic（Amete Mihret）日历 |
    | NSCalendarIdentifierHebrew | 希伯来日历 |
    | NSCalendarIdentifierIndian | 印度日历 |
    | NSCalendarIdentifierIslamic | 伊斯兰历法 |
    | NSCalendarIdentifierIslamicCivil | 伊斯兰民事日历 |   
    | NSCalendarIdentifierIslamicTabular | 表格伊斯兰历法 | 
    | NSCalendarIdentifierIslamicUmmAlQura | 伊斯兰教Umm al-Qura 日历 |
    | NSCalendarIdentifierJapanese | 日本日历 |
    | NSCalendarIdentifierPersian | 波斯日历 |
    | NSCalendarIdentifierRepublicOfChina | 中国台湾日历 |

- **calendarWithIdentifier:** 类方法

    创建由给定标识符指定的日历对象。
    
    - 参数一：NSCalendarIdentifier 类型，日历标识符。
    - 返回值：NSCalendar 对象。如果标识符未知，则为 `nil`（例如，如果它是无法识别的字符串或当前版本的操作系统不支持日历）。

- **initWithCalendarIdentifier:** 方法

    根据给定的标识符初始化日历。
    
    - 参数一：NSCalendarIdentifier 类型，日历标识符。
    - 返回值：NSCalendar 对象。如果标识符未知，则为 `nil`（例如，如果它是无法识别的字符串或当前版本的操作系统不支持日历）。

### 获取用户的日历

- **currentCalendar** 类属性

    NSCalendar 类型，只读。用户的当前日历。
    
    返回的日历由当前用户选择的系统区域设设置得到，并覆盖用户在「系统偏好设置」中指定的任何自定义设置。从此日历获得的设置不会随着「系统偏好设置」的更改而更改。
        
- **autoupdatingCurrentCalendar** 类属性

    NSCalendar 类型，只读。跟踪用户首选日历更改的日历。
    
    从此日历获得的设置会随着用户设置的更改而改变（与 `currentCalendar` 对比）。

### 获取日期组件

- **date:matchesComponents:** 方法

    返回给定日期是否与所有给定日期组件匹配。
    
    - 参数一：NSDate 类型，执行计算的日期。
    - 参数二：NSDateComponents 类型，要匹配的日期组件。
    - 返回值：BOOL 类型，如果给定日期与给定组件匹配，则为 `YES`，否则为 `NO`。

    此方法可用于确定通过 `nextDateAfterDate:matchingUnit:value:options:` 或`enumerateDatesStartingAfterDate:matchingComponents:options:usingBlock:` 等方法计算的日期是否是精确的，或者由于不存在的时间而需要进行调整。

- **component:fromDate:** 方法

    返回给定日期的指定日期组件的值。
    
    - 参数一：NSCalendarUnit 类型，指定的日期组件类型。
    - 参数二：NSDate 类型，执行计算的日期。
    - 返回值：NSInteger 类型，日期组件的值。
    
- **components:fromDate:** 方法

    返回表示给定日期的日期组件。 
    
    - 参数一：NSCalendarUnit 类型，返回的日期组件对象中包含的组件。
    - 参数二：NSDate 类型，执行计算的日期。
    - 返回值：NSDateComponents 类型，参数二分解为参数一中指定的组件。如果日期超出接收器的定义范围或者无法执行计算，则返回 `nil`。
    
    > ⚠️ **注意**
    >
    > 此方法中的参数一指定了什么值，那么返回的 NSDateComponents 对象才会具有对应的属性值，否则其属性值都是 `NSDateComponentUndefined`（等于 `NSIntegerMax`）。并且返回的 NSDateComponents 对象具体属性值对应传入的日期。示例如下代码。
    
    ```Objective-C
    NSDate *currentDate = [NSDate date];    // 2019-06-02 03:25:05 UTC
    NSCalendar *calendar = [NSCalendar calendarWithIdentifier:NSCalendarIdentifierGregorian];
    NSDateComponents *components1 = [calendar components:NSCalendarUnitDay 
                                                fromDate:currentDate];
    NSLog(@"%ld", components1.year);         // 输出为：9223372036854775807
    NSLog(@"%ld", components1.month);        // 输出为：9223372036854775807
    NSLog(@"%ld", components1.day);          // 输出为：2
    
    NSDateComponents *components2 = [calendar components:NSCalendarUnitYear | NSCalendarUnitMonth | NSCalendarUnitDay
                                                fromDate:currentDate];
    NSLog(@"%ld", components2.year);         // 输出为：2019
    NSLog(@"%ld", components2.month);        // 输出为：6
    NSLog(@"%ld", components2.day);          // 输出为：2
    ```
    
- **components:fromDate:toDate:options:** 方法

    返回两个给定日期之间的差值作为日期组件。
    
    - 参数一：NSCalendarUnit 类型，返回的日期组件对象中包含的组件。
    - 参数二：NSDate 类型，计算的开始日期。
    - 参数三：NSDate 类型，计算的结束日期。
    - 参数四：NSCalendarOptions 类型，计算选项。
    - 返回值：NSDateComponents 类型，其组件由参数一指定，并使用参数四的选项来计算参数三和参数二之间的差异。如果日期超出接收器的定义范围或者无法执行计算，则返回 `nil`。

    > ⚠️ **注意**
    >
    > 此方法中的参数一指定了什么值，那么返回的 NSDateComponents 对象才会具有对应的属性值，否则其属性值都是 `NSDateComponentUndefined`（等于 `NSIntegerMax`）。并且返回的 NSDateComponents 对象具体属性会根据参数一变化。例如，两个日期之间差距 1 年 2 个月 3 天。如果，参数一只有 NSCalendarUnitDay，那么 1 年 2 个月 3 天会转化为具体天数；如果参数一是 NSCalendarUnitYear 和 NSCalendarUnitDay，那么 1 年 2 个月 3 天会把不够一年的部分全部表示为天数，以此类推。
    
    ```Objective-C
    NSDate *date = [NSDate dateWithTimeIntervalSince1970:852652800];  // 1997-01-07 16:00:00 UTC
    NSDate *currentDate = [NSDate date];        // 2019-06-02 03:25:05 UTC
    NSCalendar *calendar = [NSCalendar calendarWithIdentifier:NSCalendarIdentifierGregorian];
    // 只设置 NSCalendarUnitDay
    NSDateComponents *components1 = [calendar components:NSCalendarUnitDay
                                                fromDate:date
                                                  toDate:currentDate
                                                 options:0];
    NSLog(@"%ld", components1.year);     // 输出为：9223372036854775807
    NSLog(@"%ld", components1.month);    // 输出为：9223372036854775807
    NSLog(@"%ld", components1.day);      // 输出为：8180
    // 只设置 NSCalendarUnitMonth
    NSDateComponents *components3 = [calendar components:NSCalendarUnitMonth
                                                fromDate:date
                                                  toDate:currentDate
                                                 options:0];
    NSLog(@"%ld", components1.year);     // 输出为：9223372036854775807
    NSLog(@"%ld", components1.month);    // 输出为：268
    NSLog(@"%ld", components1.day);      // 输出为：9223372036854775807
    // 只设置 NSCalendarUnitYear
    NSDateComponents *components2 = [calendar components:NSCalendarUnitYear
                                                fromDate:date
                                                  toDate:currentDate
                                                 options:0];
    NSLog(@"%ld", components1.year);     // 输出为：22
    NSLog(@"%ld", components1.month);    // 输出为：9223372036854775807
    NSLog(@"%ld", components1.day);      // 输出为：9223372036854775807
    // 设置 NSCalendarUnitYear、NSCalendarUnitMonth 和 NSCalendarUnitDay
    NSDateComponents *components2 = [calendar components:NSCalendarUnitYear | NSCalendarUnitMonth | NSCalendarUnitDay
                                                fromDate:date
                                                  toDate:currentDate
                                                 options:0];
    NSLog(@"%ld", components1.year);     // 输出为：22
    NSLog(@"%ld", components1.month);    // 输出为：4
    NSLog(@"%ld", components1.day);      // 输出为：25                 
    ```

- **components:fromDateComponents:toDateComponents:options:** 方法

    返回作为日期组件给出的开始日期和结束日期之间的差值。
    
    - 参数一：NSCalendarUnit 类型，返回的日期组件对象中包含的组件。
    - 参数二：NSDateComponents 类型，计算的开始日期组件对象。
    - 参数三：NSDateComponents 类型，计算的结束日期组件对象。
    - 参数四：NSCalendarOptions 类型，计算选项。
    - 返回值：NSDateComponents 类型，由参数一指定，并使用参数四的选项来计算参数二和参数三之间的差异。如果日期超出接收器的定义范围，或者计算无法执行，则返回nil。

- **componentsInTimeZone:fromDate:** 方法

    返回日期的所有日期组件，就像在给定的时区（而不是接收日历的时区）中一样。
    
    - 参数一：NSTimeZone 类型，返回日期组件时使用的时区。此值覆盖接收方的时区。
    - 参数二：NSDate 类型，执行计算的日期。
    - 返回值：NSDateComponents 类型，包含来自给定日期的所有组件的对象。

### 获取日历信息

- **calendarIdentifier** 属性

    NSCalendarIdentifier 类型，只读。日历的标识符。
    
- **firstWeekday** 属性

    NSUInteger 类型，日历对象第一个工作日的索引。
    
- **locale** 属性

    NSLocale 类型，日历对象的地区。
    
- **timeZone** 属性

    NSTimeZone 类型，日历对象的时区。
    
- **maximumRangeOfUnit:** 方法

    返回给定单位可以采用的值的最大范围限制。
    
    - 参数一：NSCalendarUnit 类型，日历单位。
    - 返回值：NSRange 类型，最大范围。

    例如，在公历中，日单位的最大值范围是 1-31。
    
- **minimumRangeOfUnit:** 方法

    返回给定单位可以采用的值的最小范围限制。
    
    - 参数一：NSCalendarUnit 类型，日历单位。
    - 返回值：NSRange 类型，最小范围。

    例如，在公历中，日单位的最小值范围是 1-28。
    
- **rangeOfWeekendStartDate:interval:containingDate:** 属性

    返回给定日期是否属于周末时段，如果是，则通过引用返回周末范围的开始日期和时间间隔。
    
### 计算日期

-  **NSCalendarOptions** 枚举   

    NSCalendarOptions 枚举类型表示一些涉及日历的算术操作选项，具体值如下：
    
    | NSCalendarOptions 枚举值 | 说明 |
    | :-: | :-: |
    | NSCalendarWrapComponents | 指定为 NSDateComponents 对象指定的组件应递增并在溢出时回绕到 `0` 或 `1`，但不应导致更高的单位递增。 |
    | NSCalendarMatchStrictly | 指定操作应根据需要向前或向后移动以查找匹配项。 |
    | NSCalendarSearchBackwards | 指定操作应向后移动以查找给定日期之前的上一个匹配项。 |
    | NSCalendarMatchPreviousTimePreservingSmallerUnits | 指定当在给定 NSDateComponents 对象中指定的下一个最高单元的下一个实例结束之前没有匹配时间时，此方法使用缺失单元的先前现有值并保留较低单元的值。 |
    | NSCalendarMatchNextTimePreservingSmallerUnits | 指定当在给定NSDateComponents对象中指定的下一个最高单元的下一个实例结束之前没有匹配时间时，此方法使用缺失单元的下一个现有值并保留较低单元的值。 |
    | NSCalendarMatchNextTime | 指定当在给定NSDateComponents对象中指定的下一个最高单元的下一个实例结束之前没有匹配时间时，此方法使用缺失单元的下一个现有值，并且不保留下部单元的值。 |
    | NSCalendarMatchFirst | 指定如果有两个或更多匹配时间，则操作应返回第一个匹配项。|
    | NSCalendarMatchLast | 指定如果有两个或更多匹配时间，则操作应返回最后一次出现。|

- **dateFromComponents:** 方法

    使用 NSDateComponents 对象计算并返回 NSDate 对象。
    
    - 参数一：NSDateComponents 类型，日期组件对象。
    - 返回值：NSDate 类型，如果接收者无法将参数一转换为 NSDate 对象，则返回 `nil`。

    如果提供的日期组件不足以完全指定绝对时间，则日历将使用其选择的默认值。当信息不一致时，日历可能会忽略某些组件参数，或者该方法可能返回 `nil`。

    以下示例显示如何使用此方法创建一个 NSDate 对象，以表示公历的 1965 年 1 月 6 日 14:10:00。
    
    ```Objective-C
    NSDateComponents *comps = [[NSDateComponents alloc] init];
    [comps setYear:1965];
    [comps setMonth:1];
    [comps setDay:6];
    [comps setHour:14];
    [comps setMinute:10];
    [comps setSecond:0];
    NSDate *date = [gregorian dateFromComponents:comps];
    ```

- **dateByAddingComponents:toDate:options:** 方法

    将 NSDateComponents 对象添加到给定的 NSDate 对象，再使用指定选项计算并返回新的 NSDate 对象。
    
    - 参数一：NSDateComponents 类型，日期组件对象。
    - 参数二：NSDate 类型，参与计算的日期对象。
    - 参数三：NSCalendarOptions 类型，计算选项。
    - 返回值：NSDate 类型，新的日期对象。如果日期超出接收者的定义范围或者无法执行计算，则返回 `nil`。
    
    以下示例显示如何使用现有日历（公历）将 2 个月和 3 天添加到当前日期和时间：
    
    ```Objective-C
    NSDate *currentDate = [NSDate date];    // 2019-06-01 02:37:48 UTC
    NSDateComponents *comps = [[NSDateComponents alloc] init];
    [comps setMonth:2];
    [comps setDay:3];
    NSDate *date = [gregorian dateByAddingComponents:comps toDate:currentDate options:0];  // 2019-08-04 02:37:48 UTC
    ```
    
- **dateByAddingUnit:value:toDate:options:** 方法

    将给定的日期组件单位的值添加到给定的 NSDate 对象，再使用指定选项计算并返回新的 NSDate 对象。
    
    - 参数一：NSCalendarUnit 类型，日期组件单位。
    - 参数二：NSInteger 类型，参数一日期组件单位的对应值。
    - 参数三：NSDate 类型，参与计算的日期对象。
    - 参数四：NSCalendarOptions 类型，计算选项。
    - 返回值：NSDate 类型，新的日期对象。如果日期超出接收者的定义范围或者无法执行计算，则返回 `nil`。

- **dateBySettingUnit:value:ofDate:options:** 方法

    - 参数一：NSCalendarUnit 类型，日期组件单位。
    - 参数二：NSInteger 类型，参数一日期组件单位的对应值。
    - 参数三：NSDate 类型，参与计算的日期对象。
    - 参数四：NSCalendarOptions 类型，计算选项。
    - 返回值：NSDate 类型，新的日期对象。

- **dateBySettingHour:minute:second:ofDate:options:** 方法

    使用给定时间和 NSDate 对象，再使用指定的计算选项计算并返回新的 NSDate 对象。
        
    - 参数一：NSInteger 类型，小时的值。
    - 参数二：NSInteger 类型，分钟的值。
    - 参数三：NSInteger 类型，秒钟的值。
    - 参数四：NSDate 类型，参与计算的日期对象。
    - 参数五：NSCalendarOptions 类型，计算选项。
    - 返回值：NSDate 类型，新的日期对象。
    
- **dateWithEra:year:month:day:hour:minute:second:nanosecond:** 方法

    使用给定的组件创建新的 NSDate 对象。
    
    - 参数一：NSInteger 类型，纪元。
    - 参数二：NSInteger 类型，年。
    - 参数三：NSInteger 类型，月。
    - 参数四：NSInteger 类型，日。
    - 参数五：NSInteger 类型，时。
    - 参数六：NSInteger 类型，分。
    - 参数七：NSInteger 类型，秒。
    - 参数八：NSInteger 类型，纳秒。
    - 返回值：NSDate 类型，新的日期对象。如果组件与有效日期不对应，则为 `nil`。

- **dateWithEra:yearForWeekOfYear:weekOfYear:weekday:hour:minute:second:nanosecond:** 方法

    使用给定组件创建的新的 NSDate 对象，基于年和周。
    
    - 参数一：NSInteger 类型，纪元。
    - 参数二：NSInteger 类型，ISO 8601 标准的年份。
    - 参数三：NSInteger 类型，ISO 8601 标准的周序号。
    - 参数四：NSInteger 类型，星期几。
    - 参数五：NSInteger 类型，时。
    - 参数六：NSInteger 类型，分。
    - 参数七：NSInteger 类型，秒。
    - 参数八：NSInteger 类型，纳秒。
    - 返回值：NSDate 类型，新的日期对象。如果组件与有效日期不对应，则为 `nil`。

### 比较日期

- **compareDate:toDate:toUnitGranularity:** 方法

    将两个给定日期基于指定的组件来排序。
    
    - 参数一：NSDate 类型，比较的第一个日期。
    - 参数二：NSDate 类型，比较的第二个日期。
    - 参数三：NSCalendarUnit 类型，日期组件单位。
    - 返回值：NSComparisonResult 类型，表示排序顺序的常数。

- **isDate:equalToDate:toUnitGranularity:** 方法

    指示两个日期是否相等。
    
    - 参数一：NSDate 类型，比较的第一个日期。
    - 参数二：NSDate 类型，比较的第二个日期。
    - 参数三：NSCalendarUnit 类型，日期组件单位。
    - 返回值：BOOL 类型，如果两个日期对于大于或等于给定单位的所有单位都具有相等的日期组件，则为 `YES`，否则为 `NO`。

- **isDate:inSameDayAsDate:** 方法

    指示两个日期是否在同一天。
    
    - 参数一：NSDate 类型，比较的第一个日期。
    - 参数二：NSDate 类型，比较的第二个日期。
    - 返回值：BOOL 类型，如果两个日期都在同一天内，则为 `YES`，否则为 `NO`。

- **isDateInToday:** 方法

    指示给定日期是否在「今天」。
    
    - 参数一：NSDate 类型，执行计算的日期。
    - 返回值：BOOL 类型，如果给定日期在「今天」，则为 `YES`，否则为 `NO`。

- **isDateInTomorrow:** 方法

    指示给定日期是否在「明天」。
    
    - 参数一：NSDate 类型，执行计算的日期。
    - 返回值：BOOL 类型，如果给定日期在「明天」，则为 `YES`，否则为 `NO`。

- **isDateInYesterday:** 方法

    指示给定日期是否在「昨天」。
    
    - 参数一：NSDate 类型，执行计算的日期。
    - 返回值：BOOL 类型，如果给定日期在「昨天」，则为 `YES`，否则为 `NO`。

- **isDateInWeekend:** 方法

    指示给定日期是否在日历和日历的区域设置定义的周末时段内。
    
    - 参数一：NSDate 类型，执行计算的日期。
    - 返回值：BOOL 类型，如果给定日期在周末期间，则为 `YES`，否则为 `NO`。
    