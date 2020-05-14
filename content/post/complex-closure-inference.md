---
title: "闭包类型推断"
date: 2019-12-04T21:37:11Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>cannot_<wbr>infer_<wbr>closure_<wbr>result_<wbr>type</code>|unable to infer [complex] closure return type; add explicit type to disambiguate|
"
---

如果闭包里只有一个表达式，Swift 会利用闭包的函数体、签名和上下文来进行类型推断。比如下面这段代码里，仅使用 `翻倍` 的函数体即可将其类型推断为 `(Int) -> Int`：

```swift
let 翻倍 = {
  $0 * 2
}
```

但对于不是单个表达式的闭包，其函数体就不会被用于类型推断。这和 Swift 语言其他部分的类型推断方法是一致的：一次处理一条语句。例如下面这段代码就会报错，因为既无法通过 `偶数翻倍` 的上下文推断类型，也没有提供闭包的签名：

```swift
// error: unable to infer complex closure return type; add explicit type to disambiguate
let 偶数翻倍 = { x in
  if x.isMultiple(of: 2) {
    return x * 2
  } else {
    return x
  }
}
```

要解决这个问题，我们可以在上下文提供额外的信息：

```swift
let 偶数翻倍: (Int) -> Int = { x in
  // ...
}
```

或者写明闭包的签名：

```swift
let 偶数翻倍 = { (x: Int) -> Int in
  // ...
}
```
