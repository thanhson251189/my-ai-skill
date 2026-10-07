# Rust

## Lệnh kiểm tra trước khi báo hoàn thành
```bash
cargo fmt --check
cargo clippy --all-targets -- -D warnings
cargo test
```
`-D warnings` là bắt buộc để cảnh báo thật sự làm lệnh thất bại; thiếu nó thì "không có cảnh báo" chỉ là lời nói.

## Thực hành
- **Xử lý lỗi:** ứng dụng/CLI dùng `anyhow::Result` (kèm `.context(...)`); thư viện dùng `thiserror` để định nghĩa kiểu lỗi riêng.
- **Không dùng `.unwrap()` trong mã chạy chính**, vì panic sẽ làm sập chương trình khi gặp dữ liệu bất ngờ. Dùng `?`, pattern matching, hoặc `.expect("lý do chi tiết vì sao điều này không thể xảy ra")`. Trong test thì dùng `.unwrap()` thoải mái.
- **Hạn chế `.clone()` không cần thiết**; ưu tiên mượn (`&str`, `&[T]`) thay vì sở hữu khi chỉ đọc.
- Tránh over-engineering với generic/trait/lifetime phức tạp khi kiểu cụ thể là đủ. Chỉ trừu tượng hóa khi có từ 3 trường hợp dùng thật.
- Dùng thư viện chuẩn trước; cân nhắc kỹ trước khi thêm crate mới.
- Code `unsafe` chỉ khi thật cần, kèm comment `// SAFETY:` giải thích vì sao an toàn.
