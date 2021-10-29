---
title: Series Rust là cái rỉ sét - Bài 1 - Setup để code Rust như nào?
tags: Rust
---
Chào mừng các bạn đã trở lại, hi vọng rằng sau khi đọc [Bài 1]() các bạn đã có cảm tình ban đầu với Rust, bài hôm nay chúng ta sẽ bàn về các thành phần và cách setup môi trường để code Rust.
<!-- more-->
# 1. Cài đặt
Là một lập trình viên mình tin rằng việc setup Rust không có gì khó với các bạn.
* Linux: `curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`
* Windows: Download và run [rustup-init.exe](https://static.rust-lang.org/rustup/dist/i686-pc-windows-gnu/rustup-init.exe)

Sau khi cài đặt xong bạn có thể kiểm tra bằng `rustc --version`, ok giờ bài viết mới bắt đầu, phần quan trọng mình muốn nói đến là các thành phần sau khi bạn đã cài đặt.
# 2. Các thành phần chính
Sau khi cài đặt chúng ta sẽ cần quan tâm tới các thành phần sau:

## a. rustup
Công cụ quản lý version, toolchain của Rust, nó vô cùng hữu dụng khi bạn cần kiểm tra bộ cài Rust của mình đang như thế nào. Một số lệnh hay dùng:
* `rustup show` - Hiển thị thông tin về Rust như version, vị trí cài đặt.
* `rustup update` - Update toàn bộ Rust của bạn đến bản latest, bao gồm *cargo*, *rust-analyzer*, *rustc*, *rust-docs*, *rustfmt* ...
* `rustup doc --book` - Mở Rust book mà không cần internet, quá ngon.
* `rustup --help` - Help, tôi không nhớ hết cmd thì có thứ này.

## b. rustc
rustc = rust compiler

Đây là trình biên dịch của rust, bạn có thể biên dịch 1 file *.rs thành binary bằng công cụ này và chạy nó. Một số lệnh cơ bản:

* `rustc main.rs` - Biên dịch file `main.rs` ra binary và bạn có thể run nó trực tiếp, nhưng tất nhiên chúng ta sẽ chỉ dùng `rustc` cho chương trình 1 file đơn giản, khi quản lý project chúng ta dùng `cargo`.
* `rustc main.rs -o okla` - Biên dịch file `main.rs` ra file có tên `okla`. Mặc định không có `-o name` rustc sẽ sinh ra 1 file có tên giống với file `.rs`.

Đọc thêm tại [rustc book](https://doc.rust-lang.org/stable/rustc/index.html)
## c. rustfmt
`rustfmt` là công cụ format code, cú pháp đơn giản `rustfmt [options] <file>`

## d. rust-analyzer
LSP server của Rust, đây là thứ giúp bạn có autocomplete, error check, ... khi code rust đó. Nó là công cụ không thể thiếu khi dev Rust. Bạn cũng không cần lo lắng cách cài đặt, nó sẽ được tải tự động khi bạn dùng các plugin, extension... khi dùng editor, IDE.

## **e. cargo**
Đây là thành phần bạn sẽ dùng thường xuyên nhất, cargo là package manager của Rust tương tự như `npm`, `yarn`, `pip`, `gem` ... Hiện đại chưa, cái này ăn đứt bên C rồi. Các package/thư viện trong Rust được gọi là **crate**, dưới đây là các lệnh cơ bản:

* `cargo new project-name` - Khởi tạo 1 project Rust mới.
* `cargo run` - Build rồi run project.
* `cargo build` - Build project, default là build bản **debug**, để build bản **release** các bạn thêm option `--release`.
* `cargo install crate` - Install crate
* `cargo update` - Install/update các crate có trong file Cargo.toml
 
Đọc thêm tại [Cargo book](https://doc.rust-lang.org/stable/cargo/)
# 3. Setup môi trường để code Rust
Rust là một ngôn ngữ hiện đại nên nhiều tool có sẵn nên cách setup cũng không có gì phức tạp.
* `vscode` - các bạn dùng extension [Rust](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust)
* `atom` - [ide-rust](https://atom.io/packages/ide-rusthttps://atom.io/packages/ide-rust)
* `nvim` - Đây là editor mình dùng chủ yếu, các bạn muốn nhanh gọn lẹ ít config thì có thể dùng [coc.nvim](https://github.com/neoclide/coc.nvim) rồi sau đó `:CocInstall coc-rust-analyzer`, nhưng riêng mình dùng combo [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig), [nvim-lsp-installer](https://github.com/williamboman/nvim-lsp-installer), [nvim-cmp](https://github.com/hrsh7th/nvim-cmp), [rust-tools.nvim](https://github.com/simrat39/rust-tools.nvim). Câu chuyện setup nvim nó dài lắm, mình sẽ có bài riêng sau nếu nhiều người quan tâm.
* `IntelliJ` - là fan của JetBrain thì plugin [này](https://www.jetbrains.com/rust/) sẽ giúp bạn biến IntelliJ IDEA, Clion thành Rust IDE luôn.

Có bạn hỏi code Rust thì dùng IDE/Editor nào là xịn nhất, ngon nhất, thì mình xin trả lời luôn là `vim` nhé :]]. Đùa chút thôi, tất cả extension, plugin mình nêu phía trên đều dùng chung một LSP server là **rust-analyzer**, cho nên các `warning`, `error`, `hint` trên các editor/ide đều sẽ giống nhau và đồng nhất. Nên bạn quen cái gì thì xài cái đó, Rust sinh sau đẻ muộn nên rút được nhiều kinh nghiệm làm LSP server rùi.
# 4. Khởi tạo một project Rust 
Sau khi đã có đủ đồ nghề trong tay, ta hãy bắt đầu làm quen với Cargo.

`cargo new first-project`

Ngày sau khi khởi tạo bạn sẽ thấy cấu trúc thư mục này.

![](/imgs/5.png)

Thư mục `src` sẽ là nơi chứa code của bạn và file `main.rs` là khởi nguồn của mọi con bug sau đó =]]. Tại đây chúng ta quan tâm hơn cả là file `Cargo.toml`
```toml
[package]
name = "first-project"
version = "0.1.0"
edition = "2021"

[dependencies]
```

Đây là file config của project Rust (tương tự package.json trong nodejs). Các `crate` bạn install sẽ được liệt kê dưới `[dependencies]`

# 5. Kết bài
Đây là những đồ chơi và khái niệm cơ bản nhất trước khi bắt đầu vào hành trình chinh phục [Rust book](https://doc.rust-lang.org/stable/book/). Trong bài tiếp theo chúng ta sẽ nhúng tay vào code thử một minigame gằng Rust.
