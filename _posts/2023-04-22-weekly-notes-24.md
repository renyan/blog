---
layout: post
title: "一周格致随想（第24期）ChatGPT生成的一份Rust和WebAssembly学习案例"
description: "一周格致随想（第24期）ChatGPT生成的一份Rust和WebAssembly学习案例"
author: "魏仁言"
date:  2023-04-22 23:44:42 +0800
categories: tech notes
toc: true
---
最近学习运用New Bing 和Google Bard ，生成一份rust 和WebAssembly 案例Hello World代码。 

## Rust实现WebAssembly基础Demo
 
```rust
//hello.rs
// This is a simple WASM program that prints "Hello, World!" to the console.

use std::io::Write;

#[export_name = "add"]
pub fn add(a: i32, b: i32) -> i32 {
    return a + b;
}

#[export_name = "tos"]
pub fn tos() -> *const u8 {
    return b"Hello, World!\n".as_ptr();
}
//导出方法
#[export_name = "say"]
pub fn say() {
    let mut stdout = std::io::stdout();
    stdout.write_all(b"Hello, World!\n").unwrap();
}
```

* Build:

` rustc --crate-type=cdylib --target=wasm32-unknown-unknown hello.rs `

* Web 代码:

```html
<!DOCTYPE html>
<html>

<head>
    <title>Hello, World!</title>
</head>

<body>
    <script>

        function init() {
            console.log("wasm:init");

            fetch('hello.wasm')
                .then(response => response.arrayBuffer())
                .then(bytes => WebAssembly.compile(bytes))
                .then(module => WebAssembly.instantiate(module, {
                    env: {
                        memory: new WebAssembly.Memory({ initial: 256 }),
                    }
                }))
                .then(instance => {
                    // Call an exported function
                    var result = instance.exports.add(1, 2);
                    console.log(result);

                    var result = instance.exports.tos();
                    console.log(result);

                });

            console.log("result:");

        }
        init();

    </script>
</body>

</html>

```

* 解决CRQS问题,启动本地Web Server

` ruby -run -ehttpd . -p8000`

验证显示结果：
http://localhost:8000/hello.html

终端输出结果：
```html
3
hello.html:53 Hello from rust, !

```

以上样例只支持基础的类型，对于String等类型，不支持返回数据。

```
问：请给我介绍下rust 语言的类型与wasm 类型关系

答：
Rust语言和WebAssembly(Wasm)都是近年来受欢迎的技术,用于构建高效且安全的应用程序。它们在类型系统方面有一些相关但又不同的概念:
在Rust语言中,主要有以下几种类型:
• 原生类型:整型(i8到i128)、浮点型(f32和f64)和布尔型(bool)。这些类型直接映射到Wasm值类型。
• 数组类型:ARRAY[Elem; N],其中Elem是元素类型,N是大小。Wasm也支持类似的Array类型。
• 复合类型:元组( Tuple )、记录(struct)和枚举。Wasm支持结构体,但不支持元组和枚举。任何元组或枚举需要在Wasm中手动展开。
• 指针类型:指向其他类型的指针(&T)。Wasm不支持指针,所有引用要通过句柄(handles)代替。
• 所有权类型:声明资源拥有权和借用权的类型,如String。Wasm不支持这些所有权机制,所有资源都视为本地拥有。
• 功能类型:具有参数和返回值的类型(Fn)。Wasm支持将Rust闭包转换为WebAssembly函数。
在Wasm中,主要的类型包括:
• 值类型:i32、i64、f64、f64、等(与Rust原生类型对应)。
• 结构体:支持类似C的结构体。字段必须有固定大小。
• 数组类型:支持一维和多维数组,采用 WASM 语法定义。
• 句柄类型:用于表示借用权和互动对象,对应于Rust的不可变和可变borrow检查。
• 复合类型:仅支持有限的树状数据结构。不支持元组或枚举。
• 函数类型:接受值参数和返回值,用于表示 WebAssembly 函数。

```

### 利用Wasm-Pack打包并解决类型影射问题

hello-wasm代码库生成
` cargo  new -lib hello-wasm`

**wasm-pack**
安装wasm-pack打包：
`cargo install wasm-pack`

添加依赖包：
```rust
[dependencies]
wasm-bindgen = "0.2.84"
```

引入wasm_bindgen包后的新样例代码hello.rs
```rust
// use std::io::Write;
// #[cfg(target_arch = "wasm32")]
use wasm_bindgen::prelude::*;

// #[cfg(target_arch = "wasm32")]
#[wasm_bindgen]
// #[export_name = "add"]
pub fn add(left: i32, right: i32) -> i32 {
    return left + right;
}

// #[cfg(target_arch = "wasm32")]
#[wasm_bindgen]
// #[export_name = "say"]
pub fn say() -> JsValue {
    return JsValue::from_str("Hello from rust, !");
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }
}


```

编译hello-wasm package
` wasm-pack build -t web`

这将要生成以下文件：
```
pkg
├── hello_wasm.d.ts
├── hello_wasm.js
├── hello_wasm_bg.wasm
└── hello_wasm_bg.wasm.d.ts
```

```html
<!DOCTYPE html>
<html>

<head>
    <title>Hello, World!</title>
</head>

<body>
    <!-- Rust -->
    <script type="module">
        import init, { add,say } from "../pkg/hello_wasm.js";
        init().then(() => {

            // import add from instance;
            var result = add(1, 2);
            console.log(result);

            var result = say("WebAssembly");

            console.log(result);
        });
    </script>
</body>

</html>

```

New Bing 和Bard  使用过程中的一些心得，最好使用英文进行询问，回答的结果比较好，可以针对答案进行多次交叉验证询问，尤其是会提供一些新的库案例，不知道的话，可以再结合库的文档进行交叉学习，这样比较高效。

另外ChatGPT 应用于新技术学习，它可以快速查询库的使用方法，及设计模式的样例代码，Nginx 配置参数等，效率提升不少。尤其是阅读老系统代码，开源库代码，是个很好的解释工具，关键是它还能转译为新的语言实现。

 2023/04/22
