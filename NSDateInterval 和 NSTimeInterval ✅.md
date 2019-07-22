# NSDateInterval 和 NSTimeInterval ✅

## NSDateInterval

NSDateInterval 继承自 NSObject，它是表示特定开始日期和结束日期之间的时间跨度的对象。NSDateInterval 类提供了一个编程接口，用于计算日期间隔并能够确定某些日期是否在其中，以及比较日期间隔并检查它们是否相交等。

NSDateInterval 对象主要有 `startDate`、`endDate` 和 `duration` 三个属性属性，分别表示「开始日期」、「结束日期」和「持续时间」。开始日期和结束日期两者可以相等，在这种情况下，其持续时间为 `0`。但是，结束日期不能早于开始日期。

还可以使用 NSDateIntervalFormatter 类创建适合在当前语言环境中显示的 NSDateInterval 对象的字符串表示形式。

### NSDateInterval 属性和方法

#### 创建 NSDateInterval 实例

- **init** 方法

    将开始日期和结束日期设置为当前日期来初始化 NSDateInterval 实例。
    
    - 返回值：NSDateInterval 实例。
    
- **initWithStartDate:duration:** 方法

    使用给定的开始日期和持续时间初始化 NSDateInterval 实例。
    
    - 参数一：NSDate 类型，开始日期。
    - 参数二：NSTimeInterval 类型，时间间隔，单位秒。不能小于 `0`。
    - 返回值：NSDateInterval 实例。
    
- **initWithStartDate:endDate:** 方法

    使用给定的开始日期和结束日期初始化 NSDateInterval 实例。
    
    - 参数一：NSDate 类型，开始日期。
    - 参数二：NSDate 类型，结束日期，不能早于开始日期。
    - 返回值：NSDateInterval 实例。

#### 开始、结束日期和持续时间

- **startDate** 属性

    NSDate 类型，只读。开始日期。
    
- **endDate** 属性

    NSDate 类型，只读。结束日期。
    
- **duration** 属性

    NSTimeInterval 类型，只读。持续时间。
    
#### 比较

- **compare:** 方法

    比较两个 NSDateInterval 实例。
    
    - 参数一：NSDateInterval 类型，用于比较的 NSDateInterval 实例。
    - 返回值：NSComparisonResult 类型，指示接收者和参数一两者的时间顺序。

        - 如果接收者的开始日期早于参数一的开始日期，或两者的开始日期相等且接收者的持续时间小于参数一的持续时间，则返回 `NSOrderedAscending`。

        - 如果接收者的开始日期晚于参数一的开始日期，或两者的开始日期相等且接收者的持续时间大于参数一的持续时间，则返回 `NSOrderedDescending`。

        - 如果接收者的开始日期和持续时间等于参数一的值，则返回 `NSOrderedSame`。

    参考：[compare: 方法](https://developer.apple.com/documentation/foundation/nsdateinterval/1641636-compare?language=objc)
    
- **isEqualToDateInterval:** 方法

    指示接收者是否等于指定的 NSDateInterval 实例。
    
    - 参数一：NSDateInterval 类型，比较的 NSDateInterval 实例。
    - 返回值：BOOL 类型，如果两者的开始日期和持续时间相等，则为 `YES`，否则为 `NO`。

#### 确定相交和包含

- **intersectsDateInterval:** 方法

    指示接收者是否与指定的 NSDateInterval 实例相交。

    - 参数一：NSDateInterval 类型，检查是否相交的 NSDateInterval 实例。
    - 返回值：BOOL 类型，如果返回值和参数一有交集，则返回 `YES`，否则返回 `NO`。

- **intersectionWithDateInterval:** 方法

    返回接收者与指定的 NSDateInterval 实例之间的交集。
    
    - 参数一：NSDateInterval 类型，指定的 NSDateInterval 实例。
    - 返回值：NSDateInterval 类型，接收者与参数一之间的交集，如果没有交集则返回 `nil`。
    
- **containsDate:** 方法

    指示接收者是否包含指定日期。
    
    - 参数一：NSDate 类型，指定日期。
    - 返回值：BOOL 类型，如果接受者包含参数一的日期，返回 `YES`，否则返回 `NO`。

## NSTimeInterval

NSTimeInterval 是别名类型，它是 double 类型的别名，用来表示时间间隔，单位是秒。它在 10,000 年的范围内产生亚毫秒精度。NSTimeInterval 不指定唯一的时间点，甚至不指定特定时间之间的跨度。将时间间隔与一个或多个已知参考点组合产生 NSDate 或 NSDateInterval 值。