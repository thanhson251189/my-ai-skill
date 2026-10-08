# Shell / Bash

## Lệnh kiểm tra trước khi báo hoàn thành
Có `shellcheck` thì chạy trên đúng file vừa sửa. Có `shfmt` thì chạy `shfmt -d` trên file đó. Chỉ `shfmt -w` trước khi `-d` nếu script mới, hoặc dự án đã có `.editorconfig` hay cấu hình mà shfmt đang dùng. Script có sẵn mà không có cấu hình: không `-w`, vì mặc định tab của shfmt viết lại cả file. Không hardcode tên `script.sh`. Nhiều file thì làm từng file đã đổi. File `.ps1` không dùng `shfmt`, `shellcheck` hay shebang bash. Không tự cài. Thiếu công cụ thì báo và đưa lệnh để người dùng tự chạy. Đó không phải fail của code vừa sửa.

```bash
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
