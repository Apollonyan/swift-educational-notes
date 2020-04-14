---
title: "关于 Swift 错误诊断的教育性注释"
date: 2019-11-07T21:18:07Z
lastmod: 2020-03-27T22:29:48Z
swiftURL: https://github.com/apple/swift/blob/master/docs/Diagnostics.md#educational-notes
---

> 译者注：简单来说，如果普通的编译器会告诉你：
> > “你的苹果掉地上了，要不尝试把它捡起来？”
>
> 那与之对应的教育性注释就会是《万有引力简述》。

## 教育性注释

教育性注释是附在诊断中用于解释相关语言概念的简短文档，通过在第一次遇到错误信息时提供学习资料来达到帮助逐渐揭秘 Swift 的目标。在极少数的情况下，它们还允许主要的诊断消息使用更专业的术语，而不用担心像“名义类型”（nominal type）这样的词会对新手不够友好。

当在命令行输出诊断的时候，如果通过 `-print-educational-notes` 驱动选项启用了的话，会在主要信息之后打印教育性注释。当在 IDE 中展示的时候，它应该被折叠到指示箭头、信息按钮、或者类似的里面来避免导致找不着重点。

教育性注释应该：

- 解释单个语言概念。这样就能把它们在多个诊断之间共享，并保持清晰、简洁、明了。
- 使用完整的（英文）句子。和主要的诊断信息相比，教育性注释为更长的格式，因此没有比较省略字词或标点符号。
- 一般不超过 3 至 4 个自然段。教育性注释应该清楚易消化，而且太长的信息可能会给命令行用户交互带来问题。
- 应该简单易懂。教育性注释应该对初学者也很友好，同时需要避免假定读者会有多余的背景知识。我们的目标不仅仅是帮助用户明白这些诊断是什么意思，而且还要把错误和警告转变成豁然开朗的契机。
- 包含《The Swift Programming Language》里相关章节的链接。
- 使用 Markdown 格式编写，但避免使用多余的标记对终端用户体验产生负面影响。

## 如何贡献新的（英文）教育性注释

添加新的教育性注释是了解给 Swift 源码做贡献，同时产生显著影响的好方法！具体步骤如下：

1. Follow the [directions in the README](https://github.com/apple/swift#getting-sources-for-swift-and-related-projects) to checkout the Swift sources locally. Being able to build the Swift compiler is recommended, but not required, when contributing a new note.
2. Identify a diagnostic to write an educational note for. To associate an educational note with a diagnostic name, you'll need to know its internal identifier. The easiest way to do this is to write a small program which triggers the diagnostic, and run it using the `-debug-diagnostic-names` compiler flag. This flag will cause the internal diagnostic identifier to be printed after the diagnostic message in square brackets.
3. Find any closely related diagnostics. Sometimes, what appears to be one diagnostic from a user's perspective may have multiple variations internally. After determining a diagnostic's internal identifier, run a search for it in the compiler source. You should find:
    - An entry in a `Diagnostics*.def` file describing the diagnostic. If there are any closely related diagnostics the note should also be attached to, they can usually be found nearby.
    - Each point in the compiler source where the diagnostic is emitted. This can be helpful in determining the exact circumstances which cause it to be emitted.
4. Add a new Markdown file in the [`userdocs/diagnostics/`](https://github.com/apple/swift/tree/master/userdocs/diagnostics/) directory in the swift repository containing the contents of the note. When writing a note, keep the writing guidelines from the section above in mind. The existing notes in the directory are another useful guide.
5. Associate the note with the appropriate diagnostics in [`EducationalNotes.def`](https://github.com/apple/swift/blob/master/include/swift/AST/EducationalNotes.def). An entry like <code>EDUCATIONAL_NOTES(<wbr>property_wrapper_failable_init, <wbr>"property-wrapper-requirements.md")</code> will associate the note with filename `property-wrapper-requirements.md` with the diagnostic having an internal identifier of `property_wrapper_failable_init`.
6. If possible, rebuild the compiler and try recompiling your test program with `-print-educational-notes`. Your new note should appear after the diagnostic in the terminal.
7. That's it! The new note is now ready to be submitted as a pull request on GitHub.

如果你在完成上面步骤的时候遇到了困难或者有任何的问题，欢迎在 Swift 论坛发帖提问，或者用半成品在 GitHub 提交一个合并拉取请求草稿。
