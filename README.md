# universal-software-engineering

Quy chuẩn viết code tinh gọn cho agent. Skill nằm trong thư mục `universal-software-engineering`.

Repo chưa có file `LICENSE`. Chưa chọn giấy phép thì không xem code này là tự do sử dụng.

## Cài

Đã kiểm tra bằng `npx skills` 1.7.1: `npx skills add . -l` thấy skill ở thư mục gốc, không cần `--full-depth`.

```bash
npx skills add . --skill universal-software-engineering -a pi -y
```

Đổi `-a` theo agent (`claude-code`, `codex`, `cursor`, `github-copilot`, ...). Danh sách agent nằm trong `npx skills add --help`.

Lệnh trên cài Pi vào `.agents/skills/` (trong dự án) hoặc `~/.agents/skills/` nếu thêm `-g`. Pi cũng đọc `~/.pi/agent/skills/` và `.pi/skills/` nếu chép tay. Agent khác dùng thư mục mà CLI của nó ghi ra.

Sau khi cài, mở lại agent hoặc chạy lệnh reload của agent đó. Gọi tay bằng `/skill:universal-software-engineering`.
