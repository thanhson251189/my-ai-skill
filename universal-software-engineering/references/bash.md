# Shell / Bash

## Lệnh kiểm tra trước khi báo hoàn thành
Có `shfmt` thì format đúng file vừa sửa bằng `shfmt -w`, rồi kiểm tra bằng `shfmt -d`. Có `shellcheck` thì chạy trên đúng file đó. Không hardcode tên `script.sh`. Nhiều file thì làm từng file đã đổi. Không tự cài. Thiếu công cụ thì báo và đưa lệnh để người dùng tự chạy. Đó không phải fail của code vừa sửa.

```bash
shfmt -w path/to/the-changed-script.sh
shfmt -d path/to/the-changed-script.sh
shellcheck path/to/the-changed-script.sh
```

## Thực hành
- Script chạy trực tiếp thì mở đầu bằng:
  ```bash
  #!/usr/bin/env bash
  set -euo pipefail
  ```
  để script dừng khi có lỗi, biến chưa khai báo hoặc lỗi trong pipe. Không thêm khối này vào file được `source`, trừ khi file đó đã dùng. `grep` và `diff` trả về 1 khi không khớp; chỗ cố ý kiểm tra "không thấy" thì không để `set -e` dừng script.
- **Luôn bọc biến trong ngoặc kép:** `"$VAR"`, `"${arr[@]}"`, để tránh lỗi tách từ và glob với tên file có khoảng trắng.
- Dùng `$(command)` thay cho backtick.
- Dùng `[[ ... ]]` thay cho `[ ... ]` trong Bash.
- Dọn dẹp file tạm bằng `trap '...' EXIT`.
- Nếu logic phức tạp (xử lý chuỗi/dữ liệu nhiều), cân nhắc chuyển sang Python thay vì cố nhồi vào Bash.
