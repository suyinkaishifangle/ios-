# Objective-C 属性和存取方法

## 属性

属性（property）是 Objective-C 2.0 引入的新特性，它整合了新的预编译指令和新的属性访问器语法。所有的属性都是基于变量的，声明一个属性，也就相当于声明了一个成员变量。

> ⭐️ **扩展**
>
>  「属性」是英语中 property 翻译过来的意思。但是 attribute 也译作「属性」，要注意区分两者。在开发中我们会提到某个控件的属性，比如 UILabel 的字体、颜色等等，这里的属性就是指 attribute。

在没有属性之前，必须为每一个成员变量都编写 setter 和 getter 方法，这个过程枯燥乏味并且耗时，并且成员变量越多，需要编写的就越多。属性的出现就解决了这一问题，它能够简化接口代码。

在 Objective-C 中使用属性时，需要使用 `@property` 和 `@synthesize` 关键字，它们都是预编译指令。`@property` 的实际作用是自动声明对应属性的 setter 和 getter 方法，而 @synthesize 则表示创建了该属性的 setter 和 getter 方法，编译器将在编译时自动合成（synthesize）并添加实现成员变量的 setter 和 getter 方法代码。在 Xcode 4.5 及以后的版本中，默认自动使用 `@synthesize` 关键字，无需手动编写。

`@property` 关键字在 `@interface` 和 `@end` 之前使用，用来声明一个属性。所以它可以放在 .h 文件中的接口部分中，也可以放在 .m 文件的类扩展部分中。放在不同的位置，也会有不同的效果，具体如下：

- 在 .h 文件接口部分使用，表示这个属性是开放的，外部的类可以访问。
- 在 .m 文件的类扩展部分使用，表示这个属性只有这个类内部可以访问。

`@synthesize` 关键字用在类的实现部分声明该属性。所有的属性都是基于变量的，所以在合成 getter 和 setter 方法的时候，编译器会自动创建与属性名称相同且前面带下划线 `_` 的成员变量。但是也可以自定义成员变量的名称，使用 `@synthesize` 关键字在其后用 `属性名 = 自定义成员变量名` 的方式来指定某个成员变量的名字（不过实际开发很少这么做）。

```Objective-C
// .h 文件
@interface ClassName ()

@property NSString *name;   // NSString 指属性的类型，name 即属性的名称
@property NSNumber *number;
...
@end

// .m 文件

@implementation

@synthesize name;           // 此时默认成员变量名为 _name
@synthesize number = _num;  // 自定义成员变量名为 _num
...
@end
```

前面提到编译器会自动创建成员变量、合成存取方法，但是某些时候并不需要编译器自动生成存取方法和成员变量。这时就需要使用 `@dynamic` 关键字告诉编译器不要生成任何代码或创建相应的成员变量。

`@dynamic` 还有一个作用，就是用于重写父类的属性。例如，如果在子类中定义了一个属性，这个属性和父类中的一个属性同名，这时编译器会产生警告。这时就需要使用 `@dynamic` 关键字禁止编译器自动生成成员变量和存取方法，然后再在子类中手动创建成员变量和存取方法。

### 属性修饰符

属性修饰符是在 `@property` 后面使用括号增加用来修饰属性的关键字。大体可以分为一下几种类型：

- 与内存管理有关

    - **assign**（默认）
    
        该修饰符指定只是对属性进行简单赋值，不更改对所赋的值的引用计数。这个指示符主要适用于 NSInteger、CGFloat 等基础非对象类型，以及 short、float、double、结构体等各种 C 数据类型。

    - **retain**

        在 setter 方法中，需要对传入的对象进行引用计数加 1 的操作。简单来说，就是对传入的对象拥有所有权，只要对该对象拥有所有权，该对象就不会被释放。（适用于 MRC）

    - **strong**

        强引用，需要对传入的对象进行引用计数加 1 的操作。简单来说，就是对传入的对象拥有所有权。strong 跟 retain 的意思相同并产生相同的代码。（适用于 ARC）
        
    - **weak**

        弱引用，需要对传入的对象不进行引用计数加 1 的操作。简单来说，就是对传入的对象没有所有权，当该对象引用计数为 0 时，即该对象被释放后，用 weak 声明的成员变量指向 `nil`，即成员变量的值为 0，不会造成野指针。
        
    - **unsafe_unretained**

        与 weak 基本相似，不过区别是当对象的引用计数为 0 被释放后，用 unsafe_unretained 声明的成员变量不会指向 `nil`，所以有些时候会导致程序崩溃。开发中均使用 weak 而不使用 unsafe_unretained。

    - **copy**

        当调用 setter 方法对成员变量赋值时，会将被赋值的对象复制一个副本，再将该副本赋值给成员变量。copy 修饰符会将原成员变量所引用对象的引用计数减 1。当成员变量的类型是可变类型，或其子类是可变类型时，被赋值的对象有可能在赋值之后被修改，如果程序不需要这种修改影响 setter 方法设置的成员变量的值，此时就可考虑使用 copy 修饰符。

    > ⚠️ **注意**
    >
    > 1. assign 用来修饰非 Objective-C 对象，weak 用来修饰 Objective-C 对象。
    > 2. strong 和 retain 相同，但是在 ARC 中使用 strong，在 MRC 中使用 retain。
    > 3. 只要是 NSString 或其子类类型的属性，都用 copy 修饰。

