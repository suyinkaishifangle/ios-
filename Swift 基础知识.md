# Swift 基础知识

## 语言的特点

1. 支持面向过程和面向对象编程。
2. 代码编写更加简洁明了。
3. 支持直接闭包。
4. 使用自动引用计数（ARC）来简化内存管理。
5. Swift 只需要一个.swift 文件，而 Objective-C 需要.h头文件和.m实现文件。
6. Swift 可以不使用分号（;）作为代码的分隔符(但是使用逗号也没有任何问题)。
7. Swift 能够自动识别初始化了的常量和变量的值。

## 标识符

> 标识符：是变量、常量、方法、函数、枚举、结构体、类、协议等由开发人员指定的名字。

Swift 中的标识符命名规则和大部分的编程语言类似，如下：

- 区分大小写，myName 和 myname 是两个不同的标识符。
- 标识符只能包含字母、数字和下划线（_），且不能以数字开头。
- 标识符不能是关键字，但是可以包含关键字（如果要使用关键字作为标识符，可在关键字前后添加（`）符号）。

注意：
> Swift 中的字母采用的是 [Unicode](https://baike.baidu.com/item/Unicode) 编码。Unicode 包含了世界上所有文字的编码，甚至是一些符号表情，如😊，😂等。这些符号实际上也是 Unicode 编码，而不是图片，所以这些在 Swift 中都可以用作标识符。

## 关键字

> 关键字：是事先定义的有特殊意义的标识符，也叫保留字。

Swift 的关键字没有规律性，有大些、小写，还有下划线等，不像 Java 关键字全是小写。但是一定要记住，在 Swift 中关键字是区分大小写的。

Swift 中的数值类型关键字都是大写字母开头，例如：Int、Double、Bool 等。

## 常量和变量

Swift是[强类型](https://baike.baidu.com/item/%E5%BC%BA%E7%B1%BB%E5%9E%8B/5074514?fr=aladdin)（Strong type）语言，常量和变量在使用前都需要声明（定义）。

**声明和初始化的含义**
> 声明：
> 当一个计算机程序需要调用内存空间时，对内存发出的“占位”指令，称为“声明”，声明后的变量或常量可能是有值的，因为对应内存的区域还没有初始化，声明只是指定了这个变量或常量的值的类型。
>  
> 初始化：
> 即为变量或者常量赋初值，一般声明和初始化都是同时进行的，这样有利于避免因为变量或常量的值未初始化而引发的许多错误。

Swift 中的常量和变量声明时必须使用`: type`格式来确定该常量或变量的类型；如果在声明的同时进行初始化，那么可以不用`: type`格式来确定该常量或变量的类型，Swift 会自动识别。但是如果需要该常量或变量指定类型，那么就需要使用`: type`格式，如下.

```Swift
let str: String;        // 仅声明了常量的类型为 String
var number: Int;        // 仅声明了变量的类型为 Int
let name = "iOS";       // 没有指定类型，Swift 会自动识别为 String 类型
var score = 70.5;       // 没有指定类型，Swift 会自动识别为 Double 类型
var num: Double = 100;  // 虽然初始化了值为 100，看起来是整型。
                        // 但是手动指定了变量值为 Double 类型，实际上的值为 100.0
```

### 常量定义

> 常量的值一旦确定就不能修改。

Swift 在声明常量时，需要在标识符前面加上关键字`let`。注意不能对常量的值进行修改。

```Swift
let name = "iOS"; 
let number: Int;
```

上面代码中第二行的读作：“声明一个叫做 number 的常量，他的类型是 Int，初始值是 100”，这是给number 的常亮添加了类型标注。

注意：
实际上，并不需要经常使用类型标注。如果你在定义一个常量或者变量的时候就给他设定一个初始值，那么 Swift 就像类型安全和类型推断中描述的那样，几乎都可以推断出这个常量或变量的类型，例如常量 name 就是 String 类型。在上面 number 常量的例子中，没有提供初始值，所以 number 这个变量使用了类型标注来明确它的类型而不是通过初始值的类型推断出来的。

### 变量定义

Swift 在声明变量时，需要在标识符前面加上关键字`var`。

```Swift 
var num = 100;
var score: Double = 87;
```

### 选择常量还是变量

如果在开发过程中，选择 var 或 let 都能满足需求的话，那么最好选择 let。如果数据类型是引用数据类型（类声明类型），则最好声明位 let ，let 声明的引用数据类型不会改变引用（即指针），但可以改变其内容。

Swift 中的数据类型分为：值类型和引用类型。值类型包括：整型、浮点型、布尔型、字符、字符串、元组、集合、枚举、结构体。而类属于引用类型。“引用”的本质是指向对象的指针，它是一个地址。例如：

```Swift 
class Person {
    var name: String;
    var age: Int;
    
    init (name : String, age : Int) {
        self.name = name;
        self.age = age;
    }
}

