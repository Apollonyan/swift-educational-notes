---
title: "名义类型（nominal type）"
date: 2019-11-02T20:48:38Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>non_nominal_<wbr>no_<wbr>initializers</code>|non-nominal type `X` does not support explicit initialization|\n
|<code>non_nominal_<wbr>extension</code>|non-nominal type `X` cannot be extended|\n
|<code>associated_type_<wbr>witness_<wbr>conform_<wbr>impossible</code>|candidate can not infer `X` = `Y` because `Y` is not a nominal type and so can't conform to `Z`|
"
---

在 Swift 语言中，名义类型（nominal type）是指有名字的类型。换言之，它是在代码里某个地方通过声明这个类型来定义的。举个例子，名义类型包含所有的类、结构体和枚举类型；它们都需要先定义才能使用。而名义类型之所以特别，是因为他们能够被扩展，通过 `类型名称()` 直接初始化，还能够遵循协议。

任何不是名义类型的类型都是非名义类型（non-nominal type），那它们相对应的也就没有这些功能。因为非名义类型一般是通过组合其他类型得来的，所以偶尔也会被称为结构类型（structural type）。非名义类型包括像 `(Int) -> (String)` 这样的函数类型、`(Int, String)` 这样的枚举类型、`Int.Type` 这样的元类型（metatype）和 `Any` 以及 `AnyObject` 这样的特殊类型。

一个协议是否为名义类型，取决于它出现在什么地方。当在被定义，以及像 `extension 某个协议 { ... }` 这样被扩展时，`某个协议` 是一个具体的协议类型，因此它是名义类型，可以被扩展、可以遵循协议。然而，当被用作常量或变量的类型时，`某个协议` 其实替代了一个非名义但实际存在（existential）的类型。这就导致

```swift
let value: 某个协议 = 某个协议()
```

这样的代码是没法编译的。因为在这种情景下 `某个协议` 是非名义类型，所以它不能被直接初始化。
