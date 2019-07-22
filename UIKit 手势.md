# UIKit 手势

https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures?language=objc

手势识别器（Gesture Recognizer）是处理视图中触摸事件最简单的方法。可以在一个视图上附加一个或多个手势识别器。手势识别器封装了处理和解释该视图的传入事件所需的所有逻辑，并将它们与已知模式相匹配。检测到匹配时，手势识别器会通知其分配的目标对象，该目标对象一般是视图控制器。

手势识别器使用目标-动作（ target-action）设计模式发送消息。例如 UITapGestureRecognizer 对象在视图中检测到点击手势时，它会调用视图控制器的动作方法，示意图如下：

![用户单击触摸事件](media/15502397086771/%E7%94%A8%E6%88%B7%E5%8D%95%E5%87%BB%E8%A7%A6%E6%91%B8%E4%BA%8B%E4%BB%B6.png)

手势识别器有两种类型：**连续**和**离散（不连续）**。在识别手势后，离散手势识别器只会调用一次动作方法，而连续手势识别器会多次调用动作方法。例如，用户手指在屏幕上平移，每当触摸位置更改时，UIPanGestureRecognizer 对象都会调用其动作方法。

## UIGestureRecognizer

UIGestureRecognizer 是手势识别器的基类，继承自 NSObject 类，它是一个抽象类。在 UIKit 框架中提供了 7 个具体的手势识别类，他们都是 UIGestureRecognizer 的子类，具体子类如下表：

| UIGestureRecognizer 子类 | 说明 |
| :--: | :--: |
| UITapGestureRecognizer  | 点击手势 |
| UIPinchGestureRecognizer | 捏合手势 |
| UIRotationGestureRecognizer | 旋转手势 |
| UISwipeGestureRecognizer | 滑动（轻扫）手势 |
| UIPanGestureRecognizer | 平移手势 |
| UIScreenEdgePanGestureRecognizer | 屏幕边缘平移手势 |
| UILongPressGestureRecognizer | 长按手势 |

UIGestureRecognizer 类定义了一组可以为所有具体手势识别器配置的常见行为。它还可以与其代理对象（采用UIGestureRecognizerDelegate 协议的对象）进行通信，从而实现对某些行为的更详细的自定义。

手势识别器会识别对指定视图进行的触摸操作，因此手势识别器必须与该视图相关联，调用 UIView 的 `addGestureRecognizer:` 方法将手势识别器和视图建立关联。但是在触摸事件处理流程中，手势识别器处于观察者的角色，其不是视图层级结构的一部分，所以也不参与视图的响应链。

> ⚠️ **注意**
>
> 手势识别器不仅识别与其关联的视图的触摸操作，也会识别该视图的所有子视图的触摸操作。

### 手势识别器的状态

手势识别器的 `state` 属性用于传达对象当前的识别状态。手势识别器的状态由 UIGestureRecognizerState 类型的枚举常量表示。其中一些状态仅适用于连续手势，具体枚举值如下：

| UIGestureRecognizerState 枚举值 | 说明 |
| :--: | :-- |
| UIGestureRecognizerStatePossible | 手势识别器尚未识别其手势，可能正在评估触摸事件。<br>此状态时不发送其动作消息。 |
| UIGestureRecognizerStateBegan | 手势识别器接收被识别为连续手势的触摸。<br>它在运行循环的下一个周期中发送其动作消息。 |
| UIGestureRecognizerStateChanged | 手势识别器接收被识别为改变连续手势的触摸。<br>它在运行循环的下一个周期中发送它的动作消息。 |
| UIGestureRecognizerStateEnded | 手势识别器接收到被识别为结束连续手势的触摸。<br>它在运行循环的下一个周期中发送它的动作消息，<br>并将其状态重置为 UIGestureRecognizerStatePossible。 |
| UIGestureRecognizerStateCancelled | 手势识别器已经接收到导致连续手势取消的触摸。<br>它在运行循环的下一个周期发送它的动作消息，<br>并将其状态重置为 UIGestureRecognizerStatePossible。 |
| UIGestureRecognizerStateFailed | 手势识别器接收到无法识别的多点触摸序列。<br>此状态时不发送其动作消息，<br>但将其状态重置为 UIGestureRecognizerStatePossible。 |
| UIGestureRecognizerStateRecognized | 等于 UIGestureRecognizerStateEnded。 |

