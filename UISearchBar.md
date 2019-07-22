# UISearchBar

UISearchBar 称为「搜索栏」，其继承自 UIView，是接收用户搜索信息的专用视图。它包含用于输入搜索信息的文本框，和其他功能的一些按钮（例如搜索按钮、书签按钮和取消按钮）。搜索栏实际上不执行任何搜索操作，它其实就是一个稍加包装的文本框。

> 🔥 **提示**
> 
> 因为 UISearchBar 的一些局限性，所以常手动封装 UITextField 来作为搜索栏使用。

## UISearchBar 结构

搜索栏是一个视图，它由一个 UITextField 对象和一个 UIImageView 对象构成。在视图层次结构中如下图所示：

![UISearchBar 结构](media/15524641616402/UISearchBar%20%E7%BB%93%E6%9E%84.png)

其中 `searchBarBackgound` 是 UIImageView 类型的对象，用作搜索栏的背景视图；`searchTextField` 是 UITextField 类型的对象，用于接收用户的输入内容。但是，它们都是私有的对象，UIKit 并不提供这两个对象的接口，所以无法直接修改它们，不过可以使用 KVC 来获取到它们。实际中常获取本文框来进行相关的修改，这个文本框标识符为 `searchField`。

搜索栏中的文本框的高度是固定的，无法通过设置搜索栏的 frame 值来改变（searchField 的高度固定为 36pt），搜索栏的 frame 值只能改变整个背景的高度。

## UISearchBar 外观

### 文本内容

- **placeholder** 属性

    NSString 类型，搜索栏文本框中没有其他文本时显示的字符串，默认值为 `nil`。

- **prompt** 属性

    NSString 类型，显示在搜索栏的顶部的单行文本，默认值为 `nil`。

- **text** 属性

    NSString 类型，搜索框中的内容文本，默认值为 `nil`。

### 显示外观

- **searchBarStyle** 属性

    UISearchBarStyle 类型，搜索栏样式，默认值为 `UISearchBarStyleDefault`。
    
    UISearchBarStyle 是枚举类型，效果如下：

    | UISearchBarStyle 枚举值 | 说明 |
    | :-: | :-: |
    | UISearchBarStyleDefault | 同 UISearchBarStyleProminent |
    | UISearchBarStyleProminent | 搜索栏有半透明背景，搜索文本框不透明 |
    | UISearchBarStyleMinimal | 搜索栏没有背景，搜索文本框半透明 |
 
    > 🔥 **提示**
    >
    > 使用 `UISearchBarStyleMinimal` 样式时，当用户点击搜索栏，搜索栏的文本框颜色会先加深后恢复，给用户一种点击效果，并且此样式不会有背景图片，只会显示搜索框的文本框。
    
- **tintColor** 属性

    UIColor 类型，要应用于搜索栏中的关键元素的色调颜色。

    此属性在搜索栏中用于修改文本光标的颜色。

- **barStyle** 属性

    UIBarStyle 类型，一种条形样式，用于指定搜索栏的外观，默认值为 `UIBarStyleDefault`。此属性可与 `searchBarStyle` 属性一起使用。

    > 🔥 **提示**
    >
    > 使用 `UIBarStyleBlack` 值时的效果为搜索框内背景和文字的颜色会加深，如果此时搜索框的 `searchBarStyle` 属性值为 `UISearchBarStyleMinimal`，点击搜索框也不会再有颜色变化，并且搜索框为灰色。如果此时搜索框的 `searchBarStyle` 属性值为 `UISearchBarStyleDefault`，那么搜索框的颜色背景的颜色是偏黑色。

- **barTintColor** 属性

    UIColor 类型，要应用于搜索栏背景的色调颜色。默认值为 `nil`。

    受 `translucent` 属性影响，此颜色为默认为半透明效果，除非将半透明属性设置为 `NO`。

