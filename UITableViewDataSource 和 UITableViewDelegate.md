# UITableViewDataSource 和 UITableViewDelegate

## UITableViewDataSource

UITableViewDataSource 为表视图对象提供构建和修改表视图所需的信息。它通知表视图对象节的数量和每个节中的行数。数据源可以实现可选方法来配置表视图的其他内容：比如插入、删除和排序单元行等。

许多方法都以 NSIndexPath 对象作为参数。表视图在 NSIndexPath 上了声明一个类别，使开发者能够获取到行索引（`row` 属性）和节索引（`section` 属性），并从给定的行索引和节索引（`indexPathForRow:inSection:class` 方法）构造索引路径。（每个索引路径中的第一个索引标识该节，下一个标识该行。）

### 配置表视图

- **tableView:cellForRowAtIndexPath:** 方法

    **必须实现**。要求数据源对象将单元行插入表视图。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位节和行的索引路径。
    - 返回值：UITableViewCell 类型，单元行对象，不能为 `nil`。

    在此方法的实现中，为给定的索引路径创建并配置适当的单元行对象。使用表视图的 `dequeueReusableCellWithIdentifier:forIndexPath:` 方法创建单元格对象，该方法可以重用单元格行。
  
    创建单元行之后，使用适当的数据值更新单元行的相关属性。永远不要主动调用这个方法，如果希望从表视图中检索单元行对象，则使用表视图的 `cellForRowAtIndexPath:` 方法。

- **tableView:numberOfRowsInSection:** 方法

    **必须实现**。要求数据源对象返回表视图的给定节中的行数。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSInteger 类型，定位节的索引路径。
    - 返回值：NSInteger 类型，节的行数。

- **numberOfSectionsInTableView:** 方法

    要求数据源对象返回表视图中的节数。

    - 参数一：UITableView 类型，表视图对象。
    - 返回值：NSInteger 类型，表视图的节数。如果为实现此方法，则默认实现的返回值为 `1`。

- **tableView:titleForHeaderInSection:** 方法

    询问数据源对象表视图的节头标题。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSInteger 类型，定位节的索引路径。
    - 返回值：NSString 类型，用作节头标题的字符串，返回 `nil` 则该节将没有节头标题。

    表视图使用固定字体样式的节头标题。如果需要自定义的字体样式，则使用表视图的 `tableView:viewForHeaderInSection:` 方法来自定义节头视图。

- **tableView:titleForFooterInSection:** 方法

    询问数据源对象表视图的节尾标题。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSInteger 类型，节的索引号。
    - 返回值：NSString 类型，用作节头标题的字符串，返回 nil 则该节将没有节尾标题。

    表视图使用固定字体样式的节尾标题。如果需要自定义的字体样式，则使用表视图的 `tableView:viewForHeaderInSection:` 方法来自定义节头视图。

### 配置索引

- **sectionIndexTitlesForTableView:** 方法

    要求数据源对象返回表视图各个节的标题。

    - 参数一：UITableView 类型，表视图对象。
    - 返回值：NSArray 类型，存储字符串的数组。

    该方法返回一个字符串数组，数组中的字符串对应表视图的各个节头的标题，所有标题会显示在表视图右侧的索引列表中。表视图的样式必须是普通样式 `UITableViewStylePlain`。例如，对于按字母顺序排列的列表，可以返回一个包含字符串的数组从 `A` 到 `Z`。

- **tableView:sectionForSectionIndexTitle:atIndex:** 方法

    要求数据源对象返回节索引，该索引具有给定标题和节头标题索引。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSString 类型，表视图节索引中的标题。
    - 参数三：NSInteger 类型，在 `sectionIndexTitlesForTableView:` 返回的数组中标识节标题的索引。
    - 返回值：NSInteger 类型，节的索引。

    此方法的参数传递节索引的标题和节索引列表中的索引，然后返回引用节的索引。需要说明的是，这里有两个索引： `sectionIndexTitlesForTableView:` 返回的数组中的节标题的索引和表示图的节的索引。前者是作为传入的参数，后者是作为返回值。
    
    请注意，`sectionIndexTitlesForTableView:` 返回的节标题数组可以比表视图中的实际节数少。

### 编辑单元行

- **tableView:canEditRowAtIndexPath:** 方法

    要求数据源对象验证单元行是否可编辑。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：BOOL 类型，如果允许单元行编辑，返回 `YES`，否则返回 `NO`。

    此方法允许数据源指定特定的行是否可以编辑。可编辑的行会在其单元行中显示插入或删除按钮。如果未实现此方法，则默认所有行都是可编辑的。不可编辑的行忽略单元行对象的 `editingStyle` 属性，并且不会对单元行进行缩进。

