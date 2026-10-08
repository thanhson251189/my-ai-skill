# Shell / Bash

## Lệnh kiểm tra trước khi báo hoàn thành
Có `shellcheck` thì chạy trên đúng file vừa sửa. `shfmt` chỉ là cổng kiểm tra khi script mới, hoặc dự án đã có `.editorconfig` hay cấu hình mà shfmt đang dùng: `shfmt -w` rồi `shfmt -d` trên file đó. Script có sẵn mà không có cấu hình: không chạy `shfmt`. Mặc định tab sẽ viết lại cả file. Nếu lỡ chạy `shfmt -d` trên script đó, thoát 1 chỉ là lệch style cũ, không phải fail của thay đổi này. Script mới hoặc đã có cấu hình: sau `shfmt -w`, `shfmt -d` phải im và thoát 0. Không hardcode tên `script.sh`. Nhiều file thì làm từng file đã đổi. File `.ps1` không dùng `shfmt`, `shellcheck` hay shebang bash. Không tự cài. Thiếu công cụ thì báo và đưa lệnh để người dùng tự chạy. Đó không phải fail của code vừa sửa.

`shellcheck` luôn chạy được khi có binary. Khối `shfmt` chỉ khi script mới hoặc đã có cấu hình như trên:

```bash
shellcheck path/to/the-changed-script.sh
shfmt -w path/to/the-changed-script.sh
shfmt -d path/to/the-changed-script.sh
```

## Thực hành
Chỉ cho script Bash. Không thêm shebang, `set -euo pipefail` hay `[[ ]]` vào file `.ps1`.
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
