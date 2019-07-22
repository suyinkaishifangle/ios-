# NSLocale

NSLocale 继承自 NSObject，表示一个包含这个地区的语言与文化习俗的基础类。一个 NSLocale 的实例包含了针对这个地区内特定一群人的所有语言文化基准，其中包括：语言；键盘布局；数字、日期和时间格式；货币；排序和分类；符号、颜色使用等。

每一个 NSLocale 实例对应着一个地区标识符，例如 en_US，fr_FR，ja_JP 和 en_GB，这些标识符包含一个语言码（例如 en 代表英语）和一个国家/地区码（例如 US 代表美国）。并且每一个 NSLocale 实例都包含了语言码、国家/地区码、文字码、货币码、货币符号等一系列只读属性（更多属性参考下文中「获取地区的相关信息」）。这些属性可以帮助开发者来根据用户的不同喜好设置展示不同的文字、界面等等。

可以使用 `initWithLocaleIdentifier:` 方法初始化任意数量的 NSLocale 对象。但是一般使用当前用户首选项的 NSLocale 实例。使用 `currentLocale` 属性获取与当前用户的首选项匹配的 NSLocale 实例。如果需要在用户更改地区时收到通知，需要注册 `NSCurrentLocaleDidChangeNotification` 通知。或者可以使用`autoupdatingCurrentLocale` 属性来获取使用用户配置设置自动更新的 NSLocale 实例。

经常将 NSLocale 与格式器结合使用。例如，NSDateFormatter 类具有一个 `locale` 属性，可确保将日期转换为符合用户对日期格式的期望的字符串。默认情况下，`locale` 属性指示用户的当前地区。可以将其设置为另一个地区对象以获取不同的输出。

对于 iOS 系统来说，更改地区的首选项设置会重新启动 Springboard 并退出正在运行的应用程序，以此让用户重新运行 iOS 应用程序时能够使用该应用程序的新的语言设置。对于 Mac 应用程序，在重新启动应用程序之前，语言不会更改。iOS 中的地区首选项在 ⚙️ **设置 ➤ 通用 ➤ 语言和地区** 选项中，并且分别有语言选项和地区选项。

## 本地化和国际化

- 本地化（localization，简称 l10n），将应用程序适应某一个特定市场的操作。
- 国际化（internationalization，简称 i18n），将应用程序本地化的准备操作。

国际化是本地化的必要不充分条件。国际化是指去本地化，移除本地语言写的提示信息，异常信息，区域信息等，采用国际标准或者提取资源。本地化是在国际化的基础上针对不同地区进行个性化。

## 相关概念

### 语言码