- 与读写性有关

    - **readwrite**（默认）

        指示编译器合成 setter 和 getter 方法，表示属性生成的成员变量可读可写。

    - **readonly**

        指示编译器只合成 getter 方法，不合成 setter 方法，表示该属性生成的成员变量是只读的。**如果重写了 getter 方法，需要手动创建成员变量**。

- 与线程有关

    - **atomic**（默认）

        atomic 指定合成的存取方法是否为原子操作。所谓原子操作，主要是指线程是否安全。使用 atomic 意味着只有一个线程能够同时进入存取方法的方法体中。虽然 atomic 是线程安全的，但是会造成性能下降，很少使用。
  
    - **nonatomic**

        nonatomic 跟 atomic 刚好相反。表示非原子的，可以被多个线程访问，线程可能不安全。它的性能比 atomic 高，适合内存小的移动设备。iOS 发开中一般使用 nonatomic。

- 与合成方法有关

    - **setter**

        setter 修饰符用于为合成 setter 方法指定自定义方法名。例如 setter=xyz:（setter 方法带参数，所以要加冒号）表示设置该属性的成员变量的 setter 方法名为 xyz: 。
  
    - **getter**

        getter 修饰符用于为合成 getter 方法指定自定义方法名。例如 getter=abc 表示设置该属性的成员变量的 getter 方法名为 abc。
        
    > ⚠️ **注意**
    >
    > 虽然使用 setter 和 getter 修饰符能够修改存取方法的方法名，但是成员变量的名字并没有修改，默认成员变量的名字仍然是 `_属性名`。

## 存取方法

存取（accessor）方法是用来读取或改变对象的成员变量的方法。存取方法中为对象的成员变量赋值的方法被称为 「setter」方法，而读取对象的成员变量的值的方法被称为「getter」方法。

> ⭐️ **[Why use getters and setters/accessors?](https://stackoverflow.com/questions/1568091/why-use-getters-and-setters-accessors)**
> 
> 「计算机科学领域的任何问题都可以通过增加一个间接的中间层来解决」，使用存取方法间接的访问对象中的变量是一种非常有用的做法。

一般来说存取方法都是成对的出现，一个用来设置属性的值，另一个用来读取属性的值，不过有时候也只有一个 getter 方法或者只有一个 setter 方法。对应存取方法的命名，Cocoa 有固定的规则：

- setter 方法：根据其更改的属性的名称命名，并加上前缀 `set`。
- getter 方法：根据其返回的属性的名称命名，不加上前缀 `get`。

> 🔥 **提示**
> 
> 有些语言（例如 Java）的 getter 方法可能需要加上前缀 `get`，但是这个词在 Cocoa 中有特殊的含义，Cocoa 规定 getter 方法不能够加上前缀 `get`。

## 点表达式

Objective-C 的点表达式（`.`）看起来与 C 语言中的结构体访问以及 Java 语言中的对象访问有点类似。点语法不是 Objective-C 语言的特性，而是 Xcode 特性，Xcode 会把这样的语法翻译成调用 setter 方法或者 getter 方法，所以点语法的本质就是方法调用。

如果点表达式出现在了等号（`=`）的左边，调用成员变量的 setter 方法。如果点表达式出现在了等号（`=`）的右边，调用成员变量的 getter 方法。

| 操作 | 点语法语句 | 使用消息表达式 |
| :--: | :--: | :--: |
| 调用 setter | obj.name = val; | [obj setName:val]; |
| 调用 getter | val = obj.name; | val = [obj name]; |
