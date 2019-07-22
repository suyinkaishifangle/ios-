# UIStackView

## UIStackView 简介

UIStackView 是在 iOS 9 的新推出的视图类。

UIStackView 是 UIView 的子类，它用于在列或行中布置视图集合，例如导航栏上的一行按钮就可以用 UIStackView 来布局。使用 UIStackView 只需要其视图的位置和（可选）大小。 然后由 UIStackView 管理其内容的布局和大小。

> ⚠️ **注意**
>
> UIStackView 是 UIView 的非渲染子类，也就是说，它不提供自己的任何用户界面。相反，它只管理其子视图的位置和大小。因此，UIView 某些属性（如 backgroundColor）对 UIStackView 的外观没有影响。同样，不能覆盖 layerClass、drawRect: 或 drawLayer:inContext: 方法。

## UIStackView 的布局

UIStackView 管理其 arrangeSubviews 属性中的所有子视图的布局，它使用「自动布局」来定位 arrangeSubviews 数组中的视图并调整其大小，并且由一些属性来控制其布局。

> ⭐️ **提示**
>
> UIStackView 的 arrangeSubviews 数组中的视图无法使用其 frame 来布局自身，它们是由 UIStackView 管理其布局。

### UIStackView 的轴

UIStackView 的子视图会以在 arrangeSubviews 数组中的顺序沿着 UIStackView 的轴（axis 属性）布置。

- **axis** 属性

  UILayoutConstraintAxis 类型，布置视图的轴线。

  此属性确定布置视图的方向。 默认值为 UILayoutConstraintAxisHorizontal，具体 UILayoutConstraintAxis 枚举类型如下：

  | UILayoutConstraintAxis 枚举值 | 说明 |
  | :--: | :--: |
  | UILayoutConstraintAxisHorizontal | 子视图按行布置（水平布置） |
  | UILayoutConstraintAxisVertical | 子视图按列布置（垂直布置） |


**沿轴方向和垂直轴方向**

沿轴方向是指当前 UIStackView 的轴所在的水平或垂直方向。例如默认轴是水平的，那么沿轴方向指的就是水平方向，并且此时垂直轴方向就是指垂直方向。

### UIStackView 布局边界

在按行布局的 UIStackView 中，首先布置的视图的前缘会对齐到 UIStackView 的前缘，最后布置的视图的后缘会对齐到 UIStackView 的后缘；在按列布局的 UIStackView 中，首先布置的视图的顶部会对齐到 UIStackView 的顶部，最后布置的视图的底部对齐会到 UIStackView 的底部。但是这都是默认的情况，可以使用 la→_→tMarginsRelativeArrangement 属性来将 UIStackView 的子视图固定到相关边距而不是对齐其边界。

- **layoutMarginsRelativeArrangement** 属性

  BOOL 类型，用于确定 UIStackView 是否相对于其边距来布置其子视图。
  
  默认值为 NO，此时 UIStackView 的子视图会对齐到它的边界。如果此属性值为 YES，则 UIStackView 的子视图固定到相关边距而不是对齐其边界。

### UIStackView 子视图布局

因为 arrangedSubviews 数组视图的布局由 UIStackView 来管理，所以  arrangedSubviews 数组视图会将 UIStackView 的可用空间填满。

对于沿轴方向的可用空间，由 UIStackView 的 distribution 属性控制。而对于垂直于轴方向的可用空间，由
UIStackView 的 alignment 属性控制。

