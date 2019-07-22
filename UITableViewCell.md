# UITableViewCell

https://developer.apple.com/documentation/uikit/uifont/creating_self-sizing_table_view_cells?language=objc

UITableViewCell 继承自 UIView，称为「单元行」，用于展示在表视图（UITableView）上的一个单元行（cell）视图。表视图的一行就是一个 UITableViewCell 对象。表视图的行数不受限制，即 UITableViewCell 对象的数量不受限制。

UITableViewCell 的具体作用是设置和管理单元行内容和背景（包括文本，图像和自定义视图），管理单元行选择状态和高亮显示状态，管理附件视图以及启动单元行内容编辑。

## UITableViewCell 结构

单元行的 `contentView` 属性是 UITableViewCell 中所有内容的父视图。另外单元行还有可以用于显示附件视图的属性，附件视图可以显示一个基于事件的图标，比如详细信息图标、对勾图标等。UITableViewCell 结构示意图如下（虽然下图中 contentView 没有充满整个 UITableViewCell，但是实际上 contentView 是占满整个 UITableViewCell 的）：

![UITableViewCell 的布局](media/15500300920422/UITableViewCell%20%E7%9A%84%E5%B8%83%E5%B1%80.png)

负责单元行中的内容显示的是 `contentView`，它有三个子视图。其中两个是 UILabel 类型的对象，分别是单元行的 `textLabel` 和 `detailTextLabel` 属性所指向的对象。第三个是 UIImageView 类型的对象，即单元行的 `imageView` 属性所指向的对象。他们层次结构示意图如下：

![UITableViewCell 的层次结构](media/15500300920422/UITableViewCell%20%E7%9A%84%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84.png)

> ⚠️ **注意**
>
> 自定义单元行必须将子视图必须添加到 `contentView` 中，不能直接添加到单元行对象本身上。因为当单元行进入「编辑模式」时，为了显示删除按钮，`contentView` 的大小会改变。如果直接向单元行添加子视图，那么处于「编辑模式」时，删除控件和移位控件就可能被挡住。

- **contentView** 属性

    UIView 类型，只读。用于展示单元行内容的子视图，自定义的子视图必须添加到这个视图上。

- **textLabel** 属性

    UILabel 类型，只读。用于设置单元行预定义样式中 textLabel 视图的字符串内容。
  
- **detailTextLabel** 属性

    UILabel 类型，只读。用于设置单元行预定义样式中 detailTextLabel 视图的字符串内容。
  
- **imageView** 属性

    UIImageView 类型，只读。用于设置 UITableViewCell 预定义样式中 imageView 视图的图片内容。
  
## UITableViewCell 样式

在初始化单元行对象的时候，需要传递一个 UITableViewCellStyle 类型的参数，它决定使用哪些子视图以及它们在 `contentView` 中的位置。UITableViewCellStyle 是枚举类型，它包含了单元行的样式，具体枚举值如下表：

| UITableViewCellStyle 枚举值 | 实际效果 |
| :-: | :-: |
| UITableViewCellStyleDefault | ![UITableViewCellStyleDefault](media/15500300920422/UITableViewCellStyleDefault.png) |
| UITableViewCellStyleValue1 | ![UITableViewCellStyleValue1](media/15500300920422/UITableViewCellStyleValue1.png) |
| UITableViewCellStyleValue2 | ![UITableViewCellStyleValue2](media/15500300920422/UITableViewCellStyleValue2.png) |
| UITableViewCellStyleSubtitle | ![UITableViewCellStyleSubtitle](media/15500300920422/UITableViewCellStyleSubtitle.png) |

## UITableViewCell 附件视图

附件视图是单元行中用于显示一个基于事件的图标，比如详细信息图标、对勾图标等。以下属性用于设置其附件视图：