- **tableView:commitEditingStyle:forRowAtIndexPath:** 方法

    要求数据源对象提交表视图中单元行的插入或删除操作。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：UITableViewCellEditingStyle 类型，单元行编辑样式。
    - 参数三：NSIndexPath 类型，单元行的索引路径。

    当用户在处于编辑模式下的表视图中点击与单元行对象关联的插入或删除按钮时，表视图会调用此方法。如果用户点击单元行左侧的删除按钮，表视图会在单元行右侧显示确认删除按钮来确认删除。数据源通过调用表视图的 `insertRowsAtIndexPaths:withRowAnimation:` 或 `deleteRowsAtIndexPaths:withRowAnimation:` 方法来提交插入或删除操作。
    
    不应该在此方法的实现中调用 `setEditing:animated:` 方法。如果由于某种原因必须在延迟后使用 `performSelector:withObject:afterDelay:` 方法调用它。
    
    要启用表视图的滑动删除功能（用户在某一单元行中水平向左滑动来显示确认删除按钮），则必须实现此方法。
    
### 排序单元行

- **tableView:canMoveRowAtIndexPath:** 方法

    询问数据源对象是否可以将单元行移动到表视图中的另一个位置。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：BOOL 类型，允许单元行移动，返回 `YES`，否则返回 `NO`。

    默认情况下，如果实现了 `tableView:moveRowAtIndexPath:toIndexPath:` 方法，则在表视图处于编辑模式时，会显示排序控件。如果单元行不被允许移动，那么该单元行就不会显示排序控件。

- **tableView:moveRowAtIndexPath:toIndexPath:** 方法

    告诉数据源对象将表视图中的单元行移动到另一个位置。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位单元行移动之前的索引路径。
    - 参数三：NSIndexPath 类型，定位单元行移动之后的索引路径。

    当用户按下单元行中的排序控件并拖动时不会调用此方法，直到拖动结束松开排序控件时，表视图对象才会调用此方法。
        
## UITableViewDelegate 

表视图对象的代理对象必须采用 UITableViewDelegate 协议。协议的可选方法允许代理对象管理选择，配置节头和节尾，帮助删除和重新排序单元行，以及执行其他操作。

许多方法都以 NSIndexPath 对象作为参数。表视图在 NSIndexPath 上了声明一个类别，使开发者能够获取到行索引（`row` 属性）和节索引（`section` 属性），并从给定的行索引和节索引（`indexPathForRow:inSection:class` 方法）构造索引路径。（每个索引路径中的第一个索引标识该节，下一个标识该行。）
  
### 配置单元行

- **tableView:heightForRowAtIndexPath:** 方法

    告诉代理对象单元行的高度。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：CGFloat 类型，指定行的高度。

    该方法可以用来指定具有不同高度的行。如果实现此方法，则它返回的值将覆盖表视图的 `rowHeight` 属性指定的值。每次显示表视图时都会在设置每个单元行的时候调用该方法，所以如果在有大量的单元行（大约 1000 个或更多）的表视图中，使用这个方法设置单元行高度会出现严重的性能问题。

    > ⚠️ **注意**
    >
    > 1. 在设置单元行高度时，并不是一个单元行调用一次这个方法，而是一个单元行会调用多次。
    > 2. **如果单元行数量太多，并不会一次把所有单元行的高度计算出来，而是当用户滑动表视图时才会继续调用此方法。**例如有 1000 个单元行，而屏幕上只能显示 5 个单元行，那么在加载时此方法会调用大概 40 次（因为不是一行仅调用一次），继续滑动表视图时此方法才会继续调用。  
    
- **tableView:estimatedHeightForRowAtIndexPath:** 方法

    告诉代理对象单元行的估计高度。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：CGFloat 类型，指定行的估计的高度，如果没有估计则返回 UITableViewAutomaticDimension 常量。

    提供估计行的高度可以在加载表视图时改善用户体验。如果表包含可变高度行，则计算所有高度可能会很耗费性能，因此会导致更长的加载时间。使用估计行高度会将几何计算的一些成本从表视图加载时推迟到表视图滑动时。但是实际高度和预估高度差值的动态变化在滑动过快时可能会产生「跳跃」现象，所以此时的预估高度和真实高度越接近越好。
    
    > 🔥 **提示**
    >
    > 在实际开发中，如果单元行的数量非常多，但是所有单元行的高度都相同，那么只需要设置表示图的 `rowHeight` 属性；如果所有单元行的高度不一定相同，那么最好使用此方法来估计计算单元行的高度。
    
- **tableView:indentationLevelForRowAtIndexPath:** 方法

    要求代理对象返回单元行的缩进级别（一般用不上此方法）。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。

