# 强制消除 Xcode 的警告 ✅

在实际的开发中，可能会偶尔遇到一些「良性的」警告，例如使用已经弃用的方法产生的警告、未使用变量的警告和未实现某个方法的警告等等。有警告代表着代码存在一定的问题，但是这些问题在某些时候是可以忽略的。在 Xcode 中针对这样的问题，可以使用预处理命令来消除一些警告。

首先，Xcode 在我们编写代码的时候，Xcode 底层的 [Clang](https://zh.wikipedia.org/wiki/Clang) 的静态分析器会执行诊断（diagnostic），进行代码的词法分析、语法分析，中间代码生成等等。这样编辑器就能够检查目前代码中存在的问题，所以能够及时提醒开发者代码中存在的问题（当然有时候也会有错误的提示🌚），不过这并不影响 Clang 静态分析器强大的诊断功能带来的好处，关于 Clang 诊断的更多内容参考 [Clang Compiler User’s Manual - Controlling Errors and Warnings](http://clang.llvm.org/docs/UsersManual.html#controlling-errors-and-warnings)。

如果需要消除编译器的产生的警告，所以就需要让编译器知道：这里不需要产生警告。使用 `#pragma` 预编译指令，具体如下代码：

```Objective-C
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-相关命令"
// 需要忽略警告的一行或多行代码
#pragma clang diagnostic pop
```

`clang` 代表是 Xcode 的底层的编译器，`diagnostic` 的意思是「诊断」，`push` 和 `pop` 即是推送和弹出，`ignored` 的意思是「忽略」，`"-相关命令"` 是指 clang 具体的警告命令，例如：在未使用变量时产生的警告命令是 `-Wunused-variable`。如果要忽略具体的警告信息，就要使用对应的警告命令，常用的警告命令如下表：

| clang 常用警告命令 | 对应警告信息 |
| :-: | :-: |
| -Wdeprecated-declarations | 方法已经被弃用 |
| -Wincompatible-pointer-types | 不兼容的指针类型 |
| -Warc-retain-cycles | 循环引用 |
| -Wunused-variable | 未使用变量 |
| -Wundeclared-selector | 未实现某个方法 |
| -Warc-performSelector-leaks | 使用 performSelector |

clang 常用警告命令表中的仅仅是一小部分警告命令，更多的警告命令及信息可以查看 [clangwarnings](https://github.com/NiklasRosenstein/clangwarnings)。

**在 Xcode 中如何查看警告命令**

每一种警告都有对应的警告命令，不过在 Xcode 工作区中显示的都是警告的描述信息，如果想要查看某一种警告对应的 clang 命令，可以在 Xcode 左侧的工作区中点击警告选项，然后找到具体的警告提示，再选择 **Reveal in Log** 选项，之后便会跳转到 clang 日志中对应的警告命令处，实际步骤参考下图：

![查看 Xcode 警告命令](media/15501494579403/%E6%9F%A5%E7%9C%8B%20Xcode%20%E8%AD%A6%E5%91%8A%E5%91%BD%E4%BB%A4.png)

