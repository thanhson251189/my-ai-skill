# Go

## Lệnh kiểm tra trước khi báo hoàn thành
Chạy lệnh của dự án nếu đã có. Nếu chưa có, chỉ kiểm tra file và package vừa sửa:

```bash
gofmt -w path/to/changed.go
gofmt -l path/to/changed.go
go vet ./path/to/package
go test ./path/to/package
```

`gofmt -l` luôn thoát mã 0 dù file chưa format, nên không dùng riêng lệnh đó để kết luận đạt. Fail nếu lệnh in ra đường dẫn. Không chạy `gofmt -w .` trên repo đã có code. Dự án mới, cả cây là phần vừa tạo, thì dùng `gofmt -w .`, `gofmt -l .` và `go test ./...`. Chưa có `go.mod` thì `go mod init <module>` trước, chỉ khi đó là dự án mới. Chưa có module path thì hỏi một câu, không bịa. Không chạy `go mod init` trong repo đã có code chỉ để `go vet` chạy được. `go test` thoát 0 khi package không có file test; báo chưa có test, không coi là đã kiểm thử.

`golangci-lint run` chỉ khi dự án đã cấu hình hoặc công cụ đã có sẵn. Không tự cài. Với code có goroutine, chạy thêm `go test -race` trên package vừa sửa nếu toolchain chạy được race detector. Không chạy được vì thiếu C compiler hoặc race detector thì báo và bỏ qua; đó không phải fail của code vừa sửa.

## Thực hành
- **Kiểm tra lỗi tường minh ngay sau khi gọi hàm** và bọc thêm ngữ cảnh:
  ```go
  if err != nil {
      return fmt.Errorf("read config: %w", err)
  }
  ```
- Ưu tiên **table-driven tests** cho kiểm thử đơn vị khi dự án đã có test.
- Quản lý tài nguyên bằng `defer` ngay sau khi khởi tạo thành công (đóng file, unlock mutex, đóng response body).
- Giữ interface nhỏ và định nghĩa ở nơi sử dụng, không ở nơi cài đặt.
