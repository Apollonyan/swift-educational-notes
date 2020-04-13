---
title: "使用要求带有 `Self` 或关联类型的协议"
date: 2020-03-02T03:21:08Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>unsupported_<wbr>existential_<wbr>type</code>|protocol `X` can only be used as a generic constraint because it has `Self` or associated type requirements|
"
---

在 Swift 语言中，协议可以被用作类型，作为泛型约束，又或是不透明类型的一部分：

```swift
// CustomStringConvertible 可以被用作一个类型，
func foo(bar: CustomStringConvertible) { /* ... */ }

// ……或作为对泛型 'T' 的约束，
func bar<T: CustomStringConvertible>(baz: T) { /* ... */ }

// ……或作为不透明类型的一部分。
func baz() -> some CustomStringConvertible { /* ... */ }
```

虽然 Swift 中所有的协议都可以被用作泛型约束以及不透明类型的一部分，但并不是所有的协议都能被用作类型。准确地说，如果一个协议的要求中使用了 `Self` 或者关联类型，那么它就不能被作为类型使用。`Identifiable` 就是这样的一个例子：其 `var id: ID { get }` 这个要求使用了关联类型 `ID`。因此，下面这段代码是不能编译的：

```swift
func foo(bar: Identifiable) { /* ... */ }
// error: protocol 'Identifiable' can only be used as a generic constraint because it has Self or associated type requirements
```

之所以像 `Identifiable` 这样要求使用 `Self` 或者关联类型的协议不能被用作类型，是因为那样的类型实际上一般并没有什么用处。对于 `var id: ID { get }` 这样有 `Self` 或者关联类型的要求，既然我们并不知道其中的关联类型具体是什么，那自然也就无法使用。

在使用被 `Self` 或者关联类型要求约束的泛型协议时，不透明类型或者自己实现类型擦除能够解决大多数的问题。欲知详情，请参阅《The Swift Programming Language》中 [协议](https://swiftgg.gitbook.io/swift/swift-jiao-cheng/21_protocols)、[泛型](https://swiftgg.gitbook.io/swift/swift-jiao-cheng/22_generics) 和 [不透明类型](https://swiftgg.gitbook.io/swift/swift-jiao-cheng/23_opaque_types) 这些章节。