- **tableView:willDisplayCell:forRowAtIndexPath:** 方法

    告诉代理对象表视图将要显示单元行。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：UITableViewCell 类型，将要显示的单元行对象。
    - 参数三：NSIndexPath 类型，单元行的索引路径。

    表视图在使用参数二绘制行之前将此消息发送给代理，从而允许代理在显示参数二对象之前对其进行自定义。此方法使代理有机会重写表视图早先设置的基于状态的属性，如选择和背景颜色。在代理返回后，表视图只设置 `alpha` 和 `frame` 属性，然后只在它们滑进或滑出时动画行时设置。
    
- **tableView:didEndDisplayingCell:forRowAtIndexPath:** 方法

    告诉代理对象已经从表视图中移除了单元行视图。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：UITableViewCell 类型，被移除的单元行。
    - 参数三：NSIndexPath 类型，单元行的索引路径。

    使用此方法可以检测何时从表视图中移除单元行视图，而不是监视视图本身来查看它何时出现或消失。在屏幕上滑动表视图使单元行视图消失的时候会调用这个方法。
    
- **tableView:shouldSpringLoadRowAtIndexPath:withContext:** 方法

    对单元行的「spring-loading」（弹簧加载）行为进行微调。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 参数三：采用 UISpringLoadedInteractionContext 协议的 id 类型，用于修改弹簧加载行为的上下文对象。使用此对象指定行中关联的弹簧加载动画的位置。
    - 返回值：BOOL 类型，如果行允许弹簧加载的相互作用，返回 `YES`，否则为 `NO`。

    如果要有选择地禁用与单元行的弹簧加载交互，则重写此方法。例如，对于表示叶内容而不是内容文件夹的行，您可能返回NO。 如果未实现此方法，则表视图会在当前未拖动的行上对该行执行弹簧加载动画。要修改这些动画，请修改提供的上下文对象。例如，可以使用上下文对象将弹簧加载动画应用于单元行的单个子视图而不是整行。
    
### 配置节头和节尾

以下方法中的「🔥 **提示**」部分都是在 `UITableViewStylePlain` 类型的表视图中的实验结果。如果表视图的节头或节尾的高度都相同，那么最好使用表视图的 `sectionHeaderHeight` 属性和 `sectionFooterHeight` 属性来设置节头、节尾的高度，这样能够节省性能，不需要每次调用代理方法。

- **tableView:viewForHeaderInSection:** 方法

    告诉代理对象表视图中节头显示的视图。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位节的索引路径。
    - 返回值：UIView 类型，指定节头显示的视图。

    > 🔥 **提示**
    >
    > 1. 不需要为返回的视图设置 frame 值（即是设置了也不会生效），该视图宽度是表视图的宽度，高度是 `tableView:heightForHeaderInSection:` 方法指定的高度。
    > 2. 如果实现了此方法但是既没有为表视图的 `sectionHeaderHeight` 属性赋值，也没有实现 `tableView:heightForHeaderInSection:` 方法，那么返回的视图默认有一个高度（在 iPhone 6s 上为 28pt）。
    
- **tableView:viewForFooterInSection:** 方法

    告诉代理对象表视图中节尾显示的视图。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位节的索引路径。
    - 返回值：UIView 类型，指定节尾显示的视图。

    > 🔥 **提示**
    >
    > 1. 不需要为返回的视图设置 frame 值（即是设置了也不会生效），该视图宽度是表视图的宽度，高度是 `tableView:heightForFooterInSection` 方法指定的高度。
    > 2. 如果实现了此方法但是既没有为表视图的 `sectionFooterHeight` 属性赋值，也没有实现 `tableView:heightForFooterInSection:` 方法，那么返回的视图默认有一个高度（在 iPhone 6s 上为 28pt）。
    
    - **tableView:heightForHeaderInSection:** 方法

    告诉代理对象节头的高度。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位节的索引路径。
    - 返回值：CGFloat 类型，指定的节头的高度。

    > 🔥 **提示**
    >
    > 1. 如果实现了此方法，则表视图中设置节头高度的 `sectionHeaderHeight` 属性无效。
    > 2. 只有在实现 `tableView:viewForHeaderInSection:` 代理方法时，才会调用此方法，否则不会调用此方法。
    
- **tableView:heightForFooterInSection:** 方法

    告诉代理对象节尾的高度。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位节的索引路径。
    - 返回值：CGFloat 类型，指定的节尾的高度。

    > 🔥 **提示**
    >
    > 1. 如果实现了此方法，则表视图中设置节尾高度的 `sectionFooterHeight` 属性无效。
    > 2. 只有在实现 `tableView:viewForFooterInSection:` 代理方法时，才会调用此方法，否则不会调用此方法。
    
