# iOS 数据持久化

## NSUserDefaults

### NSUserDefaults 简介

NSUserDefaults 是 Foundation 框架中的一个单例类。它是用户默认数据库的接口，可以在应用程序持久存储键值对。iOS 应用程序只有一个 NSUserDefaults 类的实例，叫做**默认对象**。

NSUserDefaults 类把数据以键值对的形式存储在应用程序的默认对象中。在应用程序运行时，使用 NSUserDefaults 对象从用户的默认对象中读取应用程序使用的默认值（例如可以用来判断用户之前运行应用程序时是否是登录状态，如果是则可以直接进入主界面，否则进入用户登录页面）。

默认对象必须是一个[属性列表](https://zh.wikipedia.org/zh-hans/%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8)，属性列表是一种 XML 格式的文件，拓展名为 plist。因为是 plist 文件，所以能够存储的类型必须是 plist 文件可以存储的类型：即 NSData，NSString，NSNumber，NSDate，NSArray 或 NSDictionary 类型。如果要存储图片，可以先将其**归档**为 NSData 类型，再存入 plist 文件。

这个属性列表存储在应用沙盒文件的 **Library/Preferences** 路径中，这个文件夹中的文件用来保存应用的所有偏好设置，iOS 的系统应用「设置」会在该目录中查找应用的相关设置信息。iTunes 同步设备时会备份该目录。

> <i class="fas fa-exclamation-circle"></i> 注意
> 
> 1. 如果 NSArray 或者 NSDictionary 中存在不是上面提到的几种类型中的，那么也是无法存储的。例如在 NSDictionary 中存在 NSNull 类型，就无法存储。特别要注意这一点，因为从服务器中返回的数据如果为空，那么一般会解析成 NSNull 类型，此时是无法存储在 plist 文件中的。
> 2. plist 文件中存储的对象时不可变的，比如无法存储 NSMutableArray 类型的对象。

### NSUserDefaults 常用方法和属性

#### 获取 NSUserDefaults 默认对象

- `standardUserDefaults` 属性

    返回 NSUserDefaults 的对象。一般在程序中使用 `[NSUserDefaults standardUserDefaults]` 语句来获取单例类对象。

    如果 NSUserDefaults 对象尚不存在，则使用包含以下域名称的搜索列表创建它，顺序如下：
    
    1. 仅对托管设备而言，包含管理员设置的默认值的域
    2. NSArgumentDomain，由从应用程序参数解析的缺省值组成
    3. 对于仅由教育机构管理的设备，包含iCloud键值存储中设置的默认值的域
    4. 由应用程序的bundle标识符标识的域
    5. NSGlobalDomain，由所有应用程序都可以看到的默认值组成
    6. NSRegistrationDomain，一组临时默认值，应用程序可以设置这些值以确保搜索总是成功的

#### 存储默认值

- `setObject:forKey:` 对象方法

    为指定键名设置值。无返回值。
    
    - 第一个参数：id 类型，要存储的对象。
    - 第二个参数：NSString 类型，与值关联的键名。

#### 获取默认值

- `objectForKey:` 对象方法

    从指定的键名获取对象。
    
    - 第一个参数：NSString 类型，键名。
    - 返回值：与指定键名关联的对象，如果未找到键则返回 nil。

#### 删除默认值

- `removeObjectForKey:` 对象方法

    删除指定的默认值。无返回值。
    
    - 第一个参数：NSString 类型，键名。
    
#### 强制数据更新

- `synchronize` 对象方法

    这个方法是属性列表马上强制写入默认数据库。这种方法是不必要的，不应该使用。
    
    在设置了属性列表之后，但是属性列表并不是马上写入默认数据库中存储的。如果需要马上存储，则可以调用此方法。
    
    - 返回值：如果数据已成功保存到磁盘，则返回 YES，否则返回 NO。


<head> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
<script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">