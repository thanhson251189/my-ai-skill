# Rust

## Lệnh kiểm tra trước khi báo hoàn thành
Chạy lệnh của dự án nếu đã có. Nếu chưa có, dùng:

```bash
cargo fmt --check
cargo clippy --all-targets -- -D warnings
cargo test
```

`-D warnings` chỉ dùng khi dự án đã coi warning là lỗi, hoặc khi đang tạo dự án mới. Không tự thêm cờ này vào dự án đang cho phép warning.

## Thực hành
- **Xử lý lỗi bằng `Result` và kiểu lỗi sẵn có của crate.** Chỉ thêm `anyhow` cho app/CLI, hoặc `thiserror` cho thư viện, khi lỗi cần ngữ cảnh hay kiểu riêng mà thư viện chuẩn làm code rối hơn, và chỉ khi được phép thêm dependency.
- **Không dùng `.unwrap()` trong mã chạy chính**, vì panic sẽ làm sập chương trình khi gặp dữ liệu bất ngờ. Dùng `?`, pattern matching, hoặc `.expect("lý do chi tiết vì sao điều này không thể xảy ra")`. Trong test thì dùng `.unwrap()` thoải mái.
- **Hạn chế `.clone()` không cần thiết**; ưu tiên mượn (`&str`, `&[T]`) thay vì sở hữu khi chỉ đọc.
- Tránh over-engineering với generic/trait/lifetime phức tạp khi kiểu cụ thể là đủ. Chỉ trừu tượng hóa khi có từ 3 trường hợp dùng thật.
- Dùng thư viện chuẩn trước; cân nhắc kỹ trước khi thêm crate mới.
- Code `unsafe` chỉ khi thật cần, kèm comment `// SAFETY:` giải thích vì sao an toàn.
