---
title: "Nominal types"
date: 2019-11-02T20:48:38Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|`non_nominal_`<wbr>`no_`<wbr>`initializers`|non-nominal type `X` does not support explicit initialization|\n
|`non_nominal_`<wbr>`extension`|non-nominal type `X` cannot be extended|\n
|`associated_type_`<wbr>`witness_`<wbr>`conform_`<wbr>`impossible`|candidate can not infer `X` = `Y` because `Y` is not a nominal type and so can't conform to `Z`|
"
draft: true
---

In Swift, a type is considered a nominal type if it is named.  In other words, it has been defined by declaring the type somewhere in code. Examples of nominal types include classes, structs and enums, all of which must be declared before using them. Nominal types are an important concept in Swift because they can be extended, explicitly initialized using the `MyType()` syntax, and may conform to protocols.

In contrast, non-nominal types have none of these capabilities. A non-nominal type is any type which is not nominal. They are sometimes called "structural types" because they are usually obtained by composing other types. Examples include function types like `(Int) -> (String)`, tuple types like `(Int, String)`, metatypes like `Int.Type`, and special types like `Any` and `AnyObject`.

Whether the name of a protocol refers to a nominal or non-nominal type depends on where it appears in code. When used in a declaration or extension like `extension MyProtocol { … }`, `MyProtocol` refers to a protocol type, which is nominal. This means that it may be extended and conform to protocols. However, when written as the type of a constant or variable, `MyProtocol` instead refers to a non-nominal, existential type. As a result, code like `let value: MyProtocol = MyProtocol()` is not allowed because `MyProtocol` refers to a non-nominal type in this context and cannot be explicitly initialized.
