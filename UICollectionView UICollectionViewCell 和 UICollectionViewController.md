# UICollectionView UICollectionViewCell 和 UICollectionViewController

## UICollectionView

UICollectionView 是用来展示集合视图，实现多列布局。UICollectionView 和 UITableView 使用方式类似，但是 UITableView 仅能展示一列数据。可以简单的把  UICollectionView 理解成多列 UITableView。iOS 系统的「照片」使用的就是 UICollectionView。

### UICollectionView 的风格

UICollectionView 的风格布局不像 UITableView 使用枚举值来定义样式，UICollectionView 的布局使用 UICollectionViewLayout 类来实现。详细见后文。

#### UICollectionView 的 section、row、item



### UICollectionViewLayout 及其子类

由于 UICollectionView 比 UITableView 要复杂得多，因此没有按照类似于UITableView 的 style 的方式来定义，而是专门使用了一个类来对 UICollectionView 的布局和状态进行描述，这就是 UICollectionViewLayout。

在 `initWithFrame:collectionViewLayout:` 初始化方法中，第一个参数是整个 UICollectionView 的位置，第二个参数是一个 UICollectionViewLayout 类型的对象。

在官方文档中对 UICollectionViewLayout 类的描述是「用于为集合视图生成布局信息的抽象基类」。可以看出 UICollectionViewLayout 类其实是一个基类，在平常的开发中使用的都是它的子类 。

UICollectionViewLayout 有两个子类，UICollectionViewFlowLayout 和 UICollectionViewTransitionLayout。

#### UICollectionViewFlowLayout

在开发中常用 UICollectionViewFlowLayout 类来设置 UICollectionView 的布局方式。 它将项目组织成网格的具体布局对象，每个节都有可选的页眉和页脚视图。该类使用 UICollectionViewFlowLayoutDelegateFlowLayout 协议来设置每个节和网格中项目，页眉和页脚的大小。

#### UICollectionViewDelegateFlowLayout 协议

UICollectionViewDelegateFlowLayout 协议继承 UICollectionViewDelegate 协议。

所以其实可以看作像 UITableView 使用 UITableViewDelegate 协议来设置 UITableView 样式的方式一样，当 UICollectionView 的布局使用 UICollectionViewFlowLayout 时， UICollectionView 使用 UICollectionViewDelegateFlowLayout 协议来设置 UICollectionView 的样式。

UICollectionViewDelegateFlowLayout 协议中只有可选方法。

- `collectionView:layout:sizeForItemAtIndexPath:` 指定 item 的 cell 的大小。

- `collectionView:layout:minimumLineSpacingForSectionAtIndex:` 行或列之间的间距。

- `collectionView:layout:minimumInteritemSpacingForSectionAtIndex:` item 之间的间距。

### UICollectionView 的数据源