- **accessoryType** 属性

    UITableViewCellAccessoryType 类型，单元行的标准附件视图的类型（正常状态时）。默认值是 `UITableViewCellAccessoryNone`。
    
    如果通过 `accessoryView` 属性设置了自定义附件视图，则忽略此属性的值。如果单元行已启用且附件类型为 `UITableViewCellAccessoryDetailDisclosureButton`，则附件视图将跟踪触摸，并在轻触时向数据源对象发送 `tableView:accessoryButtonTappedForRowWithIndexPath:` 消息。

    UITableViewCellAccessoryType 是一个枚举类型，用于显示一些系统自带的附件视图，它的具体枚举值如下表：

    | UITableViewCellAccessoryType 枚举值 | 显示效果 |
    | :-: | :-: | :-: |
    | UITableViewCellAccessoryNone | 无 |
    | UITableViewCellAccessoryDisclosureIndicator | ![UITableViewCellAccessoryDisclosureIndicator](media/15500300920422/UITableViewCellAccessoryDisclosureIndicator.png) |
    | UITableViewCellAccessoryCheckmark | ![UITableViewCellAccessoryCheckmark](media/15500300920422/UITableViewCellAccessoryCheckmark.png) |
    | UITableViewCellAccessoryDetailButton | ![UITableViewCellAccessoryDetailButton](media/15500300920422/UITableViewCellAccessoryDetailButton.png) |
    | UITableViewCellAccessoryDetailDisclosureButton | ![UITableViewCellAccessoryDetailDisclosureButton](media/15500300920422/UITableViewCellAccessoryDetailDisclosureButton.png) |

- **accessoryView** 属性

  UIView 类型，在单元行右侧（正常状态时）使用的自定义附件视图。此属性的默认值为 `nil`。

  使用此属性会覆盖 `accessoryType` 属性指定的效果。将自定义的视图指定给此属性来设置自定义的附件视图。

- **editingAccessoryType** 属性

  UITableViewCellAccessoryType 类型（参考 accessoryType 属性），单元行的标准附件视图的类型（编辑状态时）。默认值为 `UITableViewCellAccessoryNone`。
  
  如果通过 `editingAccessoryView` 属性设置了编辑模式的自定义附件视图，则忽略此属性的值。如果单元行已启用且附件类型为 `UITableViewCellAccessoryDetailDisclosureButton`，则附件视图将跟踪触摸，并在轻触时向委托对象发送 `tableView:accessoryButtonTappedForRowWithIndexPath:` 消息。

- **editingAccessoryView** 属性

  UIView 类型，在单元行右侧（编辑状态时）使用的自定义附件视图，此属性的默认值为 `nil`。

  使用此属性会覆盖 `editingAccessoryType` 属性指定的效果。将自定义的视图指定给此属性来设置自定义的附件视图。

## UITableViewCell 管理选择

UITableViewCell 的 `selected` 属性和 `highlighted` 属性是用于表示单元行当前的状态，但是二者并不是相互冲突的状态。
 
### 选中状态

当 `selected` 属性的值为 `YES` 时，表示单元行对象处于被选中的状态；属性的值为 `NO` 时，则表示单元行没有处于被选中的状态。

当用户触摸单元行对象来选中单元行时，会调用其 `setSelected:animated:` 方法。但是在不同的时候，调用 `setSelected:animated:` 方法的次数和传递的参数不同：

1. 在 `tableView:cellForRowAtIndexPath:` 方法返回单元行对象之后会调用一次 `setSelected:animated:` 方法，并且此时参数的值都为 `NO`。

2. 当用户**第一次**（之前没有任何单元行被选中）点击一个单元行对象时，调用一次该单元行对象的 `setSelected:animated:` 方法，并且参数一的值为 `YES`。

3. 当用户点击**另外的**单元行时，会先调用一次已经被选中的单元行对象的 `setSelected:animated:` 方法，且参数一的值为 `NO`。然后再调用这次点击的单元行对象的 `setSelected:animated:` 方法，并且参数一的值为 `YES`。

4. 如果用户点击的是已经被选中的单元行对象，那么会调用一次该单元行对象的 `setSelected:animated:` 方法，且参数一的值为 `YES`。

> ⚠️ **注意**
> 
> 因为默认情况下，只能有单元行对象一个处于选中状态，所以上面的调用过程是表示单选。如果要实现多选，需要开发者通过表视图的代理方法来进行相关设置。
> 
> 总之：无论是单选还是多选，与 `setSelected:animated:` 方法都没有关系，这个方法和 `selected` 属性只会将用户最后一次点击的单元行对象设置为选中状态，将其他的单元行对象设置为非选中状态，并提供相应的显示效果，但是真正的「选中逻辑」需要开发者来实现。

