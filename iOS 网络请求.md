# iOS 网络请求

## 基本概念

- 客户端（Client）：移动应用（iOS、Android等应用）。
- 服务器（Server）：为客户端提供服务、提供数据、提供资源的机器。
- 请求（Request）：客户端向服务器索取数据的一种行为。
- 响应（Response）：服务器对客户端的请求作出反应，一般指返回数据给客户端。

> 图1：网络请求过程

![网络请求过程](https://blog-image-1256099768.cos.ap-chengdu.myqcloud.com/blog-image/%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E8%BF%87%E7%A8%8B.png)

### 服务器

前面讲了服务器是为客户端提供服务、提供数据、提供资源的机器。服务器也可以分为两种：

- 远程服务器（外网服务器、正式服务器）
    
    应用上线后使用的服务器，供全体用户使用。
    
- 本地服务器（内网服务器、测试服务器）

    应用处于开发、测试阶段使用的服务器，仅供开发人员、测试人员使用。
    
### URL

URL，全称是 Uniform Resource Locator，中文名「统一资源定位器」。通过 URL 能够找到互联网上唯一的一个资源，互联网上的每个资源都有唯一的一个 URL。

URL 的基本格式：`协议://主机地址/路径`

- 协议：是指资源查找和资源传输方式，不同的协议代表着不同的资源查找和资源传输方式。
- 主机地址：存放资源的主机（服务器）的域名或者 IP 地址。
- 路径：资源在主机（服务器）中的具体位置。
- 参数：参数是指传递给服务器的具体数据，比如登录的账号、密码等。（参数只在 GET 请求的 URL 中）

### URL 中常见的协议

- http：超文本传输协议，访问的是远程的网络资源。
- https：即 http 的安全版。
- ftp：传输文件协议，访问的是共享主机的文件资源。
- file：获取本地文件协议，访问的是本地计算机上的资源。
- mailto：发邮件协议，访问的是电子邮件地址。

### HTTP 协议

超文本传输协议（HTTP，HyperText Transfer Protocol）是互联网上应用最为广泛的一种网络协议。它用来规定客户端和服务器之间的数据传输格式。

HTTP 协议的特点

- 简单快速

    因为 HTTP 协议简单，所以 HTTP 服务器的程序规模小，因而通信速度很快。
    
- 灵活

    HTTP 允许传输各种各样的数据。
    
- 0.9 和 1.0 非持续连接，1.1 版本使用持续连接

   非持续连接就是限制每次连接只处理一个请求，服务器对客户端的请求作出响应后，马上断开连接，节省传输时间。
   
### HTTP 的通信过程
   
完整的 http 通信分为两个步骤：请求和响应
 
- 请求：客户端向服务器索要数据
 
    HTTP 协议规定，一个完整的由客户端发送给服务器的 HTTP 请求中包含以下内容：
    
    - 请求头：包含了对客户端环境的描述，客户端请求信息等。
    - 请求体：客户端发给服务器的具体数据，比如文件数据。
    
    GET 和 POST 请求都是有请求头的，但是 GET 请求没有请求体。
    
- 响应：服务器返回给客户端数据
    
     HTTP 协议规定，一个完整的由服务器返回给客户端的 HTTP 响应中包含以下内容：
     
     - 响应头：包含了对服务器的描述，对返回数据的描述等。
     - 响应体：服务器返回给客户端的具体数据，比如文件数据。

> 图2：HTTP 的通信过程

![HTTP 的通信过程](https://blog-image-1256099768.cos.ap-chengdu.myqcloud.com/blog-image/HTTP%20%E5%8D%8F%E8%AE%AE%E9%80%9A%E4%BF%A1%E8%BF%87%E7%A8%8B.png) 

> 表1：常见响应状态码

| 状态码 | 英文名 | 中文描述 |
| :--:| :--: | :--: |
| 200 | OK | 请求成功 |
| 400 | Bad Request | 客户端请求语法错误，服务器无法解析 |
| 404 | Not Found | 服务器无法根据客户端的请求找到资源 |
| 500 | Internal Server Error| 服务器内部错误，无法完成请求 |

### HTTP 协议的请求方法

在 HTTP/1.1 版本中，定义了 8 种发送 http 请求的方式：GET、POST、OPTIONS、HEAD、PUT、DELETE、TRACE、CONNECT、PATCH。
  
不同的请求方法表示对资源的操作方式不同，例如增（PUT）、删（DELETE）、改（POST）、查（GET）。但是实际上主要用到的是 GET 和 POST 方法， GET 和 POST 方法也可以办到增、删。
   
GET 和 POST 请求的主要区别表现在参数传递上。

- GET

    在请求 URL 后面以 `?` 的形式加上发给服务器的参数，多个参数之间使用 `&` 隔开。比如：
    
    `http://www.test.com/login?username=123&password=456&type=JSON`
    
    协议（http）+ 主机地址（www.test.com）+ 接口名称（login）+ ? + 参数1（username=123）+ & + 参数2（password=456）+ & + 参数3（type=JSON）

- POST

    发给服务器的参数全部放在「请求体」中。比如：
    
    `http://www.test.com/login`
    
在具体的开发中，要根据实际应用来选择使用哪种请求方式。如果要传输大量数据，比如上传文件，只能使用 POST 请求。并且 GET 的安全性比 POST 差，如果只是数据查询，最好使用 GET。

总之一句话：查询数据用 GET，其他如增、删、改用 POST。

### HTTPS 协议
   
HTTPS 和 HTTP 的区别主要为以下四点：

- https 协议需要到 ca 申请证书，一般免费证书很少，需要交费。
- http 是超文本传输协议，信息是明文传输，https 则是具有安全性的 ssl 加密传输协议。
- http 和 https 使用的是完全不同的连接方式，用的端口也不一样，前者是 80，后者是 443。
- http 的连接很简单，是无状态的。HTTPS 协议是由 SSL+HTTP 协议构建的可进行加密传输、身份认证的网络协议，比 http 协议安全。

## iOS 中网络请求的方案

iOS 中常见的发送 HTTP 请求的方案有两种：

- 苹果原生的（自带的）

    - NSURLConnection：这个类的请求方法在 iOS 9.0 已废弃，使用 NSURLSession 替换。
    - NSURLSession：功能比 NSURLConnection 更强大，苹果推荐使用。
    - CFNetWork：NSURL 的底层，纯 C 语言。

- 第三方框架

    - AFNetWorking：最强大的第三方网络框架，简单易用，维护使用者众多，功能强大。
    
### 网络请求常用到的类

- NSURL 

    请求地址
    
- NSURLRequest

    一个 NSURLRequest 对象就代表一个请求，它包含一个 NSURL 对象、请求头、请求体、请求方式、请求超时等等。和 NSArray、NSString一样，它是不可修改的类。
    
- NSMutableURLRequest
    
    它和 NSURLRequest 的关系就像 NSArray、NSString 和 NSMutableArray、NSMutableString 一样。 
    
- NSURLResponse

    用来存放服务器返回的响应头信息的类。
    
- NSHTTPURLResponse 

    用来存放 HTTP 请求服务器返回的响应头信息的类。
    
> 注意
> 
> 在 NSURLConnection 对象的方法中都是使用 NSURLResponse 作为返回的响应头信息的存储类，但是这个类没有提供请求状态码 statusCode 属性，所以需要将 NSURLResponse 类型强制转换成 NSHTTPURLResponse 类型来获取 statusCode 属性。

### NSURLConnection 的使用

NSURLConnection 主要有两个作用。

1. 发送网络请求和建立客户端与服务器之间的连接。（即建立连接）
2. 发送数据给服务器，接受服务器的返回数据。（即发送接收数据）

NSURLConnection 的使用步骤：

1. 创建一个 NSURL 对象，设置请求路径。
2. 传入 NSURL 创建一个 NSURLRequest 对象，设置请求头和请求体。
3. 使用 NSURLConnection 发送请求。

> 图3：NSURLConnection 使用过程

![NSURLConnection 使用过程](https://blog-image-1256099768.cos.ap-chengdu.myqcloud.com/blog-image/NSURLConnection.png)




