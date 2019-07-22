# UIControl ✅

UIControl 继承自 UIView，它是 UIButton、UISwitch 和 UITextField 等控件的基类。UIControl 是一种视觉元素，用户响应用户指定动作的交互。

UIControl 的子类如下表：

| UIControl 子类 | 名称 | 
| :-: | :-: |
| UIButton | 按钮 |
| UITextField | 文本框 |
| UISwitch | 开关 |
| UISlider | 滑块 |
| UIDatePicker | 日期选择器 |
| UIPageControl | 分页控制器 |
| UISegmentedControl | 分段控制器 |

## UIControl 状态

控件的状态决定了它的外观和它与用户交互的能力。并且控件一次可以处于多个状态，控件可以根据其状态进行不同的配置。例如，可以将 UIButton 对象配置为：在正常状态时显示一个图像，在高亮状态显示时显示另一个图像。

控件的状态由 UIControlState 类型定义，UIControlState 是一个枚举类型，具体值如下：

| UIControlState 枚举值 | 说明 |
| :-: | :-- |
| UIControlStateNormal | 正常状态。控件已启用但未选中。 |
| UIControlStateHighlighted | 高亮状态。当触摸事件进入控件的边界时，控件将高亮显示；<br>当触摸抬起事件发生或触摸事件退出控件的边界时，控件将取消高亮状态。|
| UIControlStateDisabled | 禁用状态。与控件的交互没有任何效果。 |
| UIControlStateSelected | 选中状态。对于大部分控件，此状态对行为或外观没有影响。<br>某些控件（如 UISegmentedControl 类）使用此状态来更改其外观。 |
| UIControlStateFocused | 聚焦状态。与 3D Touch 有关，很少使用。 |
| UIControlStateApplication | 附加的控制状态标志可供应用程序使用。从来不用。 |
| UIControlStateReserved | 为内部框架使用保留的控制状态标志。从来不用。 |

> ⚠️ **注意**
>
>  UIControlState 的枚举是位移枚举，通过其定义中的值推断出控件不能同时处于 `Normal` 和 `Highlighted` 状态，但是控件可以同时处于 `Disabled`、`Selected` 和 `Normal/Highlighted` 三种状态。

UIControl 也有一些属性用于设置或获取控件当前状态，如下：

- **state** 属性

  UIControlState 类型，只读。控件当前所处的状态。

- **enabled** 属性

  BOOL 类型，指示控件是否启用，默认值为 `YES`。

  将此属性设置为 `NO` 将会禁用控件，控件会一直处于 `Disabled` 状态，并且可能会以不同的方式绘制自身的外观，并且控件会忽略所有触摸事件，不能够与用户进行交互。

- **selected** 属性

  BOOL 类型，指示控件是否处于选定状态，默认值为 `NO`。

  将此属性的值设置为 `YES` 会是控件处于 `Selected` 状态。大多数控件在被选中时不会改变它们的外观或行为，但有些控件例外。例如，UISegmentedControl 类会使用此属性判断是否选择了一个分段，并在它被绘制时以不同方式绘制自身。

- **highlighted** 属性

  BOOL 类型，指示控件是否处于高亮状态，默认值为 `NO`。

  当此属性的值为 `YES` 时，控件将会高亮显示，并且其会处于 `Highlighted` 状态。控件会自动设置并清除此状态来响应对应的触摸事件。

## UIControl 事件

UIControl 使用「目标-动作」机制来传递用户与应用的交互的事件。使用 UIControl 的 `addTarget:action:forControlEvents:` 方法来为控件添加动作方法。

其中动作方法必须采用一下三种形式之一，其中的 sender 参数表示调用该动作方法的控件对象，event 参数表示触发控件相关事件的 UIEvent 对象。

```Objective-C
- (void)doSomething;
- (void)doSomething:(id)sender;
- (void)doSomething:(id)sender forEvent:(UIEvent*)event;
```

当用户以特定方式与控件交互时，将调用类似上面的动作方法。UIControlEvents 类型定义了控件可以接受的用户交互类型，这些交互主要与控件中的特定触摸事件相关联。配置控件时，必须使用 `addTarget:action:forControlEvents:` 方法指定触发动作方法时的事件的对应的动作方法。

定义触发控件事件的 UIControlEvents 类型是一个枚举类型，它是用来描述控件可能发生的事件类型的常量。

