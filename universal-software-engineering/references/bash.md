# Shell / Bash

## Lệnh kiểm tra trước khi báo hoàn thành
```bash
shellcheck script.sh
shfmt -d script.sh     # nếu có cài shfmt
```

## Thực hành
- Luôn mở đầu bằng:
  ```bash
  #!/usr/bin/env bash
  set -euo pipefail
  ```
  để script dừng khi có lỗi, biến chưa khai báo hoặc lỗi trong pipe.
- **Luôn bọc biến trong ngoặc kép:** `"$VAR"`, `"${arr[@]}"`, để tránh lỗi tách từ và glob với tên file có khoảng trắng.
- Dùng `$(command)` thay cho backtick.
- Dùng `[[ ... ]]` thay cho `[ ... ]` trong Bash.
- Dọn dẹp file tạm bằng `trap '...' EXIT`.
- Nếu logic phức tạp (xử lý chuỗi/dữ liệu nhiều), cân nhắc chuyển sang Python thay vì cố nhồi vào Bash.
