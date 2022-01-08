# Rust设计模式

[![gitlocalized ](https://gitlocalize.com/repo/6974/zh/badge.svg)](https://gitlocalize.com/repo/6974/zh?utm_source=badge)

[在线阅读地址]: https://poly000.github.io/patterns-zh/
[DeepL]: https://deepl.com/

这是一本开源的关于Rust设计模式与习语的书，您可以在[这里][在线阅读地址]阅读。

本书使用了[DeepL]进行翻译，然后人工润色。感谢DeepL的优质翻译服务！

感谢《Rust Design Pattern》这本优秀的书！

感谢 [rust-lang-cn](https://github.com/rust-lang-cn) 的
[中英文术语对照表](https://rustwiki.org/wiki/translate/english-chinese-glossary-of-rust)

## 贡献

You are missing content in this repository that can be helpful for others, and
you are eager to explain it? Awesome! We are always happy about new contributions
(e.g. elaboration or corrections on certain topics) to this project.

You can check the [Umbrella issue](https://github.com/rust-unofficial/patterns/issues/116)
for all the patterns, anti-patterns, and idioms that could be added.

We suggest reading our [Contribution guide](./CONTRIBUTING.md) to get more information
on how contributing to this repository works.

## Building with mdbook

This book is built with [mdbook](https://rust-lang.github.io/mdBook/). You can
install it by running `cargo install mdbook`.

If you want to build it locally you can run one of these two commands in the root
directory of the repository:

- `mdbook build`

  Builds static html pages as output and place them in the `/book` directory by
  default.

- `mdbook serve`

  Serves the book at `http://localhost:3000` (port is changeable, take a look at
  the terminal output to be sure) and reloads the browser when a change occurs.

## 许可证

此仓库内容以**MPL-2.0**许可证发布，见[LICENSE](./LICENSE).