- **tableView:estimatedHeightForHeaderInSection:** 方法

    告诉代理对象节头的估计高度。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位节的索引路径。
    - 返回值：CGFloat 类型，指定的节头的估计高度。

    估计高度可以在加载表视图时改善用户体验。如果表视图中节头的高度不同，则计算不同的节头高度可能会很耗费性能，因此会导致更长的加载时间。而使用估计高度可以将几何计算的时间从表视图加载时推迟到表视图滑动时。

    此方法和 `tableView:heightForHeaderInSection:` 方法类似，不过区别是即使没有实现 `tableView:viewForHeaderInSection:` 代理方法，此方法也会被调用，但是不会显示任何节头视图。
    
- **tableView:estimatedHeightForFooterInSection:** 方法

    告诉代理对象节尾的估计高度。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位节的索引路径。
    - 返回值：CGFloat 类型，指定的节尾的估计高度。

    估计高度可以在加载表视图时改善用户体验。如果表视图中节尾的高度不同，则计算不同的节尾高度可能会很耗费性能，因此会导致更长的加载时间。而使用估计高度可以将几何计算的时间从表视图加载时推迟到表视图滑动时。
    
    此方法和 `tableView:heightForFooterInSection:` 方法类似，不过区别是即使没有实现 `tableView:viewForFooterInSection:` 代理方法，此方法也会被调用，但是不会显示任何节头视图。
    
- **tableView:willDisplayHeaderView:forSection:** 方法

    告诉代理对象即将显示节头的视图。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：View 类型，节头将要显示的视图。
    - 参数三：NSIndexPath 类型，定位节的索引路径。

- **tableView:willDisplayFooterView:forSection:** 方法

    告诉代理对象即将显示节尾的视图。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：View 类型，节尾将要显示的视图。
    - 参数三：NSIndexPath 类型，定位节的索引路径。

- **tableView:didEndDisplayingHeaderView:forSection:** 方法

    告诉代理对象已经从表视图中移除了节头视图。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：UIView 类型，被移除的节头视图。
    - 参数三：NSIndexPath 类型，定位节的索引路径。

    使用此方法可以检测何时从表视图中移除节头视图，而不是监视视图本身以查看它何时出现或消失。在屏幕上滑动表视图使节头视图消失的时候会调用这个方法。如果没有节头视图则不会调用此方法。

- **tableView:didEndDisplayingFooterView:forSection:** 方法

    告诉代理对象已经从表视图中移除了节尾视图。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：UIView 类型，被移除的节尾视图。
    - 参数三：NSIndexPath 类型，定位节的索引路径。

    使用此方法可以检测何时从表视图中移除节尾视图，而不是监视视图本身以查看它何时出现或消失。在屏幕上滑动表视图使节尾视图消失的时候会调用这个方法。如果没有节尾视图则不会调用此方法。
    
### 配置附件视图

- **tableView:accessoryButtonTappedForRowWithIndexPath:** 方法

    告诉代理用户点击了与单元行关联的内置附件视图。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    
    如果单元行使用 `accessoryType` 属性设置的内置附件视图，那么点击 `UITableViewCellAccessoryDetailDisclosureButton` 和 `UITableViewCellAccessoryDetailButton` 样式的内置附件视图时，会调用此代理方法，使用 `accessoryType` 属性中的其他样式不会调用。如果单元行使用的是 `accessoryView` 属性设置的附件视图或者没有设置附件视图，也不会调用此方法。
    
- **tableView:editActionsForRowAtIndexPath:** 方法

    询问代理是否响应单元行中滑动显示的操作。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：NSArray 类型，表示行的操作的 UITableViewRowAction 对象的数组。每个操作都用于创建用户可以单击的按钮。

    如果要为单元行提供自定义操作，则可以使用此代理方法。当用户在单元行中水平滑动时，表视图会将单元行的内容移到一边来显示操作按钮。点击其中一个操作按钮可执行与操作对象一起存储的处理程序块。

    如果未实现此方法，则表视图会在用户滑动行时显示标准附件按钮。
    
    > ⚠️ **注意**
    >
    > 推荐使用 `tableView:leadingSwipeActionsConfigurationForRowAtIndexPath:` 和 `tableView:trailingSwipeActionsConfigurationForRowAtIndexPath:` 代理方法来处理单元行的滑动。
    
#### UITableViewRowAction

UITableViewRowAction 称为「单元行操作按钮」，其实就是用于单元行的按钮，不过它继承自 NSObject，而不是 UIButton。在可编辑的表视图中，某一行水平滑动时默认情况下会显示删除按钮。使用 UITableViewRowAction 类可以为表视图中的单元行定义一个或多个要显示的自定义操作按钮。每个操作按钮表示要执行的单个操作，并包括相应按钮的文本、格式化信息和行为等。
    
要向表视图的单元行添加自定义操作按钮，需要在表视图的代理对象中实现 `tableView:editActionsForRowAtIndexPath:` 方法。
    
