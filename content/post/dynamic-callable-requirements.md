---
title: "`@dynamicCallable` 的实现要求"
date: 2020-01-13T02:08:37Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>invalid_<wbr>dynamic_callable_<wbr>type</code>|`@dynamicCallable` attribute requires `X` to have either a valid `dynamicallyCall(withArguments:)` method or `dynamicallyCall(withKeywordArguments:)` method|\n
|<code>missing_<wbr>dynamic_callable_<wbr>kwargs_<wbr>method</code>|`@dynamicCallable` type `X` cannot be applied with keyword arguments; missing `dynamicCall(withKeywordArguments:)` method|
"
---

如果用修饰符[^1] `@dynamicCallable` 标记一个类型，那它还必须要实现 `dynamicallyCall(withArguments:)` 和/或 `dynamicallyCall(withKeywordArguments:)` 方法，否则会导致编译错误。需要注意的是，必须要实现 `dynamicallyCall(withKeywordArguments:)` 才能在调用时使用关键字参数/实际参数标签。

有效的 `dynamicallyCall(withArguments:)` 实现必须：

- 是一个实例方法，而不是 `static` 或者 `class` 方法。
- 接受一个类型遵循 `ExpressibleByArrayLiteral` 协议的参数——通常这会是 Swift 自带的 `Array` 类型。
- 返回一个有效的类型。

有效的 `dynamicallyCall(withKeywordArguments:)` 实现必须：

- 是一个实例方法，而不是 `static` 或者 `class` 方法。
- 接受一个类型遵循 `ExpressibleByDictionaryLiteral` 协议的参数。这个可以是 `Dictionary`、`KeyValuePairs`（用于支持重复的关键字参数）或者其他遵循该协议的类型。
- 让参数类型的关联类型 `Key` 遵循 `ExpressibleByStringLiteral` 协议。这个会是参数关键字的类型。
- 让参数类型的关联类型 `Value` 以及返回类型为有效的类型。

[^1]: attribute，又译作“特性”。