- **translucent** 属性

    BOOL 类型，指示搜索栏是否为半透明，默认值为 `YES`。

    如果搜索栏具有自定义背景图像，且图像的任何像素的 alpha 值都小于 1.0，此属性默认为 `YES`，否则为 `NO`。

- **backgroundImage** 属性

    UIImage 类型，搜索栏的背景图片。

    如果不是可拉伸图像，并且图片宽度不够将被平铺。

- **inputAccessoryView** 属性

    UIView 类型，搜索栏键盘的自定义输入附件视图，默认值为 `nil`。
  
    此属性不为 `nil` 时，此属性表示将放置在搜索栏的系统提供的键盘上的自定义输入附件视图。

### 按钮配置

- **showsBookmarkButton** 属性

    BOOL 类型，是否显示书签按钮，默认值为 `NO`。

- **showsCancelButton** 属性

    BOOL 类型，是否显示取消按钮，默认值为 `NO`。

    如果应用程序在 iPhone 上运行，当此属性的值为 `YES` 时，则会显示取消按钮。对于在 iPad 上运行的应用程序，将忽略此属性的值，并且不显示取消按钮。

- **showsSearchResultsButton** 属性

    BOOL 类型，是否显示搜索结果按钮，默认值为 `NO`。

- **setShowsCancelButton:animated:** 方法

    可选择使用动画设置取消按钮的显示状态。

    - 参数一：BOOL 类型，是否显示取消按钮。
    - 参数二：BOOL 类型，使用动画更改取消按钮的显示状态。

## UISearchBarDelegate

UISearchBarDelegate 协议实现一些使搜索栏起作用的的可选方法，这些方法和文本框的代理方法基本一致。需要使用搜索栏的 `delegate` 属性指定其代理对象，其代理方法如下：

### 编辑文本

- **searchBar:textDidChange:** 方法

    告诉代理对象用户更改了搜索文本框的文本。

    - 参数一：UISearchBar 类型，委托方对象。
    - 参数二：NSString 类型，搜索文本框中当前的文本。

    **从搜索文本字段中清除文本时也会调用此方法**。

- **searchBarShouldBeginEditing:** 方法

    询问代表是否应该在指定的搜索栏中开始编辑。

    - 参数一：UISearchBar 类型，委托方对象。
    - 返回值：BOOL 类型，返回 `YES` 则可以编辑。

- **searchBarTextDidBeginEditing:** 方法

    告诉代理对象用户在搜索栏中开始编辑。

    - 参数一：UISearchBar 类型，委托方对象。

- **searchBarShouldEndEditing:** 方法

    询问代理对象是否应在指定的搜索栏中停止编辑。
    
    - 参数一：UISearchBar 类型，委托方对象。
    - 返回值：BOOL 类型，返回 `YES` 则停止编辑。

- **searchBarTextDidEndEditing:** 方法

    告诉代理对象用户在搜索栏中已经停止编辑。

    - 参数一：UISearchBar 类型，委托方对象。
  
- **searchBar:shouldChangeTextInRange:replacementText:** 方法

    询问代理对象是否应该用指定的文本替换指定范围内的文本。

    - 参数一：UISearchBar 类型，委托方对象。
    - 参数二：NSRange 类型，要更改的文本范围。
    - 参数三：NSString 类型，替换范围内现有文本的文本。
    - 返回值：BOOL 类型，如果范围内的文本应替换为文本，则返回 `YES`，否则为 `NO`。

    此代理方法参考 UITextField 的 `textField:shouldChangeCharactersInRange:replacementString:` 代理方法。

### 点击按钮

- **searchBarBookmarkButtonClicked:** 方法

    告诉代理对象点击了书签按钮。

    - 参数一：UISearchBar 类型，委托方对象。

- **searchBarCancelButtonClicked:** 方法

    告诉对象点击了取消按钮。

    - 参数一：UISearchBar 类型，委托方对象。

