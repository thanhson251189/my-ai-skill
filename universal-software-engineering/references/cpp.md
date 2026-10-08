# C / C++

## Công cụ mặc định (chỉ khi dự án chưa có)
- Format: `clang-format`, nếu đã có sẵn.
- Cảnh báo khi biên dịch file mới: GCC/Clang dùng `-Wall -Wextra`; MSVC dùng `/W4`. Không tự bật `-Werror` hay `/WX` nếu dự án chưa bật.
- Phân tích tĩnh: `clang-tidy` chỉ khi dự án đã dùng hoặc công cụ đã có sẵn.

## Lệnh kiểm tra trước khi báo hoàn thành
Chạy target build và test của dự án (`cmake`, `make`, Meson, MSBuild...). Theo cờ cảnh báo và bộ test đang có.

Nếu chưa có hệ build, biên dịch đúng file vừa sửa với cờ cảnh báo ở trên, rồi chạy test nếu có. Không bịa test runner.

Sanitizer (`-fsanitize=address,undefined`, hoặc AddressSanitizer của MSVC nếu bản compiler có) chỉ dùng khi compiler hỗ trợ và không làm hỏng build hiện tại.

## Thực hành
- Theo chuẩn C++ dự án đang đặt. Không tự nâng `-std` hay `/std`. Dự án mới thì C++17 là đủ, trừ khi cần API chỉ có ở chuẩn mới hơn.
- **RAII:** `std::unique_ptr` mặc định cho sở hữu; `std::shared_ptr` chỉ khi thật sự cần sở hữu chung.
- **Không dùng raw pointer để sở hữu bộ nhớ** (`new`/`delete` thủ công). Raw pointer chỉ để quan sát (non-owning).
- Ưu tiên container chuẩn. `std::string_view` chỉ từ C++17. `std::span` chỉ từ C++20. Không dùng chúng khi chuẩn dự án thấp hơn.
- Với C thuần: kiểm tra giá trị trả về của mọi hàm cấp phát/IO, giải phóng tài nguyên ở một điểm thoát rõ ràng.
