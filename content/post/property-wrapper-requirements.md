---
title: "属性包装器的实现要求"
date: 2020-02-28T04:00:06Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>property_wrapper_<wbr>no_<wbr>value_<wbr>property</code>|property wrapper type `X` does not contain a non-static property named `Y`|\n
|<code>property_wrapper_<wbr>wrong_<wbr>initial_<wbr>value_<wbr>init</code>|`X` parameter type (`Y`) must be the same as its `wrappedValue` property type `Z` or an `@autoclosure` thereof|\n
|<code>property_wrapper_<wbr>failable_<wbr>init</code>|property wrapper initializer `X` cannot be failable|\n
|<code>property_wrapper_<wbr>type_<wbr>requirement_<wbr>not_<wbr>accessible</code>|[`private`/`fileprivate`/`internal`/`public`/`open`] `XXX Y` cannot have more restrictive access than its enclosing property wrapper type `Z` (which is [`private`/`fileprivate`/`internal`/`public`/`open`])|
"
---

如果用修饰符[^1] `@propertyWrapper` 标记一个类型，那它还必须满足一定的条件才会是有效的属性包装器。

首先，所有的属性包装器类型都必须要有一个叫做 `wrappedValue` 的属性。这个属性不能是静态的，而且至少要有和属性包装器类型相同的访问级别。属性包装器的 `projectedValue` 属性（如果有的话）也会受到如此限制。

其次，属性包装器不能有可失败的构造器[^2]。另外，如果属性包装器的构造器有一个 `wrappedValue` 参数，那这个参数的类型必须和属性 `wrappedValue` 的类型相同，又或者是返回那个类型的 `@autoclosure`。

[^1]: attribute，又译作“特性”。
[^2]: initializer，又译作“初始化器”。