- 离散手势识别器的状态顺序：

    `Possible` ➤ `Ended` 
    
    或
    
    `Possible` ➤ `Failed` 

    不连续手势不会通过 `Began` 和 `Changed` 状态进行转换，也不会有 `Cancelled` 状态。
  
- 连续手势识别器的状态顺序：

    `Possible` ➤ `Began` ➤ `Changed`(可选的) ➤ `Ended` 或 `Cancelled` 或 `Failed`
  
    如果手势识别器收到取消触摸，则转换为 `Cancelled` 状态。如果连续手势识别器无法将多点触摸顺序解释为其手势，则它们将转换为 `Failed` 状态。

### 创建和移除手势识别器的方法

识别手势使用的是 UIGestureRecognizer 的子类，但是创建子类都是使用父类的初始化方法。

- **initWithTarget:action:** 方法

    使用目标-动作选择器初始化手势识别器对象。

    - 参数一：id 类型对象，它是接收器识别手势时发送的动作消息的接收者。不能为 `nil`。
    - 参数二：一个选择器，用于标识目标实现的方法，以处理接收方识别的手势。 不能为 `NULL`。
    - 返回值：初始化后的手势识别器对象。

    这个方法能够初始化手势识别器并且为其添加一个目标-动作选择器。在初始化之后，还可以调用 `addTarget:action:` 方法将其他目标-动作选择器与手势识别器对象关联起来。

    > ⭐️ **提醒**
    >
    > 一个手势识别器对象可以关联多个目标动作。

- **addTarget:action:** 方法

    将目标-动作选择器添加到手势识别器对象。

    - 参数一：id 类型对象，它是接收器识别手势时发送的动作消息的接收者。不能为 `nil`。
    - 参数二：一个选择器，用于标识目标实现的方法，以处理接收方识别的手势。 不能为 `NULL`。

- **removeTarget:action:** 方法

    从手势识别器对象中删除目标-动作选择器。

    - 参数一：id 类型对象，它是接收器识别手势时发送的动作消息的接收者。指定为 `nil` 时删除所有目标。
    - 参数二：一个选择器，用于标识目标实现的方法，以处理接收方识别的手势。 指定为 `NULL` 时删除所有动作。

### 手势识别器的属性和方法

- **delegate** 属性

  手势识别器的代理。遵守 UIGestureRecognizerDelegate 代理协议。

**获得识别器的状态和视图**

- **state** 属性

    UIGestureRecognizerState 类型，手势识别器的当前状态。

- **view** 属性

  UIView 类型，只读。手势识别器附加的视图对象。

- **enabled** 属性

  BOOL 类型，指示是否启用手势识别器。默认值为 `YES`。

  如果在手势识别器当前正在识别手势时将此属性更改为 `NO`，则手势识别器将转换为已取消状态。
  
**获得手势的触摸和位置**

- **numberOfTouches** 属性

    NSUInteger 类型，只读。返回手势识别器识别的手势中涉及的触摸点数量。
    
    此属性的值返回的是 UITouch 的对象数，即当前手势中的触摸对象的数量（一个手指触摸在屏幕上即是一个  UITouch 对象）。

- **locationInView:** 方法

    返回触摸在指定视图坐标系中的位置。

    - 参数一：UIView 类型，发生手势的 UIView 对象。指定为 `nil` 则表示窗口。
    - 返回值：CGPoint 类型，用于标识触摸位置的点。 
    
    位置点的坐标系是以参数一视图的左上角为坐标原点的坐标系。如果把参数一设置为 `nil`，则位置点的坐标系是窗口坐标系。对于连续手势，这个点的位置会跟随手指不断的变化，甚至手指已经移动出了手势所附加的视图，依旧会返回该坐标点。例如：手指从视图中平移到视图外，整个过程中都会返回触摸对象在视图坐标系中的位置点，直到手指抬起。

- **locationOfTouch:inView:** 方法

    返回指定触摸对象在指定视图坐标系中的位置。
    
    - 参数一：NSUInteger 类型，手势识别器维护的专用数组中 UITouch 对象的索引。
    - 参数二：UIView 类型，发生手势的 UIView 对象。指定为 `nil` 则表示窗口。
    - 返回值：CGPoint 类型，用于标识触摸位置的点。

    详细参考 `locationInView:` 方法。

**延迟触摸和取消触摸**

