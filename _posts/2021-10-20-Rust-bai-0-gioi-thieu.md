---
title: Series Rust là cái rỉ sét - Bài 0: Rust là gì?
tags: Rust
---
# Tác giả, tác phẩm

Rust (phiên âm: rớtss) là một ngôn ngữ lập trình, ra đời vào năm 2006 bởi một kỹ sư của Mozilla - Graydon Hoare, nhưng mãi cho tới tận 2014 mới release bản stable 1.0 và người ta cũng chỉ quan tâm mốc thời gian này thôi.

<!-- more -->

Rust là một ngôn ngữ lập trình hệ thống đa mục đích, được quảng cáo là có tốc độ xé gió như exciter, chống leak ram hiệu quả như honda wave tiết kiệm xăng, chặn đứng được bug liên quan tới data race (bug hay gặp khi nhiều thread cùng đọc/ghi một vùng nhớ), các bug liên quan tới NULL (vd: NullPointException), nói chung rất chi là safe-and-memory - Taylor Rust, nhưng khác biệt ở chỗ Rust không thuê nhân viên dọn rác (garbage collector)! Mà không thuê thì sẽ tiết kiệm tài nguyên, tiết kiệm thời gian hơn rồi phải không.

Hello world của Rust
```rust
fn main() {
  println!("Hello World!");
}
```
Bạn thấy sao? Có phải đang khó chịu vì đến cái chữ function cũng viết tắt không? Lại còn chấm than trong keyword nữa :))

# Rust lái xe nhanh mà vẫn an toàn ?
## Đánh đổi giữa tốc độ và sự an toàn 
Trước hết bạn hãy nhìn vào hình dưới đây

![](/imgs/1.PNG)

Khi làm việc với Assembly bạn cần phải làm việc với ô nhớ, địa chỉ ô nhớ, tập lệnh ứng với loại CPU đang sử dụng, hãy nhìn đoạn mã Assembly hello world dưới đây và cảm ơn Chúa.
```
;Copyright (c) 1999 Konstantin Boldyshev <konst@linuxassembly.org>
;
;"hello, world" in assembly language for Linux
;
;to build an executable:
;       nasm -f elf hello.asm
;       ld -s -o hello hello.o

section .text
; Export the entry point to the ELF linker or loader.  The conventional
; entry point is "_start". Use "ld -e foo" to override the default.
global _start

section .data
msg db  'Hello, world!',0xa ;our dear string
len equ $ - msg         ;length of our dear string

section .text

; linker puts the entry point here:
_start:

; Write the string to stdout:

mov edx,len ;message length
mov ecx,msg ;message to write
mov ebx,1   ;file descriptor (stdout)
mov eax,4   ;system call number (sys_write)
int 0x80    ;call kernel

; Exit via the kernel:

mov ebx,0   ;process' exit code
mov eax,1   ;system call number (sys_exit)
int 0x80    ;call kernel - this interrupt won't return
```

Nhưng bù lại cho sự tổn thương tâm hồn này đó là Assembly rất gần với mã máy (machine/binary code) ít lớp trừu tượng che phủ, do đó việc biên dịch cũng như thực thi sẽ trực tiếp và nhanh hơn.

Càng nhiều lớp trừu tượng che phủ, chúng ta sẽ càng code ít hơn nhưng bù lại không thể biết được bên dưới compiler đã thêm thắt cái gì vào code của mình. Vd khi lập trình C, C++ chúng ta khai báo biến cùng kiểu của nó (strong typed language) tức là ta đã chỉ định cho compiler biết trước đây là kiểu dữ liệu gì, cần bao nhiêu bộ nhớ, cấp phát tại heap hay stack ... Nhưng với python, js ta không cần khai báo kiểu, đồng nghĩa với việc máy tính sẽ phải thêm 1 bước nội suy ra xem đây là kiểu dữ liệu gì để còn cấp phát bộ nhớ và kết quả là sẽ chậm đi khá nhiều.

