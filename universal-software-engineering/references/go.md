# Go

## Lệnh kiểm tra trước khi báo hoàn thành
```bash
gofmt -l .          # không in ra gì là đạt
go vet ./...
golangci-lint run
go test ./...
```

## Thực hành
- **Kiểm tra lỗi tường minh ngay sau khi gọi hàm** và bọc thêm ngữ cảnh:
  ```go
  if err != nil {
      return fmt.Errorf("mô tả thao tác: %w", err)
  }
  ```
- Ưu tiên **table-driven tests** cho kiểm thử đơn vị.
- Quản lý tài nguyên bằng `defer` ngay sau khi khởi tạo thành công (đóng file, unlock mutex, đóng response body).
- Giữ interface nhỏ và định nghĩa ở nơi sử dụng, không ở nơi cài đặt.
- Với code có goroutine, chạy thêm `go test -race ./...`.
