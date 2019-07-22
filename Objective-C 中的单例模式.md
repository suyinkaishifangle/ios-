# Objective-C 中的单例模式

## 单例模式和单例类

**单例（singleton）模式**是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例的特殊类。通过单例模式可以保证系统中，应用该模式的类一个类只有一个实例（对象）。如果一个类始终只能创建一个实例，那么这个类称为**单例类**。

无论应用程序请求多少次，单例类都会返回相同的对象。一般的类允许调用者根据需要创建尽可能多的类对象，而对于单例类，每个进程只能有一个类的对象。单例对象提供对其类资源的全局访问点。

下图是单例类和一般类的对比示意图：

![单例类和一般类的对比](media/15571476977444/%E5%8D%95%E4%BE%8B%E7%B1%BB%E5%92%8C%E4%B8%80%E8%88%AC%E7%B1%BB%E7%9A%84%E5%AF%B9%E6%AF%94.png)

通过工厂方法从单例类中获取全局对象。该类在第一次请求时延迟地创建其唯一对象，然后确保不能创建其他对象。有的单例类还可以防止调用者复制，保留或释放对象。

## 创建单例类

在某些时候，程序多次创建某个类的多个对象对于整个程序来说并没有什么意义，相反，频繁的创建对象、回收对象带来的系统开销问题还可能造成应用程序性能的下降。

单例类可以通过 static 全局变量来实现，该变量用于保存已创建的单例对象。每次程序需要获取单例对象时，先判断该 static 全局变量是否为 nil，如果该全局变量为 nil，则初始化一个对象并赋值给该 static 全局变量。具体实例代码如下：

```Objective-C
// UserModel.h 文件
#import <Foundation/Foundation.h>
@interface UserModel : NSObject

+ (instancetype)sharedInstance;

@end

// UserModel.m 文件
#import "UserModel.h"

static UserModel *instance = nil;

@implementation UserModel

+ (instancetype)sharedInstance {
    static dispatch_once_t token;
    dispatch_once(&token, ^{
        instance = [[self alloc] init];
    });
    return instance;
}

- (instancetype)init {
    if (self = [super init]) {
        ... 
    }
    return self;
}

@end
```

上述代码中，GCD 的 dispatch_once 函数会保证相关的块必定会执行，而且只执行一次，最重要的是这个方法是线程安全的。

然后再重写了 init 方法，为创建的单例对象做初始化，可以对其属性进行赋值操作等。