- **searchBarSearchButtonClicked:** 方法

    告诉对象点击了键盘上的搜索按钮。

    - 参数一：UISearchBar 类型，委托方对象。

- **searchBarResultsListButtonClicked:** 方法

    告诉代理人点击了搜索结果列表按钮。

    - 参数一：UISearchBar 类型，委托方对象。

# ⌛️UISearchController

UISearchController 称为「搜索控制器」，其继承自 UIViewController，用于根据用户与搜索栏的交互来管理搜索结果的显示。

搜索控制器使用两个自定义视图控制器对象。第一个视图控制器显示可搜索内容，第二个视图控制器显示搜索结果，为了方便，称第一个为视图控制器为「**搜索内容控制器**」，称第二个视图控制器为「**搜索结果控制器**」。搜索内容控制器是应用程序主界面的一部分。初始化搜索控制器时，将搜索结果控制器传递给 initWithSearchResultsController: 初始化方法。

每个搜索控制器都提供一个搜索栏对象，必须将搜索控制器的搜索栏集成到搜索内容控制器的界面中。并且还需要手动设置一个符合 UISearchResultsUpdating 协议的代理对象，将其指定给搜索控制器的 searchResultsUpdater 属性，当用户与搜索栏交互时，使用该协议的唯一方法搜索内容并将结果发送到搜索结果视图控制器。

> ⚠️**注意**
>
> 尽管 UISearchController 对象是一个视图控制器，但是也不应该在界面中直接显示它。如果想显式地显示搜索结果界面，需要将搜索控制器包装在 UISearchContainerViewController 对象中并显示该对象。

## UISearchController 方法和属性

### 初始化

- **initWithSearchResultsController:** 方法

  使用指定的视图控制器初始化并返回搜索控制器，这个视图控制器用来显示搜索结果。

  - 参数一：UIViewController 类型，显示搜索结果的视图控制器。
  
    如果要在显示可搜索内容的同一视图控制器中显示搜索结果，则指定为 `nil`。

  - 返回值：初始化后的 UISearchController 对象。

  创建搜索控制器后，始终将对象分配给searchResultsUpdater属性。 搜索控制器使用该对象更新搜索结果。

### 管理搜索

- **searchBar** 属性

  UISearchBar 类型，只读。搜索栏。

  将此属性指定的搜索栏对象集成到视图控制器的界面中。用户与搜索栏的交互由搜索控制器对象自动处理，该对象在搜索内容发生更改时通知 searchResultsUpdater 属性中的对象。

  如果要使用 UISearchBar 的自定义子类，需要子类化 UISearchController，并实现此属性以返回自定义搜索栏。

- **searchResultsUpdater** 属性

  采用 UISearchResultsUpdating 协议的 id 类型，负责更新搜索结果控制器内容的对象。

  使用 UISearchResultsUpdating 协议的方法搜索内容并将结果发送给展示搜索结果的视图控制器。此属性指向的对象通常是集成搜索栏的视图控制器的对象。

- **searchResultsController** 属性

  UIViewController 类型，只读。显示搜索结果的视图控制器。

  当用户在搜索栏中输入搜索内容文本时，搜索控制器会立即显示此属性指向的视图控制器，而不显示任何动画。需要将搜索结果传递给此视图控制器，以将其展示。

- **active** 属性

  BOOL 类型，呈现的搜索界面状态。此属性的默认值为 `NO`。

  当用户点击搜索栏的文本框时，搜索控制器会自动显示搜索结果的控制器。通常，需要获取此属性的值来确定是否显示搜索结果。但是，可以将此属性设置为 `YES` 以强制显示搜索界面，即使用户没有点击搜索栏的文本框也是如此。

### 配置界面

下面两个属性只有当搜索控制器分配给在导航项（UINavigationItem）的 searchController 属性时才会生效。

