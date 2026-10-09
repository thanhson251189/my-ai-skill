# Python

## Công cụ mặc định (chỉ khi dự án chưa có)
- Môi trường và dependency: `uv` (dùng `venv` chỉ khi không cài được uv)
- Lint và format: `ruff`
- Kiểm tra kiểu: `pyright` (hoặc `mypy` nếu dự án đã dùng)
- Test: `pytest`

## Lệnh kiểm tra trước khi báo hoàn thành
Nếu dự án đã có lệnh kiểm tra (script, Makefile, CI), chạy lệnh đó. Không chạy `ruff` hay `pyright` trên dự án đang dùng công cụ khác.

- Cài công cụ vào dự án rồi chạy từ môi trường đó, không dùng bản cài tạm ngoài lockfile. Chạy trong thư mục gốc của dự án mới.
- Thư mục cha đã có `pyproject.toml` mà đây là project riêng thì không chạy `uv add` ở thư mục cha.
- Chưa có `pyproject.toml` thì chạy `uv init` trước khối dưới; đã có thì không chạy. Cả cây là phần vừa tạo, nên được kiểm tra cả cây:

```bash
uv add --dev ruff pyright pytest
uv run ruff format .
uv run ruff check .
uv run pyright
```

Chỉ chạy `uv run pytest` khi đã có file test. Pytest thoát 5 khi không thu thập được test; đó không phải fail. Không thêm `addopts` chỉ để lệnh thoát 0.

Dự án đã có code thì không dùng khối lệnh trên. Đã có công cụ nhưng không có script gom: ví dụ chỉ khi dự án dùng cả uv và ruff/pyright thì `uv run ruff format path/to/file.py`, `uv run ruff check path/to/file.py`, rồi `uv run pyright path/to/file.py`. Trình quản lý hay công cụ khác thì dùng đúng lệnh của dự án đó.

## Thực hành
- **Type hint cho hàm mới và hàm vừa sửa** (tham số và kiểu trả về). Không thêm hint hàng loạt cho hàm không đụng tới. Script một lần hoặc prototype không bắt buộc. Type hint chỉ có giá trị khi có pyright/mypy chạy kiểm tra.
- Dùng `pathlib.Path` cho đường dẫn, không nối chuỗi thủ công.
- Dùng module `logging` thay cho `print()` trong mã ứng dụng (print chỉ chấp nhận trong script nhỏ hoặc output CLI có chủ đích).
- **Bắt exception cụ thể**, không dùng `except:` trần hay `except Exception: pass`, vì sẽ che mất lỗi thật.
- Không dùng giá trị mặc định có thể thay đổi (`def f(x=[])`); dùng `None` rồi khởi tạo trong hàm.
- Đọc cấu hình/secret từ biến môi trường (`os.environ`), không hardcode.
