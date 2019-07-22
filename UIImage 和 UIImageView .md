# UIImage 和 UIImageView 

> ❎ 未完成
> 
> 1. 动画图像视图。
> 2. 可拉伸图像视图。

## UIImage 

UIImage 继承自 NSObject，它是管理应用程序中图片数据的对象。

使用 UIImage 对象来表示所有类型的图片数据，UIImage 类能够管理底层平台支持的所有图片格式的数据。图片对象可能包含打算在动画中使用的单个图片或一系列图片。

一般通过几种不同的方式使用 UIImage 对象：

- 将 UIImage 分配给 UIImageView 对象，用来在界面中显示图片。
- 使用图片自定义系统控件，如按钮，滑块和分段控件等。
- 将图片直接绘制到视图或其他图形上下文中。
- 将图片传递给可能需要图片数据的其他 API。

虽然 UIImage 对象支持所有平台原生图片格式，但建议对应用程序中的大多数图片使用 PNG 或 JPEG 格式的文件。 图片对象针对这两种格式在读取和显示方面进行了优化，这些格式提供了比大多数其他图片格式更好的性能。**由于 PNG 格式无损，因此用于应用界面中的图片最好使用 PNG 格式**。

### 创建 UIImage

使用此类的方法创建图片对象时，必须将现有图片数据放在文件或数据结构中。无法创建空图片并将内容绘制到其中。创建图片对象有很多选项，每种选项最适合特定情况：

**从缓存加载图像**

- **imageNamed:** 类方法

    返回与指定文件名关联的图像对象。
    
    - 参数一：NSString 类型，文件的名称。如果这是第一次加载图像，则该方法在应用程序的主包中查找具有指定名称的图像。对于 PNG 图像，可以省略文件扩展名。对于所有其他文件格式，请始终包含文件扩展名。
    - 返回值：UIImage 类型，指定文件的图像对象，如果方法找不到指定的图像，则为 `nil`。

    此方法在系统缓存中查找具有指定名称的图像对象，并返回最适合主屏幕的图像的变体。如果匹配的图像对象尚未在缓存中，该方法将从磁盘或可用资源目录中定位和加载图像数据。系统可以在任何时候清除缓存的图像数据以释放内存，不过只能够清除在缓存中但当前未使用的图像。

    如果图像文件只显示一次并希望确保它不会被添加到系统的缓存中，则应使用 `imageWithContentsOfFile:` 方法创建图像，这样图像并不会存储在缓存中，从而节省内存。
    
- **imageNamed:inBundle:compatibleWithTraitCollection:** 类方法

    返回与指定的特征集合兼容的图像对象。
    
    - 参数一：NSString 类型，图像的名称。对于资源目录中的图像，需指定图像资源的名称。对于 PNG 图像，可以省略文件扩展名。对于所有其他文件格式，请始终包含文件扩展名。
    - 参数二：NSBundle 类型，包含图像文件或资源目录的包。指定 `nil` 则会搜索应用程序的主包。
    - 参数三：UITraitCollection 类型，与图像的预期环境相关的特征。使用此参数可确保加载图像的正确变体。如果指定 `nil`，则此方法使用与主屏幕关联的特征。

    此方法在系统缓存中查找具有指定名称的图像对象，并返回最适合主屏幕的图像的变体。如果匹配的图像对象尚未在缓存中，该方法将从磁盘或可用资源目录中定位和加载图像数据。系统可以在任何时候清除缓存的图像数据以释放内存，不过只能够清除在缓存中但当前未使用的图像。
    
    此方法和 `imageNamed:` 作用类似，都会把加载的图片缓存起来。
    
**从文件加载图像**

使用这些方法创建的图像对象都不会被缓存。下面都是的方法都是类方法，UIImage 也提供了对应的对象方法，为了方便就直接调用类方法，所以这里不列出其对应的对象方法。除了下面的方法，还有一些使用 CGImage 和 CIImage 对象创建图像的方法，因为不常用所以没有列出。
    
- **imageWithContentsOfFile:** 类方法

    在指定路径上从文件加载图像数据，创建并返回图像对象。
    
    - 参数一：NSString 类型，文件的完整或部分路径。
    - 返回值：UIImage 类型，创建的图像对象，如果无法创建图像则返回 `nil`。

- **imageWithData:** 类方法

    使用指定的图像数据创建并返回图像对象。
    
    - 参数一：NSData 类型，图像数据。
    - 返回值：UIImage 类型，创建的图像对象，如果无法创建图像则返回 `nil`。

