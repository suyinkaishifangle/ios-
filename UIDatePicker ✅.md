# UIDatePicker ✅

UIDatePicker 继承自 UIControl，称为「日期选择器」，它是用于输入日期和时间值的控件。

只能使用日期选择器来处理时间和日期的选择。如果要处理列表中任意项的选择，则使用 UIPickerView 对象。

日期选择器使用目标-动作（Target-Action）设计模式在用户更改所选日期时通知应用。要在日期选择器的值更改时收到通知，需要使用`UIControlEventValueChanged` 事件注册操作方法。在运行时，日期选择器会调用操作方法以响应用户选择日期或时间。

使用 `addTarget:action:forControlEvents:` 方法将日期选择器连接到操作方法，操作方法采用以下三种形式之一：

```Objective-C
- (void)doSomething;
- (void)doSomething:(id)sender;
- (void)doSomething:(id)sender forEvent:(UIEvent*)event;
```

## UIDatePicker 方法和属性

**配置日期选择器模式**

- **datePickerMode** 属性

    UIDatePickerMode 类型，日期选择器的模式，默认模式是 `UIDatePickerModeDateAndTime`。

    使用此属性可更改日期选择器显示的信息类型。它确定日期选择器是否允许选择日期，时间，日期和时间，或倒计时时间。
    
    UIDatePickerMode 枚举类型如下：
    
    | UIDatePickerMode 枚举值 | 说明 |
    | :-: | :-: |
    | UIDatePickerModeTime | 以上午/下午（可选）、小时和分钟显示。<br>例如：`下午 | 6 | 53` |
    | UIDatePickerModeDate | 以年，月，日为单位显示。<br>例如：`2019年 | 4月 | 8日` |
    | UIDatePickerModeDateAndTime | 以月，日，星期，上午/下午，小时和分钟显示。<br>例如：`4月7日周日 | 下午 | 8 | 40` |
    | UIDatePickerModeCountDownTimer | 倒数计时器模式，以小时和分钟显示。<br>例如：`5小时 | 3分钟` |
    
    > ⚠️ **注意**
    >
    > 1. 以上样式中显示的实际项目及其顺序取决于系统的语言设置，以上都是在系统语言为「中文」时的样式。如果系统语言为其他语言，那么每个样式中的项目顺序可能会发生变化。
    > 2. 当 **⚙️「设置」➤ 通用 ➤ 日期与时间 ➤ 24小时制** 为打开状态时，日期选择器中的时间会自动采用 24小时制，而不会显示「上午/下午」，反之同理。

**管理日期和日历**

- **calendar** 属性

    NSCalendar 类型，用于日期选择器的日历。
    
    此属性的默认值对应于 **⚙️「设置」➤ 通用 ➤ 语言与地区 ➤ 日历** 中配置的日历。这相当于通过调用 NSCalendar 类方法 `currentCalendar` 返回的值。将此属性设置为 `nil` 等同于将其设置为其默认值。
    
- **date** 属性

    NSDate 类型，日期选择器显示的日期。默认值是创建 UIDatePicker 对象时的日期。
    
    使用此属性可获取和设置指定的日期。当模式设置为 `UIDatePickerModeCountDownTimer` 时，此属性无效，需要使用 `countDownDuration` 属性。
    
- **countDownDuration** 属性

    NSTimeInterval 类型，当 `datePickerMode` 属性设置为倒数计时器模式时，日期选择器显示的值。
    
    当日期选择器的 `datePickerMode` 属性设置为 `UIDatePickerModeCountDownTimer` 时，使用此属性来获取和设置当前选定的值。此属性的类型为 NSTimeInterval，因此以秒为单位进行测量，但日期选择器仅显示小时和分钟。 如果日期选择器的模式不是 `UIDatePickerModeCountDownTimer`，则此属性无效，而是指日期属性。 默认值为 0.0，最大值为 23:59（86,399 秒）。

- **locale** 属性

    NSLocale 类型，日期选择器使用的地区设置。默认值是 NSLocale 的 `currentLocale` 属性返回的当前语言环境，或日期选择器日历使用的语言环境。Locales 封装了有关语言或文化方面的信息，例如日期格式。
    
- **timeZone** 属性

    NSTimeZone 类型，日期选择器显示的日期中反映的时区。
    
    默认值为 `nil`，它告诉日期选择器使用 NSTimeZone 类属性 `localTimeZone` 返回的当前时区或日期选择器日历使用的时区。
    
- **setDate:animated:** 方法

    设置日期选择器中显示的日期，并提供设置动画的选项。
    
    - 参数一：NSDate，要在日期选择器中显示的新日期。
    - 参数二：BOOL 类型，是否显示动画。

**配置日期**

- **maximumDate** 属性

    NSDate 类型，日期选择器可以显示的最大日期。默认值为 `nil`，表示没有最大日期。
    
    使用此属性可配置在日期选择器界面中选择的最大日期。此属性以及 `minimumDate` 属性可以用来指定有效的日期范围。如果最小日期值大于最大日期值，则忽略这两个属性。倒数计时器模式（`UIDatePickerModeCountDownTimer`）中也会忽略最小和最大日期。
    
- **minimumDate** 属性

    NSDate 类型，日期选择器可以显示的最小日期。默认值为 `nil`，表示没有最小日期。
    
    使用此属性可配置在日期选择器界面中选择的最小日期。此属性以及 `maximumDate` 属性可以用来指定有效的日期范围。如果最小日期值大于最大日期值，则忽略这两个属性。倒数计时器模式（`UIDatePickerModeCountDownTimer`）中也会忽略最小和最大日期。
    
- **minuteInterval** 属性

    NSInteger 类型，日期选择器应显示分钟的时间间隔。
    
    使用此属性设置分钟轮显示的间隔（例如，15 分钟）。间隔值必须是 60 的因数；如果不是，则使用默认值。默认值和最小值为 1；最大值为 30。