语言码是表示语言的代码。使用两个字母的 *ISO 639-1* 标准（首选）或三个字母的 *ISO 639-2* 标准。如果某一种语言没有 *ISO 639-1* 代码可用，则使用 *ISO 639-2* 代码。例如，夏威夷语没有 *ISO 639-1* 代码，所以使用 *ISO 639-2* 代码。下表是部分语言代码，有关 *ISO 639-1* 和  *ISO 639-2* 代码的完整列表，参考[表示语言名称的代码](http://www.loc.gov/standards/iso639-2/php/English_list.php)。

| 语言 | ISO 639-1 标准 | ISO 639-2 标准 |
| :-: | :-: | :-: |
| 汉语 | zh | chi/zho |
| 英语 | en | eng |
| 法国 | fr | fre |
| 德语 | de | ger |
| 日语 | ja | jpn |
| 夏威夷 | 无 | haw |

### 国家/地区码

国家/地区码是表示国家/地区的代码。使用 *ISO 3166-1* 标准的二位字母代码，部分国家/地区码如下表所示，有关 *ISO 3166-1* 代码的完整列表，参考[维基百科：ISO 3166-1](https://zh.wikipedia.org/wiki/ISO_3166-1)。

| 国家/地区 | ISO 3166-1 标准 |
| :-: | :-: |
| 中国 | CN |
| 香港 | HK | 
| 澳门 | MO | 
| 台湾 | TW | 
| 法国 | FR | 
| 英国 | GB |
| 德国 | DE | 
| 美国 | US | 
| 日本 | JP | 

### 文字码

文字码是表示文字种类的代码，使用 *ISO 15924* 标准的四位的字母编码，部分文字码如下表所示，有关 *ISO 15924* 代码的完整列表，参考[维基百科：ISO 15924 列表](https://zh.wikipedia.org/wiki/ISO_15924%E5%88%97%E8%A1%A8)。

| 文字种类 | ISO 15924 标准 |
| :-: | :-: |
| 简体中文 | Hans |
| 繁体中文 | Hant | 

### 货币码

货币码是表示货币或资金名称的代码，使用 *ISO 4217* 标准的三位字母编码，部分货币码如下表所示，有关 *ISO 4217* 代码的完整列表，参考[维基百科：ISO 4217 列表](https://zh.wikipedia.org/wiki/ISO_4217)。

| 货币名称 | ISO 4217 标准 |
| :-: | :-: |
| 人民币元 | CNY |
| 欧元 | EUR | 
| 美元 | USD |

### 语言标识符

语言标识符标识一种语言、方言或文字。指定语言标识符有三种方式：

- 指定语言使用语言码。
- 指定语言的方言使用连字符 `-` 将语言码与语言码组合。
- 指定语言的文字使用连字符 `-` 将语言码与文字码组合在一起。

| 语言标识符语法 | 描述 | 例子 |
| :-: | :-: | :-: |
| 语言码 | 仅指定语言 | 汉语：zh<br>英语：en<br>法语：fr |
| 语言码-国家/地区码 | 指定语言及方言 | 中国的中文：zh-CN<br>香港的中文：zh-HK |
| 语言码-文字码 | 指定语言及文字 | 简体中文：zh-Hans<br>繁体中文：zh-Hant|

### 地区标识符

地区标识符标识区域及其文化约定。其在 api 中使用，比如 NSLocale、NSDateFormatter、NSNumberFormatter 和 NSCalendar 类需要地区来格式化数据。例如日期，时间和数字的格式。

要指定地区标识符，使用下划线 `_` 将语言标识符与国家/地区码组合，如下表所示。例如，英国的英语使用者的地区标识符是 `en_GB`，而美国的英语使用者的地区标识符是 `en_US`。

| 地区标识符语法 | 描述 | 例子 |
| :-: | :-: | :-: |
| 语言码 | 仅指定语言，未指定地区和文字 | zh<br>en |
| 语言码-国家/地区码 | 指定语言及地区，未指定文字 | zh-CN<br>zh-HK |
| 语言码-文字码 | 指定语言及文字，未指定地区 | zh-Hans<br>zh-Hant|
| 语言码-文字码_国家/地区码 | 指定语言、地区和文字 | zh-Hans_CN<br>zh-Hans_HK |

当存在歧义时，仅在地区标识符中使用文字指示符。例如，由于繁体中文是香港的默认语言，使用 `zh_HK`，其中 `zh` 是繁体中文的语言代码，`HK` 是香港地区的代码。对于在香港使用的简体中文，使用 `zh-Hans_HK` 作为地区标识符，其中 `zh-Hans` 是简体中文的文字码。

地区标识符的后面能够添加上一些键值对，通过使用 `@` 符号与前面的部分分隔，这些键值对表示 NSLocale 实例的部分地区信息属性的值，如果没有对应键值对的属性值是默认的。。例如：`en_US@calendar=japanese`，后面的 `@calendar=japanese` 表示使用的日历为 `日本历`，而其他的地区信息的属性值为默认。

## NSLocale 属性和方法

### 创建和初始化

- **localeWithLocaleIdentifier:** 方法

    返回使用给定的地区标识符初始化的 NSLocale 对象。
    
    - 参数一：NSString 类型，地区标识符。
    - 返回值：NSLocale 对象。

- **initWithLocaleIdentifier:** 方法

    使用给定的地区标识符初始化的 NSLocale 对象。
    
    - 参数一：NSString 类型，地区标识符。
    - 返回值：NSLocale 对象。

### 获取用户的地区和语言设置

- **preferredLanguages** 类方法

    包含 NSString 对象的 NSArray 类型，只读。用户首选语言的有序列表。
    
    此属性的数组的字符串就是地区标识符。此属性获取的值是用户在 ⚙️ **设置** ➤ **通用** ➤ **语言与地区** ➤ **首选语言顺序** 列表中设置的语言，并且会加上 ⚙️ **设置** ➤ **通用** ➤ **语言与地区** ➤ **地区** 选项中设置的国家/地区代码。例如 **首选语言顺序** 列表为：简体中文、Engsih；并且 **地区** 选项为：香港，那么此属性的数组中的值按顺序是：`zh-Hans-HK`、`en-HK`。
    
    > 🔥 **提醒**
    >
    > **iPhone 语言** 选项是修改 iOS 系统及应用程序的语言。默认没有修改过 **iPhone 语言** 选项的设备没有出现 **首选语言顺序** 列表，需要修改一次 **iPhone 语言** 选项，然后就会出现 **首选语言顺序** 列表，但是 **iPhone 语言** 选项所指定的语言会是 **首选语言顺序** 列表中的第一项。

- **currentLocale** 属性

    NSLocale 类型，只读。表示用户的应用程序当前的 NSLocale 对象。
    
    下面对此属性的 NSLocale 实例的一些相关属性值的由来做一些说明：
    
    - `languageCode` 属性和 `scriptCode` 属性

        由 ⚙️ **设置** ➤ **通用** ➤ **语言与地区** ➤ **首选语言顺序** 列表中具有的语言和应用程序支持的语言来确定。
        
        例如：**首选语言顺序** 列表为：「简体中文、Engsih」；如果应用程序支持「简体中文」，那么 `languageCode` 属性值为 `zh`，`scriptCode` 属性值为 `Hans`。如果应用程序不支持「简体中文」，但是支持「Engsih」，那么 `languageCode` 属性值为 `en`，`scriptCode` 属性值为 `nil`（因为英语没有文字码）。
        
        > ⚠️ **注意**
        >
        > 一般 iOS 应用程序至少支持英语，当 **首选语言顺序** 列表中不包含英语但应用程序只支持英语时，那么`languageCode` 属性值也会为 `en`。
    
    - `countryCode` 属性、`currencyCode` 属性和 `currencySymbol` 属性

        由 ⚙️ **设置** ➤ **通用** ➤ **语言与地区** ➤ **地区** 选项决定。

        例如：**地区** 选项为：「香港」，`countryCode` 属性值为：`HK`；`currencyCode` 属性值为：`HKD`；`currencySymbol` 属性值为：`HK$`。
        
    - `calendarIdentifier` 属性

        由 ⚙️ **设置** ➤ **通用** ➤ **语言与地区** ➤ **日历** 选项决定。
        
        例如：**日历** 选项为：「佛教日历」，`calendarIdentifier` 属性值为：`buddhist`。
        
    - `localeIdentifier` 属性

        主要由 `languageCode` 属性 `scriptCode` 属性以及 `countryCode` 属性构成，但是如果某一些属性值不是默认的，那么会以类似 `calendar=buddhist` 的形式添加在 `@` 符号后面，并以 `:` 分号隔开。例如：`zh-Hant_HK@collation=pinyin;currency=CNY;calendar=buddhist`

- **autoupdatingCurrentLocale** 类属性

    NSLocale 类型，只读。跟踪用户当前首选项地区。
    
    其实例的属性参考 NSLocale 类的 `currentLocale` 属性，它们两者的区别是此属性能够跟踪用户的修改地区和语言的相关设置，而 `currentLocale` 属性不行。大多数情况使用此属性。
    
- **systemLocale** 类属性

    NSLocale 类型，只读。系统初始的本地化信息。
    
    其中大部分的地区相关的信息的属性的值都是 `nil`，但是地区标识符不是 `nil`，而是空的字符串对象。例如：`[NSLocale systemLocale].countryCode] = nil;`
    
- **NSCurrentLocaleDidChangeNotification** 通知

    NSNotificationName 类型，指示用户的语言和地区已更改。
    
    如果应用显示受语言和地区影响的内容（日期，时间，数字等），请注册此通知，使用它触发应用程序界面的更新。
    
### 标识符之间的转换

- **canonicalLocaleIdentifierFromString:** 类方法

    返回给定地区标识字符串的规范标识符。
    
    - 参数一：NSString 类型，地区标识符。
    - 返回值：NSString 类型，地区的规范标识符。

- **canonicalLanguageIdentifierFromString:** 类方法

    通过将任意地区标识字符串映射到规范标识符，返回规范的语言码。
    
    - 参数一：NSString 类型，任意地区标识符的字符串表示形式。
    - 返回值：NSString 类型，表示指定的任意地区标识符的规范语言码。

- **componentsFromLocaleIdentifier:** 类方法

    返回解析地区标识符结果的字典。
    
    - 参数一：NSString 类型，由语言、文字、国家/地区、变体和关键字/值对组成的地区标识符。例如：`en_US@calendar=japanese`。
    - 返回值：NSDictionary 类型，它将字符串解析为地区标识符的结果。键是与地区标识符组件对应的 NSLocaleKey 常量，值是 NSString 类型的常量。

    例如，地区标识符 `en_US@calendar=japanese` 产生具有三个条目的字典：`NSLocaleLanguageCode=en`，`NSLocaleCountryCode=US`，以及 `NSLocaleCalendar=NSJapaneseCalendar`。
    
- **localeIdentifierFromComponents:** 类方法

    从给定字典中指定的组件返回地区标识符。
    
    - 参数一：NSDictionary 类型，包含地区标识符的字典。
    - 返回值：NSString 类型，由语言、文字、国家/地区、变体和关键字/值对组成的地区标识符。

    此方法与 `componentsFromLocaleIdentifier:` 方法刚好相反，例如把字典 `{NSLocaleLanguageCode =“en”，NSLocaleCountryCode =“US”，NSLocaleCalendar = NSJapaneseCalendar}` 变为 `en_US@calendar=japanese` 标识符。
    
### 获取地区的相关信息
    
- **localeIdentifier** 属性

    NSString 类型，只读。地区标识符。

    地区标识符的例子：`en_GB`，`es_ES_PREEURO` 和 `zh-Hant_HK_POSIX@collation=pinyin;currency=CNY`。
    
    传递 NSLocaleIdentifier 键时，此属性包含 `objectForKey:` 方法返回的相同值。
   
    此属性中保存的值可能与用于初始化的地区标识符不同，因为 NSLocale 可能会在初始化期间对其进行规范化。
    
- **countryCode** 属性
    
    NSString 类型，只读。国家/地区码。
    
- **languageCode** 属性

    NSString 类型，只读。语言码。

- **scriptCode** 属性

    NSString 类型，只读。文字码。
    
- **currencyCode** 属性

    NSString 类型，只读。货币码。

- **currencySymbol** 属性

    NSString 类型，只读。货币符号。
    
    例如：`¥`、`$` 和 `€`。
    
- **variantCode** 属性
    
    NSString 类型，只读。variant 码。
    
- **exemplarCharacterSet** 属性

    NSCharacterSet 类型，只读。地区的示例字符集。
    
- **collationIdentifier** 属性

    NSString 类型，只读。排序规则标识符。
     
- **collatorIdentifier** 属性

    NSString 类型，只读。排序器标识符。
    
- **usesMetricSystem** 属性

    BOOL 类型，只读。指示 NSLocal 实例是否使用公制。
    
- **decimalSeparator** 属性

    NSString 类型，只读。小数分隔符。
    
- **groupingSeparator** 属性

    NSString 类型，只读。分组分隔符。
     
     例如：`,` 和 ` `（空格）。
     
- **calendarIdentifier** 属性

    NSString 类型，只读。日历标识符。
    
- **quotationBeginDelimiter** 属性

    NSString 类型，只读。开始双引号。
    
    例如：`“`、`„`、`«` 和 `「`。
    
- **quotationEndDelimiter** 属性

    NSString 类型，只读。结束双引号。
    
    例如：`”`、`“`、`»` 和 `」`。
    
- **alternateQuotationBeginDelimiter** 属性

    NSString 类型，只读。开始单引号。
    
    例如：`‘`、`‹` 和 `『`。
    
- **alternateQuotationEndDelimiter** 属性

    NSString 类型，只读。结束单引号。
    
    例如：`’`、`›` 和 `』`。

### 获取地区的本地显示信息

- **localizedStringForLocaleIdentifier:** 方法

    返回指定地区标识符的本地化字符串。
    
    - 参数一：NSString 类型，想要本地化名称的地区标识符。
    - 返回值：NSString 类型，本地化名称。

    例如：
    
    - 在 en_US 地区调用此方法，参数一传递 `fr_FR`，生成字符串 `French(France)`。
    - 在 zh-CN 地区调用此方法，参数一传递 `fr_FR`，生成字符串 `法文（法国）`。

    此等价于调用 `displayNameForKey:value:` 方法来传递 `NSLocaleIdentifier` 键和参数一的值。

    > ⚠️ **注意**
    >
    > 无论应用程序是否支持该本地化的语言，此方法返回的字符串文字类型不受影响。例如：**首选语言** 列表只有英语，但是 **地区** 是 `zh-CN`，返回的文字依旧是「简体中文」，下面类似的方法同理。
    
更多类似的方法参考官方文档。  
    
### 获取标识符和代码

- **availableLocaleIdentifiers** 类属性

    包含 NSString 对象的 NSArray 类型，只读。系统可用的地区标识符列表。
    
    地区标识符以语言码开头，通常包括国家/地区码，偶尔还包含文字码。
    
- **ISOCountryCodes** 类属性

    包含 NSString 对象的 NSArray 类型，只读。已知国家/地区码列表。
    
- **ISOLanguageCodes** 类属性

    包含 NSString 对象的 NSArray 类型，只读。已知语言码列表。
    
- **commonISOCurrencyCodes** 类属性

    包含 NSString 对象的 NSArray 类型，只读。**常见**货币码列表。
    
- **ISOCurrencyCodes** 类属性

    包含 NSString 对象的 NSArray 类型，只读。已知货币码列表。
    
    此属性相对于 `commonISOCurrencyCodes` 属性货币码的数量要多。
    
### 通过键访问地区信息

- **objectForKey:** 方法

    返回与指定键对应的组件的值。
    
    - 参数一：NSLocaleKey 类型，对应值的组件。
    - 返沪值：id 类型，对应键的对象。

- **displayNameForKey:value:** 方法

    返回给定地区组件值的显示名称。
    
    - 参数一：NSLocaleKey 类型，地区属性键的值。
    - 参数二：id 类型，键的值。
    - 返回值：NSString 类型，值的显示名称。

- **NSLocaleKey** 别名

    NSString 类型，用于访问地区组件的键。
    
    NSLocaleKey 的部分全局变量如下表：
    
    | NSLocaleKey 类型 | 说明 | 示例 |
    | :-: | :-: | :-: |
    | NSLocaleIdentifier | 地区标识符 | `en_GB`、`es_ES_PREEURO`、<br>`zh-Hant_HK_POSIX@collation=pinyin;currency=CNY` |
    | NSLocaleCountryCode | 国家/地区码 | `CN`、`HK`、`TW` | 
    | NSLocaleLanguageCode | 语言码 | `zh`、`en`、`fr` |
    | NSLocaleScriptCode | 文字码 | `Hans`、`Hant` |
    | NSLocaleVariantCode |  variant 码 | `POSIX`、`PREEURO` |
    | NSLocaleExemplarCharacterSet | 示例字符集 |  |
    | NSLocaleCalendar | 日历 |  |
    | NSLocaleCollationIdentifier | 排序规则 | `pinyin` |
    | ... | ... | ... |
    
    
    
    