- **cancelsTouchesInView** 属性

    BOOL 类型，用于影响在识别手势时是否将触摸传递到视图，默认值为 `YES`。
    
    当此属性的值为 `YES` 并且接收者识别它的手势时，等待的手势的触摸不会被传递到视图，之前传递的触摸会通过`touchescancel:withevent:message` 发送到视图来取消。如果一个手势识别器不识别它的手势，或者如果这个属性的值为 `NO`，视图将接收多点触摸序列中的所有触摸。
    
- **delaysTouchesBegan** 属性

    BOOL 类型，用于确定手势识别器是否延迟将开始阶段中的触摸发送到其视图，默认值为 `NO`。
    
    当此属性的值为 `NO` 时，视图将与手势识别器并行分析 UITouchPhaseBegan 和 UITouchPhaseMoved 中的触摸事件。当属性的值为 `YES` 时，窗口会暂停 UITouchPhaseBegan 阶段中触摸对象到附加视图传递。如果手势识别器随后识别其手势，这些触摸对象将被取消。但是，如果手势识别器无法识别其手势，则窗口会以 `touchesBegan:withEvent:` 消息（以及后续可能的 `touchesMoved:withEvent:` 消息将这些触摸对象传递给视图，以通知其触摸的当前位置）。
    
    将此属性设置为 `YES`，以防止视图在 UITouchPhaseBegan 阶段处理任何可能被识别为此手势的触摸。
    
- **delaysTouchesEnded** 属性

    BOOL 类型，用于确定手势识别器是否延迟将结束阶段中的触摸发送到其视图，默认值为 `YES`。
    
    当此属性的值为 `YES` 并且接收器正在分析触摸事件时，窗口会暂停 UITouchPhaseEnded 阶段中触摸对象到附加视图的传递。如果手势识别器随后识别其手势，则这些触摸对象将被取消（通过 `touchesCancelled:withEvent:` 消息）。如果手势识别器无法识别其手势，则窗口在调用视图的 `touchesEnded:withEvent:` 方法时传递这些对象。将此属性设置为 `NO` 可在手势识别器分析相同触摸时将 UITouchPhaseEnded 中的触摸对象传递到视图。

**识别不同手势**

- **requiresExclusiveTouchType** 属性

    BOOL 类型，指示手势识别器是否同时考虑不同类型的触摸。默认值为 `YES`。

    **注意，是否同时只接受一种触摸类型，而不是是否同时只接受一种手势**。当此属性的值为`YES`时，手势识别器会自动忽略其类型与初始触摸类型不匹配的新触摸。当值为 `NO` 时，手势识别器接收在 `allowedTouchTypes` 属性中列出的所有触摸类型。

- **allowedTouchTypes** 属性

    NSArray 类型，表示可以识别的触摸类型。默认值包含所有触摸类型。
  
    触摸类型是 UITouchType 枚举，如下表：

  | UITouchType 枚举值 | 说明 | 
  | :-: | :-: |
  | UITouchTypeDirect | 直接接触屏幕产生的触摸 |
  | UITouchTypeIndirect | 触摸不是由于与屏幕接触而产生的。<br>例如，Apple TV 遥控器的触控板产生间接触摸 |
  | UITouchTypePencil | Apple Pencil 设备屏幕交互时产生的触摸 |

**指定手势识别器之间的优先级**

- **requireGestureRecognizerToFail:** 方法

    在创建对象时，在手势识别器和另一个手势识别器之间创建依赖关系（即设置两个手势识别器的优先级）。

    - 参数一：UIGestureRecognizer 类型，另一个手势识别器对象。

    当手势识别器对象 A 作为接收者，手势识别器对象 B 作为参数一，并且两个手势同时满足响应手势方法的条件时，B 优先响应，A 不响应。如果 B 不满足条件，A 满足响应手势方法的条件，则 A 响应。例如：单击和双击手势并存时，如果不做处理，那么就只能触发单击手势的事件。此时就可以用这个方法来设置优先响应双击手势。

    此方法的接收者与另一个手势识别器建立关系，延迟接收者从 `Possible` 状态转换。接收者转换到的状态取决于参数一传递的对象发生的情况：
  
    - 如果参数一对象转换为 `Failed`，则接收者转换到其正常的下一个状态。
    - 如果参数一对象转换为 `Recognized` 或 `Began`，则接收者将转换为 `Failed`。

## UIGestureRecognizerDelegate

由手势识别器的代理实现的一组方法，用于调整应用程序的手势识别行为，下面方法都是选择实现的方法。

