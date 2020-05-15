---
title: "临时指针"
draft: true
date: 2020-05-08T01:32:59Z
summary: "
|诊断名称|错误信息|\n
|--|--|\n
|<code>cannot_<wbr>pass_<wbr>type_<wbr>to_<wbr>non_ephemeral</code>
|cannot pass `X` to parameter; argument `Y` must be a pointer that outlives the call [to `Z`]|\n
|<code>cannot_<wbr>pass_<wbr>type_<wbr>to_<wbr>non_ephemeral_<wbr>warning</code>
|passing `X` to parameter, but argument `Y` should be a pointer that outlives the call [to `Z`]|\n
|<code>cannot_<wbr>use_<wbr>inout_<wbr>non_ephemeral</code>
|cannot use inout expression here; argument `Y` must be a pointer that outlives the call [to `Z`]|\n
|<code>cannot_<wbr>use_<wbr>inout_<wbr>non_ephemeral_<wbr>warning</code>
|inout expression creates a temporary pointer, but argument `Y` should be a pointer that outlives the call [to `Z`]|\n
|<code>cannot_<wbr>construct_<wbr>dangling_pointer</code>
|initialization of `X` results in a dangling [buffer] pointer|\n
|<code>cannot_<wbr>construct_<wbr>dangling_pointer_<wbr>warning</code>
|initialization of `X` results in a dangling [buffer] pointer|
"
---

Swift 能将函数参数隐式转化为只在短期内有效的临时指针，而这些指针只在调用该函数期间有效。以下的几种情况都能创建临时指针：

- **隐式转化通过 `&` 传递的输入输出参数为指针**：

  ```swift
  func foo(bar: UnsafePointer<Int>) { /*...*/ }
  var x: Int = 42
  foo(bar: &x)
  ```

  在上面的例子中，传递给 `foo` 的 `bar` 是一个指向 `x` 的临时指针，且这个指针只在 `foo` 返回前有效。

  然而，并不是所有输入输出参数都会被转化为临时指针。静态属性，元组和结构体中的实例属性，以及全局变量，只要是没有属性观察器的存储属性，都可以通过输入输出参数的形式转化为非临时指针。

  ---

- **隐式转化字符串为指针**:

  ```swift
  func foo(bar: UnsafePointer<Int8>) { /*...*/ }
  var x: String = "hello, world!"
  foo(bar: x)
  ```

  在上面的例子中，传递给 `foo` 的 `bar` 是一个指向字符串 `x` 它 UTF-8 字符编码缓冲区的临时指针。同样，这个指针只在 `foo` 返回前有效。

  ---

- **隐式转化数组为指针**:

  ```swift
  func foo(bar: UnsafePointer<Bool>) { /*...*/ }
  var x: [Bool] = [true, false, true]
  foo(bar: x)
  ```

  在上面的例子中，传递给 `foo` 的 `bar` 是一个指向 `x` 数组中元素的临时指针。这个指针依旧只在 `foo` 返回前有效。

Temporary pointers may only be passed as arguments to functions which do not store the pointer value or otherwise allow it to escape the function's scope. The Swift compiler is able to diagnose some, but not all, violations of this rule. Misusing a temporary pointer by allowing it to outlive the enclosing function call results in undefined behavior. For example, consider the following incorrect code:

```swift
var x = 42
let ptr = UnsafePointer(&x)
// 使用指针 ptr
```

This code is invalid because the initializer of `UnsafePointer` stores its argument, causing it to outlive the `UnsafePointer` initializer call. Instead, this code should use `withUnsafePointer` to access a pointer to `x` with an explicitly defined scope:

```swift
var x = 42
withUnsafePointer(to: &x) { ptr in
  // Do something with ptr, but don't allow it to escape this closure!
}
```

It's important to note that 以 `withUnsafe` 打头的函数 can also result in undefined behavior if used improperly. For example, the following incorrect code is equivalent to the original temporary pointer example:

```swift
var x = 42
let ptr = withUnsafePointer(to: &x) { $0 }
// 使用指针 ptr
```

上面代码中返回并赋值给 `ptr` 的指针，是从 `withUnsafePointer` 里逃逸出来的指向 `x` 的指针。但因为这个指针只在函数返回前有效，所以这是错误的写法。

---

欲了解更多和正确使用指针有关的知识，请参阅 [`UnsafePointer`](https://developer.apple.com/documentation/swift/unsafepointer) 和其他 Swift 标准资料库相关类型的文档。

<!--
[^1]: 在标准资料库中要求长期有效的非临时指针的地方目前会使用 `@_nonEphemeral` 内部修饰符标记。
-->
