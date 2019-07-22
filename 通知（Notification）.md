# 通知（Notification）

> 📚 官方文档：**[Notification Programming Topics](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html#//apple_ref/doc/uid/10000043-SW1)**

提到通知，就不得不提到观察者模式：

> **观察者模式**
> 
> 一个目标对象管理所有相依于它的观察者对象（一对多），并且在它本身的状态改变时主动发出通知。观察者模式又叫做发布-订阅（Publish/Subscribe）模式。

iOS 中的观察者模式的实现涉及的几个概念：

- **通知**

    通知包含的信息封装在 NSNotification 对象中。通知中包含了一些信息，例如通知名称、可选字典等。在应用程序中，可以指定在某个时刻向通知心中发送通知，例如：按下某个按钮时。

- **观察者**

    iOS 中的观察者没有封装成为单独的类，任何对象都可以是观察者。观察者在等待通知的到来，如果收到了对应的通知，那么观察者就能够执行相关的操作。观察者收到的通知来自于通知中心，前提是观察者必须在通知中心「注册」。

- **通知中心**

    一个通知中心可以接收多个通知，当收到一个通知时，通知中心会向每一个已注册的观察者分发这个通知，但是只有与这个通知对应的观察者才能够执行对应的操作。

- **通知队列**

    通知通常是立刻发布到通知中心的，不过可以让通知在通知队列中排队，通知队列在指定的延迟之后再将通知发送到通知中心，并可以根据某些指定条件合并类似的通知。
    
    在通知中心分发通知前，将通知放置到通知队列中，可以被延迟到当前运行循环结束或直到运行循环空闲才发布通知。重复的通知可以合并在一起，这样即使发布了多个通知，最终也只会发布一个通知。通知队列按照队列的先进先出（FIFO）顺序发布，当通知移动到队列的最前面时，队列将其发布到通知中心，通知中心再将通知分发给所有已注册为观察者的对象。

    通知队列作为通知中心的缓冲区，用于通知的合并和异步发布。每个线程都有一个默认通知队列，该队列与进程的默认通知中心相关联。可以创建自己的通知队列，也可以为每个中心和线程创建多个队列。

在对象之间传递信息的标准方法是消息传递——即一个对象调用另一个对象的方法。然而，消息传递要求消息的发送者知道接收者是谁以及它响应什么消息。但不是任何情况下消息的发送者都知道接收者，并且考虑到在不同的模块之间降低耦合性，所以引入广播模型：一个对象发布一个通知给通知中心，通知中心将该通知分发给所有观察者。

![通知发布过程示意图](media/15528760368056/%E9%80%9A%E7%9F%A5%E5%8F%91%E5%B8%83%E8%BF%87%E7%A8%8B%E7%A4%BA%E6%84%8F%E5%9B%BE.png)

> 🔥 **提示**
> 
> 一个观察者不仅仅能够观察一个通知，而是能够观察多个通知。一个发布者也可以发布多个通知。

任何对象都可以发布通知，其他对象都可以作为观察者在通知中心注册，以便在通知中心发布通知时接收通知。发送通知的对象，通知中包括的对象以及通知的观察者可以全部是不同的对象或相同的对象。发送通知的对象无需了解有关观察者的任何信息。不过，观察者至少需要知道通知的名称，才能够在收到对应的通知时执行相关操作。

> ⚠️ **注意**
>
> 在多线程应用程序中，通知始终在发布通知的线程中传递，这可能与观察者在通知中心注册的线程不同。

## NSNotification

NSNotification 继承自 NSObject，它是一个容器，其保存用于通知的信息。它通过通知中心向所有注册的观察者发布通知。NSNotification 对象是不可变的，一个 NSNotification 对象就是一个通知，它包含以下三部分：

- 名称（name）：通知的名称。
- 对象（object）：想要发送给观察者的对象，一般常使用 `self`，即发布通知的对象自身。
- 可选字典（userInfo）：存储其他相关对象的字典。

通常不直接创建通知对象，而是调用 NSNotificationCenter 的 `postNotificationName:object:` 和 `postNotificationName:object:userInfo:` 方法来创建通知对象。

> 🔥 **提示**
> 
> 通知的名字通常定义为字符串常量，和其他字符串常量一样，应该在公共接口中使用 `UIKIT_EXTERN` 声明，在实现中进行私有定义。类似的，userInfo 字典里的键值也应该定义成字符串常量，应该在文档中清晰地注明哪个键对应哪种类型的值。

示例代码：

```Objective-C
// .h 文件
// 通知
UIKIT_EXTERN NSString *const XXNotification;
// userInfo 键名
UIKIT_EXTERN NSString *const XXKey1;        // 值类型：NSNumber
UIKIT_EXTERN NSString *const XXKey2;        // 值类型：NSString
UIKIT_EXTERN NSString *const XXKey3;        // 值类型：NSData

// .m 文件
NSString * const XXNotification = @"XXNotification";
UIKIT_EXTERN NSString *const XXKey1 = @"XXKey1"       
UIKIT_EXTERN NSString *const XXKey2 = @"XXKey2";       
UIKIT_EXTERN NSString *const XXKey3 = @"XXKey3";
```

**属性和方法**

- **notificationWithName:object:** 类方法

    返回具有指定名称和对象的通知对象。
    
    - 参数一：NSNotificationName 类型，通知的名称，不能为 `nil`。
    - 参数二：id 类型，传递给观察者的对象。可以为 `nil`。

    在 Objective-C 中，NSNotificationName 是 NSString 类的别名；在 Swift 中，通知名称使用嵌套的 NSNotificationName 类型。

- **notificationWithName:object:userInfo:** 类方法

    返回具有指定名称，对象和字典的通知对象。
    
    - 参数一：NSNotificationName 类型，通知的名称，不能为 `nil`。
    - 参数二：id 类型，传递给观察者的对象，可以为 `nil`。
    - 参数三：NSDictionary 类型，包含信息的字典，可以为 `nil`。

- **name** 属性

    NSNotificationName 类型，只读。通知的名称。
    
- **object** 属性

    id 类型，只读。与通知关联的对象。
    
- **userInfo** 属性

    NSDictionary 类型，只读。与通知关联的用户信息字典。

## NSNotificationCenter

NSNotificationCenter 继承自 NSObject，称为「通知中心」，通知中心采用分发机制，将通知分派给每一个已注册的观察者。并且它还可以发布通知、添加和移除观察者。

通知中心会维护分发表，当通知发布到通知中心时，通知中心会扫描分发表，并向表中的每一个观察者发送通知。

每个正在运行的应用程序都有一个默认通知中心（defaultCenter），不过也可以创建新的通知中心。通知中心只能在一个程序中发布通知（如果要将通知发布到其他进程或从其他进程接收通知，需要使用 NSDistributedNotificationCenter，仅支持 macOS 系统）。

- **defaultCenter** 类属性

    NSNotificationCenter 类型，只读。应用程序的默认通知中心。
    
    发布到应用程序的所有系统通知都会发布到此实例对象。
    
    如果应用程序使用通知较为频繁，那么可能需要创建并发布通知到自定义的通知中心，而不仅仅是发布通知到默认通知中心。因为当通知发布到通知中心时，通知中心会扫描已注册的观察者列表，这可能会降低应用程序的运行速度。通过在一个或多个通知中心发布通知来减少工作量，从而提高整个应用程序的性能。
    
**添加和删​移除观察者**

- ⌛️**addObserverForName:object:queue:usingBlock:** 方法

    向通知中心的分发表添加一个条目，该条目包括通知队列和要添加到队列的块，以及可选的通知名称和对象。
    
    - 参数一：NSNotificationName 类型，观察者的名称。只有和名称相同名称的通知才能将块添加到操作队列。如果传递 `nil`，则通知中心不会使用通知的名称来决定是否将块添加到操作队列。
    - 参数二：id 类型，观察者想要接收的对象（该对象发布通知），只有和此对象相同的发送者发送的通知才会传递给观察者，如果传递 `nil`，通知中心不会使用通知的发送者来决定是否将其传递给观察者。
    - 参数三：NSOperationQueue 类型，添加块的操作队列。如果传递 `nil`，则块在发布线程上同步运行。
    - 参数四：代码块，收到通知时要执行的块。该块由通知中心复制并保存（副本），直到删除观察者注册为止。该块有一个参数：
        - 参数一：NSNotification 类型，通知。
    - 返回值：id 类型，匿名的观察者对象。

- **addObserver:selector:name:object:**

    向通知中心的分发表添加一个条目，其中包含一个观察者和一个通知选择器，以及一个可选的通知名称和对象。

    - 参数一：id 类型，观察者对象。
    - 参数二：选择器，指定通知中心发送通知给对应观察者时调用的方法，方法必须只有一个参数（即 NSNotification 对象）。
    - 参数三：NSNotificationName 类型，观察者的名称。如果传递 `nil`，通知中心不会使用通知的名称来决定是否将其传递给观察者。
    - 参数四：id 类型，传递给观察者的对象。如果传递 `nil`，则通知中心不会使用通知的发布者来决定是否将其传递给观察者。

    如果应用程序在 iOS 9.0 及更高版本，或者 macOS 10.11 及更高版本，不需要在它的 `dealloc` 方法中注销观察者。否则，应该在观察者或传递给这个方法的任何对象被释放之前调用 `removeObserver:name:object` 方法。

- **removeObserver:name:object:** 方法

    从通知中心的分发表中删除匹配的条目。
    
    - 参数一：id 类型，观察者对象。
    - 参数二：NSNotificationName 类型，要从分派表中删除的通知的名称。如果为 `nil`，则通知中心不使用通知名称作为删除的条件。
    - 参数三：id 类型，传递给观察者的对象。如果传递 `nil`，则通知中心不使用通知传递的对象作为删除条件。

    如果应用程序在 iOS 9.0 及更高版本，或者 macOS 10.11 及更高版本，不需要在它的 `dealloc` 方法中注销观察者。否则，应该在观察者或 `addObserverForName:object:queue:usingBlock:` 或 `addObserver:selector:name:object:` 方法中解除分配中指定的任何对象之前调用此方法或 `removeObserver:` 方法。

- **removeObserver:** 方法

    从通知中心的分发表中删除指定观察者的所有条目。
    
    - id 类型，观察者对象。

**发布通知**

- **postNotification:** 方法

    将通知发布到通知中心。
    
    - 参数一：NSNotification 类型，通知。

- **postNotificationName:object:userInfo:** 方法

    创建具有指定名称，对象和可选信息的通知，并将其发布到通知中心。
    
    - 参数一：NSNotificationName 类型，通知的名称。
    - 参数二：id 类型，传递给观察者的对象。
    - 参数三：NSDictionary 类型，通知的可选信息。

- **postNotificationName:object:** 方法

    创建具有指定名称和对象的通知，并将其发布到通知中心。
    
    - 参数一：NSNotificationName，通知的名称。
    - 参数二：id 类型，传递给观察者的对象。

    此方法等同于调用 `postNotificationName:object:userInfo:` 方法并将 `nil` 传递给参数三。
        
## ⌛️NSNotificationQueue