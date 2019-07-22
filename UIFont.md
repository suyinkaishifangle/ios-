# UIFont

> ⌛️ **待完成**
> 
> 1. 导入外部字体并使用
> 2. 动态处理字体

UIFont 继承自 NSObject，它是获取和设置字体信息的接口。UIFont 提供对字体特征的访问，还为系统提供在布局期间使用的字体字形信息的访问。

不使用 alloc 和 init 初始化方法创建 UIFont 对象。使用 UIFont 的类方法（例如 `preferredFontForTextStyle:`）来查找和检索所需的字体对象。这些方法检查具有指定特征的现有字体对象，如果存在则返回它。否则，它们会根据所需的字体特征创建一个新的字体对象。

字体对象是不可变的，因此可以安全地在应用程序中的多个线程中使用它们。

在 UIKit 中的字体大小均使用 pt 为单位来描述。

## 字体的相关知识

- **字体系列名称和字体名称**

    字体系列名称（familyName）是指整个字体的系列，例如：`PingFang SC`。
    
    PingFangSC 中包含了不同粗细的字体，一般跟随在字体系列名称后面，例如：`PingFangSC-Regular`。这种包含字体系列名称和字体样式名称的就叫做字体名称（fontName）。
    
    使用下面的代码可以打印出 iOS 设备上可用的所有字体名称及字体系列名称：
    
    ```Objective-C
    NSArray *familys = [UIFont familyNames];
    for (int i = 0; i < familys.count; i++) {
        NSString *family = [familys objectAtIndex:i];
        NSLog(@"family = %@",family);
        
        NSArray *fonts = [UIFont fontNamesForFamilyName:family];
        for (int j = 0; j < fonts.count; j++) {
            NSString *font = [fonts objectAtIndex:j];
            NSLog(@"font = %@",font);
        }
    }
    ```

## UIFont 属性和方法

### 创建字体

- **preferredFontForTextStyle:** 类方法

    返回指定文本样式的系统字体对象，并根据用户选定的内容大小类别进行适当缩放。
    
    - 参数一：UIFontTextStyle 类型，要返回字体的文本样式。
    - 返回值：UIFont 对象。

    UIFontTextStyle 是别名，包含描述用于字体的首选样式的常量（例如：正文、标注、脚注的字体等），参考 [UIFontTextStyle](https://developer.apple.com/documentation/uikit/uifonttextstyle?language=objc)。
    
- **fontWithName:size:** 类方法

    创建并返回指定字体名称和大小的字体对象。
    
    - 参数一：NSString 类型，完全指定的字体名称。 此名称包含字体系列名称和字体的特定样式信息。
    - 参数二：CGFloat 类型，缩放字体的大小（以 pt 为单位，下同），该值必须大于 0.0。
    - 返回值：UIFont 对象。

- **fontWithDescriptor:size:** 类方法

    返回与给定字体描述符匹配的字体。
    
    - 参数一：UIFontDescriptor 类型，要匹配的字体描述符。
    - 参数二：CGFloat 类型，缩放字体的大小。如果大于 0.0，则它优先于描述符中的 UIFontDescriptorSizeAttribute。
    - 返回值：UIFont 对象。

    在大多数情况下，一般简单地使用 `fontWithName:size:` 来创建标准缩放字体。
    
- **fontWithSize:** 方法

    返回与接收者相同但具有指定大小的字体对象。
    
    - 参数一：CGFloat 类型，新字体对象的字体大小，该值必须大于 0.0。
    - 返回值：UIFont 对象。
    
### 创建系统字体

- **systemFontOfSize:** 类方法

    返回用于指定大小的标准接口项的字体对象。
    
    - 参数一：CGFloat 类型，字体的大小，该值必须大于 0.0。
    - 返回值：UIFont 对象。
    
- **systemFontOfSize:weight:** 类方法

    返回用于指定大小和权重的标准接口项的字体对象。
    
    - 参数一：CGFloat 类型，字体的大小，该值必须大于 0.0。
    - 参数二：UIFontWeight 类型，字体的权重。参考 [Font Weights](https://developer.apple.com/documentation/uikit/uifontdescriptor/font_weights?language=objc)。
    - 返回值：UIFont 对象。

- **boldSystemFontOfSize:** 类方法

    返回用于以指定大小的粗体字体呈现的标准接口项的字体对象。
    
    - 参数一：CGFloat 类型，字体的大小，该值必须大于 0.0。
    - 返回值：UIFont 对象。

- **italicSystemFontOfSize:** 类方法

    返回用于以指定大小以斜体显示的标准接口项的字体对象。
    
    - 参数一：CGFloat 类型，字体的大小，该值必须大于 0.0。
    - 返回值：UIFont 对象。

- **monospacedDigitSystemFontOfSize:weight:** 类方法

    返回用于需要数字之间固定距离的标准接口项的字体对象。
    
    - 参数一：CGFloat 类型，字体的大小，该值必须大于 0.0。
    - 参数二：UIFontWeight 类型，字体的权重。
    - 返回值：UIFont 对象。

    在 iOS 9 及更高版本中，系统字体使用比例间距。显示数字数据时，可以使用此方法检索用于显示该数据的等宽字体。使用等宽字体，每个数字占用相同的空间，这使得更界面更加美观易读。
    
### 获取可用的字体名称

- **familyNames** 类属性

    只包含 NSString 的 NSArray 类型，只读。返回系统可用的**字体系列名称**的数组。 
    
    字体系列名称对应于字体的基本名称，例如「Times New Roman」。可以将返回的字符串传递给 `fontNamesForFamilyName:` 方法，以检索该系列可用的字体名称列表。然后可以使用相应的字体名称来检索实际的字体对象。
    
- **fontNamesForFamilyName:** 类方法

    返回指定字体系列中可用的字体名称数组。
    
    - 参数一：NSString 类型，字体系列的名称。
    - 返回值：只包含 NSString 的 NSArray 类型，数组中每个对象都包含与指定系列关联的字体名称。

    获取到字体名称之后，可以将字体名称作为参数传递给 `fontWithName:size:` 方法以检索实际的字体对象。
    
### 获取字体信息

- **familyName** 属性

    NSString 类型，只读。字体系列名称。
    
    字体系列名称用于标识一种或多种特定字体。此属性中的值仅供应用程序的内部使用，不应显示。
    
- **fontName** 属性

    NSString 类型，只读。字体名称。
    
    此属性中的值仅供应用程序的内部使用，不应显示。
    
- **pointSize** 属性

    CGFloat 类型，只读。接收者的点（point）大小，或具有非标准矩阵的字体的有效垂直点大小。
    
**获取系统字体信息**

- **labelFontSize** 类属性

    CGFloat 类型，只读。返回用于 UILabel 的标准字体大小。
    
    在 iOS 中属性的值为 17。

- **buttonFontSize** 类属性

    CGFloat 类型，只读。返回用于 UIButton 的标准字体大小。
    
    在 iOS 中属性的值为 18。
    
- **smallSystemFontSize** 类属性

    CGFloat 类型，只读。返回标准小型系统字体的大小。
    
    在 iOS 中属性的值为 12。
    
- **systemFontSize** 类属性

    CGFloat 类型，只读。返回标准系统字体的大小。
    
    在 iOS 中属性的值为 14。