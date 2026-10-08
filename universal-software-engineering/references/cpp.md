# C / C++

## Cờ cảnh báo khi chưa có hệ build
- GCC/Clang: `-Wall -Wextra`. MSVC: `/W4`. Không tự bật `-Werror` hay `/WX` nếu dự án chưa bật.
- Không cài `clang-format` hay `clang-tidy` cho dự án mới. Chỉ dùng khi đã có sẵn, và format chỉ khi có file cấu hình như mục dưới.

## Lệnh kiểm tra trước khi báo hoàn thành
Chạy target build và test của dự án (`cmake`, `make`, Meson, MSBuild...). Theo cờ cảnh báo và bộ test đang có.

Nếu chưa có hệ build, chỉ biên dịch khi file vừa sửa là chương trình độc lập: có `main`, không cần include path hay thư viện của dự án. Lệnh tạm, không ghi vào file build. File không dịch riêng được thì báo chưa có hệ build. Không bịa include path, không tạo CMake hay Makefile chỉ để biên dịch được, không bịa test runner.

`clang-format -i` chỉ khi đã có `.clang-format` hoặc `_clang-format` ở file hay thư mục cha. Không truyền `-style=` tự bịa. Chỉ có binary mà không có file cấu hình thì không format: style mặc định LLVM viết lại cả file.

Sanitizer chỉ thêm vào lệnh biên dịch tạm của file vừa sửa, khi compiler hỗ trợ. GCC/Clang: `-fsanitize=address,undefined`. MSVC: `/fsanitize=address` (không có undefined sanitizer tương đương; không truyền cờ `-fsanitize` cho `cl`). Không ghi cờ đó vào CMake, Makefile hay file build lâu dài của dự án.

## Thực hành
- Theo chuẩn C++ dự án đang đặt. Không tự nâng `-std` hay `/std`. Dự án mới thì C++17 là đủ, trừ khi cần API chỉ có ở chuẩn mới hơn.
- **RAII:** `std::unique_ptr` mặc định cho sở hữu; `std::shared_ptr` chỉ khi thật sự cần sở hữu chung.
- **Không dùng raw pointer để sở hữu bộ nhớ** (`new`/`delete` thủ công). Raw pointer chỉ để quan sát (non-owning).
- Ưu tiên container chuẩn. `std::string_view` chỉ từ C++17. `std::span` chỉ từ C++20. Không dùng chúng khi chuẩn dự án thấp hơn.
- Với C thuần: kiểm tra giá trị trả về của mọi hàm cấp phát/IO, giải phóng tài nguyên ở một điểm thoát rõ ràng.
