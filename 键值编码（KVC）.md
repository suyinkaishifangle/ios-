# 键值编码（KVC）

键值编码（key-value coding，简称 KVC）不是 Objective-C 的特性，而是 Cocoa 提供的一种特性。它是一种间接更改对象状态的方式，其实现的方式是使用字符串表示要更改的对象状态。

最基本的 KVC 由 NSKeyValueCoding 协议提供支持，最基本的操作属性的两个方法如下：

- `setValue:forKey:` 为指定属性设置值。
- `valueForKey:` 获取指定属性的值。

这两个方法都是通过 NSString 对象来指定被操作属性的，其中 `setValue:forKey:` 方法的参数一是 id 类型，所以传入的必须是对象，参数二用于传入属性名字符串。`valueForKey:` 方法在 Objective-C 运行时中使用元数据打开对象并进入其中查找需要的信息。在 C 或 C++ 语言中不能执行这样的操作。

- 对于 `setValue:forKey:` 方法，其底层的机制执行步骤如下：

    1. 优先调用和参数二名字相同的属性的 setter 方法来设置该属性的值。
    2. 如果该属性没有 setter 方法，KVC 机制会继续搜索名为 `_属性名` 的成员变量，无论该成员变量是否是外部能够访问到的，如果成员变量存在则为其赋值。
    3. 如果也没有名为 `_属性名` 的成员变量，KVC 机制会继续搜索名为 `属性名` 的成员变量，无论该成员变量是否是外部能够访问到的，如果成员变量存在则为其赋值。
    4. 如果也没有名为 `属性名` 的成员变量，系统将会执行该对象的 `setValue:forUndefinedKey:` 方法。

- 对于 `valueForKey:` 方法，其底层的机制执行步骤如下：

    1. 优先调用和参数一名字相同的属性的 getter 方法来获取该属性的值。
    2. 如果该属性没有 getter 方法，KVC 机制会继续搜索名为 `_属性名` 的成员变量，无论该成员变量是否是外部能够访问到的，如果成员变量存在则获取其值。
    3. 如果也没有名为 `_属性名` 的成员变量，KVC 机制会继续搜索名为 `属性名` 的成员变量，无论该成员变量是否是外部能够访问到的，如果成员变量存在则获取其值。
    4. 如果也没有名为 `属性名` 的成员变量，系统将会执行该对象的 `valueforUndefinedKey:` 方法。

> ⚠️ **注意**
> 
> `setValue:forUndefinedKey:` 方法和 `valueforUndefinedKey:` 方法的实现都是引发一个异常，这个异常将会导致程序退出。

并且对于 KVC，Cocoa 会自动装箱和开箱标量值。也就是说当使用 `valueForKey:` 方法时，它自动将标量值（int、float 和 struct）放入 NSNumber 或 NSValue 中；当使用 `setValue:forKey:` 方法时，它自动将标量从这些对象中取出。注意仅 KVC 具有这样的自动装箱和开箱功能，常规方法调用和属性语法不具备这种功能。示例代码如下：

```Objective-C
@interface ViewController ()

@property (nonatomic, assign) int number;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    NSNumber *num = [NSNumber numberWithInt:100];
    // 从 NSNumber 对象中开箱取出值赋给 int 类型的成员变量
    [self setValue:num forKey:@"number"];   
    // 将获取到的 int 类型的成员变量的值装箱为 NSNumber 对象
    // 注意 %@ 是打印对象
    NSLog(@"数据是：%@", [self valueForKey:@"number"]);
}

@end
```

> 🔥 **为什么要使用 KVC 来操作，直接使用 setter 和 getter 方法不可以吗？**
> 
> 实际上，通过 KVC 操作成员变量的方式比直接用 setter 和 getter 操作成员变量的方式性能差，但是 KVC 通过字符串来操作成员变量，这个字符串可以是变量，也可以是常量，因此具有很高的灵活性。并且 KVC 方式能够操作一些外部无法调用的成员变量，特别是需要修改一些苹果提供的库中的私有成员变量时非常有用。

## 键路径

除了通过键设置值外，键值编码还支持指定键路径，就像文件系统路径一样。这些路径的深度是任意的，具体取决于对象图（object graph，可以表示对象之间的关系）的复杂度，例如可以使用类似 `car.interior.airconditioner.fan.velocity` 这样的键路径。

KVC 协议中操作键路径的两个方法如下：

- `setValue:forKeyPath:` 为指定路径的属性设置值。
- `valueForKeyPath:` 获取指定路径的属性的值。

上面两个操作路径的方法和 `setValue:forKey:` 与 `valueForKey:` 方法的执行机制相同。

如果使用某个键值来访问一个 NSArray 数组，它实际上会查询相应数组中的每个对象的属性，然后将查询结果打包到另外一个数组中并返回。在 KVC 中，通常认为对象中的 NSArray 具有一对多的关系，如果键路径中含有一个数组属性，则该键路径的其余部分将被发送给数组中的每个对象。如下代码：

```Objective-C
NSMutableArray *mutableArray = [NSMutableArray array];
    
for (int i = 0; i < 5; i++) {
    SXTest *test = [[SXTest alloc] init];
    [mutableArray addObject:test];
}

// number 是 SXTest 类中的一个私有属性    
NSArray *array = [self valueForKeyPath:@"mutableArray.number"];    
```

`valueForKeyPath:` 方法将路径分解并从左向右进行处理，NSArray 实现 `valueForKeyPath:` 的方式是循环遍历它的内容并向每个对象发送消息。不过要注意的是，不能在键路径中索引这些数组，例如使用 mutableArray[0].number 来获取数组第一个元素的属性值。

KVC 还支持通过一些运算符来对成员变量的值进行相关的运算，例如 `@count` 计算总数，`@sum` 计算总和，`@max` 和 `@min` 计算最大值和最小值等等。例如上面的代码，使用`NSNumber *max = [self valueForKeyPath:@"mutableArray.@max.number"];` 可以计算出数组个元素中 number 成员变量值最大的值。

> ⚠️ **注意**
> 
> KVC 的功能虽然强大，但是一定不能够滥用，KVC 需要解析字符串来计算需要的答案，因此速度较慢。另外编译器无法对其进行错误检查。


