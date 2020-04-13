---
title: "Property Wrapper Implementation Requirements"
date: 2020-02-28T04:00:06Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>property_wrapper_<wbr>no_<wbr>value_<wbr>property</code>|property wrapper type `X` does not contain a non-static property named `Y`|\n
|<code>property_wrapper_<wbr>wrong_<wbr>initial_<wbr>value_<wbr>init</code>|`X` parameter type (`Y`) must be the same as its `wrappedValue` property type `Z` or an `@autoclosure` thereof|\n
|<code>property_wrapper_<wbr>failable_<wbr>init</code>|property wrapper initializer `X` cannot be failable|\n
|<code>property_wrapper_<wbr>type_<wbr>requirement_<wbr>not_<wbr>accessible</code>|[`private`/`fileprivate`/`internal`/`public`/`open`] `XXX Y` cannot have more restrictive access than its enclosing property wrapper type `Z` (which is [`private`/`fileprivate`/`internal`/`public`/`open`])|
"
draft: true
---

If a type is marked with the `@propertyWrapper` attribute, it must meet certain requirements to be a valid property wrapper.

First, all property wrapper types must have a property named `wrappedValue`. This property cannot be static and must have the same access level as the property wrapper type. If the property wrapper provides a `projectedValue` property, it is subject to the same requirements.

Second, none of a property wrapper's initializers may be failable. Additionally, if a property wrapper initializer has a `wrappedValue` parameter, the type of that parameter must either be the same as the type of the `wrappedValue` property or an `@autoclosure` of that type.
