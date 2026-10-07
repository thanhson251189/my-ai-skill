---
name: universal-software-engineering
description: Quy chuẩn kỹ thuật phần mềm đa ngôn ngữ cho code tinh gọn, clean code. Hãy dùng skill này bất cứ khi nào người dùng viết, sửa, refactor, review, debug hoặc thiết lập dự án bằng Python, Rust, TypeScript/JavaScript, Go, C/C++, Shell/Bash, SQL, hoặc VBA/Apps Script/Office Scripts, kể cả khi họ không nhắc đến "quy chuẩn" hay "best practice". Skill quy định công cụ lint/format/test, cách xử lý lỗi và tiêu chuẩn "hoàn thành".
---

# Nguyên tắc chung (mọi ngôn ngữ)

- **Theo dự án trước.** Nếu dự án đã có công cụ hoặc cấu hình sẵn (pyproject, Cargo.toml, biome.json, .eslintrc, Makefile...), dùng đúng thứ đó. Chỉ áp dụng mặc định trong các file tham chiếu khi dự án chưa có gì.
- **Đơn giản và dễ đọc, không phải ngắn nhất.** Chọn giải pháp ít thành phần nhất mà vẫn dễ hiểu. Không code golf. Không tạo abstraction khi chưa có ít nhất 3 chỗ dùng lại thực tế.
- **Không thêm dependency** nếu thư viện chuẩn làm được. Mỗi dependency mới là thêm bề mặt bảo trì và rủi ro bảo mật.
- **Không nuốt lỗi âm thầm.** Mọi lỗi phải được log kèm ngữ cảnh hoặc trả về qua kiểu dữ liệu xử lý lỗi rõ ràng, vì lỗi bị nuốt sẽ biến thành bug khó truy vết về sau.
- **Không hardcode secret** (API key, mật khẩu, token). Đọc từ biến môi trường hoặc file cấu hình nằm ngoài git.
- **Hỏi lại trước thay đổi lớn:** refactor diện rộng, đổi cấu trúc thư mục, đổi thư viện/framework chính.
- **Khi sửa nhỏ, chỉ hiển thị phần thay đổi** (hàm hoặc khối lệnh) kèm vài dòng ngữ cảnh xung quanh để biết vị trí. Nếu thay đổi lớn hoặc rải nhiều chỗ, in lại cả file cho dễ theo dõi.

# Comment và giải thích (người dùng thiên về vibe coding)

Người dùng có thể không tự đọc từng dòng code, nên cần hiểu dự án ở mức tổng thể mà không làm code rối.

- **Không comment từng dòng** và không comment lặp lại điều code đã nói rõ (`i += 1  # tăng i`).
- **Comment chỉ để giải thích "vì sao"**: lý do, ràng buộc hoặc đánh đổi không hiển nhiên (ví dụ giới hạn của API, workaround cho bug, lý do chọn thuật toán).
- **Mỗi hàm/module công khai có docstring ngắn** (một hai câu): làm gì, nhận gì, trả về gì. Dùng đúng dạng của ngôn ngữ (docstring Python, `///` Rust, JSDoc/TSDoc, doc comment Go...).
- **Đặt tên rõ nghĩa** thay vì tên ngắn kèm comment giải thích.
- **Sau khi hoàn thành một tính năng hoặc thay đổi đáng kể**, tóm tắt ngắn bằng ngôn ngữ đơn giản: code làm gì, luồng chạy ra sao, file nào đã đổi và vì sao. Tránh thuật ngữ khi không cần; nếu dùng thì giải thích ngắn.
- Với dự án mới, tạo hoặc cập nhật `README.md` (hoặc `NOTES.md`) ngắn: mục đích, cấu trúc thư mục, cách chạy/test, các quyết định thiết kế chính.
- Nếu người dùng yêu cầu giải thích sâu hơn cho một đoạn cụ thể, giải thích ngoài code (trong câu trả lời), không nhét vào file.

# Cấu trúc dự án

Tổ chức theo tính năng: **mỗi tính năng một thư mục**, trong đó chia file theo trách nhiệm, **không tách mỗi hàm một file**. Ngưỡng cảnh báo: file khoảng 300 dòng, hàm khoảng 50 dòng; khi vượt thì báo và đề xuất cách tách, không tự ý refactor. Đọc `references/project-structure.md` khi tạo dự án mới, thêm tính năng mới, hoặc khi cần tách/gộp file.

# Quy trình làm việc

Áp dụng cho mọi ngôn ngữ: khi sửa lỗi, tìm nguyên nhân gốc rồi mới vá (không đoán mò); giữ đúng phạm vi yêu cầu, thấy vấn đề khác thì báo chứ không tự sửa; hỏi xác nhận trước thao tác khó hoàn tác (xóa hàng loạt, `reset --hard`, `DROP TABLE`...); dùng lockfile và `.gitignore` đúng. Đọc `references/workflow.md` khi sửa lỗi, khởi tạo dự án, hoặc sắp làm thao tác nguy hiểm.

# "Hoàn thành" nghĩa là gì

Chỉ coi là xong khi đã **chạy thật** các lệnh kiểm tra của ngôn ngữ (xem file tham chiếu) và báo lại kết quả thực tế. Không được tuyên bố "xong" khi chưa chạy.

1. Linter/compiler không còn cảnh báo.
2. Code đã được format đúng chuẩn.
3. Có test cho luồng chính và các trường hợp biên, và test pass.

**Ngoại lệ:** với script dùng một lần hoặc prototype, chỉ cần format + lint; test là tùy chọn. Nếu không chạy được lệnh (thiếu công cụ, không có môi trường), nói rõ điều đó và liệt kê lệnh để người dùng tự chạy.

# Chọn file theo ngôn ngữ

Chỉ đọc file của ngôn ngữ đang làm việc:

| Ngôn ngữ | File |
|---|---|
| Cấu trúc thư mục, tách/gộp file | `references/project-structure.md` |
| Sửa lỗi, git, thao tác nguy hiểm, lockfile | `references/workflow.md` |
| Python | `references/python.md` |
| Rust | `references/rust.md` |
| TypeScript / JavaScript | `references/typescript.md` |
| Go | `references/go.md` |
| C / C++ | `references/cpp.md` |
| Shell / Bash | `references/bash.md` |
| SQL / Database | `references/sql.md` |
| VBA / Apps Script / Office Scripts | `references/office-automation.md` |

Nếu dự án dùng nhiều ngôn ngữ, đọc file của từng ngôn ngữ liên quan.
