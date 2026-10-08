# Quy trình làm việc (áp dụng cho mọi ngôn ngữ)

Bốn nhóm quy tắc dưới đây không phụ thuộc ngôn ngữ. Với lệnh cụ thể của từng ngôn ngữ, xem file cùng thư mục với file này.

## 1. Sửa lỗi: tìm nguyên nhân gốc trước khi vá

Không đoán mò rồi sửa thử. Đi theo thứ tự:

1. **Tái hiện lỗi:** chạy lại để thấy lỗi thật, ghi lại lệnh hoặc bước gây ra lỗi. Nếu chưa tái hiện được, nói rõ và hỏi thêm thông tin thay vì sửa theo phỏng đoán.
2. **Đọc thông báo lỗi và stack trace** kỹ; vị trí lỗi thường chỉ ra hướng đúng.
3. **Thu hẹp phạm vi:** xác định đoạn code, đầu vào hoặc thay đổi gần nhất gây ra lỗi.
4. **Nêu giả thuyết về nguyên nhân gốc** và kiểm chứng bằng log, debugger hoặc test nhỏ, không chỉ suy luận.
5. **Viết test thất bại** tái hiện lỗi khi dự án đã có chỗ đặt test, rồi sửa tối thiểu cho test pass. Test này ở lại để lỗi không quay lại. Chưa có khung test thì không tạo khung mới chỉ vì lỗi này, trừ khi người dùng yêu cầu.
6. **Chạy lại lệnh kiểm tra của dự án** (lint, format, test mà dự án đang dùng) để chắc không làm hỏng chỗ khác. Nếu lệnh đã fail từ trước, không tự sửa lỗi cũ; lấy mức nền trên bản gốc hoặc chỉ trên file vừa sửa, rồi báo riêng các lỗi có sẵn.

Không che triệu chứng: không thêm `try/catch` rỗng, không bỏ qua lỗi, không xóa test đang fail để "cho qua". Nếu sửa xong mà không giải thích được vì sao lỗi xảy ra, coi như chưa xong.

## 2. Giữ đúng phạm vi

- Chỉ sửa những gì người dùng yêu cầu. Không tiện tay refactor, đổi tên, đổi format hàng loạt hay sửa file không liên quan.
- Nếu thấy vấn đề khác trong lúc làm (bug, code xấu, dependency cũ), **báo lại ngắn gọn** và để người dùng quyết định, đừng tự sửa.
- Thay đổi nhỏ và có thể xem xét được: mỗi lần làm một việc, để người dùng dễ kiểm tra và dễ hoàn tác.
- Nếu yêu cầu mơ hồ và có nhiều cách hiểu khác nhau làm ra kết quả khác nhiều, hỏi một câu ngắn trước khi làm.

## 3. Git và an toàn thao tác

**Commit:**
- Khuyên người dùng commit thường xuyên, mỗi commit một thay đổi có ý nghĩa (một tính năng, một lỗi), với message rõ ràng nói *làm gì và vì sao*.
- Trước khi bắt đầu một thay đổi lớn, nhắc commit trạng thái đang chạy tốt để có điểm quay lại.

**Hỏi xác nhận trước khi làm việc khó hoàn tác:**
- Xóa file hoặc thư mục hàng loạt, `rm -rf`
- `git reset --hard`, `git clean -fd`, `git push --force`, viết lại lịch sử
- `DROP TABLE`, `TRUNCATE`, `DELETE` không có `WHERE`, migration phá dữ liệu
- Ghi đè file/dữ liệu người dùng, thay đổi trên môi trường thật (production)

Nói rõ việc sẽ làm và hậu quả, rồi chờ người dùng đồng ý. Ưu tiên cách an toàn hơn khi có (đổi tên/di chuyển thay vì xóa, sao lưu trước, chạy thử với `--dry-run`).

**Dự án mới cần có:**
- `.gitignore` phù hợp ngôn ngữ, không commit secret hay file sinh ra (xem bảng dưới).
- `.env.example` liệt kê tên biến môi trường cần thiết (không kèm giá trị thật); file `.env` thật nằm trong `.gitignore`.
- Nếu lỡ commit secret, báo ngay cho người dùng: cần **thu hồi/đổi secret đó**, vì xóa khỏi commit mới không đủ (lịch sử git vẫn giữ).

## 4. Khóa phiên bản dependency

Dùng lockfile và **commit nó vào git** để dự án chạy lại được giống hệt sau vài tháng. Ghim phiên bản có chủ đích; không tự nâng cấp dependency khi người dùng không yêu cầu. Không tự thêm hoặc xóa lockfile nếu việc đó ngược với loại dự án đang có.

| Ngôn ngữ | Lockfile (commit) | Nên đưa vào `.gitignore` |
|---|---|---|
| Python (uv) | `uv.lock` | `.venv/`, `__pycache__/`, `.pytest_cache/`, `.ruff_cache/` |
| Rust | `Cargo.lock` với binary. Library thì theo quy ước sẵn có của crate, không tự commit hoặc xóa | `target/` |
| TypeScript/JS | `package-lock.json`, `pnpm-lock.yaml` hoặc `bun.lock` (dùng đúng một cái) | `node_modules/`, `dist/`, `.next/` |
| Go | `go.sum` (cùng `go.mod`) | `bin/`, file thực thi build ra |
| C/C++ | `conan.lock` nếu dùng Conan. Với vcpkg thì commit `vcpkg.json`; `builtin-baseline` là field trong file đó, không phải file riêng, và không được bỏ `vcpkg.json` khỏi git | `build/`, `*.o`, `*.exe` |
| Mọi ngôn ngữ | | `.env`, `*.log`, file IDE chỉ của máy mình. Không thêm `.vscode/` vào `.gitignore` nếu repo đang commit cấu hình dùng chung |

Mọi ngôn ngữ đều thêm `.env` vào `.gitignore`. Nếu dự án dùng trình quản lý gói khác với bảng trên, theo công cụ của dự án.
