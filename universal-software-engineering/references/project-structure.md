# Cấu trúc dự án

Mục tiêu: dễ quản lý, lỗi dễ khoanh vùng, AI chỉ cần đọc phần liên quan. Nguyên tắc: khi tính năng đã đủ lớn thì một thư mục cho tính năng đó; trong thư mục chia file theo trách nhiệm, không theo từng hàm.

Nếu dự án đã có cấu trúc riêng, tuân theo nó và chỉ áp dụng file này cho phần code mới khi hợp lý.

## 1. Tổ chức theo tính năng

- Mỗi tính năng một thư mục khi nó đã đủ lớn để tách. Tính năng còn nhỏ thì một file là đủ (mục 6). Thư mục đó chứa logic, kiểu dữ liệu, truy cập dữ liệu và test của chính nó.
- Sửa hoặc xóa một tính năng chỉ nên đụng vào thư mục của nó.
- Đặt tên file theo vai trò (`service`, `models`, `repository`, `handlers`...), không đặt theo tên từng hàm.

## 2. Không tách "mỗi hàm một file"

Các hàm cùng phục vụ một việc thì nằm chung một file. Tách từng hàm làm số file bùng nổ, buộc phải nhảy qua lại để hiểu một luồng, và tốn thêm khâu khai báo/import (đặc biệt Rust, nơi mỗi file mới cần khai báo `mod`). Chỉ tách một hàm ra file riêng khi nó lớn, phức tạp, hoặc được nhiều tính năng dùng chung.

## 3. Ngưỡng cảnh báo

| Đối tượng | Cảnh báo | Nên tách chắc chắn |
|---|---|---|
| File | khoảng 300 dòng | khoảng 500 dòng |
| Hàm | khoảng 50 dòng | khi hàm làm hai việc, hoặc khoảng 100 dòng mà không đặt được một tên mô tả trọn việc của nó |

Đây là tín hiệu để xem xét, không phải luật cứng. Dấu hiệu nên tách quan trọng hơn con số:
- File làm hai việc trở lên không liên quan nhau (mô tả file mà phải dùng chữ "và").
- Một phần của file được dùng ở tính năng khác.
- Có nhóm hàm thay đổi vì lý do khác nhau (ví dụ logic nghiệp vụ lẫn với truy cập DB).
- Phải cuộn rất nhiều để tìm một hàm.

File 350 dòng mà gắn kết chặt, chỉ một trách nhiệm thì không bắt buộc tách.

**Không áp dụng ngưỡng cho:** file test, code sinh tự động, file cấu hình/dữ liệu tĩnh, và entry point (`main`) chỉ ráp các phần lại.

**Khi vượt ngưỡng:** báo cho người dùng và đề xuất cách tách (tách thành những file nào, vì sao). Không tự ý refactor, vì đây là thay đổi cấu trúc cần hỏi lại trước.

## 4. Quy tắc giữa các tính năng

- Tính năng **không import chéo** vào ruột của tính năng khác. Cần dùng chung thì đưa phần chung vào `shared/` (hoặc `common/`), và chỉ làm vậy khi có từ 3 chỗ dùng thực tế.
- Nếu tính năng A phải gọi tính năng B, gọi qua một giao diện công khai nhỏ của B (một vài hàm xuất ra rõ ràng), không với tay vào file nội bộ.
- Nếu dự án đã có chỗ đặt test, test của tính năng nằm trong hoặc cạnh thư mục của nó. Không tạo khung test mới chỉ vì tách thư mục hay vì muốn có file test.
- Tránh phụ thuộc vòng (A gọi B, B gọi lại A); nếu xảy ra, đó là dấu hiệu ranh giới tính năng chưa đúng.

## 5. Ví dụ theo ngôn ngữ

Các cây thư mục dưới đây là hình dạng khi một tính năng đã có nhiều trách nhiệm khác nhau. Không dùng chúng làm khung dựng sẵn cho dự án mới hoặc tính năng còn nhỏ. File test trong ví dụ không có nghĩa là phải tạo khung test mới. Mục 6 mới là cách bắt đầu.

### Python
```
src/
├── features/
│   ├── auth/
│   │   ├── __init__.py      # chỉ xuất giao diện công khai
│   │   ├── service.py       # logic nghiệp vụ
│   │   ├── models.py        # kiểu dữ liệu
│   │   ├── repository.py    # truy cập DB
│   │   └── test_auth.py
│   └── billing/
│       └── ...
├── shared/                  # code dùng chung (từ 3 nơi dùng trở lên)
└── main.py
```

### Rust
```
src/
├── main.rs                  # chỉ ráp các phần lại
├── features/
│   ├── mod.rs
│   ├── auth/
│   │   ├── mod.rs           # khai báo module + pub use giao diện công khai
│   │   ├── service.rs
│   │   ├── models.rs
│   │   └── repository.rs
│   └── billing/
│       └── ...
└── shared/
    └── mod.rs
```
Mỗi file mới phải được khai báo ở module cha mà dự án đang dùng (`mod.rs` hoặc `foo.rs`). Không tự tạo `mod.rs` nếu dự án không dùng kiểu đó. Dự án rất lớn thì cân nhắc tách thành nhiều crate trong một workspace thay vì thêm hàng trăm module.

### TypeScript / JavaScript
```
src/
├── features/
│   ├── auth/
│   │   ├── index.ts         # giao diện công khai
│   │   ├── service.ts
│   │   ├── types.ts
│   │   └── auth.test.ts
│   └── billing/
└── shared/
```

### Go
Tổ chức theo **package**: mỗi tính năng một thư mục/package, bên trong có nhiều file cùng package. Không chia package quá nhỏ.
```
internal/
├── auth/
│   ├── service.go
│   ├── repository.go
│   └── service_test.go
└── billing/
```

## 6. Khi tạo dự án mới

Bắt đầu đơn giản: nếu mới có một hai tính năng nhỏ, một thư mục phẳng là đủ. Chuyển sang cấu trúc theo tính năng khi dự án bắt đầu có nhiều phần tách biệt rõ ràng. Không dựng sẵn khung thư mục rỗng cho những thứ chưa tồn tại.
