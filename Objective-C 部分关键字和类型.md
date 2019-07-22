# Objective-C 部分关键字和类型

## @class

在 Objective-C 中，当一个类需要引用另一个类，即建立「依赖关系（dependency）」的时候，需要在类的头文件中建立被引用类的指针。

导入头文件使头文件和实现文件之间建立了依赖关系，如果头文件有任何变化，那么所有依赖它的文件都需要重新编译。并且因为依赖是传递的，头文件之间也相互依赖，所以重新编译的问题会更加严重。导致依赖关系问题的原因是 Objective-C 编译器需要某些信息才能工作，例如有时编译器需要知道类的全部信息（成员变量配置、方法等），而有的时候只需要知道类名即可。

Objective-C 引入关键字 `@class` 来告诉编译器：这是一个类，只会通过指针来引用它。这样编译器就能够放心，不必知道这个类的更多信息，只需要了解它是通过指针来引用即可。

示例代码：

```Objective-C
@class Engine;
@class Tire;
@interface Car: NSObject {
    Engine *engine;  // Engine类
    Tire *tires;     // Tire类
}
```

先忽略实现文件。这里编译器还不知道 Engine 和 Tire 是什么。有两种方式：一是使用 `#import` 命令导入这两个类的头文件，二是使用 `@class` 关键字声明这两个类。

- 使用 `#import` 导入头文件会告诉编译器这两个类的所有信息，包括成员变量和方法。

- 使用 `@class` 只是告诉编译器，Engine 和 Tire 是类的名称，至于是如何定义的、类中包含什么，暂时不用考虑。

在头文件中，一般只需要知道被引用的类的名称就可以了。不需要知道其内部的实体变量和方法。而在类实现文件里面，因为会用到这个引用类的内部的实体变量和方法，所以需要使用 `#import` 来包含这个被引用类的头文件。

在编译效率方面考虑，如果有很多个头文件都使用 `#import` 导入了同一个头文件，或者这些文件是依次引用的，如 A->B, B->C, C->D 这样的引用关系，如果当最开始的那个头文件有变化的话，后面所有引用它的类都需要重新编译，如果这样的类有很多的话，这将耗费大量的时间。而使用 `@class` 则不会。 

如果有循环依赖关系，如：A–>B, B–>A 这样的相互依赖关系，如果使用 `#import` 来让这两个类相互引用，还会出现编译错误，如果使用 `@class` 在两个类的头文件中相互声明，则不会有编译错误出现。 

所以一般来说，`#import` 是放在头文件中的，用于创建一个前向引用（在声明前即被实际使用），只是为了在头文件中引用这个类。在实现文件中，如果需要引用这个类的实体变量或者方法之类的，还是需要使用 `#import` 来导入头文件。

> ⚠️ **注意**
> 
> 1. 如果是 B 类继承自 A 类，则不能够在 B 类的头文件中使用 `@class A`，而只能使用 `#import "A.h"`，因为继承关系不是通过指针指向其他类，而是需要知道所有关于父类的信息。
> 
> 2. 如果子类需要使用父类的成员变量的方法或者属性，那么在父类的头文件中也是需要使用 `#import` 而不是 `@class` 关键字，因为编译器需要知道该成员变量的所有配置信息。

## @select() 和 SEL

`@select()` 在 Objective-C 中是选择器（selector），它是一个编译指令。选择器只是一个方法名称，它以 Objective-C 运行时使用的特殊方式编码，以快速执行查询。使用 `@select()` 编译指令的圆括号中的方法名称来指定选择器。

选择器可以被传递，可以作为方法的参数使用，甚至可以作为成员变量被存储（不过选择器不是对象）。使用关键字 `SEL` 来表示选择器的类型。

## self

NSObject 类是所有类的基类，其中它声明了一个名为 `isa` 的成员变量（因为继承在子类和父类之间建立了一种 「is a」（是一个）的关系，所以该成员变量叫做 `isa`），该变量保存了一个指向当前对象的类的指针（通过在 Xcode 的调试终端输入 `po isa` 命令可以打印出当前对象的类名），但是本质上 `isa` 是成员变量，需要占用内存空间（`isa` 变量的首地址就是该对象内存中的首地址）。在 NSObject.h 文件中关于 `isa` 的成员变量的定义如下：

```Objective-C
@interface NSObject <NSObject> {
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wobjc-interface-ivars"
    Class isa  OBJC_ISA_AVAILABILITY;
#pragma clang diagnostic pop
}
```

> ⭐️ **扩展**
> 
> 其中上面的三条 `#pragma clang diagnostic ...` 命令的意思是消除使用 `Class isa  OBJC_ISA_AVAILABILITY;` 这个语句时编译器产生的警告。`Class` 是一个结构体的别名，表示 Objective-C 类的类型。这个结构体中包含了一些关于类的信息（包括该类的成员变量列表、方法列表、协议列表、类名等）。这里的 `OBJC_ISA_AVAILABILITY` 是宏定义，具体是表示当前 `isa` 成员变量可用（通过头文件中的描述可知道在将来 `isa` 将被弃用）。

