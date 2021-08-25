# 贡献

## 简介

[典型用法]: https://en.wikipedia.org/wiki/Programming_idiom

这本书是Rust编程技术，(反)模式，[典型用法]以及其他表示的目录。

这本书介绍的模式**并不是规则**，但可以被视为编写地道的Rust代码的指引。
我们会整理这本书中 Rust 模式，以便学习者掌握Rust习语的权衡，并且在他们的代码中正确地使用。

如果你想要加入我们，这里有一些方式。

## 讨论区

[讨论区]: https://github.com/rust-unofficial/patterns/discussions

如果你有关于一些内容的问题或想法，而你想要得到社区的反馈，
并且认为不适合提出issue，你可以在我们的[讨论区]提出讨论。

## 撰写新文章

[模式模板]: https://github.com/rust-unofficial/patterns/blob/master/template.md
[Rustlings]: https://github.com/rust-lang/rustlings
[playground]: https://play.rust-lang.org/
[Wayback Machine]: https://web.archive.org/

在编写新文章前，请检查下列资源中有没有已存在的讨论，或者已经有人在编写：

- [Umbrella issue](https://github.com/rust-unofficial/patterns/issues/116),
- [All issues](https://github.com/rust-unofficial/patterns/issues),
- [Pull Requests](https://github.com/rust-unofficial/patterns/pulls)

如果你找不到相关的issue，而且你确定应该提issue而非[讨论区]的帖子，
请提出新的issue。这样我们就可以讨论想法，以及文档未来的内容。
也许您还会得到反馈与投入。

编写新文章时，推荐您复制[模式模板]到适当的目录，再开始编辑。
您可能不希望填写每个小节，而进行删除。您也可以加入附加部分。

请考虑编写低门槛的文章，让[Rustlings]也可以理解并跟随背后的思想过程。
这样我们就能鼓励人们在学习前期开始使用这些。

我们鼓励您编写地道的Rust代码，在[playground]上构建。

如果您使用了博文，或者通常不认为能够存在几年的内容，请使用[Wayback Machine]创建快照，并且在您的文章中使用快照。

不要忘记将您的文章加入`SUMMARY.md`，将它渲染到书中。

请尽早提出 `Draft Pull Request`，这样我们就能跟随您的进程，并且尽早进行反馈（见下节）。

## 风格指南

为了统一这本书的风格，我们建议：

- 跟随官方的Rust教程的 [风格指南](https://github.com/rust-lang/book/blob/master/style-guide.md).
- 跟随 [RFC 1574](https://github.com/rust-lang/rfcs/blob/master/text/1574-more-api-documentation-conventions.md#appendix-a-full-conventions-text).
  Tl;dr:
  - 偏好完整的类型名，例如使用`Option<T>`而非`Option`.
  - 在适用的情况下使用行注释`//`而不是块注释`/* */`。

## 在本地检查文章

在提交PR前运行命令`mdbook build`，确保这本书能正常构建。

以及运行`mdbook test`确保代码示例是正确的。

### Markdown lint

To make sure the files comply with our Markdown style we use [markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli).
To spare you some manual work to get through the CI test you can use the
following commands to automatically fix most of the emerging problems when
writing Markdown files.

- Install:

  ```sh
  npm install -g markdownlint-cli
  ```

- Check all markdown files:
  - unix: `markdownlint '**/*.md'`
  - windows: `markdownlint **/*.md`

- Automatically fix basic errors:
  - unix: `markdownlint -f '**/*.md'`
  - windows: `markdownlint -f **/*.md`

## Creating a Pull Request

"Release early and often!" also applies to pull requests!

Once your article has some visible work, create a `[WIP]` draft pull request
and give it a description of what you did or want to do. Early reviews of the
community are not meant as an offense but to give feedback.

A good principle: "Work together, share ideas, teach others."

### Important Note

Please **don't force push** commits in your branch, in order to keep commit
history and make it easier for us to see changes between reviews.

Make sure to `Allow edits of maintainers` (under the text box) in the PR so
people can actually collaborate on things or fix smaller issues themselves.