Việc tự cấp phát bộ nhớ lại có nhiều vấn đề khác như quên giải phóng bộ nhớ, giải phóng rồi nhưng vẫn truy cập vào, hoặc nhiều con trỏ cùng đọc ghi một vùng nhớ ... Và đó là cội nguồn của những nỗi đau mang tên segmentation fault trong C, C++.

Vấn đề đánh đổi là chuyện rất bình thường trong ngành lập trình hiện đại, nhưng Rust xuất hiện làm cho mọi thứ trở lên mông lung. Rust kết hợp điểm mạnh của cả hai loại trên, tức là chúng ta không cần lo lắng về việc quản lý bộ nhớ (k sợ segfault, memory leak, datarace, ...) nhưng vẫn đảm bảo tốc độ sánh ngang với C, ảo ma canada! Vậy Rust đã làm như thế nào?

## Tốc độ
Giống như C, Rust có khả năng kiểm soát tới đơn vị ô nhớ, heap, stack một cách trực tiếp, ngoài ra Rust còn tuân theo nguyên tắc "You pay for what you use", các API có sẵn của Rust được design và code tối ưu nhất có thể, compile về binary code một cách hiệu quả ít sử dụng tài nguyên nhất, không làm ảnh hưởng đến tốc độ chung của chương trình.

Việc so sánh giữa ngôn ngữ khác là không nên và không cần thiết nên mình để lại link cho ai muốn tham khảo [C vs Rust](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/gcc-rust.html), [C++ vs Rust](https://www.educative.io/blog/rust-vs-cpp)

Mặc dù là ngôn ngữ hệ thống nhưng Rust vẫn có khả năng code OOP thông qua Struct + Trait + Generic (chúng ta sẽ nói về điều này sau).

## Sự an toàn 
Trong một vài case Rust có thể không nhanh bằng C, nhưng chắc chắn nó là ngôn ngữ an toàn hơn C, C++.

Trong C, C++ việc cấp phát bộ nhớ (malloc, new) là hoàn toàn tự do, và việc giải phóng cũng ... tự do luôn :]] Chắc chắn những bạn sinh viên mới bước chân vào đời code C, C++ không ít lần quên giải phóng con trỏ, với chiếc máy tính RAM 16GB chắc nó chẳng nhằm nhò gì nhưng deploy code lên mạch nhúng thì oigioioi. C, C++ không bắt lỗi bạn quên giải phóng bộ nhớ nó chỉ leak ram thôi. Ngoài ra còn các lỗi runtime tiềm ẩn như ghi vào vùng nhớ đã giải phóng (đùa chứ quên giải phóng đã khổ rồi giờ giải phóng rồi còn quên mất để ghi đè vào), chạy multi-thread vào cùng truy xuất 1 biến ...

Giải pháp cho vấn đề này là thuê một bác lao công, thi thoảng vào dọn dẹp mớ con trỏ bạn đã cấp phát quên giải phóng và đó chính là garbage collector (GC), nhiều ngôn ngữ đã áp dụng kĩ thuật này như Go, Java, C#, python, JS ... Ưu điểm là giúp bạn tập trung vào business code, logic code nhưng nhược điểm chết người của nó chính là performance. Hãy cứ tưởng tượng cơ quan bạn có duy nhất 1 phòng họp, đúng nguyên tắc là 1 team họp xong thì phải đợi bác lao công vào dọn dẹp xong thì team khác mới được vào, lần nào cũng vậy thì hiệu suất ăn hành của dev bị giảm đi đáng kể.

Rust không dùng GC, quay lại với câu chuyện họp hành, nếu mỗi team sau khi họp xong đều có ý thức tự dọn rác của mình thì ... bác lao công thất nghiệp và team 1 đi ra thì team 2 vào quẩy luôn được. Nhưng làm sao ai cũng có ý thức tự giác đó, đơn giản thôi, hãy đặt ra các nguyên tắc trước khi rời phòng họp! Ban đầu thì có vẻ chưa quen và khó chịu, nhưng lâu dần nó thành bản năng và ý thức cá nhân. Tuyệt quá phải không nào, Rust đã làm như vậy.