- **distribution** 属性

  UIStackViewDistribution 类型，确定 UIStackView 如何沿其轴布局其 arrangedSubviews 数组视图。默认值为 UIStackViewDistributionFill。
  
  UIStackViewDistribution 是枚举类型，用于沿 UIStackView 的轴布置的视图的大小和位置，例如：轴是水平的，那么 distribution 属性控制的就是整个 UIStackView 水平方向的布局。具体枚举值如下：

  > ⚠️ **注意**
  >
  > 下面提到的大小都是指沿  UIStackView 轴方向的大小，例如默认轴方向时，下面的大小其实是指宽度，而不是整个宽度和高度。

   - **UIStackViewDistributionFill**

      填满整个 UIStackView 的轴方向。
      
      首先按照子视图的原大小填充（虽然子视图没有定义自身的大小，但是有些子视图会根据内容确定其大小，例如 UIButton 根据其图片或文字确定自身大小），如果子视图没有填满 UIStackView 的轴方向，则会根据子视图 hugging 优先级拉伸视图。如果存在歧义，则 UIStackView 会根据子视图在 arrangeSubviews 数组中的索引顺序调整子视图的大小。

      效果图如下：

      ![UIStackViewDistributionFill]($resource/UIStackViewDistributionFill.png)

      要注意，图片边缘的灰色方框是指 UIButton 中的 UIImageView，外层的蓝色边框才是整个 UIButton，最外面的整个方框即整个 UIStackView，下同。

  - **UIStackViewDistributionFillEqually**

      每个子视图的大小相等，填满整个 UIStackView 的轴方向。
      
      这种方式会直接忽略子视图的原大小，直接将每个子视图大小均匀的分布在 UIStackView 中。

      效果图如下：

      ![UIStackViewDistributionFillEqually]($resource/UIStackViewDistributionFillEqually.png)

  - **UIStackViewDistributionFillProportionally** 

      按每个子视图的比例变化，填满整个 UIStackView 的轴方向。

      这种方式是按比例同时改变子视图的大小。

      效果图如下：

      ![UIStackViewDistributionFillProportionally]($resource/UIStackViewDistributionFillProportionally.png)

  - **UIStackViewDistributionEqualSpacing**

      保持子视图之间间距相同，填满整个 UIStackView 的轴方向。

      首先按照子视图的原大小填充，如果子视图没有填满 UIStackView 的轴方向，则将剩下的空间按照子视图的之间的间距相等来分配，保证每两个子视图之间间距相同。如果存在歧义，则 UIStackView 会根据子视图在 arrangeSubviews 数组中的索引顺序调整子视图的大小。

      效果图如下：

       ![UIStackViewDistributionEqualSpacing]($resource/UIStackViewDistributionEqualSpacing.png)

  - **UIStackViewDistributionEqualCentering**

      保持子视图之间中心点距离相同，填满整个 UIStackView 的轴方向。

      首先按照子视图的原大小填充，如果此时子视图之间的中心点距离相同都相同，但是没有填满整个 UIStackView 的轴方向，则将子视图之间的中心点间距增大，直到填满整个 UIStackView 的轴方向。如果存在歧义，则 UIStackView 会根据子视图在 arrangeSubviews 数组中的索引顺序调整子视图的大小。

      效果图如下：

      ![UIStackViewDistributionEqualCentering]($resource/UIStackViewDistributionEqualCentering.png)

  对于除 UIStackViewDistributionFillEqually 之外的所有分布，UIStackView 在沿栈轴计算其大小时使用每个子视图的intrinsicContentSize 属性。UIStackViewDistributionFillEqually 调整所有子视图的大小，使它们具有相同的大小，然后沿其轴填充 UIStackView。⌛️如果可能，UIStackView 会拉伸所有排列的视图，以匹配沿堆栈轴具有最长内在大小的视图。

- **spacing** 属性

  CGFloat 类型，设置子视图的相邻边缘之间的距离，默认值为 0.0。

  此属性定义 UIStackViewDistributionFillProportionally 分布的排列视图之间的严格间距。并且它表示UIStackViewDistributionEqualSpacing 和 UIStackViewDistributionEqualCentering 分布的最小间距，使用负值表示子视图重叠。

