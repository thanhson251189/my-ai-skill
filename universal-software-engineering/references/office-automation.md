# Office Automation (VBA / Google Apps Script / Office Scripts)

Nguyên tắc chung: **đọc và ghi dữ liệu theo lô (mảng trong bộ nhớ)**, không thao tác từng ô trong vòng lặp. Mỗi lần gọi qua lại với bảng tính rất chậm.

## VBA
- Luôn có `Option Explicit` ở đầu module.
- Không dùng `.Select` / `.Activate`; thao tác trực tiếp trên đối tượng Range/Worksheet.
- Đọc cả vùng vào mảng (`arr = rng.Value`), xử lý trong bộ nhớ, rồi ghi lại một lần.
- Tắt cập nhật màn hình khi chạy: `Application.ScreenUpdating = False`, và **luôn bật lại** (kể cả khi có lỗi, dùng `On Error GoTo` để dọn dẹp).

## Google Apps Script
- Dùng `getValues()` / `setValues()` theo vùng, không `getValue()` từng ô.
- Dùng `LockService` để tránh xung đột khi nhiều lần chạy cùng lúc ghi vào một sheet.
- Gọi `SpreadsheetApp.flush()` khi cần đảm bảo thứ tự ghi.
- Lưu ý giới hạn thời gian chạy của Apps Script; chia lô nếu dữ liệu lớn.

## Office Scripts (Excel trên web, viết bằng TypeScript)
- Áp dụng quy tắc TypeScript (`references/typescript.md`).
- Đọc/ghi theo vùng với `getValues()` / `setValues()`, hạn chế gọi API cho từng ô.