- **obscuresBackgroundDuringPresentation** 属性

  BOOL 类型，指示在搜索过程中是否隐藏基础内容，此属性的默认值为 `YES`。

  当此属性的值为 `YES` 时，只要用户与搜索栏交互，搜索控制器就会隐藏包含可搜索内容的视图控制器。当此属性为 `NO` 时，搜索控制器不会遮挡原始视图控制器。
  
  此属性仅控制原始视图控制器是否最初被遮挡。当用户在搜索栏中输入文本时，搜索控制器会立即显示搜索结果控制器及结果。如果使用相同的视图控制器显示可搜索的内容和搜索结果，建议将此属性设置为 `NO`。 

- **hidesNavigationBarDuringPresentation** 属性

  BOOL 类型，指示搜索时是否应隐藏导航栏，默认值为 `YES`。

## UISearchResultsUpdating

UISearchResultsUpdating 协议只包含一个方法，用于根据用户输入搜索栏的信息更新搜索结果。

- **updateSearchResultsForSearchController:** 方法

  当搜索栏成为第一个响应者或用户在搜索栏内进行更改时调用。**必须实现**。

  - 参数一：UISearchController 类型，委托方对象。

  只要搜索控制器的搜索栏成为第一响应者或对搜索控制器的搜索栏中的文本进行了更改，就会自动调用此方法。一般用于在此方法内执行所需的过滤和更新操作。

## UISearchControllerDelegate

UISearchControllerDelegate 协议包含一组搜索控制器的代理方法。

### 代理方法

- **willPresentSearchController:** 方法

  在将要自动显示搜索控制器时调用。

  - 参数一：UISearchController 类型，委托方对象。

  仅在自动显示搜索控制器时调用此方法。如果明确显示搜索控制器，则不会调用它。

- **didPresentSearchController:** 方法

  在已经自动显示搜索控制器时调用。

  - 参数一：UISearchController 类型，委托方对象。

  仅在自动显示搜索控制器时调用此方法。如果明确显示搜索控制器，则不会调用它。

- **willDismissSearchController:** 方法

  在将要自动关闭搜索控制器时调用。

  - 参数一：UISearchController 类型，委托方对象。

  仅在自动关闭搜索控制器时调用此方法。如果明确显示搜索控制器，则不会调用它。

- **didDismissSearchController:** 方法

  在已经自动关闭搜索控制器时调用。

  - 参数一：UISearchController 类型，委托方对象。

  仅在自动关闭搜索控制器时调用此方法。如果明确显示搜索控制器，则不会调用它。

- **presentSearchController:** 方法

  提供指定的搜索控制器。

  - 参数一：UISearchController 类型，委托方对象。

  当用户在搜索控制器中开始编辑或者 active 属性设置为 `YES` 时，将调用此方法。如果未实现此方法或自行显示控制器，则会执行默认演示。


## 搜索栏的实现

在 iOS 中实现搜索栏目前主要有两种方式：一种是直接使用 UISearchBar，将其添加到界面上，然后通过 UISearchBarDelegate 协议中的方法来实现搜索功能；另外一种是使用 UISearchController，这种方式常配合导航项（UINavigationItem）使用。

> ⭐️ **提示**
>
> 一般来说，在项目主要是使用 UISearchBar，因为 UISearchController 中很多地方存在问题。

## 导航项添加 UISearchController

可以将 UISearchController 对象指派给视图控制器的导航项（UINavigationItem）的 searchController 属性，这样直接将搜索栏添加到导航栏中。当用户下拉列表到表视图（UITableView）的顶端时，搜索栏会逐渐出现，而当用户上拉表视图时，搜索栏会逐渐消失。不过搜索栏的出现和消失并不是在导航栏原来的位置，而是会逐渐增加导航栏的高度来适应搜索栏的出现。

要注意，导航项的 searchController 属性是 iOS 11 加入的，所以在更低的版本中无法使用该属性，不过可以使用表视图的表头视图来代替，例如：`tableView.tableHeaderView = searchController.searchBar;`，效果参考系统应用程序「设置」。