- **alignment** 属性

  UIStackViewAlignment 类型，确定 UIStackView 如何垂直于其轴布局其 arrangedSubviews 数组视图。默认值为 UIStackViewAlignmentFill。
  
  UIStackViewAlignment 是枚举类型，用于垂直于 UIStackView 的轴布置的视图的大小，例如：轴是水平的，那么 alignment 属性控制的就是整个 UIStackView 垂直方向的布局。具体枚举值如下：

  - **UIStackViewAlignmentFill**

    填满整个 UIStackView 的垂直于轴的方向。

  - **UIStackViewAlignmentLeading**

    用于轴是垂直方向的布局，子视图的前缘对齐其 UIStackView 的前缘。
    
    对于轴是水平方向的 UIStackView，使用这个值相当于 UIStackViewAlignmentTop。

  - **UIStackViewAlignmentTop**

    用于轴是水平方向的布局，子视图的顶部对齐其 UIStackView 的顶部。

    对于轴是垂直方向的 UIStackView，使用这个值相当于 UIStackViewAlignmentLeading。

  - **UIStackViewAlignmentCenter**

    子视图的中心和沿轴方向的中心对齐。

  - **UIStackViewAlignmentTrailing**

    用于轴是垂直方向的布局，子视图的后缘对齐其 UIStackView 的后缘。
    
    对于轴是水平方向的 UIStackView，使用这个值相当于 UIStackViewAlignmentBottom。      
   
  - **UIStackViewAlignmentBottom**

    用于轴是水平方向的布局，子视图的底部对齐其 UIStackView 的底部。

    对于轴是垂直方向的 UIStackView，使用这个值相当于 UIStackViewAlignmentTrailing。

## 其他属性和方法

- **arrangedSubviews** 属性

  NSArray 类型，只读。布置在 UIStackView 对象中的子视图数组。

  如果 arrangedSubviews 数组中的某个子视图调用自己的 removeFromSuperview: 方法，那么该子视图就会从 arrangedSubviews 数组中删除。

  > ⚠️ **注意**
  >
  > 并不是 UIStackView 的所有子视图都包含在 arrangedSubviews 数组中，arrangedSubviews 数组中的视图可能只是 UIStackView 子视图的一部分。
  > - 当 UIStackView 向其 arrangeSubviews 数组添加视图时，它会将该视图添加为子视图（如果尚未添加）。
  > - 当从 UIStackView 中删除子视图时，如果该子视图在 arrangeSubviews 数组中，UIStackView 也会将其从 arrangeSubviews 数组中删除。
  > - 当从 arrangeSubviews 数组中删除视图时，该视图不会从 UIStackView 的子视图中删除，但是 UIStackView 不会再管理该视图大小和位置，但视图仍然是视图层次结构的一部分，如果可见，则在屏幕上呈现。
  > **但是实际开发既然使用 UIStackView，那么肯定是要其管理子视图的大小和位置，所以上面属性中提到的子视图没有特殊说明都表示 arrangeSubviews 数组中的视图**。

- **initWithArrangedSubviews:** 方法

  使用包含子视图的数组初始化 UIStackView 对象。

  - 参数一：NSArray 类型，子视图的数组。
  - 返回值：UIStackView 对象。

- **addArrangedSubview:** 方法

  将视图添加到 UIStackView 的 arrangedSubviews 数组末尾。

  - 参数一：UIView 类型，要添加的视图。

- **insertArrangedSubview:atIndex:** 方法

  将视图插入到 UIStackView 的 arrangedSubviews 数组的指定索引处。

  - 参数一：UIView 类型，要添加的视图。
  - 参数二：NSUInteger 类型，arrangedSubviews 数组的索引。

  此值不得大于此数组中当前视图的数量。如果索引超出范围，则此方法抛出 NSInternalInconsistencyException 异常。添加子视图时，UIStackView 会将被添加的视图附加到其子视图数组的末尾。索引仅影响 arrangeSubviews 数组中的视图顺序。它不会影响子视图数组中视图的顺序。
  
- **removeArrangedSubview:** 方法

  从 UIStackView 的 arrangedSubviews 数组删除指定的视图。

  - 参数一：UIView 类型，要删除的视图。

  视图的位置和大小将不再由 UIStackView 管理。但是，此方法不会从 UIStackView 的子视图数组中删除视图。因此，视图仍显示为视图层次结构的一部分。

  要在调用 UIStackView 的 removeArrangedSubview: 方法后阻止视图出现在屏幕上，请通过调用视图的removeFromSuperview 方法从子视图数组中显式删除视图，或将视图的 hidden 属性设置为 YES。

⌛️在 iOS 11 之后增加了下面几个属性和方法来自定义指定的视图的间距：