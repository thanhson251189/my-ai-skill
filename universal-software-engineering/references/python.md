# Python

## Công cụ mặc định (khi dự án chưa có)
- Môi trường & dependency: `uv` (dùng `venv` chỉ khi không cài được uv)
- Lint & format: `ruff`
- Kiểm tra kiểu: `pyright` (hoặc `mypy` nếu dự án đã dùng)
- Test: `pytest`

## Lệnh kiểm tra trước khi báo hoàn thành
```bash
ruff check .
ruff format --check .
pyright            # hoặc: mypy .
pytest
```

## Thực hành
- **Type hints đầy đủ** cho tham số và kiểu trả về của mọi hàm. Type hint chỉ có giá trị khi có pyright/mypy chạy kiểm tra.
- Dùng `pathlib.Path` cho đường dẫn, không nối chuỗi thủ công.
- Dùng module `logging` thay cho `print()` trong mã ứng dụng (print chỉ chấp nhận trong script nhỏ hoặc output CLI có chủ đích).
- **Bắt exception cụ thể**, không dùng `except:` trần hay `except Exception: pass`, vì sẽ che mất lỗi thật.
- Không dùng giá trị mặc định có thể thay đổi (`def f(x=[])`); dùng `None` rồi khởi tạo trong hàm.
- Đọc cấu hình/secret từ biến môi trường (`os.environ`), không hardcode.
