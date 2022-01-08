# RAII with guards

## 简介

[RAII](https://zh.wikipedia.org/wiki/RAII) 意味着 "Resource Acquisition is Initialisation" 这个糟糕的名字。该模式的本质是，资源初始化在对象的构造函数中完成，终结在析构函数中完成。这种模式在Rust中得到了扩展，即使用RAII对象保护某些资源，并依靠类型系统来确保访问总是通过guard object完成。

## 示例

Mutex guard是标准库中这种模式的基本示例（此处为实际实现的简化版本）

```rust,ignore
use std::ops::Deref;

struct Foo {}

struct Mutex<T> {
    // We keep a reference to our data: T here.
    //..
}

struct MutexGuard<'a, T: 'a> {
    data: &'a T,
    //..
}

// Locking the mutex is explicit.
impl<T> Mutex<T> {
    fn lock(&self) -> MutexGuard<T> {
        // Lock the underlying OS mutex.
        //..

        // MutexGuard keeps a reference to self
        MutexGuard {
            data: self,
            //..
        }
    }
}

// Destructor for unlocking the mutex.
impl<'a, T> Drop for MutexGuard<'a, T> {
    fn drop(&mut self) {
        // Unlock the underlying OS mutex.
        //..
    }
}

// Implementing Deref means we can treat MutexGuard like a pointer to T.
impl<'a, T> Deref for MutexGuard<'a, T> {
    type Target = T;

    fn deref(&self) -> &T {
        self.data
    }
}

fn baz(x: Mutex<Foo>) {
    let xx = x.lock();
    xx.foo(); // foo is a method on Foo.
    // The borrow checker ensures we can't store a reference to the underlying
    // Foo which will outlive the guard xx.

    // x is unlocked when we exit this function and xx's destructor is executed.
}
```

## 为什么这样做

当一个资源必须在使用后终结，RAII可用于完成此终结。如果终结后访问该资源会产生错误，这个模式也能用于避免一些错误。

## 优点

防止资源未终结和资源在终结后使用的错误。

## 讨论

RAII是个有用的模式，可保证资源被正确释放或终结。我们可以使用Rust中的借用检查，静态地防止在终结后使用资源所产生的错误。

借用检查的核心目的，是保证到数据的引用不会比数据本身的生命周期更长。RAII guard模式之所以有效是因为guard对象包含了对底层资源的引用，并且只暴露了这种引用。Rust确保guard对象的生命周期不能比底层资源本身的更长，通过guard对象对资源的引用亦然。如果你想了解这是如何工作的，审视`deref`的签名会有些帮助，该签名不包含生命周期省略。

```rust,ignore
fn deref<'a>(&'a self) -> &'a T {
    //..
}
```

返回的到资源的引用与`self` (`'a`)生命周期相同。借用检查由此保证到`T`的引用生命周期比`self`短。

注意实现 `Deref` 并不是这个模式的核心部分，这只是让guard对象用起来更人性化。为guard实现一个`get`方法也能工作。

## 另见

[在析构器中终结](../../idioms/dtor-finally.md)

RAII在C++更为常用：

[zh.cppreference.com](http://zh.cppreference.com/w/cpp/language/raii), [维基百科](https://zh.wikipedia.org/wiki/RAII).

[风格指南](https://doc.rust-lang.org/1.0.0/style/ownership/raii.html)，目前只是个占位符