- **rowActionWithStyle:title:handler:** 类方法

    创建并返回一个新的单元行的操作按钮。  
        
    - 参数一：UITableViewRowActionStyle 类型，应用于操作按钮的样式特征。
    - 参数二：NSString 类型，操作按钮中显示的字符串。
    - 参数三：代码块，当用户单击与此操作关联的按钮时要执行的代码块。用户选择这个对象所代表的动作时，UIKit 在应用的主线程上执行处理代码块。这个参数不能为 `nil`。此块没有返回值，并接受以下参数：

        - 参数一：UITableViewRowAction 类型，即用户选择的操作对象。
        - 参数二：NSIndexPath 类型，单元行的索引路径。

    - 返回值：单元行操作按钮对象。
        
    指定的样式和处理代码块以后无法更改，但是操作按钮的标题可以更改。
        
- **style** 属性

    UITableViewRowActionStyle 类型，只读，操作按钮的样式。
        
    此属性的值是在创建操作按钮时设置，设置之后不能更改。 
        
    UITableViewRowActionStyle 类型是枚举类型，具体值如下：
        
    | UITableViewRowActionStyle 类型 | 说明 |
    | :-: | :-: |
    | UITableViewRowActionStyleDefault | 默认样式，红色背景。 |
    | UITableViewRowActionStyleDestructive | 等于默认样式。 |
    | UITableViewRowActionStyleNormal | 标准样式，灰色背景。 |
    
    > 🔥 **提示**
    >
    > 不同的样式仅仅是操作按钮的背景颜色不同。所以可以通过其 `backgroundColor` 属性来更改颜色，而样式使用默认的就行。
        
- **title** 属性

    NSString 类型，动作按钮的标题。
        
- **backgroundColor** 属性

    UIColor 类型，操作按钮的背景颜色。
        
    此属性指定操作按钮的背景颜色。如果未指定此属性的值，UIKit 将根据 `style` 属性中的值指定默认颜色。
        
- **backgroundEffect** 属性  

    UIVisualEffect 类型，操作按钮的视觉效果。
        
    为此属性指定视觉效果对象会将该效果添加到操作按钮的背景中。

### 管理选择

在用户触摸单元行并抬起时（手指离开屏幕时）调用 `tableView:willSelectRowAtIndexPath:` 方法，随后调用 `tableView:willDeselectRowAtIndexPath:` 和 `tableView:didDeselectRowAtIndexPath:` 方法取消选中之前的单元行，最后再调用 `tableView:didSelectRowAtIndexPath:` 来选中用户触摸的单元行。

> 🔥 **提示**
> 
> 整个过程是连续调用的，如果用户之前没有选中任何单元行，那么在第一次的调用过程中不会调用取消选中的两个代理方法。

- **tableView:willSelectRowAtIndexPath:** 方法

    告诉代理对象即将选中单元行。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位即将选中的单元行的索引路径。
    - 返回值：NSIndexPath 类型，确认选中的单元行。如果不希望选中该行，则返回 `nil`，此时不会调用 `tableView:didSelectRowAtIndexPath:` 方法。

    在用户触摸单元行并抬起之前不会调用此方法，而是在抬起时（手指离开屏幕时）调用此方法。当表视图处于编辑模式时，不会调用此方法，除非表视图允许在编辑期间进行选择（即表视图的 `allowsSelectionDuringEditing` 属性为 `YES`）。
  
- **tableView:didSelectRowAtIndexPath:** 方法

    告诉代理对象已经选中单元行。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位已经选中行的索引路径。

    此方法用来处理选中单元行时的操作，其中参数二就是 `tableView:willSelectRowAtIndexPath:` 方法的返回对象。

    当表视图处于编辑模式时，不会调用此方法。除非表视图允许在编辑期间进行选择（即表视图的 `allowsSelectionDuringEditing` 属性为 `YES`）。

- **tableView:willDeselectRowAtIndexPath:** 方法

    告诉代理对象即将取消选中单元行。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位即将取消选中的单元行的索引路径。
    - 返回值：NSIndexPath 类型，确认取消选中的单元行。如果不希望取消选中单元行，则返回 `nil`。此时不会调用 `tableView:didDeselectRowAtIndexPath:` 方法。

    只有存在选中的单元行，并且将要选中其他行时，才会调用此方法。

- **tableView:didDeselectRowAtIndexPath:** 方法

    告诉代理对象已经取消选中单元行。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位已经取消选中的单元行的索引路径。

    这个方法用来处理取消选中单元行时的操作。其中参数二就是 `tableView:willDeselectRowAtIndexPath:` 方法的返回对象。
  
### 编辑单元行

