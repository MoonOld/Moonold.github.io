---
title: Rustlings疑难记录
subtitle: 将做Rustlings过程中觉得疑惑或者有趣的地方单独列出来
date: 2023-04-07T14:03:22+08:00
draft: false
description: 将做Rustlings过程中觉得疑惑或者有趣的地方单独列出来
keywords: rust
tags:
  - Rust
categories:
  - Lab
toc: true
lightgallery: true
# See details front matter: https://fixit.lruihao.cn/documentation/content/#front-matter
---
<!--more-->

## Ownership
在```./exercises/vecs/vecs1.rs```中
``` rust
fn array_and_vec() -> ([i32; 4], Vec<i32>) {
    let a = [10, 20, 30, 40]; // a plain array
    let v = Vec::from(a);// TODO: declare your vector here with the macro for vectors

    (a, v)
}
```
代码这里的第三行很明显是将a的所有权转移进了from函数中，但是结尾却可以返回为一个元组，按道理来说a在这已经没有生命周期了才对。查看rust源码如下
``` rust
fn from(s: [T; N]) -> Vec<T> {
        <[T]>::into_vec(
            #[rustc_box]
            Box::new(s),
        )
    }
```
也就是调用了数组类型的into_vec方法。这里使用了i32类型，那么调用的就是i32的into_vec方法。到这一步我突然就有点明白为什么这个数组和vec可以共存了。与Java不同，数组本身可能不是一种完全的类型，只要数组元素类型T实现了Copy特征，传递进from的就是Copy的数组。为了验证这个想法，我写了下面这个验证函数。
``` rust
fn array_and_vec() -> ([String; 4], Vec<String>) {
    let a = ["abc".to_string(), "sd".to_string(), "sdf".to_string(), "df".to_string()]; // a plain array
    let v = Vec::from(a);// TODO: declare your vector here with the macro for vectors

    (a, v)
}
```
果不其然，报错如下
``` text
error[E0382]: use of moved value: `a`
  --> exercises/vecs/vecs1.rs:12:6
   |
9  |     let a = ["abc".to_string(), "sd".to_string(), "sdf".to_string(), "df".to_string()]; // a plain array
   |         - move occurs because `a` has type `[String; 4]`, which does not implement the `Copy` trait
10 |     let v = Vec::from(a);// TODO: declare your vector here with the macro for vectors
   |                       - value moved here
11 |
12 |     (a, v)
   |      ^ value used here after move
```
但是实际上并不如此。`array`类型只是自动实现了类型允许的特征。见[array文档](https://doc.rust-lang.org/std/primitive.array.html)，摘抄如下
> Arrays of any size implement the following traits if the *element type* allows it:
> * Copy
> * Clone
> * Debug
> * IntoIterator (implemented for [T; N], &[T; N] and &mut [T; N])
> * PartialEq, PartialOrd, Eq, Ord
> * Hash
> * AsRef, AsMut
> * Borrow, BorrowMut

这里以`Copy`特征为例子探讨一下Rust的源码
``` rust
#[stable(feature = "copy_clone_array_lib", since = "1.58.0")]
impl<T: Copy, const N: usize> Copy for [T; N] {}
```
空的函数体表示直接通过位复制。也就是直接把array原封不动复制过去。在[Copy文档](https://doc.rust-lang.org/std/marker/trait.Copy.html#whats-the-difference-between-copy-and-clone)中，解释了Copy只能是bit-wise copy且不可重载。所以没办法实现类似clone的copy去防止转移所有权了。  

回到最开始的问题，答案一目了然了，这里传递的数组变量并没有转移所有权，因为数组类型自动实现了i32所实现的Copy特征。

## Cow(Copy on Write)
Cow是一个智能指针类型，实现了Copy on write。下面这段代码位于```./exercises/standard_library_types/cow1.rs```
``` rust
    // No clone occurs because `input` is already owned.
    let slice = vec![-1, 0, 1];
    let mut input = Cow::from(slice);
    match abs_all(&mut input) {
        // TODO
        Cow::Owned(_) => println!("I own this slice!"),
        _ => panic!("expected borrowed value"),
    }
```
这个问题是源于注释中 ***No clone occurs*** 这一点似乎和Copy on write的理念不符。就类似上面所有权的问题了，因为```Cow```类型的```From<Vec<T>>```方法实现如下
``` rust
fn from(v: Vec<T>) -> Cow<'a, [T]> {
        Cow::Owned(v)
    }
```
因为接受的就是所有权的转移，所以```from```方法创建新值的时候直接使用了```Owned```的包装。因此```abs_all```处理负数的逻辑实际上是在```Owned```上进行的操作，自然没有发生clone。

## Macros
1. 宏需要使用```#[macro_export]```宏来进行宏的暴露，可以使其全局可见。
``` rust
mod macros {
    #[macro_export]
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}
fn main() {
    my_macro!();
}
```
> Macros labeled with #[macro_export] are always pub and can be referred to by other crates, either by path or by #[macro_use] as described above.  
也就是说，可以直接通过```crate::path::macro!()```去使用，或者使用```#[macro_use]```去import macros。

2. 宏的匹配语句必须要以```;```分号结尾。
``` rust
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    }; // this semicolon matters
    ( $val:expr) => {
        println!("Look at this other macro: {}", $val);
    }; // this semicolon matters
}
```
按照[decl-marcos](https://veykril.github.io/tlborm/decl-macros/macros-methodical.html#macro_rules)解释， ***As noted previously, macro_rules! is itself a syntax extension, meaning it is technically not part of the Rust syntax. It uses the following forms*** 也就是必须要在每个匹配的rule后加分号，这是```macro_rules!```自己的规则，与rust语法中模式匹配是不兼容的。具体语法参见[Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html)

最后想说一句，Rustlings提供了from和into的traits实现题目，这实在是太聪明的决定了。能够独立完成这两个题目，就已经是具备了初步编码rust的能力了。
