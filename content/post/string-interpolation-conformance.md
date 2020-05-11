---
title: "遵循 `StringInterpolationProtocol` 协议"
draft: true
date: 2020-04-05T01:44:06Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>missing_<wbr>append_<wbr>interpolation</code>|type conforming to `StringInterpolationProtocol` does not implement a valid `appendInterpolation` method|\n
|<code>append_<wbr>interpolation_<wbr>static</code>|`appendInterpolation` method will never be used because it is static|\n
|<code>append_<wbr>interpolation_<wbr>void_<wbr>or_<wbr>discardable</code>|`appendInterpolation` method does not return `Void` or have a discardable result|
"
---

A type conforming to `ExpressibleByStringInterpolation` uses a helper type called `StringInterpolation` to perform its interpolation. Many types can use [`DefaultStringInterpolation`](https://developer.apple.com/documentation/swift/defaultstringinterpolation), which implements `String`'s interpolation behavior. Types can also implement custom behavior by providing their own type conforming to `StringInterpolationProtocol`.

In addition to its formal requirements, `init(literalCapacity:interpolationCount:)` and `appendLiteral(_:)`, `StringInterpolationProtocol` has an additional, informal requirement, `appendInterpolation`. String interpolations using `\()` syntax are translated into calls to matching `appendInterpolation` methods.

实现 `StringInterpolationProtocol` 协议的类型必须提供至少一个满足以下条件的 `appendInterpolation` 方法：

- 是实例方法（而不是用 `static` 或 `class` 定义的类型方法）
- 不声明返回类型，不显示返回 `Void`，且未被 `@discardableResult` 修饰符[^1]标记
- 至少要有和定义它类型相同的访问级别

除此之外，没有对 `appendInterpolation` 方法参数列表、泛型参数使用、`@available` 版本要求、是否抛出错误等其他的限制。

If `appendInterpolation` is overloaded, the Swift compiler will choose an appropriate overload using the labels and argument types of each interpolation. When choosing an overload, any accessible `appendInterpolation` instance method may be used, even if it does not meet all of the requirements above. However, if a `StringInterpolationProtocol` conformer doesn't have any `appendInterpolation` methods which meet all of the requirements, an error will be reported at compile time.

欲了解更多关于如何修改字符串插值行为的方法，请参阅 [`ExpressibleByStringInterpolation`](https://developer.apple.com/documentation/swift/expressiblebystringinterpolation) 和 [`StringInterpolationProtocol`](https://developer.apple.com/documentation/swift/stringinterpolationprotocol) 协议的标准资料库文档。

[^1]: attribute，又译作“特性”。