一个对象中的每个方法都获得了一个名为 `self` 的隐藏参数，它是一个指向接收消息的对象的指针。`self` 指向继承链中第一个类中的第一个成员变量，因为所有类都继承自 NSObject 类，并且该类的第一个成员变量是 `isa`，所以 `self` 都是指向 `isa` 变量。所以这样在对象的每个方法内就能够通过 `self` 来获取到该对象的成员变量和方法等。

## super

Objective-C 提供了一种方式，让其能够既可以重写方法的实现，又能够调用父类中的实现方法。为了调用继承的方法在父类中的实现，需要使用 `super` 关键字作为方法调用的目标。

Objective-C Runtime 中有一个 `objc_super` 结构体，用来指定实例的父类。当遇到 `super` 关键字作为消息的接收者时，编译器生成 `objc_super` 结构体。它指定应该发送消息的特定父类的类定义。**当向 `super` 发送消息时，实际上是在请求 Objective-C 向该类的父类发送消息**。如果父类中没有定义该消息，Objective-C 会继续在继承链上一级中查找。

`super` 本质是一个「编译器指令」，告诉编译器对方法进行编译时使用 `objc_msgSendSuper` 或 `objc_msgSendSuper_stret` 函数，而不是 `objc_msgSend` 等函数（以上函数具体参考 Objective-C Runtime）。

> ⚠️ **注意**
> 
> 前面虽然提到，向 `super` 发送消息就是向该类的父类发送消息，那么调用的就是该类的父类的方法。但是实际上调用 `[super class]` 和 `[self class]` 一样返回的却是该类本身，而不是该类的父类。本人猜测是因为在 NSObject 类的 `class` 方法中做了处理，所以让调用 `[super class]` 时返回的也是本身类而不是其父类（无论这个猜测是否正确，总之记住 `[super class]` 返回的也是本身类而不是父类，`superclass` 方法同理）。

```ObjectiveC
@implementation Son : Father
- (instancetype)init {
    if (self = [super init]) {
        NSLog(@"%@", NSStringFromClass([self class]));          // 输出：Son
        NSLog(@"%@", NSStringFromClass([super class]));         // 输出：Son
        NSLog(@"%@", NSStringFromClass([self superclass]));     // 输出：Father
        NSLog(@"%@", NSStringFromClass([super superclass]));    // 输出：Father
    }   
    return self;
}
```

> ⚠️ **为什么使用 `if (self = [super init])` 语句**
> 
> 符合 Objective-C 继承类初始化规范，`[super init]` 去调用父类的初始化方法，父类又继续调用 `[super init]`，直到调用根类 NSObject 类的初始化方法。根类中 `init` 负责初始化内存区域，向里面添加一些必要的属性，返回内存指针。这样延着继承链初始化的内存指针被从上到下传递，在不同的子类中向块内存添加子类必要的属性，直到当前类得到内存指针，赋值给 `self` 参数。而使用 if 语句判断是为了防止没有成功创建和初始化对象，`[super init]` 返回了 `nil` 时，不再进行后续的设置。

## const

const 是 C 语言中的关键字，并且在 Objective-C 中也可以使用。const 翻译成中文的意思是「常量」，用来修饰普通数据或指针，它的作用是固定一个标识符的值保持不变，其实也就是将标识符修饰为常量（类似 Swift 中 let 关键字的作用）。

const 的作用是修饰右边的标识符（普通数据和指针），起到限制作用，被修饰的标识符的是只读的，即常量。const 使用的位置不同也会用不同的效果。例如下面示例代码。

```ObjectiveC
/* ---- 1. 基本数据常量 ---- */
// 两种方式一样，const 修饰的是 a
const int a = 1;   // a 是常量
int const a = 1;   // a 是常量
// a = 20;          报错，不允许修改

/* ---- 2. 指针常量 ---- */
// 两种方式一样，const 修饰的是 *p1 这个指针，
// 表示指针不可变（即指针指向的内存地址不变），
// 但是该内存地址上的数据值是可变的。
const int *p1;     // *p1 是常量，p1 是变量
int const *p1;     // *p1 是常量，p1 是变量

// const 修饰的是 p1，而不是 *p1
int *const p1;     // *p1 是变量，p1 是常量

// 两种方式一样，第一个 const 修饰 *p1，第二个 const 修饰 p1
int const *const p1;  // *p1 和 p1 都是常量
const int *const p1;  // *p1 和 p1 都是常量
```

### 宏定义和 const 的区别

在 Objective-C 开发中，定义常量可以使用宏定义，也可以使用关键字 const。两者肯定有一定的区别。下表是两者的特点对比。苹果公司推荐使用 const 常量。

|| const | 宏定义 |
|:--:|:--:|:--:|
| 编译方式 | 编译阶段 | 预编译 |
| 编译检查 | 会编译检查，会编译报错 | 不做检查，不会编译报错，只是替换 |
| 优点 | 修饰标识符为常量，优化程序内存空间 | 可以定义标识符、函数、方法等 |
| 缺点 | 只能修饰标识符、对象 | 大量使用宏容易造成编译时间久 |

