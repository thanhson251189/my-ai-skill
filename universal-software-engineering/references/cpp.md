# C / C++

## Công cụ
- Format: `clang-format`; phân tích tĩnh: `clang-tidy`
- Luôn biên dịch với `-Wall -Wextra -Werror`

## Lệnh kiểm tra trước khi báo hoàn thành
- Biên dịch sạch với cờ cảnh báo ở trên.
- Chạy `clang-tidy` và `clang-format --dry-run --Werror`.
- Chạy test với sanitizer để bắt lỗi bộ nhớ:
  `-fsanitize=address,undefined` (hoặc Valgrind).

## Thực hành
- **C++ hiện đại (C++17 trở lên):** áp dụng RAII triệt để; dùng smart pointer (`std::unique_ptr` mặc định, `std::shared_ptr` chỉ khi thật sự cần sở hữu chung).
- **Không dùng raw pointer để sở hữu bộ nhớ** (`new`/`delete` thủ công). Raw pointer chỉ để quan sát (non-owning).
- Ưu tiên `std::string_view`, `std::span`, container chuẩn thay vì mảng C và con trỏ + độ dài.
- Với C thuần: kiểm tra giá trị trả về của mọi hàm cấp phát/IO, giải phóng tài nguyên ở một điểm thoát rõ ràng.
