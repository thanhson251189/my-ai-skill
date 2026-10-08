# universal-software-engineering

Quy chuẩn viết code tinh gọn cho agent. Skill nằm trong thư mục `universal-software-engineering`, không nằm ở thư mục gốc repo.

Giấy phép là MIT. `LICENSE` ở gốc repo và trong thư mục skill; bản cài chỉ mang file trong thư mục skill.

## Cài vào Pi

Pi nạp skill theo thứ tự. Trùng tên thì bản gặp trước thắng, bản sau bị bỏ qua kèm cảnh báo:

1. `.pi/skills/` của dự án, nếu dự án đã được trust
2. `.agents/skills/` của dự án và thư mục cha tới git root, nếu đã trust. Nhiều bản thì bản gần thư mục làm việc hơn thắng
3. `~/.pi/agent/skills/`
4. `~/.agents/skills/`

Đường dẫn trong `settings.json` (`skills`) của cùng scope được nạp trước bản tự tìm. `--skill` trên lệnh gọi Pi thắng các bản tự tìm.

Chỉ giữ một bản. Bản ở `~/.pi/agent/skills/` che bản ở `~/.agents/skills/`. `skills update` không sửa bản bị che. `skills list` 1.7.1 không liệt kê bản copy tay trong `~/.pi/agent/skills/`; thiếu tên trong list không có nghĩa là Pi chưa nạp skill.

`npx skills` 1.7.1 với `-a pi` không ghi vào `~/.pi/agent/skills/`. Có `-g` thì ghi `~/.agents/skills/`. Không có `-g` thì ghi `.agents/skills/` của dự án, và Pi chỉ đọc bản đó sau khi trust dự án.

Muốn CLI quản lý, xóa bản che trước:

```bash
rm -rf ~/.pi/agent/skills/universal-software-engineering
npx skills add . --skill universal-software-engineering -a pi -g -y
```

Từ GitHub, đổi `.` thành `thanhson251189/my-ai-skill`. `npx skills add . -l` thấy skill trong thư mục con, không cần `--full-depth`, vì repo không có `SKILL.md` ở gốc. Đừng thêm `SKILL.md` ở gốc: CLI sẽ ngừng tìm thư mục con trừ khi có `--full-depth`.

Công cụ chép vào `~/.pi/agent/skills/` (CC Switch hoặc copy tay) cũng là bản Pi đọc được. Đừng chạy thêm lệnh CLI ở trên cho cùng skill. Trên Windows, symlink thường thiếu quyền nên công cụ copy thành file thường. Bản đó không theo repo; sửa skill xong phải cài lại.

Đổi `-a` theo agent khác (`claude-code`, `codex`, `cursor`, ...). Danh sách nằm trong `npx skills add --help`.

Sau khi cài, mở lại Pi hoặc chạy `/reload`. Gọi tay bằng `/skill:universal-software-engineering`.
