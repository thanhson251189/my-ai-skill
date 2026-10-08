# Go

## Lệnh kiểm tra trước khi báo hoàn thành
Chạy lệnh của dự án nếu đã có. Nếu chưa có:

```bash
test -z "$(gofmt -l .)"
go vet ./...
go test ./...
```

`gofmt -l` luôn thoát mã 0 dù còn file chưa format, nên không dùng riêng lệnh đó để kết luận đạt. `test -z` fail khi còn đường dẫn được in ra. Shell không có `test` thì coi là fail nếu `gofmt -l .` in bất kỳ đường dẫn nào.

`golangci-lint run` chỉ khi dự án đã cấu hình hoặc công cụ đã có sẵn. Không tự cài. Với code có goroutine, chạy thêm `go test -race ./...` nếu toolchain chạy được race detector.

## Thực hành
- **Kiểm tra lỗi tường minh ngay sau khi gọi hàm** và bọc thêm ngữ cảnh:
  ```go
  if err != nil {
      return fmt.Errorf("mô tả thao tác: %w", err)
  }
  ```
- Ưu tiên **table-driven tests** cho kiểm thử đơn vị khi dự án đã có test.
- Quản lý tài nguyên bằng `defer` ngay sau khi khởi tạo thành công (đóng file, unlock mutex, đóng response body).
- Giữ interface nhỏ và định nghĩa ở nơi sử dụng, không ở nơi cài đặt.