- **selected** 属性

    BOOL 类型，指示单元行是否被选中，默认值为 `NO`。
    
    单元行选中状态时会影响其标签，图像和背景的外观。当单元行的选中状态设置为 `YES` 时，将绘制所选中单元行的背景，其标题为白色。如果仅仅通过此属性将选中状态设置为 `YES`，则从旧状态到新状态的过程中单元行外观的转换不会动画化。有关动画化选中状态改变时的转换，请参阅 `setSelected:animated:` 方法。
    
- **setSelected:animated:** 方法

    设置单元行的选中状态，并设置是否显示转换动画。
    
    - 参数一：BOOL 类型，`YES` 将单元行设置为选中状态，`NO`将其设置为未选中状态。
    - 参数二：BOOL 类型，`YES` 表示有转换动画，`NO` 表示无转换动画。
    
    参数二的值影响的是当 `selectionStyle` 属性不为 `UITableViewCellSelectionStyleNone` 值时的淡入淡出的转换动画。如果 `selectionStyle` 为 `UITableViewCellSelectionStyleNone`，那么参数二的值无影响。
    
    > 🔥 **提醒**
    >
    > 1. 如果在自定义单元行类中重写了 `setSelected:animated:` 方法，那么必须调用父类的实现。否则无论 `setSelected:animated:` 方法的参数一为何值，单元行对象的 `selected` 属性都不会变化。
    >
    > 2. 重写 `setSelected:animated:` 方法时如果需要展示淡入淡出的转换动画，那么必须实现父类的方法，并将参数二设置为 `YES`。默认的 `setSelected:animated:` 方法的参数二在任何情况下都是 `NO`。
    
- **selectionStyle** 属性

    UITableViewCellSelectionStyle 类型，单元行的选中样式，用于确定选中单元行时的颜色。默认值为 `UITableViewCellSelectionStyleBlue`。

    UITableViewCellSelectionStyle 是一个枚举类型，表示选中单元行的样式，具体值如下：
    
    | UITableViewCellSelectionStyle 枚举值 | 说明 |
    | :-: | :-: |
    | UITableViewCellSelectionStyleNone | 单元行被选中时无变化。 |
    | UITableViewCellSelectionStyleGray | 选中后，单元行变为灰色背景样式。 |
    | UITableViewCellSelectionStyleBlue | 选中后，单元行具有默认背景颜色。 |
    | UITableViewCellSelectionStyleDefault | 默认值。 |
    
    > 🔥 **提示**
    >
    > 虽然 UITableViewCellSelectionStyle 的枚举值有四个，但是实际上除了 `UITableViewCellSelectionStyleNone` 是无变化之外，其他的都是单元行被选中后变为灰色。
    
### 高亮状态

当 `highlighted` 属性值为 `YES` 时，单元行对象处于高亮状态；当属性值为 `NO` 时，单元行对象处于非高亮状态。

高亮状态是指当用户手指触摸到单元行，但是还没有离开屏幕时的状态，类似于按钮的高亮状态。单元行高亮状态都是短暂的，当用户手指触碰到单元行时，单元行高亮，当用户手指离开单元行时，单元行取消高亮。在不同的时候，调用 `setHighlighted:animated:` 方法的次数和传递的参数不同：

1. 在 `tableView:cellForRowAtIndexPath:` 方法返回单元行对象之后会调用一次 `setHighlighted:animated:` 方法，并且此时参数的值都为 `NO`。

2. 当用户触摸到单元行对象并且手指还未离开屏幕时，调用一次该单元行对象的 `setHighlighted:animated:` 方法，并且参数一的值为 `YES`。

3. 当用户的手指从屏幕上离开时（或者手指滑动出单元行对象的区域时），再调用一次该单元行对象的 `setHighlighted:animated:` 方法，并且参数一的值为 `NO`。
    
- **highlighted** 属性

    BOOL 类型，指示单元行是否高亮显示，默认值为  `NO` 。
    
    单元行高亮状态时会影响其标签，图像和背景的外观。当单元行的高亮状态设置为  `YES`  时，标签将以其高亮显示的文本颜色绘制（默认为白色）。如果仅仅通过此属性将高亮状态设置为  `YES` ，则从旧状态到新状态的过程中单元行外观的转换不会动画化。有关动画化高亮状态改变时的转换，请参阅 `setHighlighted:animated:` 方法。
    
    如果要设置高亮状态下的 `textLabel` 和 `detailTextLabel` 属性显示的效果，使用每个属性对应的的 `highlightTextColor` 属性；对于图像，使用 `imageView` 属性获取单元行的图像，并设置 UIImageView 对象的 `highlightedImage` 属性。