- **tableView:willBeginEditingRowAtIndexPath:** 方法

    告诉代理表视图即将进入编辑模式。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。

    当用户在表视图中向左滑动单元行时会调用此方法，表视图将其 `editing` 属性设置为 `YES`（进入编辑模式），并且该单元行显示删除按钮。在此滑动编辑模式下，表视图不显示其他插入、删除和排序按钮。
    
    此方法使代理可以将应用程序的用户界面调整为编辑模式。当表视图退出编辑模式时，表视图将调用 `tableView:didEndEditingRowAtIndexPath:` 代理方法。
    
    > ⚠️ **注意**
    >
    > 除非表视图的数据源实现 `tableView:commitEditingStyle:forRowAtIndexPath:` 方法，否则单元行的滑动不会使表视图进入编辑模式。

- **tableView:didEndEditingRowAtIndexPath:** 方法

    告诉代理表视图已退出编辑模式。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。

    用户在单元行上滑动进入编辑模式，当表视图退出编辑模式时（例如，用户点击了删除按钮或向右滑动单元行），将调用此方法。
    
- **tableView:titleForDeleteConfirmationButtonForRowAtIndexPath:** 方法

    更改滑动删除按钮的默认标题。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：NSString 类型，删除按钮显示的文字。

    默认情况下，单元行右侧的删除按钮的标题为「删除」（英文状态下为「Delete」）。
    
- **tableView:editingStyleForRowAtIndexPath:** 方法

    告诉代理对象单元行的编辑样式。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：UITableViewCellEditingStyle 类型，单元行的编辑样式。
    
    此方法可以自定义单元行的编辑样式。如果没有实现此方法但是单元行对象是可编辑的（即它的 `editing` 属性设置为 `YES`），则单元行默认具有 `UITableViewCellEditingStyleDelete` 样式（删除按钮样式）。
    
    UITableViewCellEditingStyle 是枚举类型，具体值如下表：
    
    | UITableViewCellEditingStyle | 说明 |
    | :-: | :-: |
    | UITableViewCellEditingStyleNone | 单元行没有编辑按钮。 |
    | UITableViewCellEditingStyleDelete | 单元格具有删除按钮。此按钮是带减号的红色圆圈。 |
    | UITableViewCellEditingStyleInsert | 单元格具有插入按钮。此按钮是带加号的绿色圆圈。 |
    
    这些编辑按钮的区别就是按钮不同，并且和上文提到的滑动单元行显示的删除按钮不同，这些按钮显示在单元行的左边。
    
- **tableView:shouldIndentWhileEditingRowAtIndexPath:** 方法

    询问代理在表视图处于编辑模式时是否缩进指定行的背景。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：BOOL 类型，缩进返回 `YES`，否则返回 `NO`。

    如果代理未实现此方法，则默认值返回为 `YES`。此方法与 `tableView:indentationLevelForRowAtIndexPath:` 无关。
    
    > 🔥 **提示**
    >
    > 此方法确定的是否缩进只限于当 `tableView:editingStyleForRowAtIndexPath:` 方法返回值为 `UITableViewCellEditingStyleNone` 时起作用，并且缩进的距离就是使用另外两个值的缩进距离。如果该方法返回的是 `UITableViewCellEditingStyleDelete` 或 `UITableViewCellEditingStyleInsert` 值，那么此方法设置的是否缩进并不起作用。
    
### 排序单元行

- **tableView:targetIndexPathForMoveFromRowAtIndexPath:toProposedIndexPath:** 方法

    请求代理对象返回一个新的索引路径，以移动被拖动的单元行到新的位置。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，定位被拖动单元行旧的索引路径。
    - 参数三：NSIndexPath 类型，定位被拖动单元行新的索引路径。
    - 返回值：NSIndexPath 类型，单元行的目标索引路径。如果参数三合适，则返回值一般为参数三。

    此方法允许单元行在表视图中上下移动来排序。当被拖动的单元行悬停在另一单元行上时，此单元行向被拖动的单元行原来所在的位置滑动，以便在视觉上为重新定位腾出空间。
    
### 单元行编辑菜单

如果用户长按表视图中的某一行，则将按照下列顺序调用：

1. 调用 `tableView:shouldShowMenuForRowAtIndexPath:` 方法。
    
    该方法如果返回值 `NO`，则结束调用，不会显示编辑菜单；该方法如果返回值 `YES`，则继续第 2 步（在 UITableView 对象中该方法的默认实现返回值为 `NO`）。 
    
2. 调用 `tableView:canPerformAction:forRowAtIndexPath:withSender:` 方法。

    该方法如果返回值 `NO`，则结束调用，不会显示编辑菜单；该方法如果返回值 `YES`，则会在单元行识图的上方显示编辑菜单。
    
3. 当用户点击编辑菜单中的选项时，调用 `tableView:performAction:forRowAtIndexPath:withSender:` 方法。