> ⚠️ **注意**
> 
> 在 iOS 开发中，一个相同的宏定义并不会多次分配内存空间。

## static

static 翻译成中文的意思是「静态的」，用来修饰局部变量和全局变量。

1. static 修饰局部变量时，在编译时分配内存，只初始化一次，程序结束时才释放，不过作用域不变。

2. static 修饰全局变量时，全局变量的作用域变为仅限当前文件，外部类不可访问。

    使用 Objective-C ，在一个文件声明的全局变量，其他文件也是能访问该全局变量，使用 static 修饰这个全局变量，使外部不能够访问，全局变量基本上都会用 static 修饰。

## extern

extern 翻译成中文的意思是「外部的」，用来声明外部全局变量。

### ⌛️UIKIT_EXTERN

UIKIT_EXTERN 是经过处理的 extern。

```Objective-C
#ifdef __cplusplus
#define UIKIT_EXTERN		extern "C" __attribute__((visibility ("default")))
#else
#define UIKIT_EXTERN	        extern __attribute__((visibility ("default")))
#endif
```

## id 类型 和 instancetype

`id`（泛型对象指针）关键字是 Objective-C 中独特的数据类型，可以转换为任何数据类型。换句话说，`id` 类型的变量可以存放任何数据类型的对象。在内部处理上，这种类型被定义为指向对象的指针，实际上是一个指向这种对象的成员变量的指针。

instancetype 是 clang 3.5 开始提供的一个关键字，表示某个方法返回未知类型的 Objective-C 对象。以 alloc 或 new 开头的类方法和以 autorelease、init、retain 或 self 开头的对象方法称为「关联返回类型」方法。「关联返回类型」方法的返回值类型是该方法所在的类的类型（即这个方法存在于哪个类中，那么这个方法返回的类型就是这个类）。

例如：`[NSArray alloc]` 是 `NSArray` 类型，因为类方法 alloc 就是「关联返回类型」方法。同样 `[[NSArray alloc] init]` 也是 `NSArray` 类型的。

如果不是「关联返回类型」方法，假如方法的返回值类型是 `id` 类型，那么实际调用时，这个方法返回的值类型就是 `id` 类型。而这个时候 `instancetype` 的作用就来了，它能让那些「非关联返回类型」方法的返回值类型为所在类的类型。

`instancetype` 和 `id` 的异同：

- 相同点
    
    都可以作为方法的返回类型

- 不同点
    
    1. `instancetype` 可以返回和方法所在类相同类型的对象，`id` 只能返回未知类型的对象。
    2. `instancetype` 只能作为返回值，不能像 `id` 那样作为参数。

## BOOL 类型

在 Objective-C 中的 BOOL 类型是一种对带符号的字符类型（signed char）的类型定义（typedf），使用8位的存储空间。通过 #define 宏定义把 `YES` 定义为 `1`，把 `NO` 定义为 `0`。但是 Objective-C 并不会将 BOOL 作为只能保存 YES 或 NO 的真正布尔类型来处理。

> 在 Objective-C 中，BOOL 类型的值只有 0 表示 NO，1 表示 YES，不是 C 语言的 0 表示 false，非 0 表示 true。并且实际开发中几乎不使用 0、1 来表示布尔值，均使用 YES 和 NO。

如果将一个大于 1 字节的整型值（比如 short 或 int）赋给一个BOOL变量（Xcode 会警告），那么只有低位字节会作 BOOL 值，如果刚好该低位字节为0（比如 8960，写成十六进制为 0x2300），BOOL 值会被认作是 0，即 NO 值。

## 和「空」有关的类型或关键字

| 标志 | 含义 | 例子|
| :-: | :-: | :-: |
| NULL | 表示 C 指针为空值 | `int *pointerToInt = NULL;` |
| nil | 表示 Objective-C 对象为空 | `NSString *someString = nil;` |
| Nil | 表示 Objective-C 类为空 | `Class someClass = Nil;` |
| NSNull | 一种对象类型，<br>表示空的 Objective-C 对象 | `NSMutableArray *array = [NSMutableArray array];` <br> `[array setObject:[NSNull null]];` |

## 结构体

结构体是一种数据类型，而不是对象。但是结构体中可以包含 NSString 等类。

结构体的常用的定义如下：

```ObjectiveC
struct typeName {           // 定义一个名为 typeName 的结构体
    结构成员;
    结构成员;
    ...
} 
struct typeName name;       // 定义一个名为 name 的 typeName 类型结构体变量
```

也可以使用 typedef 关键字修饰定义的结构体的别名，和 C 语言定义结构体的方式相同。

```ObjectiveC
typedef struct typeName {  // 定义一个名为 typeName 的结构体
    结构成员;
    结构成员;
    ...
} structName;              // 这个结构体的别名为 structName
structName name;           // 定义一个名为 name 的 typeName 类型结构体变量
```