- **imageWithData:scale:** 方法

    使用指定的图像数据和比例因子创建并返回图像对象。

    - 参数一：NSData 类型，图像数据。
    - 参数二：CGFloat 类型，加载图像数据时使用的比例因子。指定比例因子为 `1.0` 会使图像的大小与图像的基于像素的维度相匹配。使用不同的比例因子会更改 UIImage 的 size 属性的大小。
    - 返回值：UIImage 类型，创建的图像对象，如果无法创建图像则返回 `nil`。

**创建动画图像**

- **animatedImageNamed:duration:** 方法

    创建并返回动画图像。
    
    - 参数一：NSString 类型，文件的完整或部分路径（无后缀）。
    - 参数二：NSTimeInterval 类型，动画的持续时间。
    - 返回值：UIImage 类型。

    此方法通过将一系列数字附加到参数一中提供的基本文件名来加载一系列文件。例如，如果参数一以「image」作为其内容，则此方法将尝试从名称为 「image0」、「image1」、「image2」...「image1024」的文件中加载图像。动画图像中包含的所有图像应共享相同的大小和比例。
    
- **animatedImageWithImages:duration:** 方法

    从现有图像集创建并返回动画图像。
    
    - 参数一：NSArray 类型，一组UIImage对象。
    - 参数二：NSTimeInterval，动画的持续时间。
    - 返回值：UIImage 类型。

    动画图像中包含的所有图像共享相同的大小和比例。

### 比较 UIImage

使用相同的缓存图片数据初始化图片对象也有可能使创建的图片对象彼此不同，确定其相等性的唯一方法是使用isEqual: 方法，该方法会比较实际的图片数据。 如下示例代码：

```Objective-C
// 使用相同的图片数据初始化两个图片对象
UIImage* image1 = [UIImage imageNamed:@"MyImage"];
UIImage* image2 = [UIImage imageNamed:@"MyImage"];
 
// 正确的比较方式，该方法正确的比较图片数据
if ([image1 isEqual:image2]) {
   ...
}

// 错误的比较方式，直接比较图片对象可能无效。
if (image1 == image2) {
   ...
}
```

### UIImage 属性

**图像数据**

- **images** 属性

    NSArray 类型，对于动画图像，此属性包含构成动画的完整 UIImage 对象数组。对于非动画图像，此属性的值为 `nil`。

- **scale** 属性

    CGFloat 类型，只读。图像的比例因子。

    如果从名称中包含 `@2x` 的文件加载图像，则比例设置为 `2.0`（`@3x` 时同理）。还可以在从 Core Graphics 图像初始化图像时指定显式的比例因子。假设所有其他图像的比例因子为 `1.0`。

    如果将图像的逻辑大小（存储在 UIImage 的 size 属性的值）乘以此属性的值，则可以获得图像的像素尺寸（以像素为单位）。

- **size** 属性

    CGSize 类型，只读。图像的逻辑尺寸，以点（point）为单位。

    此属性的值反映的是图像当前方向的逻辑大小。将 size 属性的值乘以 scale 属性的值，来获取图像的像素尺寸。