> ⚠️ **注意**
> 
> 1. 必须将三个代理方法都实现才会按照描述的顺序进行调用，如果没有将三个方法都实现，那么不会调用任何一个方法。
> 2. 调用第一个方法时是需要用户长按单元行视图，所以肯定在 UITableViewCell 对象中封装了一个收拾识别器来识别用户的长按手势。所以如果在自定义的单元行对象中再添加一个长按手势的话，就会影响这些方法的调用。

- **tableView:shouldShowMenuForRowAtIndexPath:** 方法

    询问代表对象是否应该为单元行显示编辑菜单。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：BOOL 类型，如果显示编辑菜单则为 `YES`，否则为 `NO`。默认实现返回 `NO`。
    
- **tableView:canPerformAction:forRowAtIndexPath:withSender:** 方法

    询问代理对象是否应该省略单元行的编辑菜单中的「复制」或「粘贴」命令。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：选择器类型，UIResponderStandardEditActions 协议中的 `copy:` 或 `paste:` 等方法。
    - 参数三：NSIndexPath 类型，单元行的索引路径。
    - 参数四：id 类型，发送参数二中选择器消息的对象。
    - 返回值：BOOL 类型，如果与动作对应的命令出现在编辑菜单中，则为 `YES`，否则为 `NO`。默认实现返回 `NO`。

    > 🔥 **提示**
    >
    > 1. 在这一步并不是只调用一次此方法，而是会调用多次。每一次的调用时参数二的值都不一样，参数二的值分别就是 UIResponderStandardEditActions 协议中的部分方法。
    >
    > 2. 要在编辑菜单中显示对应方法的选项，那么就在参数二为该选项的方法时返回 `YES`。例如想要在编辑菜单中显示「拷贝」选项，那么就在该方法中的参数二为 `@select(copy:)` 时返回 `YES`，如果要返回其他的选项同理。
    >
    > 3. 即使在每次调用时都把返回值设置为 `YES`，也不会显示所有的选项，例如：其中一次调用时参数二为 `@select(delete:)`，此时就算返回值为 `YES`，也不会返回显示在编辑菜单中。推测应该是在 UITableView 底层进行了处理，让编辑菜单只能够显示某些选项。

- **tableView:performAction:forRowAtIndexPath:withSender:** 方法

    告诉代理对象点击单元行的编辑菜单后执行的操作。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：选择器，UIResponderStandardEditActions 协议中的 `copy:` 或 `paste:` 等方法。
    - 参数三：NSIndexPath 类型，单元行的索引路径。
    - 参数四：id 类型，发送参数二中选择器消息的对象。

    当用户点击了编辑菜单的某个选项之后，就会调用此方法，参数二就是该选项对应的方法选择器。例如：用户点击了「拷贝」选项，那么会马上调用此方法，此时参数二的就是 `@select(copy:)`。
    
    > 🔥 **提示**
    >
    > 不是点击每一个已显示的选项都会调用此方法。测试发现只有用户点击「剪切」、「拷贝」、「粘贴」时才调用此方法，而其他的选项都不会调用此方法。所以可以得出，只需要在 `tableView:canPerformAction:forRowAtIndexPath:withSender:` 方法返回 `action == @selector(cut:) || action == @selector(paste:) || action == @selector(copy:)` 这个值就行了，因为其他的选项即使展示了也不会调用此方法，无法进行处理。

### 管理高亮显示

下面三个方法的调用顺序：

1. 当用户手指在屏幕上触摸某一单元行时，调用 `tableView:shouldHighlightRowAtIndexPath:` 方法。

2. 随后立即调用 `tableView:didHighlightRowAtIndexPath:` 方法，并且该单元行处于高亮状态。

3. 当用户手指从屏幕上抬起，调用 `tableView:didUnhighlightRowAtIndexPath:` 方法，且这个方法中的参数二和前面两个方法中的参数二相同。手指抬起后，该单元行依旧处于高亮状态。

- **tableView:shouldHighlightRowAtIndexPath:** 方法

    询问代理对象是否应该高亮显示单元行。

    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：BOOL 类型，返回 `YES` 则单元行高亮显示；返回 `NO` 则单元行不会高亮显示。如果不实现此方法，默认返回值为 `YES`。

    每当用户触摸表视图中的某一个单元行时，就会调用此方法，根据此方法返回的值决定是否高亮该行。
    
    > ⚠️ **注意** 
    >
    > 实际上的单元格高亮还受 UITableViewCell 的 `selectionStyle` 属性影响。如果这个属性的值为 `UITableViewCellSelectionStyleNone`，即使此方法返回的值为 `YES`，单元行也不会高亮显示。
    
- **tableView:didHighlightRowAtIndexPath:** 方法

    告诉代理对象单元行已经高亮显示。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。

- **tableView:didUnhighlightRowAtIndexPath:** 方法

    告诉代理对象高亮显示已从单元行中删除。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。

    这个方法实际并不是用来取消高亮显示的，而是当用户手指从单元行视图上抬起时调用。
    
