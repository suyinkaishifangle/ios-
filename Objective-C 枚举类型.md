# Objective-C 枚举类型

Objective-C 支持 C 的 `emun` 关键字定义枚举类型，不过在 iOS 6 之后使用 `NS_ENUM` 和 `NS_OPTIONS` 宏定义代替 `enum`。

它们的定义如下：

```Objective-C
#define NS_ENUM(...) CF_ENUM(__VA_ARGS__)
#define NS_OPTIONS(_type, _name) CF_OPTIONS(_type, _name)
```

## NS_ENUM 

NS_ENUM 支持使用一个或两个参数。 
  
- 第一个参数是用于枚举值的整数类型。
- 第二个参数是枚举的可选类型名称。 
  
指定类型名称时，必须在宏之前加上 `typedef`，如下所示：

```Objective-C
typedef NS_ENUM(NSInteger, NSComparisonResult) {
    ...
};
```

如果不使用第二个参数时，即不为其指定类型名称，则不必加上 `typedef`，如下所示：

```Objective-C
NS_ENUM(NSInteger) {
    ...
};
```

使用 NS_ENUM 宏定义的枚举类型中每个枚举值对应一个可以指定的整型常量，第一个枚举值默认对应 `0`，之后的枚举值如果没有指定整型值常量，那么依次在第一个枚举值的整型值基础上加 1。在使用时，这个枚举中的枚举值只能使用一个。

例如：UIButton 只能指定为 UIButtonType 枚举常量中的一种。

```Objective-C
typedef NS_ENUM(NSInteger, UIButtonType) {
   UIButtonTypeCustom = 0,  
   UIButtonTypeSystem, 
   UIButtonTypeDetailDisclosure,
   UIButtonTypeInfoLight,
   UIButtonTypeInfoDark,
   UIButtonTypeContactAdd,
   UIButtonTypePlain,
   UIButtonTypeRoundedRect = UIButtonTypeSystem
};
```
  
## NS_OPTIONS

NS_OPTIONS 必须使用两个参数。

- 第一个参数是用于枚举值的整数类型。
- 第二个参数是枚举的类型名称。

指定类型名称时，必须在宏之前加上 `typedef`，如下所示：

```Objective-C
typedef NS_OPTIONS(NSInteger, NSComparisonResult) {
    ...
};
```

NS_OPTIONS 定义的枚举可以使用 [位掩码（bitmask）](https://en.wikipedia.org/wiki/Mask_(computing))，它可以将枚举值通过**按位或操作符 `|`** 组合在一起，即可以多选枚举值，这样的枚举也称为**位移枚举**。

例如：UIControl 的事件可以指定为 UIControlEvents 枚举常量中的多种组合。

```Objective-C
typedef NS_OPTIONS(NSUInteger, UIControlEvents) {  // 二进制      十进制
    UIControlEventTouchDown           = 1 <<  0,   // 0000 0000    0
    UIControlEventTouchDownRepeat     = 1 <<  1,   // 0000 0001    1  
    UIControlEventTouchDragInside     = 1 <<  2,   // 0000 0010    2
    UIControlEventTouchDragOutside    = 1 <<  3,   // 0000 0100    4
    UIControlEventTouchDragEnter      = 1 <<  4,   // 0000 1000    8
    UIControlEventTouchDragExit       = 1 <<  5,   // 0001 0000    16
    UIControlEventTouchUpInside       = 1 <<  6,   // 0010 0000    32
    UIControlEventTouchUpOutside      = 1 <<  7,   // 0100 0000    64
    UIControlEventTouchCancel         = 1 <<  8,   // 1000 0000    128
    ...
}
```