- **gestureRecognizerShouldBegin:** 方法

  询问代理手势识别器是否应该开始处理触摸事件。

  - 参数一：UIGestureRecognizer 类型，将消息发送给代理的手势识别器对象。
  
       该对象即将开始处理触摸事件以确定其手势是否正在发生。
 
  - 返回值：BOOL 类型，默认返回 `YES`，表示手势识别器继续处理触摸事件。
  
       返回 `NO` 则停止处理触摸事件，表示不识别该类型的手势。

  当手势识别器尝试转换为 UIGestureRecognizerStatePossible 状态时（即开始手势识别时），将调用此方法。返回 `NO` 会使手势识别器转换到 UIGestureRecognizerStateFailed 状态。

- **gestureRecognizer:shouldRecognizeSimultaneouslyWithGestureRecognizer:** 方法

  询问代理是否应允许两个手势识别器同时识别手势。

  - 参数一：UIGestureRecognizer 类型，将消息发送给代理的手势识别器对象。
  - 参数二：UIGestureRecognizer 类型，手势识别器对象。
  - 返回值：BOOL 类型，默认返回 `NO`，表示不能同时识别两个手势。
        
       返回 `YES` 表示允许同时识别参数一和参数二的手势。
 
  当参数一和参数二识别手势会阻止其他手势识别器识别其手势时，调用此方法。 
  
  > ⚠️ **注意**
  > 
  > 返回 `YES` 可以保证允许同时识别两个手势，但是返回 `NO` 不能保证防止同时识别，因为其他手势识别器的代理可能返回 `YES`。

- **gestureRecognizer:shouldRequireFailureOfGestureRecognizer:** 方法

  询问代理手势识别器需要是否另一个手势识别器失败。

  - 参数一：UIGestureRecognizer 类型，将消息发送给代理的手势识别器对象。
  - 参数二：UIGestureRecognizer 类型，手势识别器对象。
  - 返回值：BOOL 类型，默认返回 `NO`，表示参数一不需要参数二识别失败。

    返回 `YES` 表示参数一需要参数二识别失败。

- **gestureRecognizer:shouldBeRequiredToFailByGestureRecognizer:** 方法

  询问代理是否应该要求手势识别器通过另一个手势识别器失败。
  
## 点击手势（UITapGestureRecognizer）

点击手势是指在屏幕上简短的点击，可以实现类似按钮式的交互。点击手势能检测一个或多个手指短暂触摸屏幕，并且可以配置手指必须触摸屏幕的次数，例如配置点击手势识别器检测单击，双击或三击。但是这些手指不能从初始触摸点**显着移动**。

UITapGestureRecognizer 类用于识别点击手势。点击手势是不连续手势，并且对于手势识别器的每个状态也是不连续的。因此，相关联的动作消息在手势开始状态时发送，针对每个中间状态也会发送，并且手势的结束状态也会发送。 因此，处理点击手势的代码应该判断手势的状态，例如下面代码：

```Objective-C
- (void)handleTap:(UITapGestureRecognizer *)sender {
    if (sender.state == UIGestureRecognizerStateEnded) {
        // 处理代码
    }
}
```

> 🔥 **提示**
> 
> 在离散手势识别器中，`Possible` 状态和 `Failed` 状态是不会调用动作方法的，所以无需将其写到判断中。

处理此手势的动作方法可以通过调用 UIGestureRecognizer 类的 `locationInView:` 方法来获取手势的整体位置，如果有多次点击，那么这个位置会是第一次点击的位置；如果有多个手指触摸，则此位置是所有手指触摸视图的中心。可以通过调用 `locationOfTouch:inView:` 方法来获取特定触摸的位置。如果允许多次点击，则此位置是第一次点击的位置。

- **numberOfTapsRequired** 属性

  NSUInteger 类型，点击手势的点击次数。默认值为 `1`。
  
- **numberOfTouchesRequired** 属性

  NSUInteger 类型，点击手势的手指数量。默认为 `1`。

## 平移手势和屏幕边缘平移手势

平移手势即手指在屏幕上移动，并且手指不离开屏幕。屏幕边缘平移手势是特殊的平移手势，它要求必须从屏幕边缘开始移动。使用 UIPanGestureRecognizer 类处理平移手势，使用 UIScreenEdgePanGestureRecognizer 类处理屏幕边缘平移手势。

