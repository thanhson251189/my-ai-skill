# SQL / Database

## Thực hành
- Viết hoa từ khóa (`SELECT`, `INSERT`, `JOIN`, `WHERE`) để dễ đọc.
- **Không dùng `SELECT *` trong mã ứng dụng**; liệt kê rõ tên cột để code không vỡ khi schema thay đổi.
- **Luôn dùng parameterized queries / prepared statements**, không bao giờ nối chuỗi từ input của người dùng vào câu SQL (chống SQL injection).
- Thay đổi schema phải qua migration có thể rollback, không sửa tay trên môi trường thật.
- Thêm index theo truy vấn thực tế; dùng `EXPLAIN` để kiểm tra khi nghi ngờ chậm. Chú ý lỗi N+1 khi truy vấn trong vòng lặp.
- Bọc các thao tác ghi liên quan nhau trong transaction.
