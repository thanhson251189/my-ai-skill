# Office Automation (VBA / Google Apps Script / Office Scripts)

Nguyên tắc chung: **đọc và ghi dữ liệu theo lô (mảng trong bộ nhớ)**, không thao tác từng ô trong vòng lặp. Mỗi lần gọi qua lại với bảng tính rất chậm.

## Kiểm tra trước khi báo hoàn thành

Nhóm này không có một lệnh terminal bắt buộc. Chạy được ứng dụng thì phải làm bước kiểm tra tương ứng trước khi báo xong. Không mở được thì đưa đúng bước dưới đây để người dùng tự chạy. Đó là xong phần kiểm tra, không phải fail của code.

- **VBA:** trong trình soạn VBA, chạy Debug > Compile VBAProject. Hết lỗi biên dịch mới đạt. Sau đó chạy macro trên bản sao của workbook, không chạy trên file gốc.
- **Google Apps Script:** lưu trong trình soạn để hiện lỗi cú pháp, rồi chạy hàm trên bản sao của spreadsheet. Nếu dự án đã có `clasp`, chạy lệnh kiểm tra sẵn có của dự án; không tự cài `clasp`.
- **Office Scripts:** chạy script trên bản sao của workbook trong Excel trên web. Không áp dụng Debug > Compile của VBA cho Apps Script hay Office Scripts.

## VBA
- Luôn có `Option Explicit` ở đầu module.
- Không dùng `.Select` / `.Activate`; thao tác trực tiếp trên đối tượng Range/Worksheet.
- Đọc cả vùng vào mảng (`arr = rng.Value`), xử lý trong bộ nhớ, rồi ghi lại một lần. Một ô thì `rng.Value` là giá trị đơn, không phải mảng. Chỉ gán thẳng vào mảng khi vùng có từ hai ô; một ô thì bọc thành mảng trước khi xử lý chung.
- Tắt cập nhật màn hình khi chạy: lưu giá trị cũ của `Application.ScreenUpdating`, gán `False`, rồi khôi phục đúng giá trị cũ kể cả khi có lỗi (`On Error GoTo`). Không gán cứng `True` lúc kết thúc, vì caller có thể đang tắt màn hình.

## Google Apps Script
- Dùng `getValues()` / `setValues()` theo vùng, không `getValue()` từng ô.
- Dùng khóa của `LockService` khi nhiều lần chạy có thể ghi cùng một sheet. Lấy khóa, chờ, và luôn nhả trong `finally`:
  ```javascript
  const lock = LockService.getScriptLock();
  lock.waitLock(30000);
  try {
    // ...
  } finally {
    lock.releaseLock();
  }
  ```
- Gọi `SpreadsheetApp.flush()` khi cần đảm bảo thứ tự ghi.
- Lưu ý giới hạn thời gian chạy của Apps Script; chia lô nếu dữ liệu lớn.

## Office Scripts (Excel trên web, viết bằng TypeScript)
- Không dùng `any`. Dữ liệu từ sheet hoặc người dùng phải được kiểm tra trước khi dùng. Không cài Biome hay Vitest, và không chạy `tsc`, trừ khi dự án đã có đúng lệnh đó. Không mở `typescript.md` để lấy toolchain Node.
- Đọc/ghi theo vùng với `getValues()` / `setValues()`, hạn chế gọi API cho từng ô.
