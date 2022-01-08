# strategy

## 简介

[依赖反转原则]: https://zh.wikipedia.org/wiki/%E4%BE%9D%E8%B5%96%E5%8F%8D%E8%BD%AC%E5%8E%9F%E5%88%99

[策略模式](https://zh.wikipedia.org/zh-sg/%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F)
是一种能够分离关注点的技术。它也允许通过 [依赖反转原则] 解耦软件模块。

策略模式的基本思想是，给定一个解决特定问题的算法，我们只在抽象层面定义算法的骨架，并将具体算法的实现分成不同的部分。

这样一来，使用此算法的用户可以选择具体实现，而算法流程保持不变。
换句话说，类的抽象规范并不依赖于派生类的具体实现。但实现必须遵守抽象规范。
这也正是为何我们叫它“依赖反转”。

## 为什么这样做

想象一下，我们正在做一个每个月都生成报告的项目。
我们需要以不同格式（策略）生成报告，比如说，使用`JSON`或者`纯文本`。
但是事情并非一成不变，我们不知道将来我们会受到什么要求。
例如，我们可能需要用全新的格式生成报告，又或者修改现有格式。

## 例子

在这个例子中我们的不变量（或者说抽象）有 `Context`,`Formatter`以及`Report`,
而`Text`和`Json`是我们的 strategy结构体。这些策略需要实现 `Formatter` trait。

```rust
use std::collections::HashMap;

type Data = HashMap<String, u32>;

trait Formatter {
    fn format(&self, data: &Data, buf: &mut String);
}

struct Report;

impl Report {
    // Write should be used but we kept it as String to ignore error handling
    fn generate<T: Formatter>(g: T, s: &mut String) {
        // backend operations...
        let mut data = HashMap::new();
        data.insert("one".to_string(), 1);
        data.insert("two".to_string(), 2);
        // generate report
        g.format(&data, s);
    }
}

struct Text;
impl Formatter for Text {
    fn format(&self, data: &Data, buf: &mut String) {
        for (k, v) in data {
            let entry = format!("{} {}\n", k, v);
            buf.push_str(&entry);
        }
    }
}

struct Json;
impl Formatter for Json {
    fn format(&self, data: &Data, buf: &mut String) {
        buf.push('[');
        for (k, v) in data.into_iter() {
            let entry = format!(r#"{{"{}":"{}"}}"#, k, v);
            buf.push_str(&entry);
            buf.push(',');
        }
        buf.pop(); // remove extra , at the end
        buf.push(']');
    }
}

fn main() {
    let mut s = String::from("");
    Report::generate(Text, &mut s);
    assert!(s.contains("one 1"));
    assert!(s.contains("two 2"));

    s.clear(); // 重用缓冲区
    Report::generate(Json, &mut s);
    assert!(s.contains(r#"{"one":"1"}"#));
    assert!(s.contains(r#"{"two":"2"}"#));
}
```

## 优点

主要的优势是事务的分离。例如，在这个例子中 `Report` 不需要了解 `Json` 与 `Text`的特定实现，
同时输出实现也不关心数据如何被预处理，储存，或者提取。他们只需要得到上下文，和一个特定的trait与方法去实现——也就是，`Formatter` 和 `run`。

## 缺陷

任何 strategy 必须至少实现一个方法，所以方法的数量随着 strategy 的数量而增加。
如果有许多策略可供选择，用户必须知道它们之间的不同。

## 讨论

在上个例子中，所有策略在一个文件实现。
提供不同策略的方法有：

- 单一文件（如本例所示，类似分为模块。）
- 分为模块，例如：`formatter::json`，`formatter::text`
- 使用编译器特性标记，如 `json`特性，`text`特性
- 分为crate，例如，`json`crate, `text` crate

Serde 是实践中策略模式不错的例子。Serde允许 [完全自定义](https://serde.rs/custom-serialization.html) 的序列化行为，
只要为我们的类型手动实现`Serialize`与`Deserialize`trait。例如，我们可以简单地将`serde_json`换成`serde_cbor`，因为它们提供类似的方法。
这让助手crate`serde_transcode`远更有用，更人性化。

然而，在Rust中，我们不需要使用trait实现这个模式。

下面的玩具例子演示了使用Rust `closures`的策略模式的想法。

```rust
struct Adder;
impl Adder {
    pub fn add<F>(x: u8, y: u8, f: F) -> u8
    where
        F: Fn(u8, u8) -> u8,
    {
        f(x, y)
    }
}

fn main() {
    let arith_adder = |x, y| x + y;
    let bool_adder = |x, y| {
        if x == 1 || y == 1 {
            1
        } else {
            0
        }
    };
    let custom_adder = |x, y| 2 * x + y;

    assert_eq!(9, Adder::add(4, 5, arith_adder));
    assert_eq!(0, Adder::add(0, 0, bool_adder));
    assert_eq!(5, Adder::add(1, 3, custom_adder));
}

```

事实上，Rust已经将这个点子应用在`Options`的`map`方法。

```rust
fn main() {
    let val = Some("Rust");

    let len_strategy = |s: &str| s.len();
    assert_eq!(4, val.map(len_strategy).unwrap());

    let first_byte_strategy = |s: &str| s.bytes().next().unwrap();
    assert_eq!(82, val.map(first_byte_strategy).unwrap());
}
```

## 另见

- [策略模式](https://en.wikipedia.org/wiki/Strategy_pattern)
- [依赖反转原则](https://zh.wikipedia.org/wiki/%E4%BE%9D%E8%B5%96%E5%8F%8D%E8%BD%AC%E5%8E%9F%E5%88%99)
- [基于策略设计](https://en.wikipedia.org/wiki/Modern_C++_Design#Policy-based_design)