### ⌛️管理表视图焦点

- **tableView:canFocusRowAtIndexPath:** 方法

    询问代理对象单元行本身是否能够聚焦。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：BOOL 类型，单元行是否能够聚焦。

    此委托方法的功能相当于覆盖 UITableViewCell 的 `canBecomeFocused` 方法。如果返回 `YES`，则单元格行本身可以聚焦，这意味着它的所有内容都不能被聚焦。返回 `NO` 表示单元行本身不可聚焦，但这并不妨碍其任何内容被聚焦。如果未实现此方法，则返回值默认为 `YES`。
    
### 处理滑动操作

> 🔥 **提示**
> 
> 如果仅在单元行前部边缘自定义滑动操作，而没有在后部边缘自定义实现滑动操作，那么单元行的后部边缘自动会有「删除按钮」出现。所以如果只想要自定义单元行的一边有滑动操作，那么只能定义后部边缘。

- **tableView:leadingSwipeActionsConfigurationForRowAtIndexPath:** 方法

    返回要在单元行的前部边缘显示的滑动操作。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：UISwipeActionsConfiguration 类型，滑动操作。

    使用此方法可返回一组操作，以便在用户水平滑动单元行时显示。返回的操作显示在该行的前部边缘。例如，在从左到右的语言环境中，当用户从左到右滑动单元行时，它们显示在行的左侧。
    
- **tableView:trailingSwipeActionsConfigurationForRowAtIndexPath:** 方法

    返回要在单元行的后部边缘显示的滑动操作。
    
    - 参数一：UITableView 类型，表视图对象。
    - 参数二：NSIndexPath 类型，单元行的索引路径。
    - 返回值：UISwipeActionsConfiguration 类型，滑动操作。

    使用此方法可返回一组操作，以便在用户水平滑动单元行时显示。返回的操作显示在该行的后部边缘。例如，在从左到右的语言环境中，当用户从右到左滑动单元行时，它们显示在行的右侧。
    
#### UISwipeActionsConfiguration

UISwipeActionsConfiguration 继承自 NSObject，它是「滑动操作配置对象」，用于在单元行进行滑动时执行一组操作。

- **configurationWithActions:** 类方法

    创建具有指定操作集的滑动操作配置对象。
    
    - 参数一：只包含 UIContextualAction 类型的 NSArray 类型，要显示的滑动操作。
    - 返回值：初始化的滑动操作配置对象。

    数组中的第一个操作按钮对象显示在最外面。

- **actions** 属性

    NSArray 数组类型，只读。获取滑动操作的数组。
    
- **performsFirstActionWithFullSwipe** 属性

    BOOL 类型，指示是否自动执行第一个操作。默认值是 `YES`。
    
    当此属性设置为 `YES` 时，单元行中的一次完整滑动（即滑动距离达到最大时）将执行 `actions` 属性中列出的第一个操作。
    
#### UIContextualAction 

UIContextualAction 继承自 NSObject，它定义用户在单元行上向左或向右滑动时可以执行的操作按钮。这个类看起来和 `UITableViewRowAction` 类似，不过此类是 iOS 11 推出的，而 `UITableViewRowAction` 是 iOS 8 推出的。

**创建方法**

- **contextualActionWithStyle:title:handler:** 类方法

    使用指定的标题和处理来创建新的操作。
    
    - 参数一：UIContextualActionStyle 类型，操作按钮的样式信息。
    - 参数二：NSString 类型，操作按钮的标题。
    - 参数三：UIContextualActionHandler 类型，用户点击操作按钮时执行的处理。
    - 返回值：初始化的操作按钮。

**配置**

- **title** 属性

    NSString 类型，操作按钮上显示的标题。
    
- **backgroundColor** 属性

    UIColor 类型，操作按钮的背景颜色。
    
    此属性的默认值由 `style` 属性的值确定，该属性确定按钮的默认外观。
    
- **image** 属性

    UIImgae 类型，操作按钮展示的图片。
    
- **UIContextualActionStyle** 枚举类型

    操作按钮的样式信息的常量。
    
    | UIContextualActionStyle 枚举值 | 说明 |
    | :-: | :-: |
    | UIContextualActionStyleNormal | 正常样式，灰色背景。 |
    | UIContextualActionStyleDestructive | 删除样式，红色背景。 |

- **UIContextualActionHandler** 别名

    要响应选择操作而调用的处理块。
    
    - 参数一：UIContextualAction 类型，包含有关所选操作的信息的对象。
    - 参数二：UIView 类型，显示操作的视图。
    - 参数三：处理程序块，以便在执行操作后执行。该块没有返回值，接受以下参数:
        - 参数一：BOOL 类型，指示是否执行操作。如果执行了操作，则指定为 `YES`；如果无法执行该操作，则指定为 `NO`。