| UIControlEvents 枚举值 | 说明 |
| :-: | :-- |
| UIControlEventTouchDown | 单点触摸按下事件。用户点击屏幕或者有新手指落下的事件<br>（即已经有手指在屏幕上，此时有新的手指触摸屏幕）。 |
| UIControlEventTouchDownRepeat | 多点触摸按下事件。用户按下第二或三或第四根手指的事件 。|
| UIControlEventTouchDragInside | 手指在控件边界内拖动的事件。 |
| UIControlEventTouchDragOutside | 手指在控件边界外拖动的事件。 |
| UIControlEventTouchDragEnter | 将手指从控件边界外拖动到控件边界内的事件<br>（前提是手指落下时在控件边界内）。 |
| UIControlEventTouchDragExit | 将手指从控件边界内拖动到控件边界外的事件<br>（前提是手指落下时在控件边界内）。 |
| UIControlEventTouchUpInside | 手指在控件边界内的触摸抬起事件。 |
| UIControlEventTouchUpOutside | 手指在控件边界外的触摸抬起事件。 |
| UIControlEventTouchCancel | 因系统事件而取消控件的当前触摸的事件（如：电话呼入）。 |
| UIControlEventValueChanged | 当控件的值发生改变的事件，用于滑块等取值的控件，<br>例如在滑块松开时触发，或者在被拖动时触发。 |
| UIControlEventPrimaryActionTriggered | 未知作用。 |
| UIControlEventEditingDidBegin | 当文本框中编辑开始的事件。 |
| UIControlEventEditingChanged | 当文本框中的文本被改变的事件。 |
| UIControlEventEditingDidEnd | 当文本框中编辑结束的事件。 |
| UIControlEventEditingDidEndOnExit | 当文本框内通过按下回车键（或等价行为）结束编辑的事件。 |
| UIControlEventAllTouchEvents | 所有触摸事件。 |
| UIControlEventAllEditingEvents | 所有关于文本框编辑的事件。 |
| UIControlEventApplicationReserved | 一系列控制事件值可供应用程序使用。 |
| UIControlEventSystemReserved| 为内部框架使用保留的一系列事件。 |
| UIControlEventAllEvents | 所有事件。 |

## UIControl 外观

UIControl 为子类提供了一些 API 用于布局子类中的子视图的对齐方式。例如下列属性，但是 UIControl 本身并没有实现这些对齐方式，而是在具体子类（例如 UIButton 中）中实现。

- **contentVerticalAlignment** 属性

    UIControlContentVerticalAlignment 类型，控件边界内的内容垂直对齐方式，默认值为 `UIControlContentHorizontalAlignmentCenter`。
  
   **官方文档有误，其中说 UIControlContentVerticalAlignmentTop 是默认值，而 UIControl 的头文件实际却是 UIControlContentHorizontalAlignmentCenter 为默认值**。

    对于包含可配置文本或图像内容的控件，使用此属性在控件的边界内适当地对齐该内容。

    UIControlContentVerticalAlignment 枚举值如下：

    | UIControlContentVerticalAlignment 枚举值 | 说明 | 
    | :-: | :-- |
    | UIControlContentVerticalAlignmentCenter | 在控件的中心垂直对齐内容。 |
    | UIControlContentVerticalAlignmentTop | 在控件的顶部垂直对齐内容。  |
    | UIControlContentVerticalAlignmentBottom | 在控件的底部垂直对齐内容。 |
    | UIControlContentVerticalAlignmentFill | 垂直对齐内容以填充内容矩形<br>图像可能会被拉伸。 |

- **contentHorizontalAlignment** 属性

    UIControlContentHorizontalAlignment 类型，控件边界内的内容水平对齐方式。默认值为 `UIControlContentHorizontalAlignmentCenter`。

    UIControlContentHorizontalAlignment 枚举值如下：

    | UIControlContentHorizontalAlignment 枚举值 | 说明 | 
    | :-: | :-- |
    | UIControlContentHorizontalAlignmentCenter | 将内容水平对齐在控件的中心。 |
    | UIControlContentHorizontalAlignmentLeft | 从控件左侧水平对齐内容。  |
    | UIControlContentHorizontalAlignmentRight | 从控件右侧水平对齐内容。 |
    | UIControlContentVerticalAlignmentFill | 水平对齐内容以填充内容矩形，<br>文本可以换行，图像可以拉伸。 |
    | UIControlContentHorizontalAlignmentLeading | 从控件的前沿水平对齐内容。 |
    | UIControlContentHorizontalAlignmentTrailing | 从控件的后缘水平对齐内容。 |

上面的两个属性 `contentVerticalAlignment` 和 `contentHorizontalAlignment` 在 UIControl 中没有任何的实现，它们是在 UIControl 子类中的实现，而 UIControl 提供的仅仅是 API，参考 [Stackoverflow 上的这个问题](https://stackoverflow.com/questions/3337717/uicontrol-contenthorizontalalignment-how-to-use-in-a-subclass)。如果需要自定义 UIControl 子类并使用这两个属性，通过类似下面的代码来实现对这两个属性的使用。

```Objective-C
- (void)layoutSubviews {
    [super layoutSubviews];
    switch (self.contentVerticalAlignment) {
        case UIControlContentHorizontalAlignmentCenter:
            ...
            break;
        case UIControlContentVerticalAlignmentTop:
            ...
            break;
        case UIControlContentVerticalAlignmentBottom:
            ...
            break;
        case UIControlContentVerticalAlignmentFill:
            ...
            break;
    }
}
```