let p1 = Person(name: "Tony", age: 18);
p1.age = 20;                        // 编译通过
p1 = Person(name: "Tom", age: 15);  // 编译不能通过
```

上述代码中，定义了一个 Person 类，然后实例化了一个 p1 对象，声明了 p1 为 let，所以不能修改 p1 对象的指针，但是可以修改对象中的内容。使用`p1.age = 20`是改变对象的内容（改变了 age 的属性），但是最后一行代码试图修改 p1 对象的指针，所以会出现编译错误。如果声明的 p1 是 var，则编译可以通过。

注意：
let 和 var 关键字声明时，原则上是优先使用 let，它可以防止程序运行过程中不必要的修改并且提高程序的可读性。特别是`引用类型`声明时经常采用 let 声明，虽然从业务层面来讲并不需要一个常量，但是使用 let 可以防止`引用类型`在程序运行过程中被错误的修改。

## 数据类型

Swift 中的数据类型根据赋值或给函数传递时的不同方式，可以分为值类型和引用类型。

- 值类型：创建一个副本，把副本赋值或传递过去，这样在函数的调用过程中不会影响原始数据。
- 引用类型：把数据本身的引用（即：指针）赋值或传递过去，这样在函数的调用过程中会影响原始数据。

值类型有：整型、浮点型、布尔型、字符、字符串、元组、集合、枚举和结构体
引用类型：类

### 整型

Swift 提供8、16、32、64位形式的有符号和无符号整数，如下表：

|数据类型|名称|作用|
|:--:|:--:|:--:|
|Int8|有符号8位整型|整数在内存中占8位|
|Int16|有符号16位整型|整数在内存中占16位|
|Int32|有符号32位整型|整数在内存中占32位|
|Int64|有符号64位整型|整数在内存中占64位|
|Int|平台相关的有符号整型||
|UInt8|无符号8位整型|整数在内存中占8位|
|UInt16|无符号16位整型|整数在内存中占16位|
|UInt32|无符号32位整型|整数在内存中占32位|
|UInt64|无符号64位整型|整数在内存中占64位|
|UInt|平台相关的无符号整型||

> 在实际的 Swift 开发中我们使用的是上表中的 Int 和 UInt，除非求固定宽的整型。因为使用 Int 和 UInt，能够保持与平台的一致性（即在 32 位平台上，Int 与 Int32 宽度一致；在 64 位平台上，Int 与 Int64宽度一致，UInt 同理）。

### 浮点型

浮点型是可以包含消暑部分的数值，浮点型比整型的表数范围更大，也可以用来存储范围较大的整数。
Swift 有两种浮点型：

|数据类型|名称|说明|
|:--:|:--:|:--:|
|Float|32 位浮点数|不需要很大的浮点数时使用|
|Double|64 位浮点数|默认的浮点数类型|

### 数字表示方式

Swift 的整数数值有 4 种表示方式：

- 十进制：常用的普通整数。
- 二进制：以 0b 开头的整数。（b 不能大写，下同）
- 八进制：以 0o 开头的整数。
- 十六进制：以 0x 开头的整数，其中 10～15 分别以 a～f（此处不区分大小写）来表示。


注意：
> Swift 为了提高数值的可读性，整数和浮点数均可以添加多个 0 或使用下划线（一般是每三位加一个），两种格式均不会影响实际值。

例如：

```Swift
let number_1 = 000543;
let number_2 = 000.123;
let number_3 = 3_360_000;
```

### 整型和浮点型之间的转换

Swift 的整型和浮点型之间需要进行显示转换，浮点的 Float、Double 之间也需要进行显示的转换才能够进行计算，转换方式使用`type(var/let)`格式，如下代码。

```Swift 
var a: Int = 100;
let b: Double = 50.99;

var c: Float = Float(b);        // 必须将 b 转换成 Float 类型才能赋值给 c
var d: Double = Double(a) + b;  // 必须将 a 转换成 Double 类型才能与 b 进行运算
a = Int(b);
print(a, b, c, d);              // 最终的值 a:50 b:50.99 c:50.99 d:150.99
```

可以看出，当 Double 转换成 Int 类型的时候会直接取整数，忽略掉小数部分，没有四舍五入。同理 Float 转换成 Int 类型也是一样。而在实际的编程中，应该尽量向范围大的数据转换，这样程序更加安全。Swift 的数据范围由小到大的顺序是：Int8 < Int16 < Int32 < Int64 < Float < Double。

注意：
> 虽然常量的值不能够改变，但是可以在需要使用常量的地方将其转换为其他类型的数据，如上面代码。
> 也就是说，强制类型转换并不会改变原来变量、常量的值。

### 布尔型

布尔型（Bool）只有两个值：true 和 false，用来表示逻辑“真”或“假”。

注意：
> 在 Swift 中，不能用 0 或者非 0 来代表布尔型，其他数据类型的值，也不能转换成布尔型，但是布尔型可以转换成字符串。

### 元组类型

元组（tuple）使用圆括号把多个值组合成一个复合值，元组内的值可以使用任意类型，元组不要求其中的值具有相同的类型。

```Swift
var student_1 = ("小明", 98, 87, "优秀");
student_1 = ("小东", "3班",78, 77, "良好");  // 添加数据类型，编译报错
student_1 = ("小东", 78, 77);               // 删除数据类型，编译报错

var student: (String, Int, Int, String);
student = ("小红", 70, 90, "良好");
student = (77, 86, "小李", "良好");         // 改变数据类型，编译报错

var test: (Int, (Int, String));
teat = (20, (15, "Swift"));
```

以上是一些元组使用的示例，元组可以声明其中成员的数据类型再初始化使用，也可以直接声明初始化，Swift 自动判断其中的类型。元组成员中也可以嵌套元组，如上代码。为元组赋值时，必须为所有成员指定值。

注意：
一个标识符定义了元组其中成员的数据类型之后（即使是 var），就不能再更改其中的数据类型，也不能添加或者删除，如上代码编译报错部分。

**使用键值对定义元组**
定义元组时除了简单地用括号将多个值扩起来外，还可以用键值对`key: value`的形式来定义元组，这种形式相当于为元组的每个元素都指定名字。

```Swift 
var student_1 = (name: "小明", OC: 98 , Swift: 87, status: "优秀");
// 或者
var student_1: (name: String, OC: Int, Swift: Int, status: String);
student_1 = ("小红", 80, 77, "良好");                                 // 编译通过
student_1 = (OC: 80 ,name: "小红", status: "良好", Swift: 77);        // 编译通过
student_1 = (OC: 80.5 ,name: "小红", status: "良好", Swift: 77);      // 编译报错
student_1 = (name: "小红", status: "良好", Swift: 77);                // 编译报错 
student_1 = (OC: 80 ,name: "小红", status: "良好", Swift: 77, C: 85); // 编译报错 
```

可以看出使用键值对定义的元组，在赋值时如果指明对应的“键”的“值”，就可以不按照顺序定义，而省略了“键”之后，就必须按照顺序定义元组中的成员数据。并且也不能添加、删除和改变元组中的成员变量的数据类型。


**元组中数据成员的访问**
Swift 允许通过下标来访问或赋值元组的单个元素。元组的下标从 0 开始。如果指定了元组的“键”，也可以通过元组的“键”来访问元组中的单个元素。两种方法都使用`.`运算符来表示（嵌套元组同理），如下。

```Swift 
var student_1 = (name: "小明", OC: 98 , Swift: 87, status: "优秀");
student_1.0 = "小红";    // 使用下标赋值 
student_1.Swift = 90;   // 使用“键”赋值
print("\(student_1)");  // 打印为：(name: "小红", OC: 98, Swift: 90, status: "优秀")
```

**拆分元组**
Swift 允许将元组的元素拆分成单个的常量或变量。如果程序只需要部分元组的元素，拆分的时候可以使用下划线（_）作为不拆分部分的占位符。如下。

```Swift
var student_1 = (name: "小明", OC: 98 , Swift: 87, status: "优秀");
var (name, OC, Swift, status) = student_1;   // 将 student_1 元组全部拆分为变量

print("\(student_1)");      // 打印为：(name: "小明", OC: 98, Swift: 87, status: "优秀")
print("\(name)","\(OC)","\(Swift)","\(status)");  // 打印为：小明 98 87 优秀

var student_2 = (name: "小红", C: 80 , Java: 70, status: "良好");
let (_, C, Java, _) = student_2;           // 将 student_2 元组的 C 和 Java 拆分为常量