- **imageOrientation** 属性

    UIImageOrientation 类型，只读。图像的方向。

    图像方向会影响绘制时图像数据的显示方式。默认情况下，图像以「向上」方向显示。但是如果图像具有关联的元数据（例如 EXIF 信息），则此属性包含该元数据指示的方向。
  
    有关此属性的可能值的列表，请参阅官方文档 [UIImageOrientation](https://developer.apple.com/documentation/uikit/uiimageorientation?language=objc)。

- **resizingMode** 属性

    UIImageResizingMode 类型，只读。图像的大小调整模式。默认值为 `UIImageResizingModeTile`。

### 可伸缩图像

可伸缩图像通常用于创建可以增长或缩小以填充可用空间的背景。可以通过使用 `resizableImageWithCapInsets:` 或`resizableImageWithCapInsets:resizingMode:` 方法向现有图像添加 insets 来定义可伸缩图像。insets 将图像细分为两个或多个部分。为每个 insets 指定非零值将生成一个分成九部分的图像，如图所示。

![使用insets定义可伸展区域](media/15508274740144/%E4%BD%BF%E7%94%A8insets%E5%AE%9A%E4%B9%89%E5%8F%AF%E4%BC%B8%E5%B1%95%E5%8C%BA%E5%9F%9F.png)

每个 insets 定义了图像在给定维度中不拉伸的部分。图像顶部和底部插图内的区域保持固定高度，左右插图内的区域保持固定宽度。下图显示了九部分图像的每个部分如何伸展，因为图像本身被拉伸以填充可用空间。图像的角不会改变大小，因为它们在水平和垂直插入内部。

![九部分图像的可拉伸部分](media/15508274740144/%E4%B9%9D%E9%83%A8%E5%88%86%E5%9B%BE%E5%83%8F%E7%9A%84%E5%8F%AF%E6%8B%89%E4%BC%B8%E9%83%A8%E5%88%86.png)

⌛️

## UIImageView

UIImageView 也称为「图像视图」，其继承自 UIView，用于在界面中显示单个图像或一系列动画图像的对象。使用 UIImageView 可以高效地绘制任何使用 UIImage 对象指定的图像。

### UIImageView 内容模式

UIImageView 使用其 `contentMode` 属性和 UIImage 本身的配置来确定如何显示图像。最好指定尺寸与图像视图尺寸完全匹配的图像，不过图像视图可以缩放图像来适应其自身的空间。如果图像视图本身的大小发生变化，它会根据需要自动缩放图像。

UIImageView 的 `contentMode` 属性继承自其父类 UIView，它是 UIViewContentMode 枚举类型。在 UIImageView 中可以使用其来调整图像的显示样式，具体如下表：

| UIViewContentMode 枚举值 | 说明 | 效果 |
| :-: | :-: | :-: |
| UIViewContentModeScaleToFill | 更改图像的宽高比来缩放图像<br>以适应图像视图的大小。<br>（**图像会变形**） | ![UIViewContentModeScaleToFil](media/15508274740144/UIViewContentModeScaleToFill.png) |
| UIViewContentModeScaleAspectFit | 保持图像的宽高比来缩放图像<br>以适合图像视图的大小，<br>图像视图中没有被图像覆盖<br>到的区域是透明的。<br>（**图像不会变形**）| ![UIViewContentModeScaleAspectFit](media/15508274740144/UIViewContentModeScaleAspectFit.png) |
| UIViewContentModeScaleAspectFill | 保持图像的宽高比来缩放图像<br>以适合图像视图的大小。<br>会剪裁掉超出图像视图边界的部分。 | ![UIViewContentModeScaleAspectFil](media/15508274740144/UIViewContentModeScaleAspectFill.png) |
| UIViewContentModeCenter | 将图像放置于图像视图边界的中心<br>图像大小保持不变。 | ![UIViewContentModeCente](media/15508274740144/UIViewContentModeCenter.png) |
| UIViewContentModeTop | 将图像和图像视图边界的顶部中心对齐<br>保持大小保持不变。 | ![UIViewContentModeTop](media/15508274740144/UIViewContentModeTop.png) |
| UIViewContentModeBottom | 将图像和图像视图边界的底部中心对齐<br>保持大小保持不变。 | ![UIViewContentModeBotto](media/15508274740144/UIViewContentModeBottom.png) |
| UIViewContentModeLeft | 将图像和图像视图边界的左边中心对齐<br>保持大小保持不变。 | ![UIViewContentModeLeft](media/15508274740144/UIViewContentModeLeft.png) |
| UIViewContentModeRight | 将图像和图像视图边界的右边中心对齐<br>保持大小保持不变。 | ![UIViewContentModeRight](media/15508274740144/UIViewContentModeRight.png) |
| UIViewContentModeTopLeft | 将图像和图像视图边界的左上角对齐<br>保持大小保持不变。 | ![UIViewContentModeTopLeft](media/15508274740144/UIViewContentModeTopLeft.png) |
| UIViewContentModeTopRight | 将图像和图像视图边界的右上角对齐<br>保持大小保持不变。 | ![UIViewContentModeTopRight](media/15508274740144/UIViewContentModeTopRight.png) |
| UIViewContentModeBottomLeft | 将图像和图像视图边界的左下角对齐<br>保持大小保持不变。 | ![UIViewContentModeBottomLeft](media/15508274740144/UIViewContentModeBottomLeft.png) |
| UIViewContentModeBottomRight | 将图像和图像视图边界的右上角对齐<br>保持大小保持不变。 | ![UIViewContentModeBottomRight](media/15508274740144/UIViewContentModeBottomRight.png) |
| UIViewContentModeRedraw | 调用 setNeedsDisplay 方法在<br>更改边界时重新显示视图。<br>尽可能避免使用这个值。 | ![UIViewContentModeRedra](media/15508274740144/UIViewContentModeRedraw.png) |

> ⚠️ **注意**
> 
> 缩放图像也是比较耗费性能的操作，为了最大化图像时图代码的性能，可以考虑以下两点：
> 
> 1. **缓存常用图像的缩放版本。**如果某些大图像在缩小的缩略图视图中频繁显示，考虑提前创建缩小的图像并将其存储在缩略图缓存中。
> 2. **尽量使用大小接近图像视图的图像。**而不是将过大的图像或过小的图像分配给图像视图。

### UIImageView 透明度

图像被合成到 UIImageView 的背景上，然后合成到窗口的其他部分中。所以图像如果有一定透明度，都会显示 UIImageView 的背景。并且，UIImageView 的透明度取决于其自身的透明度和它显示的图像的透明度。当 UIImageView 及其图像都具有透明度时，UIImageView 使用 Alpha 通道混合来组合这两者。

- 如果 UIImageView 的 `opaque` 属性为 `YES`，则图像的像素将合成在 UIImageView 的背景颜色之上，并忽略 UIImageView 的 `alpha` 属性。

- 如果 UIImageView 的 `opaque` 属性为 `NO`，则图像中每个像素的 alpha 值乘以 UIImageView 的 alpha 值，结果值将成为该像素的实际 alpha 值。如果图像没有 Alpha 通道（即图像本身不透明），则图像的每个像素的 alpha 值为 `1.0`。

> ⚠️ **注意**
>
> 同时使用有透明度的图片和有透明度的 UIImageView 时（即上面第二种情况），非常耗费性能，所以一般使用不透明图像配合有透明度的 UIImageView 来实现需求透明需求，不过应该尽可能使 UIImageView 不透明。

### UIImageView 动画

⌛️

### UIImageView 属性和方法

**初始化方法**

- **initWithImage:** 方法

    返回使用指定图像初始化的图像视图。

    - 参数一：UIImage 类型，要在图像视图中显示的图像。可以指定包含动画图像序列的图像对象。
    - 返回值：UIImageView 对象。

    此方法默认将图像视图的 `userInteractionEnabled` 属性设置为 `NO` 来禁用用户与图像视图的交互。
  
    如果指定持续时间大于 `0` 的动画图像，则图像视图会自动开始播放动画。

- **initWithImage:highlightedImage:** 方法

    返回使用指定的常规和高亮显示的图像初始化的图像视图。

    - 参数一：UIImage 类型，要在图像视图中显示的普通图像。
    - 参数二：UIImage 类型，要在图像视图中显示的高亮图像。
    - 返回值：UIImageView 对象。

    此方法默认将图像视图的 `userInteractionEnabled` 属性设置为 `NO` 来禁用用户与图像视图的交互。
  
    如果指定持续时间大于 `0` 的动画图像，则图像视图会自动开始播放动画。

**配置图像视图**

- **image** 属性

    UIImage 类型，图像视图中展示的图像。

    更改此属性中的图像不会自动更改图像视图的大小。设置图像后，调用 sizeToFit 方法根据新图像和约束重新计算图像视图的大小。

    此属性设置为在初始化图像视图时指定的图像。如果未使用 `initWithImage:` 或 `initWithImage:highlightedImage:` 方法初始化图像视图，则此属性的初始值为 `nil`。

- **highlightedImage** 属性

    UIImage 类型，图像视图中展示的高亮视图。

    当图像视图的高亮显示属性为 `YES` 时，图像视图将显示此属性中的图像。如果 `highlightAnimationImages` 属性包含有效的图像集，则使用这些动画图像。

    此属性设置为在初始化时指定的图像。如果未使用 `initWithImage:highlightedImage:` 方法初始化图像视图的高亮图像，则此属性的初始值为 `nil`。
    
- **highlighted** 属性

    BOOL 类型，确定图像是否高亮显示。
    
    此属性确定图像视图显示常规图像还是高亮图像。当此属性设置为 `YES` 时，非动画图像将使用 `highlightedImage` 属性的值，动画图像将使用 `highlightedAnimationImages` 属性的值。如果这两个属性都设置为 `nil` 或高亮显示设置为 `NO`，它将使用 `image` 和 `animationImages` 属性。
    
   
    

    

