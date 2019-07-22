# 键值观察（KVO）

键值观察（Key-Value Observing，简称 KVO，也称为「键值监听」）是 Objective-C 对 观察者模式（Observer Pattern）的实现。键值观察是一种机制，允许对象通知其他对象的指定属性的更改，其同样也不是 Objective-C 的特性，而是 Cocoa 的特性。当被观察对象的某个属性的值发生更改时，观察者对象会获得通知。KVO 机制由 NSKeyValueObserving 协议提供支持。NSObject 类遵守该协议，因此所有 NSObject 的子类都可以使用该协议中的方法。

KVO 和 NSNotificationCenter 都是 Cocoa 中观察者模式的一种实现。这两者区别在于，相对于被观察者和观察者之间的关系，KVO 是一对一的，没有中心对象为所有观察者提供更改通知，而是在进行更改时将通知直接发送到观察对象。NSNotificationCenter 则是一对多的。

在 KVO 中，主要有两个对象需要搞清楚，一个是「观察者」对象，另一个是「被观察者」对象。被观察者对象就是需要改变属性值的对象，也就是具有键路径表示的属性的对象；观察者对象就是需要接收通知的对象，其中「观察者」对象和「被观察者」对象可以是同一个对象。

KVO 的编程步骤如下：

1. 被观察者对象调用 `addObserver:forKeyPath:options:context:` 方法注册观察者对象，观察者对象可以接收 被观察者对象的键路径指定的属性的变化事件。
2. 在观察者对象中重写 `observeValueForKeyPath:ofObject:change:context:` 方法，当被观察者对象的键路径指定的属性发生改变后，KVO 会回调这个方法来通知观察者对象。
3. 当观察者对象不再需要时，被观察者对象调用 `removeObserver:forKeyPath:` 或 `removeObserver:forKeyPath:context:` 方法将观察者对象移除。

> ⚠️ **注意**
> 
> KVO 是当被观察者的属性的值发生改变时才会回调观察者的方法，如果只是成员变量的值发生修改的话并不会回调观察者的方法。

## 手动 KVO

⌛️

## NSKeyValueObserving 协议

NSKeyValueObserving 协议是一种由对象采用的非正式协议，用于通知其他对象指定属性的更改。可以指定观察任何对象属性，包括简单属性，一对一关系和多对多关系。

### 注册和移除观察者

- **addObserver:forKeyPath:options:context:** 方法

    注册观察者对象，用于观察指定的键路径（谁调用这个方法就观察谁）。
    
    - 参数一：NSObject 类型，注册 KVO 观察者的对象。观察者必须重写 `observeValueForKeyPath:ofObject:change:context:` 方法。
    - 参数二：NSSting 类型，观察的键路径，不能为 `nil`。
    - 参数三：NSKeyValueObservingOptions 类型，用于指定观察通知字典中包含的内容。
    - 参数四：在 `observeValueForKeyPath:ofObject:change:context:` 方法的参数四中传递给观察者的任意数据。

    接收此消息的对象和观察者都不会被保留（即不会被强引用）。调用此方法的对象最终还必须调用 `removeObserver:forKeyPath:` 或 `removeObserver:forKeyPath:context:` 方法，以便移除观察者。
    
    NSKeyValueObservingOptions 是枚举类型，前两个枚举值用于确定在 `observeValueForKeyPath:ofObject:change:context:` 方法的参数三（字典类型）中的值内容；后两个枚举值用于确定 `observeValueForKeyPath:ofObject:change:context:` 方法的回调时机。具体枚举值如下：
    
    | NSKeyValueObservingOptions 枚举值 | 说明 |
    | :-: | :-: |
    | NSKeyValueObservingOptionNew | 指示字典应包含修改后属性值。|
    | NSKeyValueObservingOptionOld | 指示字典应包含修改前属性值。|
    | NSKeyValueObservingOptionInitial | 在注册观察者之后即接收一次回调。|
    | NSKeyValueObservingOptionPrior | 在属性更改之前和更改之后都接收一次回调。|
    
- **removeObserver:forKeyPath:** 方法

    为键路径移除指定的观察者对象。
    
    - 参数一：NSObject 类型，要移除的观察者对象。
    - 参数二：NSSting 类型，键路径。

    不能为以前没有注册为观察者的对象调用此方法。
    
    确保在 `addObserver:forKeyPath:options:context:` 方法中指定的任何对象被释放之前调用此方法（或 `removeObserver:forKeyPath:context:` 方法）。

- **removeObserver:forKeyPath:context:** 方法

    为键路径移除指定的观察者对象（相比上面的方法多了个上下文环境）。
    
    - 参数一：NSObject 类型，要移除的观察者对象。
    - 参数二：NSSting 类型，键路径。
    - 参数三：上下文。

    参数三可以传入任意类型的对象，在接收消息回调的代码中可以接收到这个对象，是 KVO 中的一种传值方式。

    通过检查上下文中的值，可以精确地确定使用哪个 `addObserver:forKeyPath:options:context:` 调用来创建观察关系。当同一个观察者多次为相同的键路径注册，但是使用不同的上下文指针时，应用程序可以明确地确定停止观察哪个对象。如果对象没有注册为观察者，则不能够调用此方法。
    
    确保在 `addObserver:forKeyPath:options:context:` 方法中指定的任何对象被释放之前调用此方法（或 `removeObserver:forKeyPath:` 方法）。

### 值变化通知

- **observeValueForKeyPath:ofObject:change:context:** 方法

    当相对于观察对象的指定键路径处的值发生更改时，回调观察对象。
    
    - 参数一：NSSting 类型，键路径。
    - 参数二：id 类型，被观察者对象。
    - 参数三：NSDictionary 类型，描述对键路径的属性值所做的更改的字典。
    - 参数四：在 `addObserver:forKeyPath:options:context:` 方法的参数四中接收自被观察者的任意数据。

    参数三是一个 NSDictionary 类型，并且它的键都是 NSKeyValueChangeKey 类型。NSKeyValueChangeKey 是一个别名，表示可以出现在字典中的键，具体包含一下几种键类型：
    
    | 键名 | 说明 |
    | :-: | :-: |
    | indexes | 此键的值是一个 NSIndexSet 对象，<br>其中包含已插入，已删除或已替换对象的索引。 |
    | king | 一个 NSNumber 对象，该值与 NSKeyValueChange <br>的枚举值之一对应的值，用于指示发生了哪种类型更改。|
    | new | 此键的值是该属性的新值。|
    | old | 此键的值是该属性的旧值。|
    | notificationIsPrior | 此键的值是包含布尔值 YES 的 NSNumber 对象。|



