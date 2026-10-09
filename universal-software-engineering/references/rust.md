# Rust

## Lệnh kiểm tra trước khi báo hoàn thành
Chạy lệnh của dự án nếu đã có. Nếu đang tạo dự án mới và cả cây là phần vừa tạo, cần có `Cargo.toml` trước các lệnh dưới. Chưa có thì `cargo init` trong thư mục dự án. `cargo new` tạo thư mục con, chỉ dùng khi package phải nằm trong thư mục mới. Chưa có tên package thì hỏi một câu, không bịa:

```bash
cargo fmt
cargo clippy --all-targets
cargo test
```

Thiếu component `clippy` thì báo và bỏ qua lệnh đó; không tự chạy `rustup component add`.

Dự án đã có code: format đúng file lá vừa sửa bằng `rustfmt --edition <edition ghi trong Cargo.toml> path/to/file.rs`. `cargo fmt` luôn format cả package kể cả khi truyền file sau `--`, nên không dùng nó khi còn file chưa format ngoài phạm vi. `rustfmt` theo khai báo `mod` sang file con: file đụng là file gốc thì file con bị format theo là tất yếu, phải báo. Không gọi `rustfmt` mà thiếu `--edition`: mặc định là 2015 và có thể sửa file sai. Chỉ phát hiện mà không ghi thì `cargo fmt --check`. `cargo test` thoát 0 khi không có test; đó không phải đã kiểm thử.

## Thực hành
- **Xử lý lỗi bằng `Result` và kiểu lỗi sẵn có của crate.** Chỉ thêm `anyhow` cho app/CLI, hoặc `thiserror` cho thư viện, khi lỗi cần ngữ cảnh hay kiểu riêng mà thư viện chuẩn làm code rối hơn, và chỉ khi được phép thêm dependency.
- **Không dùng `.unwrap()` trong mã chạy chính**, vì panic sẽ làm sập chương trình khi gặp dữ liệu bất ngờ. Dùng `?`, pattern matching, hoặc `.expect("reason this state is unreachable")`. Chuỗi trong `expect` theo ngôn ngữ của dự án, không chép tiếng Việt từ skill. Trong test thì dùng `.unwrap()` thoải mái.
- **Hạn chế `.clone()` không cần thiết**; ưu tiên mượn (`&str`, `&[T]`) thay vì sở hữu khi chỉ đọc.
- Tránh over-engineering với generic/trait/lifetime phức tạp khi kiểu cụ thể là đủ. Chỉ trừu tượng hóa khi có từ 3 trường hợp dùng thật.
- Dùng thư viện chuẩn trước; cân nhắc kỹ trước khi thêm crate mới.
- Code `unsafe` chỉ khi thật cần, kèm comment `// SAFETY:` giải thích vì sao an toàn.