print("\(student_2)");      // 打印为：(name: "小红", C: 80, Java: 70, status: "良好")
print("\(C)","\(Java)");    // 打印为：80 70
```

注意：
元组拆分之后并不是该元组就不在了或者被拆分的元素不在了，而是将被拆分的元素单独成为变量或者常量。标识符就是被拆分元素的“键”名。

### 可选类型

Swift 语言与其他语言有很大不同：Swift 所有的数据类型声明的变量或常量都不能为空值（nil）。但是在实际开发中，给变量赋值为 nil 是在所难免的，所以 Swift 为每一种数据类型提供了可选类型（optional），即在某个数据类型后面加上问号`?`和感叹号`!`（`!`通常用于可选类型的值可以确定为非空时，但不是说带`!`的可选类型就一定非空）。可选类型可以接受该类型的值和“值缺失”（即 nil）两种情况。

```Swift 
var n1: Int = 10;
n1 = nil;         // 编译出错
var n2: Int? = 10;
n2 = nil;         // 编译通过
print(n2);        // 输出为：Optional(10)
```

注意：
> Swift 中的 nil 和 Objective-C 中的 nil 完全不同。在 Objective-C 中，nil 代表一个并不存在的对象指针，而在 Swift 中，nil 不是代表指针，而是一个确定的值，用于代表“值缺失”。任何可选类型的变量都可以被赋值为 nil。

**强制解析**

Int? 类型和 Int 类型并不是相同的类型，所以无法进行相关的计算等。但是可以使用感叹号（!）进行强制解析（forced unwrapping），也叫拆包（unwrapping），拆包将可选类型变为普通类型。这个感叹号的意思相当于：可选变量中不为 nil，请提取其中的值。

拆包分为显示拆包和隐式拆包，
    
- 显示拆包：使用`?`声明的可选类型在拆包时需要使用`!`符号。
- 隐式拆包：使用`!`声明的可选类型在拆包时可以不使用（当然也可以使用）`!`符号。

```Swift
var n1: Int? = 10;
print(n1 + 100);     // 编译出错，n1 与 100是不同的两种类型
// 显示拆包
print(n1! + 100);    // 输出为：110 

var n2: Int! = 10;
// 隐式拆包
print(n2 + 100);     // 输出为：110 
```

**可选绑定**

可选绑定（optional binding）用于 if 或 while 语句中赋值并判断可选类型的变量或常量是否有值。如果可选类型的变量或常量有值就将其值赋给另一个临时的变量或常量。否则就不能将可选类型拆包。

```Swift
let str: String? = "abc";
if let tem = str {
    print("str 的值为：\(tem)");
} else {
    print("str 的值为 nil，不能拆包");  
}
// 上面代码输出为：str 的值为：abc

var num: Int? = nil;
if let tem = num {
    print("num 的值为：\(tem)");
} else {
    print("num 的值为 nil，不能拆包");
}
// 上面代码输出为：num 的值为 nil，不能拆包
```

上述代码中实际是 if 关键字后面的`let tem = str`语句在进行拆包，如果如果`str`有值，就会将其拆包并赋值给`tem`，并且整个表达式的布尔值为 true，所以输出`str 的值为：abc`。相反，`num`为 nil，就不会拆包，并且整个表达式的布尔值为 false，所以输出`num 的值为 nil，不能拆包`。

注意：
在整个代码中，tem 标识符的作用是中间变量，用来接收被拆包后的值，并且因为其是定义在 if 结构中的，所以它的作用域只在 if 后面的 {} 中，并不在 else 的 {} 中。

### 字符类型

字符串的组成单位是字符。Swift 的字符编码采用 Unicode 编码，它几乎涵盖了我们所知道的一切字符。
Unicode 编码可以有单字节编码、双字节编码和四字节编码，他们的表现形式是`\u{n}`，其中 n 为 1～8 个十六进制数。

Swift 的字符类型是 Character，要注意的是如果在声明变量时如果省略了 Character 类型声明，编译器自动推断出类型为字符串类型，而不是字符类型。

注意：
在 C 和 Objective-C 等语言中，字符是放在单引号`'`之间的。但是在 Swift 中，必须使用双引号`"`把字符括起来，而且 Swift 中的字符串也是使用双引号`"`把字符串括起来。

```Swift 
let char: Character = "😂";
var str  = "😂";
print(type(of:char));   // 输出为：Character
print(type(of:str));    // 输出为：String
```

### 字符串类型

Swift 中的字符串类型是 String，实际上 String 是一个结构体。在 Foundation 框架中的 NSString 是一个类。

创建一个字符串一般是使用变量或者常量直接给字符串赋值，可以使用`count`属性来获取字符串中的字符个数。

```Swift 
var str = "😂😂😂😂";
print(str.count);      // 输出为：4
```

**可变字符串**

Objective-C 中字符串分为可变字符串和不可变字符串。同样 Swift 中也有可变字符串和不可变字符串，即 let 声明的字符串常量是不可变字符串，var 声明的字符串变量是可变字符串。

**字符串拼接**

Swift 的字符串之间的拼接可以使用`+`和`+=`运算符。字符串和字符的拼接则需要使用 append(_ c:Character) 函数。

有时候需要将不同类型的变量、常量和表达式运算符的结果转换为字符串拼接起来，这时候就可以使用`\()`语句实现。

```Swift
var str_1 = "学习";
let str_2 = "Swift语言";
let char: Character = "😊";
str_1 += str_2;
print(str_1);        // 输出为：学习Swift语言
str_1.append(char);
print(str_1);        // 输出为：学习Swift语言😊
print("\(str_1)是一件有趣的事情\(char)!");   // 输出为：学习Swift语言😊是一件有趣的事情😊!
```

## 控制语句

### 分支语句

#### if 语句

**if 结构**

```Swift
if condition {
    code
}
```

**if-else 结构**

```Swift
if condition {
    statements
} else {
    statements
}
```

**else if 结构**

```Swift
if condition {
    statements
} else if {
    statements
} else {
    statements    
}
```

注意
与 C 和 Objective-C 语言不通的是，Swift 的 if 语句中，即便代码`code`的是单句，也不能省略大括号`{}`。并且条件`codition`不加小括号`()`。

#### switch 语句

Swift 的 switch 语句相对与 C、Objective-C、Java、C++ 等语言有很大不同，主要表现为两个方面：
1. Swift 的 switch 语句可以使用整数、浮点数、字符、字符串和元组等类型，并且其数值可以是离散的，也可以是连续的范围。
2. Swift 的 switch 语句 case 分支不需要显示地添加 break 语句，分支执行完成自动跳出 switch 语句。

