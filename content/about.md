---
title: "关于 Swift 错误诊断的教育注释"
date: 2019-11-07T21:18:07Z
lastmod: 2020-03-27T22:29:48Z
---

> 译者注：简单来说，如果普通的编译器会告诉你：
> > “你的苹果掉地上了，要不尝试把它捡起来？”
>
> 那与之对应的教育注释就会是《万有引力简述》。

## 教育注释

教育注释是附在诊断中用于解释相关语言概念的简短文档。They are intended to further Swift's goal of progressive disclosure by providing a learning resource at the point of use when encountering an error message for the first time.在极少数的情况下，他们还允许主要的诊断消息使用更专业的术语，而不用担心像“名义类型”（nominal type）这样的词会对新手不够友好。

When outputting diagnostics on the command line, educational notes will be printed after the main diagnostic body if enabled using the `-print-educational-notes` driver option. When presented in an IDE, it's expected they will be collapsed under a disclosure arrow, info button, or similar to avoid cluttering output.

教育注释应该：

- 解释单个语言概念。这样就能把它们在多个诊断之间共享，并保持清楚、简洁、明了。
- 使用完整的（英文）句子。和主要的诊断信息相比，教育注释为更长的格式，因此没有比较省略字词或标点符号。
- 一般不超过 3 至 4 个自然段。Educational notes should be clear and easily digestible. Messages which are too long also have the potential to create UX issues on the command line.
- 应该简单易懂。Educational notes should be beginner friendly and avoid assuming unnecesary prior knowledge. The goal is not only to help users understand what a diagnostic is telling them, but also to turn errors and warnings into "teachable moments".
- 包含《The Swift Programming Language》里相关章节的链接。
- 使用 Markdown 格式编写，但避免使用多余的标记对终端用户体验产生负面影响。

## 如何贡献新的（英文）教育注释

Adding new educational notes is a great way to get familiar with the process of contributing to Swift, while also making a big impact!

添加一个新教育注释的步骤如下：

1. Follow the [directions in the README](https://github.com/apple/swift#getting-sources-for-swift-and-related-projects) to checkout the Swift sources locally. Being able to build the Swift compiler is recommended, but not required, when contributing a new note.
2. Identify a diagnostic to write an educational note for. To associate an educational note with a diagnostic name, you'll need to know its internal identifier. The easiest way to do this is to write a small program which triggers the diagnostic, and run it using the `-debug-diagnostic-names` compiler flag. This flag will cause the internal diagnostic identifier to be printed after the diagnostic message in square brackets.
3. Find any closely related diagnostics. Sometimes, what appears to be one diagnostic from a user's perspective may have multiple variations internally. After determining a diagnostic's internal identifier, run a search for it in the compiler source. You should find:
    - An entry in a `Diagnostics*.def` file describing the diagnostic. If there are any closely related diagnostics the note should also be attached to, they can usually be found nearby.
    - Each point in the compiler source where the diagnostic is emitted. This can be helpful in determining the exact circumstances which cause it to be emitted.
4. Add a new Markdown file in the [`userdocs/diagnostics/`](https://github.com/apple/swift/tree/master/userdocs/diagnostics) directory in the swift repository containing the contents of the note. When writing a note, keep the writing guidelines from the section above in mind. The existing notes in the directory are another useful guide.
5. Associate the note with the appropriate diagnostics in `EducationalNotes.def`. An entry like <code>EDUCATIONAL_NOTES(<wbr>property_wrapper_failable_init, <wbr>"property-wrapper-requirements.md")</code> will associate the note with filename `property-wrapper-requirements.md` with the diagnostic having an internal identifier of `property_wrapper_failable_init`.
6. If possible, rebuild the compiler and try recompiling your test program with `-print-educational-notes`. Your new note should appear after the diagnostic in the terminal.
7. That's it! The new note is now ready to be submitted as a pull request on GitHub.

如果你在完成上面步骤的时候遇到了困难或者有任何的问题，欢迎在 Swift 论坛发帖提问，或者在 GitHub 提交一个合并拉取请求草稿。
