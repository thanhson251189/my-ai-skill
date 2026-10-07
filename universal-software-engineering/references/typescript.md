# TypeScript / JavaScript

## Công cụ mặc định (khi dự án chưa có)
- Lint & format: `Biome` (hoặc ESLint + Prettier nếu dự án đã dùng)
- Kiểm tra kiểu: `tsc --noEmit`
- Test: `Vitest`

## Lệnh kiểm tra trước khi báo hoàn thành
```bash
npx biome check .
npx tsc --noEmit
npx vitest run
```

## Thực hành
- Bật `"strict": true` trong `tsconfig.json`.
- **Không dùng `any`**; dùng `unknown` rồi thu hẹp kiểu khi chưa rõ kiểu dữ liệu. `any` làm mất toàn bộ lợi ích của TypeScript.
- Xử lý bất đồng bộ bằng `async/await`; tránh chuỗi `.then()` lồng sâu. Luôn xử lý lỗi của Promise (`try/catch` hoặc `.catch`).
- `const` mặc định; chỉ dùng `let` khi giá trị thật sự cần thay đổi. Không dùng `var`.
- Validate dữ liệu từ bên ngoài (API, input, JSON) tại biên hệ thống thay vì tin kiểu khai báo.
- Không commit secret; đọc từ biến môi trường.