```Swift
switch value {
case pattern:
    code
default:
    code
}
```

注意
每个 case 后面可以加上一个或者多个值，多个值之间使用逗号分隔，并且每个 switch 语句必须有一个 default 语句，放在所有分支的后面。每个 case 中至少要有一条语句。

#### guard 语句

guard 语句是 Swift 2 之后新添加的关键字，它的作用是在判断一个条件为 true 的情况下执行某语句，否则终止或跳过某语句。它的设计目的是替换复杂 if-else 语句的嵌套，提高程序的可读性。语法如下：

```Swift
guard condition else {
    statements
}
```

当条件表达式 condition 为 true 时跳过 statements 语句中的内容，条件表达式为 false 时执行 statements 语句中的内容。

注意：
guard 语句适用于多个 if-else 语句的嵌套，当 if-else 嵌套不多的时候，guard 语句并不能体现其优势，而当if-else 嵌套较多的时候，guard 语句的优势就会显现出来，例如。

```Swift
// 定义一个结构体
struct Student {
    let name: String?;
    let age: Int?;
    let sex: String?;
}

// 定义一个函数
func ifStudent(stu: Student) {
    if let tName = stu.name {
        print("名字是\(tName)");
        if let tAge = stu.age {
            print("年龄是\(tAge)");
            if let tSex = stu.sex {
                print("性别是\(tSex)");
            } else {
                print("不知道性别");
            }
        } else {
            print("不知道年龄");
        }
    } else {
        print("不知道名字");
    }
}

// 定义另一个函数
func guardStudent(stu: Student) {
    guard let tName = stu.name else {
        print("不知道名字");
        return;
    }
    print("名字是\(tName)");
    
    guard let tAge = stu.age else {
        print("不知道名字");
        return;
    }
    print("名字是\(tAge)");
    
    guard let tSex = stu.sex else {
        print("不知道名字");
        return;
    }
    print("名字是\(tSex)");
}
```

上面的函数 ifStudent(stu: Student) 和 guardStudent(stu: Student) 的作用完全相同，但是可以轻易的看出第二个函数的结构更加清晰，可读性更高。

### 循环语句

#### while 语句

while 语句时一种先判断的循环结构，格式如下：

```Swift
while condition {
    code
}
```

只要满足循环的条件 condition，这个语句就会一直循环下去。

#### repeat-while 语句

repeat-while 语句与 C 语言中的 do-while 的用法相同，先循环语句再判断循环条件。

```Swift
repeat {
    code
} while condition
```

#### for 语句

Swift 3 之前可以使用 C 语言风格的 for 语句。但是在 Swift 3 之后 for 语句只能与 关键字 in 结合使用，对于范围和集合进行遍历。

```Swift
for item in items {
    code
}
```

item 表示每次循环的变量，类似于 C 中 for 循环的 i 变量，items 表示循环的对象。通俗的说，就是每一次循环的时候从 itmes 中取出一个元素，将其赋值给 item。

### 跳转语句

#### break 语句

break 常用于循环语句中来强制退出循环，不执行循环结构中剩余的语句。break 也可以用于 switch 分支语句，但是 switch 默认在每个 case 分支之后隐式地添加了 break，如果一定要加上 break，程序运行也不受影响。

在循环体中使用 break 语句有两种方式：可以带标签，也可以不带标签。语法格式如下：

```Swift
break;  // 不带标签
break label; // 带标签，label 是标签名
```

定义标签的时候后面需要跟一个冒号。不带标签的 break 语句使程序跳出所在层的循环体。而标签的 break语句使程序跳出标签指示的循环体。例如：

```Swift
label_1: for x in 0...< 5 {
    label_2: for y in 0...< 5 {
        if x == y + 1 {
            break label_1;
        }
        print(x, y);
    }
}
print("End!");
// 输出为: 0 0
//        0 1
//        0 2
//        0 3
//        0 4
//        End!
```

#### continue 语句

continue 语句用来结束本次循环，进入下次循环，但如果下次循环的条件不成立就结束循环。和 break 语句一样，continue 语句也有两种使用方式：可以带标签，也可以不带标签。参考 break 带标签的使用方式。

```Swift
continue;  // 不带标签
continue label; // 带标签，label 是标签名
```

#### fallthrough 语句

fallthrough 称为贯通语句，只能使用在 switch 中。switch 中的 case 语句默认不贯通，即执行完一个 case 分支就跳出 switch 语句。如果需要多个分支贯通，就可以使用 fallthrough 语句。

```Swift
var j = 1;
var x = 4;
switch x {
case 1:
    j += 1;
case 2:
    j += 1;
case 3:
    j += 1;
    fallthrough;
case 4:
    j += 1;
    fallthrough;
default:
    j += 1;
}
print("j = \(j)");  // 输出为：3
```

### 范围与区间运算符

Swift 中定义范围可以使用闭区间（`...`）和半开区间（`..<`）运算符。
闭区间：下临界值 ≤ 范围 ≤ 上临界值
半开区间： 下零界值 ≤ 范围 < 上零界值

```Swift
90 ... 100    // 表示 90 ≤ 范围 ≤ 100
90 ..< 100    // 表示 99 ≤ 范围 < 100
```

## 原生集合类型

Swift 中提供了三种数据结构的实现：数组、Set[^Set] 和字典。字典也映射或散列表。

![CollectionTypes_intro_2x](media/15214750084125/CollectionTypes_intro_2x.png)

[^Set]: 是不能重复的集合，有的翻译为“集”，“集合”，“套”。      —— 《从零开始学Swift（第二版）》

### 数组

> 数组是一串有序的由相同类型元素构成的集合。

数组更加关心元素是否有序，而不关心是否重复。和其他编程语言一样，Swift 的数组也是以下标 0 开头。

Swift 数组类型是 Array，是一个结构体类型，是一个一维泛型[^泛型]集合。

