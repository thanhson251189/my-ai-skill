# Python

## Công cụ mặc định (chỉ khi dự án chưa có)
- Môi trường và dependency: `uv` (dùng `venv` chỉ khi không cài được uv)
- Lint và format: `ruff`
- Kiểm tra kiểu: `pyright` (hoặc `mypy` nếu dự án đã dùng)
- Test: `pytest`

## Lệnh kiểm tra trước khi báo hoàn thành
Nếu dự án đã có lệnh kiểm tra (script, Makefile, CI), chạy lệnh đó. Không chạy `ruff` hay `pyright` trên dự án đang dùng công cụ khác.

Nếu chưa có toolchain và đang tạo dự án mới, cài vào dự án rồi chạy từ môi trường đó, không dùng bản cài tạm ngoài lockfile:

```bash
uv add --dev ruff pyright pytest
uv run ruff check .
uv run ruff format --check .
uv run pyright
uv run pytest
```

Dự án đã có mà thiếu công cụ thì báo, không tự thêm.

## Thực hành
- **Type hints đầy đủ** cho tham số và kiểu trả về của mọi hàm. Type hint chỉ có giá trị khi có pyright/mypy chạy kiểm tra.
- Dùng `pathlib.Path` cho đường dẫn, không nối chuỗi thủ công.
- Dùng module `logging` thay cho `print()` trong mã ứng dụng (print chỉ chấp nhận trong script nhỏ hoặc output CLI có chủ đích).
- **Bắt exception cụ thể**, không dùng `except:` trần hay `except Exception: pass`, vì sẽ che mất lỗi thật.
- Không dùng giá trị mặc định có thể thay đổi (`def f(x=[])`); dùng `None` rồi khởi tạo trong hàm.
- Đọc cấu hình/secret từ biến môi trường (`os.environ`), không hardcode.
