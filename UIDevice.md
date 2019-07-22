# UIDevice

UIDevice 继承自 NSObject，它是当前设备的表示。

使用 UIDevice 对象来获取有关设备的信息，比如指定的名称、设备型号、操作系统名称和版本。还可以使用 UIDevice 实例来检测设备特征的变化，例如物理方向。可以使用 `orientation` 属性获取当前方向，也可以通过注册`UIDeviceOrientationDidChangeNotification` 通知来接收设备方向更改通知。在使用这些方法获取设备方向数据之前，必须使用 `beginGeneratingDeviceOrientationNotifications` 方法启用数据传递。当不再需要跟踪设备方向时，请调用 `endGeneratingDeviceOrientationNotifications` 方法以禁用通知的传递。

还可以使用 UIDevice 实例获取有关电池充电状态（由 `batteryState` 属性描述）和充电级别（由 `batteryLevel` 属性描述）的信息和通知。UIDevice 实例还提供对「接近传感器」状态的访问（由 `proximityState` 属性描述）。接近传感器检测用户是否将设备靠近他们的脸部。仅在需要时启用电池监控或接近感应。

还可以使用 `playInputClick` 实例方法在自定义输入和键盘附件视图中播放键盘输入单击。

## UIDevice 方法和属性

- **currentDevice** 类属性

    UIDevice 类型，只读。返回表示当前设备的对象。
    
- **multitaskingSupported** 属性

    BOOL 类型，只读。指示当前设备是否支持多任务处理。
    
### 识别设备和操作系统
    
- **name** 属性

    NSString 类型，只读。设备的名称。
    
    此属性的值是 ⚙️**「设置」** ➤ **通用** ➤ **关于本机** ➤ **名称** 中的值。
    
- **systemName** 属性

    NSString 类型，只读。操作系统的名称。例如：`iOS`。
    
- **systemVersion** 属性

     NSString 类型，只读。操作系统的版本。例如：`12.1.1`。
    
- **model** 属性

    NSString 类型，只读。设备的型号。例如：`iPhone` 和 `iPad`。
     
- **localizedModel** 属性

    NSString 类型，只读。设备的本地化型号，即 `model` 属性值的本地化字符串。
    
- **userInterfaceIdiom** 属性

    UIUserInterfaceIdiom 类型，只读。当前设备上使用的接口样式。
    
    对于通用应用程序，可以使用此属性来定制应用程序对特定类型设备的行为。 例如，iPhone 和 iPad 设备具有不同的屏幕尺寸，因此可能希望根据当前设备的类型创建不同的视图和控件。
    
- **identifierForVendor** 属性

    NSUUID 类型，只读。一个字母数字字符串，用于唯一标识应用程序供应商的设备。
    
### 跟踪设备方向

- **orientation** 属性

    UIDeviceOrientation 类型，只读。返回设备的物理方向。
    
    此属性的值是一个常数，表示设备当前的物理方向。可能与应用程序用户界面的当前方向不同。
    
    此属性的值总是返回 0，除非通过调用 `beginGeneratingDeviceOrientationNotifications` 启用了方向通知。
    
    UIDeviceOrientation 是一个枚举类型，具体枚举值如下：
    
    | UIDeviceOrientation 枚举值 | 说明 |
    | :-: | :-: |
    | UIDeviceOrientationUnknown | 无法确定设备的方向。 |
    | UIDeviceOrientationPortrait | 设备处于纵向模式，设备直立，主页按钮位于底部。 |
    | UIDeviceOrientationPortraitUpsideDown | 设备处于纵向模式，设备直立，主页按钮位于顶部。 |
    | UIDeviceOrientationLandscapeLeft | 设备处于横向模式，设备直立，主页按钮位于右侧。 |
    | UIDeviceOrientationLandscapeRight | 设备处于横向模式，设备直立，主页按钮位于左侧。 |
    | UIDeviceOrientationFaceUp | 设备与地面平行，屏幕朝上。 |
    | UIDeviceOrientationFaceDown | 设备与地面平行，屏幕朝下。 |
    
- **generatesDeviceOrientationNotifications** 属性

    BOOL 类型，只读。指示是否生成方向通知。
    
- **beginGeneratingDeviceOrientationNotifications** 方法

    开始生成设备方向变化的通知。
    
- **endGeneratingDeviceOrientationNotifications** 方法

    结束生成设备方向变化的通知。
     
### ⌛️确定当前的方向

- **UIDeviceOrientationIsPortrait()** 函数

### 获取设备电池状态

- **batteryLevel** 属性

    float 类型，只读。设备的电池电量。
    
    电池电量范围从 `0.0` 到 `1.0`。在访问此属性之前，请确保启用了电池监控。

    如果没有启用电池监控，则电池状态为 `UIDeviceBatteryStateUnknown`，此属性的值为 `-1.0`。
    
- **batteryMonitoringEnabled** 属性

    BOOL 类型。指示是否启用电池监控。此属性的默认值为 `NO`。
    
    如果应用需要通知电池状态的变化，或者想检查电池电量水平，必须启用电池监控。当此属性值为 `NO` 时：
    
    - 禁用与电池相关的通知的发布
    - 禁用读取电池电量和电池状态的功能

- **batteryState** 属性

    UIDeviceBatteryState 类型，只读。设备的电池状态。
    
    | UIDeviceBatteryState 枚举值 | 说明 |
    | UIDeviceBatteryStateUnknown | 无法确定设备的电池状态。 |
    | UIDeviceBatteryStateUnplugged | 设备未插入电源，电池正在放电。 |
    | UIDeviceBatteryStateCharging | 设备已接通电源，电池电量低于100％ |
    | UIDeviceBatteryStateFull | 设备已接通电源，电池100％充电。 |