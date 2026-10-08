# TypeScript / JavaScript

## Công cụ mặc định (chỉ khi dự án chưa có)
- Lint và format: `Biome` (hoặc ESLint + Prettier nếu dự án đã dùng)
- Kiểm tra kiểu: `tsc --noEmit`, chỉ với dự án đã có `tsconfig.json`
- Test: `Vitest`

## Lệnh kiểm tra trước khi báo hoàn thành
Nếu dự án đã có script, Makefile hoặc CI, chạy lệnh đó. Không chạy Biome trên dự án đang dùng ESLint, và không chạy Vitest trên dự án đang dùng Jest hay `node:test`.

Chạy binary hoặc script đã khai báo trong dự án. Không dùng `npx` để tải tạm gói không có trong lockfile.

Nếu chưa có toolchain và đang tạo dự án TypeScript mới, thêm công cụ vào devDependency, ghi lockfile, rồi chạy binary đã cài. Không dùng `npx` để tải gói. Chạy trong thư mục gốc của dự án mới. Thư mục cha đã có `package.json` mà đây là package riêng thì tạo `package.json` tại đây trước (`npm init -y`, hoặc lệnh init của pnpm/bun), để không ghi dependency vào package cha. Cả cây là phần vừa tạo. Tạo `tsconfig.json` ngắn: `"strict": true`, `target` và `module` khớp nơi chạy, include đúng thư mục code vừa tạo, rồi mới chạy khối dưới. Không dán file mẫu dài của `tsc --init`. Chưa có `tsconfig.json` thì không chạy `tsc` và không coi đó là lỗi code. Dự án JavaScript mới thì không cài TypeScript và không chạy `tsc`. Khối lệnh dưới chỉ cho dự án TypeScript mới. Không chạy nguyên khối cho dự án JavaScript.

```bash
npm install --save-dev typescript @biomejs/biome vitest
npm exec biome -- init
npm exec biome -- check --write .
npm exec tsc -- --noEmit
```

Dự án TypeScript mới đã chọn pnpm hoặc bun thì dùng đúng trình quản lý đó, vẫn chỉ ba gói trong lệnh trên. Không cài ba gói này vào dự án đã có. Dự án đã có công cụ nhưng không có script: chỉ chạy binary của dự án trên file vừa sửa. Dự án dùng npm thì `npm exec biome -- check --write -- path/to/file.ts`. pnpm, yarn hoặc bun thì dùng đúng lệnh exec của trình đó, không gọi `npm`. Không quét cả repo. Dự án đã có mà thiếu công cụ thì báo, không tự thêm.

Dự án JavaScript không có `tsconfig.json`: bỏ qua `tsc`. Không tự thêm TypeScript.

Chỉ chạy `npm exec vitest -- run` khi đã có file test. Chưa có thì báo chưa có test. Vitest thoát khác 0 khi không có file test; đó không phải fail. Không tạo test rỗng và không bật `passWithNoTests` chỉ để lệnh thoát 0.

Biome cảnh báo trên file vừa sửa thì sửa, dù lệnh thoát 0. Không thêm `--error-on-warnings`.

## Thực hành
- Bật `"strict": true` trong `tsconfig.json` khi đang tạo dự án TypeScript mới. Dự án đã có thì không tự bật strict nếu việc đó làm vỡ các file không liên quan.
- **Không dùng `any`**; dùng `unknown` rồi thu hẹp kiểu khi chưa rõ kiểu dữ liệu. `any` làm mất toàn bộ lợi ích của TypeScript.
- Xử lý bất đồng bộ bằng `async/await`; tránh chuỗi `.then()` lồng sâu. Luôn xử lý lỗi của Promise (`try/catch` hoặc `.catch`).
- `const` mặc định; chỉ dùng `let` khi giá trị thật sự cần thay đổi. Không dùng `var`.
- Validate dữ liệu từ bên ngoài (API, input, JSON) tại biên hệ thống thay vì tin kiểu khai báo.
- Không commit secret; đọc từ biến môi trường.