[^泛型]: 泛型是程序设计语言的一种特性，允许程序员在强类型程序设计语言中编写代码时定义一些可变的部分，那些部分在使用前必须指明。      —— 百度百科：[泛型](https://baike.baidu.com/item/%E6%B3%9B%E5%9E%8B)

注意：
> Foundation 框架中也有数组类型 NSArray。NSArray 是一个类，而不是结构体。

**数组的声明和初始化**

声明一个数组使用下列语法：

```Swift
var studentList_1: Array<type>
var studentList_2: [type]       // 简化写法
```

只声明了数组还不能够使用，没有开辟内存空间，还需要对数组进行初始化。一般声明和初始化是同时进行的。

```Swift
var studentList_1: Array<String> = ["张三","李四","王五"];
var studentList_2: [String] = ["张三","李四","王五"];
let studentList_3: [String] = ["张三","李四","王五"];
var studentList_4: [String]();        // 初始化一个 String 类型的空数组
```

使用 let 关键字声明的数组是不可变数组，必须在声明的同时初始化，一旦初始化之后就不可以修改。
使用 var 关键字声明的数组是可变数组，和 Foundation 框架中的 NSMutableArray 类型的数组类似是可变的，可以对其进行添加、删除、插入、替换等操作。

**可变数组的操作**

添加：

1. 可以使用`append(_:)`方法。
2. 也可以使用`+`或`+=`运算符将两个数组合并在一起。

```Swift
var studentList_1: [String] = ["张三","李四","王五"];
var studentList_2: [String] = ["小明","小李"];
var studentList_3: [String] = ["小红"];
studentList_1 = studentList_1 + ["小刚","小刘"];
studentList_2.append("小王");
studentList_3 += studentList_2;
print(studentList_1);     // 输出为：["张三", "李四", "王五", "小刚", "小刘"]
print(studentList_2);     // 输出为：["小明", "小李", "小王"]
print(studentList_3);     // 输出为：["小红", "小明", "小李", "小王"]
```

插入：
 
插入使用数组的`insert(_: at:)`方法。

```Swift
var studentList_1: [String] = ["张三","李四","王五"];
studentList_1.insert("小明", at: 1);
print(studentList_1);
```

删除：

删除使用数组的`remove(at:)`方法。

```Swift
var studentList_1: [String] = ["张三","李四","王五"];
studentList_1.remove(at: 1);
print(studentList_1);
```

替换：

替换数组元素直接使用对应数组的下标。

```Swift
var studentList_1: [String] = ["张三","李四","王五"];
studentList_1[0] = "小红";
print(studentList_1);
```

### 字典

Swift 中的字典允许按照某个键来访问元素。字典是由两部分集合构成的，一个是键（key）集合，另一个是值（value）集合。键集合是不能有重复元素的，而值集合可以有重复，键和值是成对出现的。

Swift 中字典类型是 Dictionary，Dictionary 也是结构体类型，也是一个泛型集合。

注意：
> Foundation 框架中也有字典类型 NSDictionary。NSDictionary 是一个类，不是结构体。

**字典声明和初始化**

声明一个字典使用下列语法：

```Swift
var studentDictionary_1: Dictionary<type1, type2>;
var studentDictionary_2: [type1: type2];     // 简化写法
```

只声明了字典还不能够使用，没有开辟内存空间，还需要对字典进行初始化。一般声明和初始化是同时进行的。

```Swift
var studentDictionary_1: Dictionary<Int, String> = [102: "张三", 105:"李四", 109:"王五"];
var studentDictionary_2 = [102: "张三", 105:"李四", 109:"王五"];
let studentDictionary_3 = [102: "张三", 105:"李四", 109:"王五"];
var studentDictionary_4 = [Int: String]();     // 初始化一个 Int: String 类型的空字典
```

使用 let 关键字声明的字典是不可变字典，必须在声明的同时初始化，一旦初始化之后就不可以修改。
使用 var 关键字声明的字典是可变字典，和 Foundation 框架中的 NSMutableDictionary 类型的字典类似是可变的，可以对其进行添加、删除、插入、替换等操作。

**可变字典的操作**

增加：

字典元素的追加简单，只要给一个不存在的键赋一个有效值，就能增加一个“键-值”对元素。

```Swift
var studentDictionary = [102: "张三", 105:"李四", 109:"王五"];
studentDictionary[110] = "小明";
print(studentDictionary);       // 输出为：[109: "王五", 102: "张三", 105: "李四", 110: "小明"]
```

删除：

删除字典元素有两种方法：

1. 给键赋值为 nil，这样就可以删除一个元素。
2. 通过字典的 `removeValue(forKey:)` 方法删除字典元素，方法返回的值是要删除的值。

```Swift
var studentDictionary = [102: "张三", 105:"李四", 109:"王五"];
studentDictionary[102] = nil;
studentDictionary.removeValue(forKey: 105);
print(studentDictionary);     // 输出为：[109: "王五"]
```

替换：

字典元素替换也有两种方法：

1. 直接给一个存在的键赋值，这样新的值会替换旧的值。
2. 通过`updateValue(_:forKey:)` 方法替换，方法返回值是要替换的值。

```Swift
var studentDictionary = [102: "张三", 105:"李四", 109:"王五"];
studentDictionary[102] = "小明";
studentDictionary.updateValue("小红", forKey: 109);
print(studentDictionary);     // 输出为：[102: "小明", 105: "李四", 109: "小红"]
```

### Set

> Swift 中的 Set 集合是由一串无序的，不重复的相同类型元素构成的集合。

如图是一个班级的 Set 集合。这些集合中有一些学生，这些学生是无序的，不能通过下标索引访问，而且没有重复的同学。

Swift 的 Set 类型是 Set。Set 是结构体类型，也是一个一维泛型集合。Foundation 框架中也有 Set 类型 NSSet。NSSet 是一个类，而不是结构体。

**Set 的声明和初始化**

声明一个 Set 类型时可以使用下面的语：

```Swift
var studentList: Set<type>    //注意 Set 类型没有像数组和字典一样的简写方式
```

只声明了字典还不能够使用，没有开辟内存空间，还需要对字典进行初始化。一般声明和初始化是同时进行的。

```Swift
let studentList_1: Set<String> = ["张三","李四","王五"];
var studentList_2 = Set<String>();     // 初始化空 Set
let studentList_3: Set<String> = ["王五","李四","张三"];

if studentList_1 == studentList_3 {
    print("元素内容相同，顺序不同的 Set 集合相等");
} else {
    print("元素内容相同，顺序不同的 Set 集合不相等");
}
// 输出为：元素内容相同，顺序不同的 Set 集合相等
```

可变 Set 集合与不可变 Set 集合之间的关系，类似于不可变数组和可变数组之间的关系。Foundation 框架中也有可变 Set 类型 NSMutableSet。

**可变 Set 的操作**

增加：

因为 Set 没有顺序，所以对 Set 进行的增加操作和插入操作作用相同。
插入元素使用 Set 集合的`insert(_:)`方法实现。

```Swift
var studentList: Set<String> = ["张三","李四","王五"];
studentList.insert("小明");
print(studentList);      // 输出为：["张三", "小明", "王五", "李四"]
```

删除：

删除元素使用 Set 集合的`removeFirst()`和`remove(_:)` 方法实现。

```Swift
var studentList: Set<String> = ["张三","李四","王五"];
studentList.removeFirst();
print(studentList);             // 输出为：["张三", "李四"]
studentList.remove("李四");
print(studentList);             // 输出为：["张三"]
```

## 函数

### 定义函数

函数的定义语法格式如下：

```Swift
func 函数名(实际参数标签 形式参数名: 参数类型) -> 返回值类型 {
    语句组;
    return 返回值;
}
```

定义 Swift 的函数的关键字是`func`，函数名需要符合标识符命名的规范；每一个函数的形式参数（即函数名后括号中的全部内容）都包含`实际参数标签`和`形式参数名`，具体规则请看下面，每个参数之间使用逗号`,`分隔。使用`->`指定返回值类型，返回值可以是单个，也可以是多个，多个值的时候使用元组类型实现，如果没有返回值，则`-> 返回值类型`可以省略，同时`return 返回值`也可以省略。

注意：

无返回值类型的函数有三种表示形式：

```Swift
// 第一种
func 函数名(实际参数标签 形式参数名: 参数类型) {
    语句组;
}

// 第二种
func 函数名(实际参数标签 形式参数名: 参数类型) -> () {
    语句组;
}

// 第三种
func 函数名(实际参数标签 形式参数名: 参数类型) -> Void{
    语句组;
}
```

**函数实际参数标签和形式参数名**

`实际参数标签`用在调用函数的时候，在调用函数的时候每一个实际参数前边都要写实际参数标签。形式参数名用在函数的实现当中。默认情况下，形式参数使用它们的形式参数名作为实际参数标签（即不写出实际参数标签的时候，形式参数名也作实际参数标签使用，如下代码中的第 2 个示例）。

在定义函数时，不使用实际参数标签也是可以的，但是不使用实际参数标签时，要么使用上面提到的用形式参数名代替，要么使用第二种方式：下划线`_`关键字代替实际参数标签，这样在调用函数的时候就可以不写实际参数标签。具体的使用方法，参考下面示例代码：


```Swift
// 1. 两个形参都写出实际参数标签
func rectangleArea(W width: Int, H height: Int) -> Int {
    let area = width * height;
    return area;
}
print("10x20 的长方形的面积：\(rectangleArea(W: 10, H: 20))");     
// 调用时都使用实际参数标签
// 结果为：10x20 的长方形的面积：200


// 2. 两个参数都使用形式参数名作为实际参数标签
func rectangleArea(width: Int, height: Int) -> Int {
    let area = width * height;
    return area;
}
print("20x30 的长方形的面积：\(rectangleArea(width: 20, height: 30))");
// 调用时都使用形式参数名代替实际参数标签
// 结果为：20x30 的长方形的面积：600


// 3. 一个参数使用 _ 关键字，另一个使用实际参数标签
func rectangleArea(_ width: Int,H height: Int) -> Int {
    let area = width * height;
    return area;
}
print("30x40 的长方形的面积：\(rectangleArea(30, H: 40))");
// 调用时使用 _ 关键字的参数省略实际参数标签，使用实际参数标签的参数写出实际参数标签
// 结果为：30x40 的长方形的面积：1200

// 4. 一个参数使用 _ 关键字，另一个使用形式参数名代替实际参数标签
func rectangleArea(_ width: Int, height: Int) -> Int {
    let area = width * height;
    return area;
}
print("40x50 的长方形的面积：\(rectangleArea(40, height: 50))");
// 调用时第一个参数使用 _ 关键字的参数省略实际参数标签，第二个使用形式参数名代替实际参数标签
// 结果为：40x50 的长方形的面积：2000
```

由上面代码可以得出总结，Swift 函数调用时，必须指出实际参数标签。但是实际参数标签有三种形式：

1. 定义时正常写出实际参数标签，调用时也使用实际参数标签。
2. 定义时使用形式参数名代替实际参数标签（看起来像是省略了实际参数标签，只写了形式参数名），调用时使用形式参数名代替实际参数标签。
3. 定义时使用“_”代替实际参数标签，调用时可以省略实际参数标签（实际上是必须省略）。

### 函数形式参数默认值

在定义函数的时候可以为形式参数设置一个默认值，当调用函数的时候可以忽略该参数。如果调用的时候不传递参数，那么默认传递形式参数默认值。例如下面的示例：

```Swift
func playGame(game: String = "英雄联盟") -> String {
    return game;
}
print("玩\(playGame(game: "绝地求生"))");   // 输出为：玩绝地求生
print("玩\(playGame())");                  // 输出为：玩英雄联盟
```

### 可变形式参数

Swift 中函数的参数个数是可以变化的，它可以接受不确定数量的输入类型形式参数（但是这个参数具有相同的类型），在参数类型名后面添加`...`的语法方式来表示这是个可变形式参数，但是一个函数只能有一个可变形式参数。例如：

```Swift
func sum(numbers: Double...) -> Double {
    var total: Double = 0;
    for number in numbers {
        total += number;
    }
    return total;
}
print(sum(100.0, 20, 30));       // 输出为：150.0
print(sum(30, 80));              // 输出为：110.0
```

### 参数引用传递

Swift 中参数的传递方式有两种：“值类型”和“引用类型”。值类型就是传递的是变量的副本，这样在调用函数的过程中，这个副本如何改变都不会影响原始数据。引用类型类似 C 语言中的指针，在给函数传递参数的时候，可以传递变量的内存地址，在函数的调用过程中会影响原始数据的变化。

Swift 中的关键字`inout`是指定参数的传递方式为引用类型。并且调用函数的时候需要在变量名前面加上地址符`&`。

```Swift
func increment(V value: inout Double, A amount: Double = 1.0) {
    value += amount;
}

var value = 10.0;

increment(V: &value);
print(value);           // 输出为：11.0

increment(V: &value, A: 100.0);
print(value);           // 输出为：111.0
```

上述函数中，函数的形式参数名是 value 和 amount 两个，使用 inout 修饰 value 为引用类型，并且设置 amount 的默认值是 1.0，因为传递的是内存地址，所以函数体中的 value 不能再使用 var 或者 let 声明。本函数要返回的结果是 value 这个参数的值，因为是直接修改原始数据，所以没有必要再写返回值类型语句。

### 函数类型

每个函数都有一个类型，使用函数的类型与使用其他数据（如：Int）类型一样，可以声明常量或者变量，也可以作为其他函数的**参数**或者**返回值**使用。先看下面定义的函数：

```Swift
func rectangleArea(width: Double, height: Double) -> Double {
    let area = width * height;
    return area;
} 

func chineseName(lastName: String, firstName: String) -> String {
    let peopleName = lastName + firstName;
    return peopleName;
}

func sayHello() {
    print("Hello, Swift");
}
```

第一个函数的类型是：`(Double, Double) -> Double`
第一个函数的类型是：`(String, String) -> String`
第一个函数的类型是：`() -> ()`

根据这个结果，可以发现函数的类型就是它的形式参数类型和返回值类型，注意格式一定要正确！函数的类型虽然长，但是可以当作普通类型来使用的。

注意：
函数类型是可以加上形式参数名的。例如：`(_ width: Double, _ height: Double) -> Double`，这同样是`(Double, Double) -> Double`的函数类型，加上形式参数名的作用是方便识别参数的作用，提高代码的可读性，不会改变函数类型，但是在形式参数名前面必须加上关键字`_`，否则编译器会报错。

**函数类型作为返回值类型使用**

把函数类型作为另一个函数的返回类型使用，示例如下：

```Swift
// 定义计算长方形面积的函数
func rectangleArea(width: Double, height: Double) -> Double {
    let area = width * height;
    return area;
}

// 定义计算三角形面积的函数
func triangleArea(bottom: Double, height: Double) -> Double {
    let area = bottom * height * 0.5;
    return area;
}

func getArea(type: String) -> (Double, Double) -> Double {
    
    var returnFunction: (Double, Double) -> Double;
    
    switch type {
    case "rect":  // rect 表示长方形
        returnFunction = rectangleArea(width:height:);
    case "tria":  // tria 表示三角形
        returnFunction = triangleArea(bottom:height:);
    default:
        returnFunction = rectangleArea(width:height:);
    }
    
    return returnFunction;
}

// 获取计算三角形面积的函数
var area: (Double, Double) -> Double = getArea(type: "tria");
print("底 10 高 13，三角形面积为：\(area(10, 15))");// 输出为：底 10 高 15，三角形面积为：75.0

// 获取计算长方形面积的函数
area = getArea(type: "rect");
print("宽 10 高 15，长方形面积为：\(area(10, 15))");// 输出为：宽 10 高 15，长方形面积为：150.0
```

上面代码中 getArea(type:) 函数定义了返回类型为`(Double, Double) -> Double`，并且声明了一个类型也为`(Double, Double) -> Double`的变量 returnFunction。可以读作：“定义一个叫做 returnFunction 的变量，它的类型是‘一个能接受两个 Double 值的函数，并返回一个 Double 值。’”
将这个新的变量指向 rectangleArea(width:height:) 或者 triangleArea(bottom:height:) 函数。”并且 getArea(type:) 函数的返回值为 returnFunction。

**函数类型作为参数类型使用**

也可以把函数类型作为另一个函数的参数类型使用，如下面的示例：

```Swift
// 定义计算长方形面积的函数
func rectangleArea(width: Double, height: Double) -> Double {
    let area = width * height;
    return area;
}

// 定义计算三角形面积的函数
func triangleArea(bottom: Double, height: Double) -> Double {
    let area = bottom * height * 0.5;
    return area;
}

func getAreaByFunc (funcName: (Double, Double) -> Double, a: Double, b: Double) -> Double{
    let area = funcName(a, b);
    return area;
}

// 获取计算三角形面积的函数
var result: Double = getAreaByFunc(funcName: triangleArea(bottom:height:), a: 10, b: 15);
print("底 10 高 13，三角形面积为：\(result)");// 输出为：底 10 高 15，三角形面积为：75.0

// 获取计算长方形面积的函数
result = getAreaByFunc(funcName: rectangleArea(width:height:), a: 10, b: 15);
print("宽 10 高 15，长方形面积为：\(result)");// 输出为：宽 10 高 15，长方形面积为：150.0
```

如上代码，定义了函数 getAreaByFunc(funcName:a:b:)，其第一个参数名是 funcName，参数类型是 `(Double, Double) -> Double`，第二个和第三个参数名分别是 a 和 b，参数类型都是 Double。
后面第一次调用函数 getAreaByFunc(funcName:a:b:)，传递的第一个参数为计算三角形面积的函数名，第二个和第三个分别是两个数值，函数返回了 Double 类型的值。
第二次调用函数 getAreaByFunc(funcName:a:b:)，传递的第一个参数为计算长方形面积的函数名，第二个和第三个分别是两个数值，函数返回了 Double 类型的值。

### 嵌套函数

前面定义的函数都是全局函数，并且将其定义在全局作用域中。我们也可以把函数定义在另一个函数的内部，称之为“嵌套函数”。例如：

```Swift
func calculate(opr: String) -> (Int, Int) -> Int {
    
    // 定义 add 函数
    func add(a: Int, b: Int) -> Int {
        return a + b;
    }
    
    // 定义 sub 函数
    func sub(a: Int, b: Int) -> Int {
        return a - b;
    }
    
    var result: (Int, Int) -> Int;
    switch opr {
    case "+":
        result = add(a:b:);
    default:
        result = sub(a:b:);
    }
    return result;
}

let f1: (Int, Int) -> Int = calculate(opr: "+");
print("10 + 5 = \(f1(10 ,5))");         // 结果为：10 + 5 = 15

let f2: (Int, Int) -> Int = calculate(opr: "-");
print("10 - 5 = \(f2(10 ,5))");         // 结果为：10 - 5 = 5
```

上面的代码首先定义函数 calculate 时，定义其返回的类型是`(Int, Int) -> Int`，这是一个函数类型的返回类型。然后在函数 calculate 中嵌套了两个函数 add 和 sub。再定义了一个变量 result，它的类型也是`(Int, Int) -> Int`，同样也是函数类型。然后使用了 switch 语句。最后定义了两个常量 f1 和 f2，并且它们也都是`(Int, Int) -> Int`类型，在定义的同时初始化调用 calculate 函数。

因为常量 f1 和 f2 和函数 calculate 的类型是相同的，所以可以将其赋值为调用 calculate 函数。

> 默认情况下，嵌套函数的作用域在外函数的内部，可以定义外函数的返回值类型为嵌套函数的函数类型，从而将嵌套函数传递给外函数，被其他调用者使用。

## 闭包

> 定义：在 Swift 中，闭包是自包含的匿名函数代码块，可作为表达式、函数参数和函数返回值，闭包表达式的运算结果是一种函数类型[^闭包定义]。

[^闭包定义]: 源至《从零开始学Swift（第二版）》，关东升 著。

闭包是可以在你的代码中被传递和引用的功能性独立模块。Swift 中的闭包和 C 以及 Objective-C 中的 blocks 很像，还有其他语言中的匿名函数也类似。

闭包能够捕获和存储定义在其上下文中的任何常量和变量的引用，这也就是所谓的闭合并包裹那些常量和变量，因此被称为“闭包”，Swift 会为你管理在捕获过程中涉及到的所有内存操作。

不必担心不熟悉“捕获（capturing）”这个概念，在后面的“值捕获”中会对其进行详细介绍。

在函数章节中有介绍的全局和嵌套函数，实际上是特殊的闭包。闭包符合如下三种形式中的一种：

- 全局函数是一个有名字但不会捕获任何值的闭包
- 嵌套函数是一个有名字且能从其上层函数捕获值的闭包
- 闭包表达式是一个轻量级语法所写的可以捕获其上下文中常量或变量值的没有名字的闭包


### 闭包表达式

闭包表达式是一种在简短行内就能写完闭包的语法。闭包表达式为了缩减书写长度又不失易读明晰而提供了一系列的语法优化。

在上一节嵌套函数中的代码，改造一下，如下：

```Swift
func calculate(opr: String) -> (Int, Int) -> Int {
    
    var result: (Int, Int) -> Int;
    
    switch opr {
    case "+":
        result = {(a: Int, b: Int) -> Int in return a + b};
    default:
        result = {(a: Int, b: Int) -> Int in return a - b};
    }
    return result;
}

let f1: (Int, Int) -> Int = calculate(opr: "+");
print("10 + 5 = \(f1(10 ,5))");          // 结果为：10 + 5 = 15

let f2: (Int, Int) -> Int = calculate(opr: "-");
print("10 - 5 = \(f2(10 ,5))");          // 结果为：10 - 5 = 5
```

上面代码中，`{(a: Int, b: Int) -> Int in return a + b}`就是 Swift 中的闭包表达式。原来代码中的嵌套函数 add 和 sub 被闭包表达式替代。

**闭包表达式的简化**

闭包表达式的语法很灵活，上面代码中把闭包表达式写成了一行。闭包表达式的标准格式如下：

```Swift
{(parameters) -> (return type) in
    statements;
}
```

其中，参数列表是指函数中的参数标签、参数名和参数类型。注意`in`关键字。类型推断是 Swift 的强项，Swift 可以根据上下文环境推测出参数类型和返回值类型。所以闭包表达式还可以简化为如下语法：

```Swift
...

result = {(a: Int, b: Int) -> Int in return a + b};

...

result = {a, b in a - b};     // 简化后

...
```

上述代码是之前的 calculate 函数中的部分代码，`...`表示省略的部分。可以看出，闭包表达式中参数的类型、括号和返回值的类型以及关键字`return`都可以省略掉。但是要注意，省略`return`关键字适用于闭包表达式只有一条 return 语句的情况。

其实上面代码中的闭包表达式还可以再简化！Swift 还提供了省略参数名的功能，使用`$0`，`$1`，`$2`... 来指定闭包中的参数。`$0`代表第一个参数，`$1`代表第二个参数，`$2`代表第三个参数，依此类推。使用省略参数名的功能就必须省略参数列表的定义和`in`关键字。如下：

```Swift
...

result = {(a: Int, b: Int) -> Int in return a + b};

...

result = {$0 - $1};     // 简化后

...
```

### 尾随闭包

闭包表达式可以作为函数的参数传递，如果闭包表达式很长，就会影响程序的可读性。尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。如下示例：

```Swift
func calculate(opr: String, funN:(Int, Int) -> Int) {
    
    switch opr {
    case "+":
        print("10 + 5 = \(funN(10, 5))");
    default:
        print("10 - 5 = \(funN(10, 5))");
    }
}

calculate(opr: "+", funN:{(a: Int, b: Int) -> Int in return a + b});
calculate(opr: "+"){(a: Int, b: Int) -> Int in return a + b};
calculate(opr: "+"){$0 + $1};

calculate(opr: "-"){
    (a: Int, b: Int) -> Int in
    return a - b;
}

calculate(opr: "-"){
    $0 - $1;
}
```

上述代码首先定义了 calculate 函数，其中最后一个参数是 funN 是 (Int, Int) -> Int 函数类型，funN 可以接受闭包表达式。后面调用 calculate 函数，将闭包表达式移动到()之外，这种形式就是尾随闭包。要注意的是，闭包必须是参数列表的最后一个参数，否则就不能使用尾随闭包写法。

### 捕获值

一个闭包能够从上下文捕获已被定义的常量和变量。即使定义这些常量和变量的原作用域已经不存在，闭包仍能够在其函数体内引用和修改这些值。

在 Swift 中，一个能够捕获值的闭包最简单的模型是嵌套函数，即被书写在另一个函数的内部。一个嵌套函数能够捕获外部函数的实际参数并且能够捕获任何在外部函数的内部定义了的常量与变量。

> 作为一种优化，如果一个值没有改变或者在闭包的外面，Swift 可能会使用这个值的拷贝而不是捕获。
Swift 也处理了变量的内存管理操作，当变量不再需要时会被释放。



## 面向对象

在 Swift 中，不仅类具有面向对象特性，结构体和枚举也具有面向对象特性。面向对象（OOP）是现代流行的程序设计方法，是一种主流的程序设计规范。OOP 的基本特征包括：封装性、继承性、多态性。

> 封装性：封装性就是尽可能隐蔽对象的内部细节，对外形成一个边界，只保留有限的对外接口使之与外部发生联系。
> 继承性：一些特殊类能够具有一般类的全部属性和方法，这称作特殊类对一般类的继承。例如，狗是一个类，柴犬、阿拉斯加、中华田园犬等就是特殊类。通常称狗这个类为**父类**，称柴犬、阿拉斯加、中华田园犬这些类为**子类**。
> 多态性：对象的多态性是指在父类中定义的属性或者方法被子类继承后，可以使同一个属性或同一个方法在父类及其各个子类中具有不同的含义，这称为多态性。

前面说到在不同的计算机语言中面向对象也有所不同，在 C++、Java 等语言中面向对象的数据类型只有类，但是在 Swift 中类、结构体、枚举都是面向对象的数据类型，具有面向对象的特征。面向对象中类创建对象的过程称为“实例化”，实例化的结果称为“实例”，类的“实例”也称为“对象”。但是结构体和枚举的实例一般不称为“对象”，这是因为结构体和枚举并不是彻底的面向对象类型，而只是包含了面向对象的一些特点，例如结构体和枚举不能继承。

### 枚举

在 C 和 Objective-C 等其他编程语言中，枚举用来管理一组相关常量的集合，使用枚举可以提高程序的可读性，使代码更加清晰且易于维护，而 Swift 中枚举的作用不仅仅是定义一组常量以及提高程序的可读性了，它还具有面向对象特性。

Swift 中使用`enum`关键字声明枚举类型，具体定义如下：

```Swift
enum 枚举名 {
    枚举的定义
}
```

“枚举名”是该枚举类型的标识符。“枚举的定义”是枚举的核心，它由一组成员值和一组相关值组成。

**成员值**

### 结构体



