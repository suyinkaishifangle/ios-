# UITableView ✅

UITableView 继承自 UIScrollView，它是 iOS 开发中使用极为广泛的控件，称为「表视图」，它包含一列单元行视图（UITableViewCell），单元行视图用来展示数据。和 UIScrollView 不同的是 UITableView 只允许垂直滚动，不能够水平滚动。UITableView 还可以进入编辑模式，用户可以在其中插入，删除和重新排序表视图的行视图。

表视图的结构分为两个部分，一部分是**「节（section）」**，另外一部分是节中包含的**「节（row）」**。可以指定每个表视图所拥有的节数和每个不同节中包含的行数。

## UITableView 属性和方法

### 配置单元行高度和布局

- **rowHeight** 属性

    CGFloat 类型，表视图中单元行的默认高度。默认值为 [UITableViewAutomaticDimension](https://developer.apple.com/documentation/uikit/uitableviewautomaticdimension?language=objc)。

    如果表视图的代理对象实现了 `tableView:heightForRowAtPath:` 代理方法，那么这个属性的值将会失效。此属性将所有的行高都设置为一个值，而 `tableView:heightForRowAtIndexPath:` 代理方法可以设置具体某一单元行的行高，但是该方法在具有大量行（大约 1000 或更多）的表视图出现严重的性能问题。所以最好使用此属性，而不是使用代理方法。
    
- **estimatedRowHeight** 属性

    CGFloat 类型，表视图中单元行的估计高度。默认值为 [UITableViewAutomaticDimension](https://developer.apple.com/documentation/uikit/uitableviewautomaticdimension?language=objc)。
    
    提供单元行高度的估计值可以提高加载表视图的性能。如果表视图中包含可变高度行，那么在表视图加载时计算它们的所有高度可能会比较耗费性能。使用估计值可以将几何计算的一些时间从表视图加载时推迟到表视图滚动时。
    
    将此属性的值设置为 `0` 将禁用估计高度，这将使表视图请求每个单元行的实际高度。如果表视图使用自动调整大小的单元行，则此属性的值不能为 `0`。
    
    在使用高度估计时，表视图会主动管理从滚动视图继承的内容 `contentOffset` 和 `contentSize` 属性。不要直接读取或修改这些属性。
    
- **cellLayoutMarginsFollowReadableWidth** 属性

    BOOL 类型，指示单元行边距是否来自可读内容的宽度。默认值为 `NO`。
    
    ⌛️在 iPad 中此属性与默认的单元行分隔线的宽度有关。
    
- **insetsContentViewsToSafeArea** 属性

    BOOL 类型，指示表视图是否将其内容视图重新定位在当前安全区域内。默认值为 `YES`。
    
    当此属性的值为 `YES` 时，表视图调整其内容视图的内嵌值，以确保内容视图不会被其他内容或设备边缘遮挡。当此属性的值为 `NO` 时，表视图不会修改其内容视图的内嵌值。

### 配置表视图外观

- **backgroundView** 属性

    UIView 类型，表视图的背景视图，默认值为 `nil`。

    当设置了此属性之后，此属性指向的视图会被加载到表视图的层次结构上作为背景，此时表视图处于该视图的下方，所以表视图的 `backgroundColor` 属性设置的颜色被遮挡住，但是单元行处于该视图的上方，所以单元行、节头视图和节尾视图不会被该视图遮住。

    不需要为该视图布局，其自动填满整个表视图的可见区域。并且在表视图移动时自动跟踪表视图，所以在界面上看起来背景视图是没有移动。
    
### 配置表视图索引

- **sectionIndexMinimumDisplayRowCount** 属性

    NSInteger 类型，用于在表视图的右边缘处显示索引列表的表行数。默认值为 `0`。
    
    此属性仅适用于 Plain 样式的表视图。
    
- **sectionIndexColor** 属性

    UIColor 类型，设置表视图索引文本的颜色。
    
    此属性用于设置表视图索引区域中显示的文本的颜色。设置为 `nil` 表示使用默认颜色。
    
- **sectionIndexBackgroundColor** 属性

    UIColor 类型，表视图索引区域的背景颜色。设置为 `nil` 表示使用默认颜色。
    
- **sectionIndexTrackingBackgroundColor** 属性

    UIColor 类型，表视图索引区域的点击颜色。设置为 `nil` 表示使用默认颜色。
    
    此属性指定当用户拖动手指时在索引区域背景中显示的颜色。 
    
- **UITableViewIndexSearch** 常量

    NSString 类型，用于将放大镜图标添加到表视图的节索引的常量。
    
    如果数据源在 `sectionIndexTitlesForTableView:` 方法中返回的字符串数组中包含此常量字符串，则节索引在相应的索引位置显示放大镜图标。 此位置通常应该是索引中的第一个标题。
    
### 获取节数和行数

- **numberOfSections** 属性

    NSInteger 类型，只读。获取当前表视图的节数。

- **numberOfRowsInSection:** 方法

    返回指定节中的行数。

    - 参数一：NSInteger 类型，表视图中节的索引号。
    - 返回值：NSInteger 类型，该节中的行数。

### 获取单元行视图及其索引路径

- **cellForRowAtIndexPath:** 方法

    返回指定索引路径的单元行对象。
    
    - 参数一：NSIndexPath 类型，单元行的索引路径。
    - 返回值：UITableViewCell 类型，单元行对象。

    如果单元格不可见或参数一超出范围，则返回 `nil`。

- **headerViewForSection:** 方法

    返回指定节索引相关联的节头视图。
    
    - 参数一：NSInteger 类型，节索引。
    - 返回值：UITableViewHeaderFooterView 类型，节头视图。

    如果指定节索引的节没有节头视图，则返回 `nil`。

- **footerViewForSection:** 方法

    返回指定节索引相关联的节尾视图。
    
    - 参数一：NSInteger 类型，节索引。
    - 返回值：UITableViewHeaderFooterView 类型，节尾视图。

    如果指定节索引的节没有节尾视图，则返回 `nil`。

- **indexPathForCell:** 方法

    返回指定单元行对象的索引路径。
    
    - 参数一：UITableViewCell 类型，单元行对象。
    - 返回值：NSIndexPath 类型，单元行的索引路径。

- **indexPathForRowAtPoint:** 方法

    给定表视图局部坐标系中的一个点，返回该点处的单元行路径。
    
    - 参数一：CGPoint 类型，表视图局部坐标系中的一个点。
    - 返回值：NSIndexPath 类型，单元行的索引路径。

    如果给定的点超出表示图的边界，则返回 `nil`。

- **indexPathsForRowsInRect:** 方法

    给定一个矩形，返回被该矩形包围的行的索引路径的数组。
    
    - 参数一：CGRect 类型，表格视图局部坐标中的一个区域。
    - 返回值：NSArray 类型，包含索引路径的数组。

- **visibleCells** 属性

    NSArray 类型，只读。获取表视图中可见的单元行。
    
    此属性的值是包含 UITableViewCell 对象的数组，每个对象表示当前表视图中的可见单元行。如果没有可见的行，则值为 `nil`。

- **indexPathsForVisibleRows** 属性

    NSArray 类型，只读。获取表视图中可见的单元行的索引路径。
    
    此属性的值是包含 NSIndexPath 对象的数组，每个对象表示当前表视图中的可见单元行的索引路径，如果没有可见的行，则值为 `nil`。
    
### 选择单元行

当用户点击表视图的一行时，通常会发生一些操作。比如点击一个单元行时将该单元行设置为选中状态，并将新视图控制器推入导航控制器的导航栈。而在视图控制器弹出导航栈时应取消选中的单元行。

- **indexPathForSelectedRow** 属性

    NSIndexPath 类型，只读。表示所选单元行的索引路径。

    如果路径无效则返回 `nil`，如果有多个选中行，则返回单元行选择数组中的第一个的索引路径。

- **indexPathsForSelectedRows** 属性

    NSArray 类型，只读。表示全部所选单元行的路径。

    此属性的值是包含索引路径对象的数组，如果没有选中的单元行，则此属性的值为 `nil`。
    
- **allowsSelection** 属性

    BOOL 类型，确定是否可以选中表视图的单元行，默认值为 `YES`。

    如果此属性的值为 `YES`，则可以选择行。如果将其设置为 `NO`，则无法选择行，也不会调用 `selectRowAtIndexPath:animated:scrollPosition:` 方法。仅当表视图未处于编辑模式时，此属性才会生效。如果要仅在编辑模式下限制单元行的选择，使用 `allowSelectionDuringEditing` 属性。
    
- **allowsMultipleSelection** 属性

    BOOL 类型，确定是否可以同时选中表视图的多个单元行，默认值为 `NO`。

    当此属性的值为 `YES` 时，点击的每一行都将选中该行。再次点击该单元行取消选中。仅当表视图未处于编辑模式时，此属性才会生效。如果要仅在编辑模式下允许选择多个单元行，使用 `allowsMultipleSelectionDuringEditing` 属性。

- **allowSelectionDuringEditing** 属性

    BOOL 类型，确定表视图在编辑模式下是否可以选中单元行。默认值为 `YES`。

- **allowsMultipleSelectionDuringEditing** 属性
 
    BOOL 类型，确定表视图是否可以在编辑模式下同时选中多个单元行。默认值为 `NO`。

    如果将其设置为 `YES`，则在编辑模式下选中的行旁边会出现复选标记（一个圆圈，未选中单元行时为灰色边框透明，选中单元行时变为蓝色并带勾）。此外，表视图在进入编辑模式时不会查询编辑样式。
    
- **selectRowAtIndexPath:animated:scrollPosition:** 方法

    选中指定的行，并且滚动表视图到某个位置。

    - 参数一：NSIndexPath 类型，单元行的索引路径。
    - 参数二：BOOL 类型，是否显示选择动画和位置变化的动画。
    - 参数三：UITableViewScrollPosition 类型，滚动结束时单元行在表视图中的相对位置。

    调用此方法不会使代理对象接收 `tableView:willSelectRowAtIndexPath:` 或 `tableView:didSelectRowAtIndexPath:message` 消息，也不会向观察者发送`UITableViewSelectionDidChangeNotification` 通知。
  
    UITableViewScrollPosition 枚举类型如下：

    | UITableViewScrollPosition | 说明 |
    | :-: | :-: |
    | UITableViewScrollPositionNone | 表视图不滚动 |
    | UITableViewScrollPositionTop | 表视图滚动，指定单元行处于顶部 |
    | UITableViewScrollPositionMiddle | 表视图滚动，指定单元行处于中间 |
    | UITableViewScrollPositionBottom | 表视图滚动，指定单元行处于底部 |
  
    虽然参数三传递 `UITableViewScrollPositionNone` 常量不会使单元行移动，但是如果需要实现这样的需求：「单元行部分显示时，选中该单元行，让该单元行完全显示并且移动最小距离」，就需要用到这个常量，并且还需要用到 `scrollToRowAtIndexPath:atScrollPosition:animated:` 方法。示例代码如下：

    ```Objective-C
    NSIndexPath *rowToSelect; 
    UITableView *myTableView;
    [myTableView selectRowAtIndexPath:rowToSelect animated:YES scrollPosition:UITableViewScrollPositionNone];
    [myTableView scrollToRowAtIndexPath:rowToSelect atScrollPosition:UITableViewScrollPositionNone animated:YES];
  ```
  
- **deselectRowAtIndexPath:animated:** 方法

    取消选中指定的单元行。

    - 参数一：NSIndexPath，单元行的索引路径。
    - 参数二：BOOL 类型，是否设置动画。

    调用此方法不会使代理对象接收 `tableView:willSelectRowAtIndexPath:` 或 `tableView:didSelectRowAtIndexPath:message` 消息，也不会向观察者发送`UITableViewSelectionDidChangeNotification` 通知。调用此方法也不会使表视图滚动到取消选中的单元行处。
    
> ⚠️ **注意**
>
> 调用 `selectRowAtIndexPath:animated:scrollPosition` 方法和 `deselectRowAtIndexPath:animated` 方法不会使代理对象接收 `tableView:willSelectRowAtIndexPath:` 或 `tableView:didSelectRowAtIndexPath:` 消息，也不会向观察者发送 `UITableViewSelectionDidChangeNotification` 通知。

- **UITableViewSelectionDidChangeNotification** 通知

    表视图中的单元行选定状态发生更改时发布通知。没有与此通知关联的 `userInfo` 字典。

### 编辑单元行和节

**批量更改**

- **beginUpdates** 方法

    开始一系列方法的调用，这些方法调用插入、删除或选择表视图的行和节。
    
    尽可能使用 `performBatchUpdates:completion:` 方法，而不是这个方法。
    
    如果想要同时动画单元行的插入、删除和选择操作，则调用此方法。还可以使用这个方法以及后面的 `endpdates` 方法来动画行高度的变化，而无需重新加载单元格。这组动画方法必须以调用 `endpdate` 方法结束。这些方法对可以嵌套。如果不在此块中执行插入、删除和选择调用，则行计数等表属性可能无效。不应在组内调用 `reloadData` 方法，如果在组中调用此方法，则必须自己执行任何动画。
    
- **endUpdates** 方法

    结束一系列方法调用，这些方法调用插入、删除、选择或选择表视图的行和节。
    
    尽可能使用 `performBatchUpdates:completion:` 方法，而不是这个方法。
    
    可以调用此方法来括起一系列以 `beginUpdates` 开头的方法调用，这些方法调用由插入、删除、选择和重新加载表视图的行和节的操作组成。当调用 `endpdates` 时，UITableView 会同时激活操作。`beginUpdates` 和 `endpdate` 的调用可以嵌套。如果不在此块中执行插入、删除和选择调用，则行计数等表属性可能无效。
 
- **performBatchUpdates:completion:** 方法

    将多个插入、删除、重新加载和移动操作作为一个组进行动画。
    
    - 参数一：代码块，执行相关插入、删除、重新加载或移动操作。除了修改表的行之外，还要更新表的数据源以反映更改。没有返回值和任何参数。
    - 参数二：代码块，当所有操作完成时要执行的一个完成处理程序块。该块没有返回值，接受以下参数:
        - 参数一：BOOL 类型，指示动画是否成功完成。如果动画因任何原因中断，此参数的值为 `NO`。

> ⚠️ **注意**
> 
> 1. `beginUpdates` 和 `endUpdates` 方法是在用在对单元行进行批量动画更改（插入、删除和选择）时调用的，具体参考 📚 [官方文档](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/TableView_iPhone/ManageInsertDeleteRow/ManageInsertDeleteRow.html#//apple_ref/doc/uid/TP40007451-CH10-SW9) 的示例代码。
> 2. 在 iOS 11 开始，推荐使用 `performBatchUpdates:completion:` 方法来代替 `beginUpdates` 和 `endUpdates` 方法。
> 3. 在 `beginUpdates` 和 `endUpdates` 方法定义的动画块中调用插入或删除方法时，表视图会延迟任何行或节的插入，直到它处理了行或节的删除为止。无论插入和删除方法的调用是如何排序的，都会遵循此顺序。

**插入**

- **insertRowsAtIndexPaths:withRowAnimation:** 方法

    在表视图中索引路径数组指定的位置插入单元行，并提供插入动画。
    
    - 参数一：NSArray 类型，包含要插入位置的索引路径的数组。如果指定位置已经存在一个单元行，那么将会把该单元行向下移动一个索引位置。
    - 参数二：UITableViewRowAnimation 类型，动画类型常量。
    
- **insertSections:withRowAnimation:** 方法

    在表视图中插入一个或多个节，并提供插入动画。
    
    - 参数一：NSIndexSet 类型，要在表视图中插入的索引集（节）。如果在指定的索引位置已经存在一个节，则将其向下移动一个索引位置。    
    - 参数二：UITableViewRowAnimation 类型，动画类型常量。

**删除**

- **deleteRowsAtIndexPaths:withRowAnimation:** 方法

    在表视图中删除由索引路径数组指定的单元行，并提供删除动画。
    
    - 参数一：NSArray 类型，包含索引路径的数组。
    - 参数二：UITableViewRowAnimation 类型，动画类型常量。

- **deleteSections:withRowAnimation:** 方法

    在表视图中删除一个或多个节，并提供插入动画。

    - 参数一：NSIndexSet 类型，节的索引集。
    - 参数二：UITableViewRowAnimation 类型，动画类型常量。

**动画常量**

- **UITableViewRowAnimation** 常量

    插入或删除行时使用的动画类型常量。它是一个枚举类型，具体值如下：
    
    | UITableViewRowAnimation 枚举值 | 说明 |
    | :-: | :-: |
    | UITableViewRowAnimationFade | 淡入或淡出效果。 |
    | UITableViewRowAnimationRight | 插入时从右侧滑入，删除时向右滑出。 |
    | UITableViewRowAnimationLeft | 插入时从左侧滑入，删除时向左滑出。 |
    | UITableViewRowAnimationTop | 插入时从顶部滑入，删除时向上滑出。 |
    | UITableViewRowAnimationBottom | 插入时从底部滑入，删除时向下滑出。 |
    | UITableViewRowAnimationNone | 没有动画。 |
    | UITableViewRowAnimationMiddle | 新旧单元格集中在它们曾经或将要占据的空间中。 |
    | UITableViewRowAnimationAutomatic | 自动选择适当的动画样式。 |
    
**移动**

- **moveRowAtIndexPath:toIndexPath:** 方法

    将指定位置的单元行移动到目标位置。
    
    - 参数一：NSIndexPath 类型，单元行旧的索引路径。
    - 参数二：NSIndexPath 类型，单元行新的索引路径。

    可以将行移动操作与 beginupdate-endpdates 块中的行插入和行删除操作组合在一起，使所有更改作为单个动画一起发生。
    
    与插入和删除方法不同，此方法不采用动画参数。对于移动的行，其将从起始位置直接设置为结束位置。与其他方法不同，此方法每次调用只允许移动一行。 如果要移动多行，可以在 beginUpdates-endUpdates 块中重复调用此方法。

- **moveSection:toSection:** 方法

    将指定的节移动到目标位置。

    - 参数一：NSInteger 类型，该节旧的索引号。
    - 参数二：NSInteger 类型，该节新的索引号。

    可以将行移动操作与 beginupdate-endpdates 块中的行插入和行删除操作组合在一起，使所有更改作为单个动画一起发生。
    
    与插入和删除方法不同，此方法不采用动画参数。对于移动的行，其将从起始位置直接设置为结束位置。与其他方法不同，此方法每次调用只允许移动一行。 如果要移动多行，可以在 beginUpdates-endUpdates 块中重复调用此方法。
    
### 滚动表视图

- **scrollToRowAtIndexPath:atScrollPosition:animated:** 方法

    滚动整个表视图，直到指定的单元行位于屏幕上的指定的位置。
    
    - 参数一：NSIndexPath 类型，单元行的索引路径。
    - 参数二：UITableViewScrollPosition 类型，滚动结束时单元行在表视图中的相对位置。
    - 参数三：BOOL 类型，是否执行动画。

    调用此方法不会使代理接收 `scrollViewDidScroll:` 消息。
    
- **scrollToNearestSelectedRowAtScrollPosition:animated:** 方法

    滚动表视图，使表视图中离指定位置最近的选中行位于该位置。
    
    - 参数一：UITableViewScrollPosition 类型，滚动结束时单元行在表视图中的相对位置。
    - 参数二：BOOL 类型，是否执行动画。

### 重用单元行和节头节尾

- **registerClass:forCellReuseIdentifier:** 方法

    在表视图中注册能够重用的单元行类。

    - 参数一：Class 类型，要重用的类（必须是 UITableViewCell 或其子类）
    - 参数二：NSString 类型，单元行的重用标识符。此参数不能为 `nil` 且不能为空字符串。

    > 🔥 **提示**
    >
    > 单元行的重用标识符可以调用 `NSStringFromClass()` 函数来得到，这样就不会因为写错重用标识符而造成的问题，下同。

- **dequeueReusableCellWithIdentifier:** 方法

    返回指定重用标识符的可重用单元行对象。

    - 参数一：NSString 类型，单元行的重用标识符，不能为 `nil`。
    - 返回值：UITableViewCell 类型，单元行对象，如果可重用单元队列中不存在此类对象，则为 `nil`。

    在数据源对象中调用此方法。此方法使现有单元行可重用，或者根据先前注册的类或 xib 文件创建新单元行，并将其添加到表视图中。**如果没有单元可供重用，并且没有注册类或 xib 文件，则此方法返回 `nil`，此时就需要判断单元行对象是否为 `nil`**。
 
    如果为指定的标识符注册了类，并且必须创建新的单元行，则此方法通过调用单元行的 `initWithStyle:reuseIdentifier:` 方法来初始化该单元行。如果现有单元行可用于重用，则此方法将调用单元行的 `prepareForReuse` 方法。对于基于 xib 的单元行，此方法从提供的 xib 文件加载单元行对象。

- **dequeueReusableCellWithIdentifier:forIndexPath:** 方法

    返回指定重用标识符的可重用单元行对象。

    - 参数一：NSString 类型，单元行的重用标识符，不能为 `nil`。
    - 参数二：NSIndexPath 类型，单元行的索引路径。此方法使用索引路径根据单元行在表视图中的位置执行其他配置。
    - 返回值：UITableViewCell 类型，单元行对象。始终返回有效单元行。

    在数据源对象中调用此方法。此方法使现有单元行可重用，或者根据先前注册的类或 xib 文件创建新单元行，并将其添加到表视图中，**使用此方法前必须注册 cell 类，且一定会返回有效的单元行对象，不会返回 `nil`，所以不需要判断单元行对象是否为 `nil`**。
 
    如果为指定的标识符注册了类，并且必须创建新的单元行，则此方法通过调用单元行的 `initWithStyle:reuseIdentifier:` 方法来初始化该单元行。如果现有单元行可用于重用，则此方法将调用单元行的 `prepareForReuse` 方法。对于基于 xib 的单元行，此方法从提供的 xib 文件加载单元行对象。

> ⚠️ **注意**
>
> `dequeueReusableCellWithIdentifier:forIndexPath:` 方法和 `registerClass:forCellReuseIdentifier:` 方法是 iOS 6 之后出现的，如果注册了单元行的重用标识符，那么最好使用 `dequeueReusableCellWithIdentifier:forIndexPath:` 方法，因为这个方法能够正确调整单元行的大小。

- **registerClass:forHeaderFooterViewReuseIdentifier:**

    在表视图中注册能够重用的节头节尾视图类。
    
    - 参数一：Class 类型，要重用的类（必须是 UITableViewHeaderFooterView 或其子类）
    - 参数二：NSString 类型，单元行的重用标识符。此参数不能为 `nil` 且不能为空字符串。

- **dequeueReusableHeaderFooterViewWithIdentifier:**

    返回指定重用标识符的可重用节头节尾视图对象。
    
    - 参数一：NSString 类型，单元行的重用标识符，不能为 `nil`。
    - 返回值：UITableViewCell 类型，单元行对象，如果可重用队列中不存在此类的对象，则为 `nil`。

### 重新加载表视图

- **hasUncommittedUpdates** 属性

    BOOL 类型，只读。指示表视图的外观是否包含其数据源中没有反映的更改。
    
    当表视图包含占位符单元行或正在处理放置并且正在重新排序时，此属性的值为 `YES`。如果此属性为 `YES`，则需要避免调用 `reloadData` 方法。
    
- **reloadData** 方法

    重新加载表视图的行和节。
    
    调用此方法来重新加载用于构造表视图的所有数据，包括单元行、节头和节尾、索引数组等等。为了提高效率，表视图只重新显示可见的行。当表视图的代理或数据源希望表视图完全重新加载其数据时，调用此方法。它不应该在插入或删除行的方法中调用，特别是在使用 `beginUpdates` 和 `endpdate` 调用实现的动画块中。
    
    > ⚠️ **注意**
    >
    > 当 `hasUncommittedUpdates` 属性为 `YES` 时，不要调用此方法。这样做会强制表视图在重新加载数据之前删除任何未提交的更改。
    
- **reloadRowsAtIndexPaths:withRowAnimation:** 方法

    使用指定的动画效果重新加载指定的单元行。
    
    - 参数一：NSArray 类型，包含一组单元行的索引路径。
    - 参数二：UITableViewRowAnimation 类型，动画类型常量。
    
    重新加载单元行会使表视图向其数据源询问该行的新单元行对象。
    
    在 `beginUpdates` 和 `endUpdates` 方法定义的动画块中调用此方法时，其行为与 `deleteRowsAtIndexPaths:withRowAnimation:` 类似。表视图传递给此方法的索引是在更新之前表视图中的状态，无论动画块中的插入，删除和重新加载方法调用的顺序如何，都是如此。
    
- **reloadSections:withRowAnimation:** 方法

    使用指定的动画效果重新加载指定的节。
    
    - 参数一：NSIndexSet 类型，节的索引集。
    - 参数二：UITableViewRowAnimation 类型，动画类型常量。
    
    在 `beginUpdates` 和 `endUpdates` 方法定义的动画块中调用此方法时，其行为与 `deleteRowsAtIndexPaths:withRowAnimation:` 类似。表视图传递给此方法的索引是在更新之前表视图中的状态，无论动画块中的插入，删除和重新加载方法调用的顺序如何，都是如此。
    
- **reloadSectionIndexTitles** 方法

    重新加载表视图右侧索引栏中的项目。
    
    此方法为提供了一种在插入或删除节之后更新节索引的方法，无需重新加载整个表。

### 获取表格的绘图区域

- **rectForSection:** 方法

    返回表视图中指定节的绘图区域。
    
    - 参数一：NSInteger 类型，表视图指定节的索引号。
    - 返回值：CGRect 类型，表视图绘制该节的区域的矩形。

- **rectForRowAtIndexPath:** 方法

    返回表视图中指定行的绘制区域。
    
    - 参数一：NSIndexPath 类型，表视图指定行的索引路径。
    - 返回值：CGRect 类型，表视图绘制该行的区域的矩形。
    
    定义表视图绘制行的区域的矩形，如果参数一无效，则定义为 `CGRectZero`。
    
- **rectForHeaderInSection:** 方法

    返回表视图中指定节头的绘图区域。
    
    - 参数一：NSInteger 类型，表视图指定节头的索引号。
    - 返回值：CGRect 类型，表视图绘制该节头的区域的矩形。
    
- **rectForFooterInSection:** 方法

    返回表视图中指定节尾的绘图区域。
    
    - 参数一：NSInteger 类型，表视图指定节尾的索引号。
    - 返回值：CGRect 类型，表视图绘制该节尾的区域的矩形。
 
## UITableView 样式

在创建表视图对象时，必须为其指定样式类型，并且创建完表视图对象之后，其样式不能修改。表视图分为两种不同的样式，由 UITableViewStyle 枚举类型控制，如下：

| UITableViewStyle | 名称 | 区别说明 |
| :-: | :-: | :-: |
| UITableViewStylePlain | 普通样式 | 节头和节尾会浮动，默认不显示节头节尾，表视图背景颜色为白色。 |
| UITableViewStyleGrouped | 分组样式 | 节头和节尾不浮动，默认显示节头和节尾，表视图背景颜色为浅灰色。 |

- **initWithFrame:style:** 方法

    初始化并返回指定样式的表视图对象。

    - 参数一：CGRect 类型，表视图 frame 值。
    - 参数二：UITableViewStyle 类型，表视图的样式。
    - 返回值：表视图对象。

    如果使用自动布局，则可以指定初始化方法中的参数一为 `CGRectZero`。如果使用 `initWithFrame:` 方法创建并初始化表视图，那么该表视图是 `Plain` 样式。
  
- **style** 属性

    UITableViewStyle 类型，只读。获取表视图的样式。

## UITableView 节和行

UITableView 的许多方法都将 NSIndexPath 对象作为参数并返回值。NSIndexPath 类是索引列表，用于表示嵌套数组树中特定位置的路径。UITableView 在 NSIndexPath 上声明了一个类别（category），它能够获取所表示的行索引（row 属性）和节索引（section 属性），并从给定的行索引和节索引（`indexPathForRow:inSection:class` 方法）构造索引路径（这都是表视图自动实现的，无需开发者手动编码）。 当创建表视图时，表视图会立即查询其数据源的索引列表，获取节数和每个节的行数，然后再获取每一行的内容。

在表视图中会到 NSIndexPath 类别中的以下属性：

- **section** 属性

    NSInteger 类型，只读。标识表视图或集合视图（UICollectionView）中的节索引号。

- **row** 属性

    NSInteger 类型，只读。标识表视图指定节的行索引号。

> ⚠️ **注意**
>
> 无论是节的索引号还是行的索引号，都是从 0 开始。

### UITableView 行分隔线

UITableView 中的分隔线是 UITableViewSeparatorView 类型的对象，它是的 UIView 类型的子类，不过它是私有类。它用来分隔单元行以及其节头节尾。它并不是 UITableView 的子视图，而是 UITableViewCell 的子视图。但是在 UITableView 中有属性能够设置这个分隔线的效果。

- **separatorStyle** 属性

    UITableViewCellSeparatorStyle 类型，表视图中单元行的分隔线样式，默认值为 `UITableViewCellSeparatorStyleSingleLine`。

  > 🔥 **提示**
  >
  > 分隔线不仅仅存在于单元行之间，节之间的分隔线也是由该属性控制，下同。

    UITableViewCellSeparatorStyle 是一个枚举常量，具体值如下：

    - `UITableViewCellSeparatorStyleNone` 不显示分隔线
    - `UITableViewCellSeparatorStyleSingleLine` 显示分隔线

- **separatorColor** 属性

    UIColor 类型，单元行分隔线的颜色，默认值是 `grayColor`。

- **separatorEffect** 属性

    UIVisualEffect 类型，单元行分隔线的效果。

- **separatorInset** 属性

    UIEdgeInsets 类型，单元行分隔线的内嵌值。

#### 隐藏多余的分隔线

Plain 样式的表视图不会根据具体的表格行数来隐藏多余的分隔线。例如，将表视图布满整个视图控制器的 view，但是只添加了几个单元行视图，其余的行都没有使用（也就是没有将整个表视图的每一行都填满），那么其他没有添加单元行视图的行也会显示分隔线，不仅影响界面的美观，而且没有任何点击效果和点击事件。

要解决这样的问题，可以选择使用分组样式的表视图，它没有这样的问题，这个样式会自动隐藏多余的分隔线。但是有些情况下必须使用普通样式，这个时候可以将表视图的表尾添加一个自定义视图，即可隐藏多余的分隔线
    
示例代码：
    
```Objective-C
tableView.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
```

#### 自定义分隔线

如果遇到在需要自定义分隔线的情况时，那么就可以将 `separatorStyle` 属性设置为 `UITableViewCellSeparatorStyleNone`，然后再在 UITableViewCell 的子类中为添加一条分隔线视图在单元行的最下面，当单元行对象添加到表视图时，就有分隔线的效果。但是这样会有一个问题，最后一个单元行对象的底部也会有分隔线，看起来不太美观。解决方法是可以在表视图的 `tableView:cellForRowAtIndexPath:` 方法中判断如果是最后一个单元行对象，那么就隐藏该单元行对象的分隔线。

## UITableView 表头和表尾

「表头」和「表尾」是表视图的两个附加视图，它们默认都不显示。表头出现在表视图中内容的最上方，不用将表视下拉就能看到表头。而表尾出现在表视图可见矩形范围内的最下方，但是要上拉表视图才能看到表尾，松手之后，表尾只会有一小部分显示出来。表头和表尾都不会浮动。

表头和表尾的高度由 `tableHeaderView` 属性和 `tableFooterView` 属性指定的视图的高度决定；它们的宽度是表视图的宽度。因此表头和表尾视图的 frame 属性值的 `x`、`y` 和 `width` 无论是何值都不会影响表头或表尾的外观。

- **tableHeaderView**

    UIView 类型，显示在表视图上方的视图（即表头）。默认值为 `nil`。

- **tableFooterView**

    UIView 类型，显示在表视图下方的视图（即表尾）。默认值为 `nil`。

> 🔥 **提示**
>
> 同一个视图对象无法同时赋值给 `tableHeaderView` 属性和 `tableFooterView` 属性。如果给两者赋值相同的视图对象，那么只会是 `tableHeaderView` 属性的值生效。

## UITableView 节头和节尾

表视图的每个节也有自己的视图，称为「节头」和「节尾」。节头显示在节的上方，节尾显示在节的下方。不同样式的表视图，节头和节尾的显示方式也不同，如下：

- Plain 样式

    默认不显示节头和节尾，且节头和节尾会浮动。
   
- Grouped 样式
 
    默认显示节头和节尾，且节头和节尾不会浮动。默认显示的节头和节尾高度都是 20pt。
    
要注意因为上一个节的节尾和下一个节的节头相邻，且颜色相同，所以看起来就像只有一个高度 40pt 的节头（第一个节的上面只有节头，最后一个节下面只有节尾）。但是不像表头和表尾的高度一样，节头和节尾的高度由表视图的 `sectionHeaderHeight` 属性和 `sectionFooterHeight` 属性设置，或者由表视图的代理方法设置。

- **sectionHeaderHeight** 属性

    CGFloat 类型，表视图中节头的高度。

- **sectionFooterHeight** 属性
    
    CGFloat 类型，表视图中节尾的高度。
    
- **estimatedSectionHeaderHeight** 属性

    CGFloat 类型，表视图中节头的估计高度。

- **estimatedSectionFooterHeight** 属性

    CGFloat 类型，表视图中节尾的估计高度。

> ⚠️ **注意**
>
> 1. 设置节头、节尾高度的代理方法拥有更高的优先级。如果实现了代理方法，那么通过属性设置的高度便无效。
> 2. 必须实现了设置节头或节尾视图的代理方法之后，设置节头或节尾高度的属性才会生效。

### UITableViewHeaderFooterView

UITableViewHeaderFooterView 继承自 UIView，它是放置在表视图中节头或节尾处显示该节的附加信息的视图。使用此类可以有效地管理表视图的节头和节尾的内容。节头节尾视图是一个可重用的视图，可以将其子类化或按原样使用。

**初始化和重用**

- **initWithReuseIdentifier:** 初始化方法

    使用指定的重用标识符初始化节头或节尾视图。
    
    - 参数一：NSString 类型，重用标识符。如果不需要重用则传递 `nil`。
    - 返回值：UITableViewHeaderFooterView 对象。

- **reuseIdentifier** 属性

    NSString 类型，只读。节头或节尾视图的重用标识符。
    
- **prepareForReuse** 方法

    准备可重用的节头或节尾视图以供重用。

**外观配置**

- **contentView** 属性

    UIView 类型，只读。节头或节尾的内容视图。
    
    要创建节头或节尾的子视图，需要将子视图添加到内容视图中。
    
- **backgroundView** 属性

    UIView 类型，节头或节尾的背景视图。
    
    此属性的视图位于 `contentView` 属性视图的后面，用于显示节头或节尾后面的静态背景内容。例如，可以将图像视图指定给此属性，并使用它来显示自定义背景图像。
    
### 取消节头节尾的浮动

需要用到 Plain 样式的表视图，但又不需要节头和节尾浮动时，那么就可以使用下面两种方法来取消其浮动。

- 方法一：使用 Grouped

    Grouped 样式和 Plain 样式的区别就在于前者会默认显示节头和节尾。假如需求是要显示节头，不显示节尾且不需要浮动，那么可以使用 Grouped 样式的表视图，节尾使用自定义视图填充，并且高度设置为 0。这时节尾就不会显示，剩下的只有节头，节头也不会浮动。

    如果要把节头或节尾高度设置为 0，使用属性设置无效，只能使用代理方法设置节头或节尾高度。

- 方法二：使用 UIScrollView 代理协议的 `scrollViewDidScroll:` 方法。

```ObjectiveC
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    if (scrollView == /* UITableView 对象*/) {
        CGFloat sectionHeaderHeight = /*Header 高度*/;
        // contentOffset 表示视图相对于原位置的偏移点
        if (scrollView.contentOffset.y <= sectionHeaderHeight && scrollView.contentOffset.y >= 0) {
            scrollView.contentInset = UIEdgeInsetsMake(-scrollView.contentOffset.y, 0, 0, 0);
        } else if (scrollView.contentOffset.y >= sectionHeaderHeight) {
            scrollView.contentInset = UIEdgeInsetsMake(-sectionHeaderHeight, 0, 0, 0);
        }
    }
}
```
 
## UITableView 数据源和代理

**表视图的数据源**

在 Cocoa Touch 中，UITableView 对象会自己查询另一个对象，以便获取需要显示的内容，这另一个对象就是 UITableView 对象的数据源对象，也就是 `dataSource` 属性所指向的对象。

UITableView 的数据源对象必须采用 UITableViewDataSource 协议，并且必须实现协议中规定的必须实现的方法。UITableViewDataSource 协议的方法提供了表视图中显示的单元行，并通知 UITableView 对象其表视图中节的数量和每个节中的行数。

- **dataSource** 属性

    id 类型，作为表视图数据源的对象。数据源对象必须采用 UITableViewDataSource 协议。 
  
- **prefetchDataSource** 属性

    id 类型，作为表视图的预取数据源的对象，接收即将到来的单元行数据需求的通知。预取数据源对象必须采用 UITableViewDataSourcePrefetch 协议，以便于在不久的将来为要显示的单元格预取数据。若要禁用预取行为，请将此属性设置为 `nil`。

**表视图的代理**

表视图需要一个代理对象，来实现其代理对象管理选择，配置节头和节尾，帮助删除和重新排序单元行，以及执行其他操作。表视图的代理对象由其 delegate 属性指定，并且要遵守 UITableViewDelegate 协议。

- **delegate** 属性

    id 类型，表视图代理的对象。代理对象必须采用 UITableViewDataSource 协议。

    
## UITableView 重用机制

iOS 设备的资源有限。如果表视图对象要显示大量的数据，就需要针对每条数据创建相应的单元行对象，因为每个单元行对象都有一定的内存需求，并且创建和释放单元行对象都需要占用 CPU 资源。如果按照有多少条数据就创建多少个单元行对象的方式来实现，那么当数据越多，内存的压力也就越大，如果滑动过快，需要短时间创建大量单元行对象，CPU 的压力也就越大。 

当然，如果单元行对象没有被强引用时，在创建之后并且滚动出屏幕时会被自动释放，这种情况下就不会一直占用内存。但是这样会耗费 CPU 的性能，用户在滚动表视图时，需要不断的创建新的单元行对象，释放旧的单元行对象，增加了 CPU 的负担。重用是处理这种问题最好的方式。

**重用机制原理**

为了提高性能，UIKit 能够重用单元行对象。假设屏幕上能够显示若干个单元行对象，当用户滑动表视图时，最上面的单元行对象就会在屏幕上消失，消失的单元行对象会放到一个**缓存队列**中等待重用（如果系统内存不足，会释放缓存队列，否则会一直保存缓存队列）。而最下面会显示新的单元行对象，每次新的单元行对象要出现在屏幕上时，数据源会首先检查缓存队列。如果缓存队列中有**可用的单元行对象**，数据源就用新的数据配置它，然后返回给表视图并展示在屏幕上，这样就不用再初始化新的单元行对象，也就不用再分配新的内存，减少内存的消耗。如果缓冲池中没有可重用的单元行对象，那么此时才会创建新的单元行对象。示意图如下：

![UITableViewCell 重用机制](media/15502382295290/UITableViewCell%20%E9%87%8D%E7%94%A8%E6%9C%BA%E5%88%B6.png)

但是有一个问题，如果需要创建不同类型的单元行子类来自定义界面，那么缓存队列中的单元行类型就不同，表视图就可能获得不同类型的单元行，比如需要使用 A 类型的单元行对象却得到的是 B 类型的单元行对象，那么就会出现问题。因此需要确保返回给表视图的单元行类型是正确的，这样才能确保返回的对象拥有特定的方法和属性以及正确的界面。每个单元行都有一个 NSString 类型的 `reuseIdentifier` 属性，称为「**重用标识符**」。当数据源让表视图获取可重用的单元行时，会传递一个标识符字符串，这样就能保证拿到的是正确的单元行对象。

> 🔥 **提示**
>
> 一般来说，重用标识符设置为单元行的类名。为了防止输错类名，可以使用 `NSStringFromClass()` 函数来返回该类名的字符串。

**重用机制实现**

重用具体实现的步骤：

1. 在使用单元行重用标识符之前需要对单元行的类进行「**注册**」。注册就是把重用标识符注册到表视图的单元行缓存队列中，表示具有这个重用标识符的单元行对象是一个可以重用的单元行对象。使用 `registerClass:forCellReuseIdentifier:` 方法或 `registerNib:forCellReuseIdentifier:` 方法进行注册。
    
2. `tableView:cellForRowAtIndexPath:` 数据源方法用于绘制表视图的每一个单元行对象。使用 `dequeueReusableCellWithIdentifier:` 方法获取指定重用标识符的单元行对象（前提是单元行缓存队列中存在相同标识符的可重用的对象），并将其添加上新的数据，然后显示到表视图的某一行上。

**重用中常见的问题**

- **重复显示部分子视图**

    如上面「 UITableViewCell 重用机制」示意图，当把被缓存的 C0 拿出来作为新的单元行展示时，C0 这个对象中的数据并没有被清空，不过因为会有新的数据放到 C0 中，所以覆盖了 C0 原来的数据，例如：C0 原来的 `textLabel` 属性的值为 `@"0"`，而新的数据 `@9` 会覆盖 `@"0"`，所以用户看到的是 `@9`。

    类似的，如果某些需求中，需要某些单元行展示额外的子视图，而某些单元行不展示额外的子视图，例如评论系统中，有些评论下有子评论，而有的没有。当存在子评论的单元行被重用时，它的子视图依旧存在，而刚好下一个单元行没有子评论，此时就发生了问题。

    解决方法：当单元行准备被重用时，会调用其 `prepareForReuse` 方法，该方法用来在单元行被重用时处理一些操作，基于上述问题，可以用此方法来解决。例如：在 `prepareForReuse` 方法中移除子评论的视图，再在单元行的实现中通过数据判断是否应该添加子评论的视图。不过要注意，该方法中的操作一定要越少越好，因为当用户滑动表视图时，会不断重用单元行对象，就会不断调用该方法，所以该方法中的操作一定越少越好。

- **不是任何时候都需要重用**

    在实际开发中，并不是任何时候都需要用到重用机制。例如一个设置界面中只存在几个单元行，而不是很多的数据。这种情况就没有必要用到重用，只需要在数据源方法中使用单元行的 `initWithStyle:reuseIdentifier:` 方法创建每个单元行的对象即可。

## UITableView 编辑模式

表视图具有「编辑模式」以及其「非编辑模式」，默认状态下表视图时处于非编辑模式的。当表视图进入编辑模式时，它会显示与其行相关联的编辑和重新排序等按钮。编辑按钮位于单元行的左侧，允许用户在表视图中插入或删除单元行。

当表视图进入编辑模式并且用户单击编辑按钮时，表视图会将一系列消息发送到其数据源对象和代理对象，但前提是它们实现了这些方法。这些方法允许数据源对象和代理对象在表视图中细化行的外观和行为，还使它们能够执行删除或插入操作。注意，即使表格视图未处于编辑模式，也可以插入或删除多个行或节，并对这些操作进行动画处理。

> ⚠️ **注意**
> 
> 表视图是否处于编辑模式与表视图能否插入、删除和排序单元行没有任何关系。

当表视图收到 `setEditing:Lanimated:message` 消息时，它进入编辑模式。在编辑模式下，表视图显示其代理对象为每行分配的编辑和排序按钮。代理对象通过返回 `tableView:editingStyleForRowAtIndexPath:` 代理方法中的单元行编辑样式来分配控件。

当表视图接收到 `setEditing:animated:` 消息时，它会向每个可见的单元行对象发送相同的消息。然后，它向数据源及其代理（如果它们实现了方法）发送一系列消息。

- **editing** 属性

    BOOL 类型，确定表视图是否处于编辑模式。默认值为 `NO`。
    
    当此属性的值为 `YES` 时，表视图处于编辑模式，在编辑模式下表视图的单元行可以在左侧显示插入或删除按钮，在右侧显示排序控件，具体取决于单元格的方式。
    
- **setEditing:animated:**

    切换表视图的编辑模式。
    
    - 参数一：BOOL 类型，是否进入编辑模式。
    - 参数二：BOOL 类型，是否执行动画。

## ⌛️ UITableView 拖动和丢弃

# UITableViewController

UITableViewController 继承自 UIViewController，是一个专门管理表视图（UITableView）的视图控制器。当界面基本全部由表视图展现时，可以使用它。

相对于 UIViewController + UITableView 的结构，UITableViewController 自动实现了以下内容：

1. 自动将 `delegate` 属性和 `dataSource` 属性设置为 `self`。
2. 实现了 `viewWillAppear:` 方法，并在首次出现时自动为表视图重新加载数据。每次显示表视图时，它都会清除其选择。可以通过更改 `clearsSelectionOnViewWillAppear` 属性中的值来禁用此行为。
3. 实现了 `viewDidAppear:` 方法，并在第一次出现时自动闪烁表视图的滚动指示器。
4. 实现了 `setEditing:animated:` 方法，并在用户点击导航栏中的 `Edit` 或 `Done` 按钮时自动切换表视图的编辑模式。
5. 能够自动调整其表视图的大小，以适应屏幕键盘的出现或消失。



