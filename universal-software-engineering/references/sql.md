# SQL / Database

## Lệnh kiểm tra trước khi báo hoàn thành
Không có một linter bắt buộc cho mọi dự án. Nếu dự án có sqlfluff, migration runner hoặc test database, chạy đúng cái đó.

Nếu không có: đọc lại câu lệnh vừa sửa, kiểm tra tham số hóa và transaction. Chỉ chạy `EXPLAIN` khi đang sửa vì chậm và có database để chạy. Không tự thêm framework migration.

## Thực hành
- Theo style SQL của dự án. Chưa có style thì viết hoa từ khóa (`SELECT`, `INSERT`, `JOIN`, `WHERE`).
- **Không dùng `SELECT *` trong mã ứng dụng**; liệt kê rõ tên cột để code không vỡ khi schema thay đổi.
- **Luôn dùng parameterized queries / prepared statements**, không bao giờ nối chuỗi từ input của người dùng vào câu SQL (chống SQL injection).
- Thay đổi schema phải qua migration có thể rollback, không sửa tay trên môi trường thật. Dự án chưa có công cụ migration thì báo, không tự chọn một cái.
- Thêm index theo truy vấn thực tế; dùng `EXPLAIN` để kiểm tra khi nghi ngờ chậm. Chú ý lỗi N+1 khi truy vấn trong vòng lặp.
- Bọc các thao tác ghi liên quan nhau trong transaction.