- **setHighlighted:animated:** 方法

    设置单元行的高亮状态，并设置是否显示转换动画。
    
    - 参数一：BOOL 类型，`YES` 将单元行设置为高亮状态，`NO` 将其设置为非高亮状态。
    - 参数二：BOOL 类型，`YES` 表示有转换动画，`NO` 表示无转换动画。

    参数二的值影响的是当 `selectionStyle` 属性不为 `UITableViewCellSelectionStyleNone` 值时的淡入淡出的转换动画。如果 `selectionStyle` 为 `UITableViewCellSelectionStyleNone`，那么参数二的值无影响。
    
    > 🔥 **提醒**
    >
    > 1. 如果在自定义单元行类中重写了 `setHighlighted:animated:` 方法，那么必须调用父类的实现。否则无论 `setHighlighted:animated:` 方法的参数一为何值，单元行对象的 `highlighted` 属性都不会变化。
    >
    > 2. 重写 `setHighlighted:animated:` 方法时如果需要展示淡入淡出的转换动画，那么必须实现父类的方法，并将参数二设置为 `YES`。默认的 `setHighlighted:animated:` 方法的参数二在任何情况下都是 `NO`。

## ⌛️ UITableViewCell 编辑模式

🍎[官方文档](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/TableView_iPhone/ManageInsertDeleteRow/ManageInsertDeleteRow.html#//apple_ref/doc/uid/TP40007451-CH10-SW1)

当单元行的 `editing` 属性值为 `YES` 时，单元行自动调用其 `setEditing:animated:` 方法，并且会讲该消息发送给表视图，表视图接收到消息后将进入编辑模式。在编辑模式下，表视图显示其代理分配给每一行的编辑控件（插入、删除和排序控件）。代理通过返回 `tableView:editingStyleForRowAtIndexPath:` 方法中的特定单元行的编辑样式来分配控件，具体的样式参考下文 `editingStyle` 属性。

当表视图接收到 `setEditing:animated:` 消息时，它为每个可见行发送相同的消息。然后，它向数据源及其代理（如果它们实现了方法）发送一系列消息：

1. 如果表视图数据源实现了 `tableView:canEditRowAtIndexPath:` 方法，则表视图将调用此方法。此方法可以决定表视图中特定的行是否能编辑，并且能忽略它们的 `editingStyle` 属性。不过大部分情况下不需要实现此方法。
2. 如果表视图代理实现了 `tableView:editingStyleForRowAtIndexPath: `方法，则表视图将调用此方法。此方法可以设置 特定行的编辑样式。
3. 用户点击编辑控件（删除或插入控件）。如果点击删除控件，在单元行的右侧会显示「删除按钮」。该按钮用来确认是否删除。
4. 表视图将 `tableView:commitEditingStyle:forRowAtIndexPath:` 消息发送到数据源。虽然它是可选方法，但是只要数据源要插入或删除单元行，就必须实现该方法。此方法需要做两件事：

    - 将 `deleteRowsAtIndexPaths:withRowAnimation:` 或 `insertRowsAtIndexPaths:withRowAnimation:` 消息发送到表视图以指示它调整其表示。
    - 通过从数组中删除引用的项或向项中添加新项来更新表示图相应的数据模型数组。

下图是这些方法的调用序列：

![表视图发送消息代理方法](media/15500300920422/%E8%A1%A8%E8%A7%86%E5%9B%BE%E5%8F%91%E9%80%81%E6%B6%88%E6%81%AF%E4%BB%A3%E7%90%86%E6%96%B9%E6%B3%95.jpg)

**通过左滑单元行进入编辑模式**

如果表视图的数据源实现了 `tableView:commitEditingStyle:forRowAtIndexPath:method:` 方法，那么用户可以通过左滑动某一单元行来进入编辑模式，并且在单元行的右侧显示「删除按钮」。

> 🔥 **提示**
> 
> 如果 `tableView:editingStyleForRowAtIndexPath:` 代理方法返回的值是 `UITableViewCellEditingStyleDelete`，那么左滑单元行不仅会进入编辑模式，还会出现确认删除的按钮。

通过这种方式进入编辑模式时，上图中的调用序列会有变化。首先是表视图检查其数据源是否已实现 `tableView:commitEditingStyle:forRowAtIndexPath:method:` 方法，如果已经实现，那么它会将 `setEditing:animated:` 消息发送给自身并进入编辑模式。在这种编辑模式下，表视图不会显示编辑和排序控件。因为这是一个用户驱动的事件，所以它传递给代理的还消息包括 `tableView:willBeginEditingRowAtIndexPath:` 和`tableView:didEndEditingRowAtIndexPath:` 两条消息。通过实现这些方法，代理可以适当地更新表视图的外观。

**单元格排序**

当表视图收到 `setEditing:animated:message` 时，它进入编辑模式。在编辑模式下，表视图显示其代理为每行分配的排序控件。经过测试发现在 iOS 12 中，排序控件的显示由 `tableView:moveRowAtIndexPath:toIndexPath:` 和 `tableView:canMoveRowAtIndexPath:` 代理方法决定，不受 `showsReorderControl` 属性影响。

当表视图收到 `setEditing:animated:` 消息时，它会将相同的消息重新发送到与其可见行对应的单元对象。之后，消息序列如下：

![在表视图中重新排序单元行时的调用序列](media/15500300920422/%E5%9C%A8%E8%A1%A8%E8%A7%86%E5%9B%BE%E4%B8%AD%E9%87%8D%E6%96%B0%E6%8E%92%E5%BA%8F%E5%8D%95%E5%85%83%E8%A1%8C%E6%97%B6%E7%9A%84%E8%B0%83%E7%94%A8%E5%BA%8F%E5%88%97.jpg)

1. 表视图将 `tableView:canMoveRowAtIndexPath:` 消息发送到其数据源（如果它实现了该方法）。在该方法中，可以选择性地指定某些行显示或不显示排序控件。
2. 用户通过排序控件在表视图中向上或向下拖动。当拖动的行悬停在表视图的一部分上时，基础行向下滑动以显示目标的位置。
3. 每次拖动的行都在目标上时，表视图就会发送 `tableView:targetIndexPathForMoveFromRowAtIndexPath:toProposedIndexPath:` 给它的代理（如果它实现了该方法）。在此方法中，代理可以拒绝拖动行的当前目标并指定备用目标。
4. 表视图发送 `tableView:moveRowAtIndexPath:toIndexPath:` 到其数据源（如果它实现了该方法）。在此方法中，数据源更新数据模型数组，该数组模型数组是表视图的显示的数据的来源，将项目移动到数组中的其他位置。

- **editing** 属性

    BOOL 类型，指示单元行是否处于可编辑状态。
    
    当单元行处于可编辑状态时，它会显示为其指定的编辑控件：绿色插入控件，红色删除控件或（在右侧）重新排序控件。使用`editingStyle` 属性和 `showsReorderControl` 属性为单元行指定这些控件。
    
- **setEditing:animated:** 方法

    切换单元行进入和退出编辑模式。
    
    - 参数一：BOOL 类型，`YES` 进入编辑模式，`NO` 离开编辑模式，默认值为 `NO`。
    - 参数二：BOOL 类型，是否动画化插入、删除和重新排序控件的出现或消失。

    这个方法的执行类似于 `setSelected:animated:` 方法，具体参考上文。
    
- **editingStyle** 属性

    UITableViewCellEditingStyle 类型，只读。单元行的编辑样式。默认值值是 UITableViewCellEditingStyleNone。
    
    UITableViewCellEditingStyle 是枚举类型，用来指定单元行编辑控件的样式。在 `tableView:editingStyleForRowAtIndexPath:` 代理方法的实现中可以为特定单元行返回此属性的值。
    
    | UITableViewCellEditingStyle 枚举类型 | 说明 |
    | :-: | :-: | :-: |
    | UITableViewCellEditingStyleNone | 单元行无编辑控件。 |
    | UITableViewCellEditingStyleDelete | 单元行具有删除控件。<br>（单元行左侧有一个减号的红色圆圈）|
    | UITableViewCellEditingStyleInsert | 单元行具有插入控件。<br>（单元行左侧有一个加号的绿色圆圈）|
    
- **showingDeleteConfirmation** 属性

    BOOL 类型，只读。单元行是否显示删除确认按钮。
    
    当用户点击删除控件（单元行左侧的红色圆圈）时，单元行在右侧显示「删除」按钮，此按钮的字符串已本地化。
    
- **showsReorderControl** 属性

    BOOL 类型，单元行是否显示排序控件。默认值为 `NO`。 
    
    排序控件是灰色，显示在单元行的右侧。用户可以拖动此控件以重新排序表视图中的单元行的顺序。如果此属性的值为 `YES`，则排序控件会临时替换单元行的附件视图。

    要显示排序控件，不仅需要设置此属性，还需要实现表视图数据源的 `tableView:moveRowAtIndexPath:toIndexPath:` 代理方法。此外，如果数据源实现了 `tableView:canMoveRowAtIndexPath:` 代理方法并返回 `NO`，则排序控件不会出现在该指定的单元行中。
    
    > ⚠️ **注意**
    >
    >  通过实际测试发现，是否显示排序控件只受 `tableView:moveRowAtIndexPath:toIndexPath:` 和 `tableView:canMoveRowAtIndexPath:` 代理方法决定，不受此属性影响。
    
- **userInteractionEnabledWhileDragging** 属性

    BOOL 类型，指示用户在拖动单元行时是否可以与其进行交互，默认值为 `NO`。

- **dragStateDidChange:** 方法

    通知单元格其拖动状态已更改。
    
    - 参数一：UITableViewCellDragState 类型，单元行的新拖动状态。

    UITableViewCellDragState 枚举类型如下：
    
    | UITableViewCellDragState 枚举值 | 说明 |
    | :-: | :-: |
    | UITableViewCellDragStateNone | 单元行不涉及拖动操作。 |
    | UITableViewCellDragStateLifting | 正在从表示图的表面进行动画处理。 |
    | UITableViewCellDragStateDragging | 当前正在拖动单元行。 |
    
## UITableViewCell 属性和方法
  
### 初始化方法

- **initWithStyle:reuseIdentifier:** 方法

  使用指定单元行样式和重用标识符来初始化单元行。

  - 参数一：UITableViewCellStyle 类型，单元行样式。
  - 参数二：NSString 类型，单元行重用标识符。
  - 返回值：初始化的 UITableViewCell 对象，如果无法创建对象，则为 `nil`。
  
  在需要使用自定义单元行时，创建新类继承自 UITableViewCell，并且要在实现文件中重写此初始化方法来初始化该自定义的单元行。
 
### 重用单元行 

- **reuseIdentifier** 属性

  NSString 类型，只读。用于标识可重用单元的字符串。

  重用标识符与表视图代理创建的单元行对象相关联，旨在将其重用（出于性能原因）。 重用标识符在 `initWithFrame:reuseIdentifier:` 方法中分配给单元对象，此后不能更改。表视图对象会维护当前可重用单元的队列，每个 单元行都有自己的重用标识符，并使它们可用于 `dequeueReusableCellWithIdentifier:` 方法中的代理。

- **prepareForReuse** 方法

    准备一个可重用的单元行，供表视图的代理重用。

    在从表视图的 `dequeueReusableCellWithIdentifier:` 方法返回可重用的单元行对象之前调用此方法，用来重置一些相关的属性，例如：在一列单元行中，有的单元行被添加了额外的子视图，而其他的单元行没有，当有子视图的单元行被重用时，如果不去掉子视图，那么子视图依旧会显示在下一个单元行上。
  
    如果单元行对象没有关联的重用标识符，则不会调用此方法。如果重写此方法，**则必须确保调用父类实现**。
  
  > 🔥 **提示**
  >
  > 虽然都是重置，但是表视图中的 `tableView:cellForRowAtIndexPath:` 方法是在重用单元行时重置该方法中设置的内容，而 `prepareForReuse` 方法重置的是与内容无关的单元行属性，例如：alpha 值、编辑和选择状态、子视图等。

### 设置单元行背景

- **backgroundView** 属性

    UIView 类型，该视图用作单元行的背景。对于 Plain 样式的表视图中的单元行，默认值为 `nil`；对于 Grouped 样式的表视图中的单元行，默认值为非 `nil`。
    
    backgroundView 属性指向的视图将作为子视图添加到所有其他子视图后面。
    
    > 🔥 **提醒**
    >
    > backgroundView 的 frame 需要手动设置，并不是默认为 UITableViewCell 的 frame。
        
- **selectedBackgroundView** 属性

    UIView 类型，该视图用作单元行在选中状态和高亮状态时时的背景。对于 Plain 样式的表视图中的单元行，默认值为 `nil`；对于 Grouped 样式的表视图中的单元行，默认值为非 `nil`。
    
    仅当单元行时处于选中或高亮状态时，UITableViewCell 才会将此属性的值添加为子视图。当选中单元行时，UITableViewCell 将此属性添加为 `backgroundView` 属性上方的子视图（如果 `backgroundView` 不为 `nil`），或者在所有其他视图后面。
    
    > 🔥 **提醒**
    >
    > 使用此属性时，单元行的 `selectionStyle` 属性值不能为 `UITableViewCellSelectionStyleNone`，否则当单元行处于选中或高亮状态时，也不会展示 `selectedBackgroundView`。
        
- **multipleSelectionBackgroundView** 属性

    UIView 类型，当表视图允许多行选择时，该视图用作选中单元行的背景视图。
    
    如果此属性不为 `nil`，则当表视图允许多行选择时，此视图将用作所选单元行的背景视图。可以通过 UITableView 的 `allowsMultipleSelection` 和 `allowsMultipleSelectionDuringEditing` 属性启用多行选择。

### ⌛️单元行状态改变的转换

- **willTransitionToState:** 方法

    在单元行转换状态之前调用。
    
    - 参数一：UITableViewCellStateMask 类型，指示单元行正在转换到的状态的位掩码。

    UITableViewCell 的子类可以实现这个方法，当单元行改变状态时，对其进行动画化。每当单元行在不同状态之间转换时（例如从正常状态转换到编辑模式），UITableViewCell 都会调用此方法。自定义单元行可以设置和定位出现在新状态下的新视图。然后单元行调用 `layoutSubviews` 方法，它可以在其中为新状态定位这些新视图的最终位置。子类在重写此方法时必须始终调用 `super`。

- **didTransitionToState:** 方法

    在单元行转换状态之后调用。
    
    - 参数一：UITableViewCellStateMask 类型，指示单元行正在转换到的状态的位掩码。

    UITableViewCell 的子类可以实现这个方法，当单元行改变状态时，对其进行动画化。每当单元行在不同状态之间转换时（例如从正常状态转换到编辑模式），UITableViewCell 都会调用此方法。此方法在动画块的末尾调用，这使自定义单元行有机会在状态更改之后进行清理（例如在状态转换之后移除编辑和排序控件）。子类在重写此方法时必须始终调用 `super`。

> ⚠️ **注意**
> 当用户滑动一个单元行来删除它时，单元行转换到 `UITableViewCellStateShowingDeleteConfirmationMask` 常量标识的状态，但是 `UITableViewCellStateShowingEditControlMask` 没有设置。

- **UITableViewCellStateMask** 枚举类型

UITableViewCellStateMask 是一个枚举类型，其常量值用于在状态之间转换时确定单元行的新状态。具体值如下：

| UITableViewCellStateMask 枚举值 | 说明 |
| :-: | :-: |
| UITableViewCellStateDefaultMask | 单元行默认状态。 | 
| UITableViewCellStateShowingEditControlMask | 单元行编辑模式时的状态。 | 
| UITableViewCellStateShowingDeleteConfirmationMask | 显示确认删除按钮的状态。 | 

### 管理内容缩进

- **indentationLevel** 属性

    NSInteger 类型，单元行内容的缩进级别。默认值为 `0`，即没有缩进。
    
    为该属性赋值为正数将从单元行分隔符的左边缘缩进单元行的内容。缩进的数量等于此属性的值乘以 `indentationWidth` 属性中的值。
    
- **indentationWidth** 属性

    CGFloat 类型，单元行内容的每个缩进级别的宽度。默认缩进宽度为 `10.0`。
    
- **shouldIndentWhileEditing** 方法

    BOOL 类型，用于控制表视图处于编辑模式时是否缩进单元格背景。默认值是 `YES`。
    
    此属性与 `indentationLevel` 属性无关。可以在 `tableView:shouldIndentWhileEditingRowAtIndexPath:` 代理方法中重写此属性值，为指定的行设置是否缩进。此属性仅对以 `Grouped` 样式的表视图有效，对 `Plain` 样式的表视图无效。    

- **separatorInset** 属性

    UIEdgeInsets 类型，单元行分隔线的内嵌值。
    
    可以使用此属性在当前单元行的分隔线与表视图的左右边缘之间添加空白区域。使用正插入值将单元行内容和单元行分隔线向中心移动，使用负值则视为 `0`。自动忽略顶部和底部的插入值。