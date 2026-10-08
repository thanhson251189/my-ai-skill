# universal-software-engineering

Quy chuẩn viết code tinh gọn cho agent. Skill nằm trong thư mục `universal-software-engineering`, không nằm ở thư mục gốc repo.

Giấy phép là MIT. `LICENSE` ở gốc repo và trong thư mục skill; bản cài chỉ mang file trong thư mục skill.

## Cài cho nhiều agent

Skill dùng chung cho Pi, Claude Code, Codex, OpenCode, Grok và Antigravity / Gemini CLI. Mỗi agent có thư mục user riêng. Cài bản đủ thư mục `universal-software-engineering/` (có `SKILL.md`, `references/`, `LICENSE`), không chỉ chép `SKILL.md`.

| Agent | Thư mục user |
|---|---|
| Pi | `~/.pi/agent/skills/` |
| Claude Code | `~/.claude/skills/` |
| Codex | `~/.codex/skills/` |
| OpenCode | `~/.config/opencode/skills/` |
| Grok | `~/.grok/skills/` |
| Antigravity / Gemini CLI | `~/.gemini/skills/` |

Trên Windows, công cụ cài thường copy thành file thường, không symlink. Các bản không tự theo repo. Sửa skill xong phải cài lại từng agent, không chỉ một chỗ.

Grok và OpenCode còn quét `~/.claude/skills`. Bản trong thư mục riêng và bản Claude phải cùng nội dung, lệch nhau thì agent có thể nạp bản cũ.

Đừng thêm `SKILL.md` ở gốc repo: CLI quản lý skill sẽ ngừng tìm thư mục con trừ khi có `--full-depth`.

## Tài liệu theo agent

- Pi: `docs/pi.md`