平移手势识别器用于需要跟踪用户手指在屏幕上移动的任务。可以使用平移手势识别器在界面中拖动对象或根据用户手指的位置更新其外观。平移手势是连续手势，因此只要触摸信息发生变化，就会调用动作方法。

一旦达到所需的初始移动量，平移手势识别器就进入开始状态。在初始更改之后，后续更改会导致手势识别器进入变化状态。 当用户的手指从屏幕上抬起时，手势识别器进入结束状态。

可以使用平移手势识别器的 `translationInView:` 方法来简化跟踪以便获取用户手指从开始触摸位置到结束触摸位置移动的距离。在手势开始时，平移手势识别器会存储用户手指的开始接触点（如果手势涉及多个手指，则手势识别器使用该组触摸的中心点）。每次手指移动时，`translationInView:` 方法报告此时距开始位置的距离。

### UIPanGestureRecognizer

UIPanGestureRecognizer 用于平移（拖动）手势。用户必须在视图上按下并移动一个或多个手指。 实现该手势识别器的动作方法可以获取其当前平移手势的距离和速度。

平移手势是连续的。当允许的最小手指数（`minimumNumberOfTouches`）移动到足以被视为平移时，手势进入 `Began` 状态。当手指继续移动时，它处于 `Changed` 状态。当所有手指都被抬起时，它处于 `Ended` 状态。一般使用下面代码判断所处的状态并执行相关操作：

```Objective-C
- (void)handlePan:(UIPanGestureRecognizer *)sender {
    if (sender.state == UIGestureRecognizerStateBegan) {
        // 处理代码
    } else if (sender.state == UIGestureRecognizerStateChanged) {
        // 处理代码
    } else  if (sender.state == UIGestureRecognizerStateEnded) {
        // 处理代码
    }
}
```

下面是 UIPanGestureRecognizer 的方法和属性：

- **maximumNumberOfTouches** 属性

    NSUInteger 类型，平移手势需要手指的最大数量。默认值为 `NSUIntegerMax`。

- **minimumNumberOfTouches** 属性

    NSUInteger 类型，平移手势需要手指的最小数量。默认值为 `1`。

- **translationInView:** 方法

    获取平移手势在指定视图的坐标系中的平移位移。

    - 参数一：UIView 类型，计算平移距离参考的视图。
    - 返回值：CGPoint 类型，参数一视图的父视图坐标系中该视图的新位置的点。

    返回值是一个 CGPoint 类型，表示在 X 方向和 Y 方向上平移的距离。
    
- **⌛️setTranslation:inView:** 方法

    设置指定视图坐标系中的平移值。
    
    - 参数一：CGPoint 类型，参数二视图的父视图坐标系中该视图的新位置的点
    - 参数二：UIView 类型，计算平移距离参考的视图。

    更改平移值会重置平移的速度。

- **velocityInView:** 方法

    获取平移手势在指定视图的坐标系中的速度。

    - 参数一：UIView 类型，计算平移速度参考的视图。
    - 返回值：CGPoint 类型，平移手势的速度，以 `pt/s` 表示。速度分为水平和垂直分量。
    
    返回值是一个 CGPoint 类型，表示速度在 X 方向和 Y 方向上的分量。

### UIScreenEdgePanGestureRecognizer

UIScreenEdgePanGestureRecognizer 用于在屏幕边缘附近开始的平移（拖动）手势。它继承自 UIPanGestureRecognizer 类。

创建屏幕边缘平移手势识别器后，在将手势识别器附加到视图之前需要为 edge 属性指定适当的值。可以使用此属性指定手势可以从哪些边缘开始。此手势识别器忽略第一次触摸后的任何触摸。

- **edges** 属性

  UIRectEdge 类型。手势的可接受起始边缘。

  指定的边缘是始终相对于应用程序的当前界面方向。这样可确保手势始终从用户界面中的相同位置发生，而不管设备的当前方向如何。

- **UIRectEdge** 枚举类型

  UIRectEdge 用于指定矩形边的常量。具体枚举值如下表：

  | UIRectEdge 枚举值 | 说明 |
  | :--: | :--: |
  | UIRectEdgeNone | 没有边缘 |
  | UIRectEdgeTop | 矩形的上边缘 |
  | UIRectEdgeLeft | 矩形的左边缘 |
  | UIRectEdgeBottom | 矩形的下边缘 |
  | UIRectEdgeRight | 矩形的右边缘 |
  | UIRectEdgeAll | 矩形的所有边缘 |

