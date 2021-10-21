---
title: Series Rust là cái rỉ sét - #1 Rust là gì?
tags: Rust
---
#1. Lai lịch, quảng cáo

Rust (phiên âm: rớtss) là một ngôn ngữ lập trình, ra đời vào năm 2006 bởi một kỹ sư của Mozilla - Graydon Hoare, nhưng mãi cho tới tận 2014 mới release bản stable 1.0 và người ta cũng chỉ quan tâm mốc thời gian này thôi. Rust là một ngôn ngữ lập trình hệ thống đa mục đích, được quảng cáo là có tốc độ xé gió như exciter, chống leak ram hiệu quả như honda wave tiết kiệm xăng, chặn đứng được bug liên quan tới data race (bug hay gặp khi nhiều thread cùng đọc/ghi một vùng nhớ), các bug liên quan tới NULL (vd: NullPointException), nói chung rất chi là safe-and-memory - Taylor Rust, nhưng khác biệt ở chỗ Rust không thuê nhân viên dọn rác (garbage collector)! Mà không thuê thì sẽ tiết kiệm tài nguyên, tiết kiệm thời gian hơn rồi phải không.

Hello world của Rust
```rust
fn main() {
  println!("Hello World!");
}
```
Bạn thấy sao? Có phải đang khó chịu vì đến cái chữ function cũng viết tắt không? Lại còn chấm than trong keyword nữa :))

<!-- more -->