Rust có những bộ nguyên tắc để đảm bảo an toàn cho chương trình, compiler sẽ báo lỗi ngay cho bạn trong lúc biên dịch, và nếu bạn không sửa thì sẽ không build được chương trình. Do đó chương trình được build ra chắc chắn đã được duyệt và khá an toàn cho quá trình runtime. Các nguyên tắc của Rust khá phức tạp mình sẽ nói một cách kỹ thuật trong những bài sau, tại bài viết này, mình sẽ trừu tượng hóa nó thành câu chuyện bất động sản:

* Một lô đất mặc định sẽ bị fix cứng giá trị ngay khi được cấp sổ đỏ. Muốn tăng giảm giá sau này thì phải khai báo kiểu khác.
* Sổ đỏ không có NULL gì hết nên không có chuyện có sổ mà không có đất (NullPointException)
* Một lô đất (ô nhớ) thì chỉ có 1 chủ (biến)
* Lô đất đã được sang tên (gán) cho chủ mới thì cấm ông chủ cũ bén mảng tới 
* Sổ đỏ chỉ tồn tại trong một khu vực (scope), ra khỏi khu vực đó thì sổ đỏ tự hủy.
* Tại một thời điểm, nhiều ông có thể đến để xem đất (read-only), nhưng chỉ có 1 ông có quyền sử dụng đất đó.

Đến đây nhiều bạn sẽ tự hỏi thế lúc code phải nhớ hết đống luật rừng này để code à, khổ sở thế, thế code C, C++ cho nó lành. Từ đã bạn ơi, bạn không cần phải tự làm hết đâu, Rust compiler sẽ giúp bạn giải phóng bộ nhớ bằng cách chèn thêm code vào code của bạn mỗi khi nó phát hiện thấy một biến không còn được sử dụng nữa. Dưới đây là vd minh họa khi Rust và C được biên dịch ra Assembly:

![](/imgs/2.png)

Assembly từ Rust có thêm vài câu lệnh để giải phóng bộ nhớ so với C, thêm một vài lệnh assembly để giữ cho chương trình không bị leak ram và lỗi tiềm ẩn khi chạy production là một đánh đổi rất hợp lý. Sẽ có nhiều người thắc mắc việc này có làm Rust chậm đi so vs C không thì câu trả lời là không. Vì nếu bạn code production bằng C thì kiểu gì bạn cũng phải thêm lệnh free vào code để giải phóng bộ nhớ thôi, chưa kể có thể free nhầm chỗ hoặc double free thì thôi xong luôn.

# Rust có thể làm được gì?
Ez, làm gì cũng được, nhưng là một ngôn ngữ lập trình hệ thống Rust chủ yếu được dùng để cạnh tranh với C/C++ về mặt performance và tăng độ an toàn cho phần mềm, OS ... có thể kể ra các công việc có thể dùng Rust:

* [Core trình duyệt](https://github.com/servo/servo)
* [Web backend](https://www.arewewebyet.org)
* [Mobile app](https://github.com/rust-unofficial/awesome-rust#mobile)
* [Viết terminal](https://github.com/wez/wezterm) (cá nhân mình dùng Wezterm được viết bằng Rust)
* [Cross-platform app](https://github.com/tauri-apps/tauri)
* [Machine learning](https://www.arewelearningyet.com/)
* [Game](https://github.com/rust-unofficial/awesome-rust#games)
* ...

Các bạn có thể tham khảo thêm tại [Awesome Rust](https://github.com/rust-unofficial/awesome-rust)

# Kết bài
Đây là bài mở đầu cho series "Rust là cái gỉ sét". Các bài tiếp theo mình sẽ đi sâu hơn theo sườn của [Rust book](https://doc.rust-lang.org/book/). Các bạn có thể coi như là series translate Rust book sang tiếng việt. Có thể có bài mình sẽ ra dạng vlog.

Ngoài ra đừng quên theo dõi [Blog chính thức của Rust](https://blog.rust-lang.org/) nơi core team đưa ra thông báo về các version, [Bản tin Rust hàng tuần](https://this-week-in-rust.org/) - nơi bạn sẽ biết được các biến động trong cộng đồng rust mỗi tuần.

See ya, God bless you!