## 长按手势（UILongPressGestureRecognizer）

长按手势可以检测一个或多个手指(或一支手写笔)长时间触摸屏幕。可以配置识别按压所需的最小持续时间以及手指必须接触屏幕的次数（长按手势识别器仅由触摸的持续时间触发，而不是手指触摸的力度）。在按下时，手指不能移动超过指定的距离，如果手指移动超出指定距离，则识别手势失败。

![长按手势](media/15502397086771/%E9%95%BF%E6%8C%89%E6%89%8B%E5%8A%BF.png)

长按手势是连续手势，所以指定的操作方法会被调用多次。当指定时间段（`minimumPressDuration`）按下所需的手指数（`numberOfTouchesRequired`）并且触摸不超出允许的移动范围（`allowableMovement`）时，手势进入 `Began` 状态；当手指移动或触摸发生任何其他变化时，手势识别器转换到 `Changed` 状态，只要手指一直保持按下，即使手指移动超出允许的移动的范围，依旧保持在 `Changed` 状态（并且在移动时会不断调用动作方法）；当任何手指被抬起时，转换为 `Ended` 状态。

> ⚠️ **注意**
> 
> `allowableMovement` 属性指定的是还未进入 `Began` 状态时的移动范围。如果手势识别器已经处于 `Changed` 状态，只要保持在长按状态，无论手指怎么移动，手势识别器都会一直保持在  `Changed` 状态，并且在移动中不断调用动作方法。

一般使用下面代码判断所处的状态并执行相关操作：

```Objective-C
- (void)handleLongPress:(UILongPressGestureRecognizer *)sender {
    if (sender.state == UIGestureRecognizerStateBegan) {
        // 处理代码
    } else if (sender.state == UIGestureRecognizerStateChanged) {
        // 处理代码
    } else  if (sender.state == UIGestureRecognizerStateEnded) {
        // 处理代码
    }
}
```

下面是 UILongPressGestureRecognizer 的方法和属性：

- **minimumPressDuration** 属性

    NSTimeInterval 类型，识别长按手势所需的最短按压时间，默认值为 `0.5`，单位秒。

- **numberOfTouchesRequired** 属性

    NSUInteger 类型，识别长按手势所需的手指数，默认值为 `1`。
    
- **numberOfTapsRequired** 属性

    NSUInteger 类型，识别长按手势所需的在视图上的点击数，默认值为 `0`。
    
- **allowableMovement** 属性

    CGFloat 类型，识别长按手势允许的在视图上的最大移动距离，默认值为 `10`。
    
## 滑动（轻扫）手势（UISwipeGestureRecognizer）

滑动是一种离散的手势，因此每个手势只发送一次相关的动作消息。当所需的手指数（`numberOfTouchesRequired`）大部分在允许的方向（`direction`）上移动到足以被视为滑动的距离时，UISwipeGestureRecognizer 识别滑动。

滑动可以慢也可以快。用户在慢速滑动时需要较高的方向精度，但距离较小；在快速滑动需要较低的方向精度，但需要较大的距离。

可以通过调用 UIGestureRecognizer 的 `locationInView:` 和 `locationOfTouch:inView:` 方法来确定滑动开始的位置。如果手势中包含多个触摸点，前一种方法会给出质心；后者会给出特定触摸的位置。

可以使用下面代码判断滑动手势的方向并执行相关操作：

```Objective-C
- (void)handleSwipe:(UISwipeGestureRecognizer *)sender {
    if (sender.direction == UISwipeGestureRecognizerDirectionLeft) {
        // 向左轻扫时的处理代码 
    } else if (sender.direction == UISwipeGestureRecognizerDirectionRight) {
        // 向右轻扫时的处理代码 
    } else if (sender.direction == UISwipeGestureRecognizerDirectionUp) {
        // 向上轻扫时的处理代码 
    } else {
        // 向下轻扫时的处理代码 
    }
}
```

- **direction** 属性

    UISwipeGestureRecognizerDirection 类型，滑动手势允许的方向，默认方向是`UISwipeGestureRecognizerDirectionRight`。
    
    UISwipeGestureRecognizerDirection 是枚举类型，表示滑动手势的方向，有 `Left`、`Right`、`Up` 和 `Down` 四种。
    
- **numberOfTouchesRequired** 属性

    NSUInteger 类型，识别滑动手势所需的手指数，默认值为 `1`。