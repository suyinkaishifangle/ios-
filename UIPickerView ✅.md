# UIPickerView ✅

UIPickerView 继承自 UIView，称为「选择器视图」。它使用类似旋转轮一样的外观来显示一个或多个值的视图。

选择器视图中能够显示一个或多个轮子（称为「组件」，就是显示的「列」），用户可以操纵这些轮子来选择项目。每个组件都有一系列表示可选择项的行（类似表视图的节和行的关系）。每一行显示一个字符串或视图，以便用户可以识别该行上的项目。用户通过将轮子选择指示器旋转到与所需的值与对齐来选择项目。

选择器视图使用一个采用 UIPickerViewDataSource 协议的对象（数据源）在选择器视图中提供要显示的数据的数量，使用一个采用 UIPickerViewDelegate 协议的对象（代理）来提供选择器视图显示的具体数据和响应用户的选择操作。

## UIPickerView 属性和方法

**获取视图选择器的外形尺寸**

- **numberOfComponents** 属性

    NSInteger 类型，只读。获取选择器视图对象的组件数量。
    
- **numberOfRowsInComponent:** 方法

    返回指定组件的行数。
    
    - 参数一：NSInteger 类型，组件的索引。
    - 返回值：NSInteger 类型，行的索引。

- **rowSizeForComponent:** 方法

    返回组件的行大小。
    
    - 参数一：NSInteger 类型，组件的索引。
    - 返回值：CGSize 类型，指定组件中的行大小。这通常是显示组件中作为行使用的最大字符串或视图所需的大小。

    选择器视图通过调用 `pickerView:widthForComponent:` 和 `pickerView:rowHeightForComponent:` 代理方法获取行大小的值并缓存它。

**重新加载视图选择器**

- **reloadAllComponents** 方法

    重新加载选择器视图的所有组件。

- **reloadComponent:** 方法

    重新加载选择器视图的指定的组件。
    
    - 参数一：NSInteger 类型，组件的索引。
    
**在视图选择器中选择行**

- **selectRow:inComponent:animated:** 方法

    在选择器视图的指定组件中选择一行。
    
    - 参数一：NSInteger 类型，行的索引。
    - 参数二：NSInteger 类型，组件的索引。
    - 参数三：BOOL 类型，是否展示动画。

- **selectedRowInComponent:** 方法

    返回指定组件中选中行的索引。
    
    - 参数一：NSInteger 类型，组件的索引。
    - 返回值：NSInteger 类型，选中行的索引，如果没有选中行，则为 `-1`。

**返回行和组件的视图**

- **viewForRow:forComponent:** 方法

    返回选择器视图指定组件的指定行使用的视图。
    
    - 参数一：NSInteger 类型，行的索引。
    - 参数二：NSInteger 类型，组件的索引。
    - 返回值：UIView 类型。

    返回值是代理对象在 `pickerView:viewForRow:forComponent:reusingView:` 方法中提供的视图。如果组件的指定行不可见，或者如果委托没有实现该方法，则返回 `nil`。
    
**管理选择器视图的外观**

- **showsSelectionIndicator** 属性

    BOOL 类型，是否显示选择指示符，默认值为 `NO`。
    
    如果属性的值为 `YES`，则选取器视图会在当前行中显示清晰的叠加层。 
    
# UIPickerViewDataSource 协议

UIPickerViewDataSource 协议必须由一个对象采用，UIPickerView 的 `dataSource` 属性指向该数据源对象。数据源为选择器视图提供组件数量和每个组件中的行数。以下两个方法都必须实现。

- **numberOfComponentsInPickerView:** 方法

    获取选择器视图应显示的组件（列）的数量。**必须实现**。
    
    - 参数一：UIPickerView，选择器视图对象。
    - 返回值：NSInteger，选择器视图组件的数量。

- **pickerView:numberOfRowsInComponent:** 方法

    获取组件的行数。**必须实现**
    
    - 参数一：UIPickerView 类型，选择器视图对象。
    - 参数二：NSInteger 类型，选择器视图组件的索引，从左到右编号。

# UIPickerViewDelegate 协议

UIPickerView 对象的代理必须采用此协议并至少实现其某些方法，以便为选择器视图提供构建自身所需的数据。代理实现此协议所需的方法，以返回每个组件中行的高度，宽度，行标题和视图内容。它还必须为每个组件的行提供内容，可以是字符串或视图。通常，代理实现其他可选方法以响应新选择或取消选择组件行。

**设置选择器视图的尺寸**

- **pickerView:rowHeightForComponent:** 方法

    设置组件的行高度。

    - 参数一：UIPickerView 类型，选择器视图对象。
    - 参数二：NSInteger 类型，组件索引。
    - 返回值：CGFloat 类型，行的高度。

- **pickerView:widthForComponent:** 方法

    设置组件的列宽度。
    
    - 参数一：UIPickerView 类型，选择器视图对象。
    - 参数二：NSInteger 类型，组件索引。
    - 返回值：CGFloat 类型，列的宽度。

**设置组件行的内容**

- **pickerView:titleForRow:forComponent:** 方法

    设置指定组件中的指定行的标题。
    
    - 参数一：UIPickerView 类型，选择器视图对象。
    - 参数二：NSInteger 类型，行索引。
    - 参数三：NSInteger 类型，组件索引。
    - 返回值：NSSting 类型，行标题的字符串。

- **pickerView:attributedTitleForRow:forComponent:** 方法

    设置指定组件中的指定行的样式化标题。
    
    - 参数一：UIPickerView 类型，选择器视图对象。
    - 参数二：NSInteger 类型，行索引。
    - 参数三：NSInteger 类型，组件索引。
    - 返回值：NSSting 类型，行标题的属性字符串。

> 🔥 **提示**
> 
> 如果同时实现 `pickerView:titleForRow:forComponent:` 方法和 `pickerView:attributedTitleForRow:forComponent:` 方法，则选择器视图会优先使用后者。但是，如果后者的实现返回 `nil`，则选择器视图将回退到使用前者返回的字符串。

- **pickerView:viewForRow:forComponent:reusingView:**

    设置指定组件中的指定行的视图。
    
    - 参数一：UIPickerView 类型，选择器视图对象。
    - 参数二：NSInteger 类型，行索引。
    - 参数三：NSInteger 类型，组件索引。
    - 参数四：UIView 类型，以前用于此行的视图对象，但现在由选取器视图隐藏和缓存。
    - 返回值：UIView 类型，行内容的视图对象。

    如果先前使用的视图（参数四）足够，则返回该视图。如果返回其他视图，则会释放先前使用的视图。选择器视图将返回的视图居中放置在行的矩形中。
    
**响应行选择**

- **pickerView:didSelectRow:inComponent:** 方法

    当用户选择组件中的行时，由选取器视图调用。
    
    - 参数一：UIPickerView 类型，选择器视图对象。
    - 参数二：NSInteger 类型，行索引。
    - 参数三：NSInteger 类型，组件索引。


    

    

    
