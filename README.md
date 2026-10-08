# universal-software-engineering

Quy chuẩn viết code tinh gọn cho agent. Skill nằm trong thư mục `universal-software-engineering`.

Giấy phép là MIT. Xem file `LICENSE`.

## Cài

Đã kiểm tra bằng `npx skills` 1.7.1: `npx skills add . -l` thấy skill ở thư mục gốc, không cần `--full-depth`.

Pi dùng cho mọi dự án thì thêm `-g`. Trước khi cài, xóa bản chép tay nếu có. Pi nạp `~/.pi/agent/skills/` trước `~/.agents/skills/`. Bản cũ còn đó thì bản CLI mới bị bỏ qua, và `skills update` không sửa bản cũ.

Trong repo đã clone:

```bash
rm -rf ~/.pi/agent/skills/universal-software-engineering
npx skills add . --skill universal-software-engineering -a pi -g -y
```

Không cần clone, lấy thẳng từ GitHub:

```bash
rm -rf ~/.pi/agent/skills/universal-software-engineering
npx skills add thanhson251189/my-ai-skill --skill universal-software-engineering -a pi -g -y
```

Đổi `-a` theo agent (`claude-code`, `codex`, `cursor`, `github-copilot`, ...). Danh sách agent nằm trong `npx skills add --help`.

`skills` 1.7.1 ghi Pi vào `~/.agents/skills/` khi có `-g`, hoặc `.agents/skills/` của dự án khi không có `-g`. Pi chỉ đọc bản trong dự án nếu dự án đã được trust. Đừng chép tay vào `~/.pi/agent/skills/` hoặc `.pi/skills/` nếu muốn CLI quản lý bản cài. Agent khác dùng thư mục mà CLI của nó ghi ra.

Sau khi cài, mở lại agent hoặc chạy lệnh reload của agent đó. Gọi tay bằng `/skill:universal-software-engineering`.
