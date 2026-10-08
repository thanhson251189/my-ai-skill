---
name: universal-software-engineering
description: >-
  Quy chuẩn kỹ thuật phần mềm đa ngôn ngữ cho code tinh gọn, viết bằng tiếng Việt. Dùng khi người dùng viết, sửa, refactor, review, debug hoặc thiết lập dự án bằng Python, Rust, TypeScript/JavaScript, Go, C/C++, Shell/Bash, SQL, hoặc VBA/Apps Script/Office Scripts, kể cả khi họ không nhắc đến quy chuẩn hay best practice. Skill quy định công cụ lint/format/test, cách xử lý lỗi và tiêu chuẩn hoàn thành.
compatibility: Không cần runtime riêng. Lệnh trong file tham chiếu chỉ là mặc định khi dự án chưa có toolchain.
---

# Nguyên tắc chung (mọi ngôn ngữ)

- **Theo dự án trước.** Nếu dự án đã có công cụ, script, Makefile, CI hoặc cấu hình (pyproject, Cargo.toml, biome.json, .eslintrc...), dùng đúng thứ đó. Lệnh trong file tham chiếu chỉ dùng khi dự án chưa có cách kiểm tra tương đương. Không cài công cụ khác và không format lại cả repo theo mặc định của skill.
- **Đơn giản và dễ đọc, không phải ngắn nhất.** Chọn giải pháp ít thành phần nhất mà vẫn dễ hiểu. Không code golf. Không tạo abstraction khi chưa có ít nhất 3 chỗ dùng lại thực tế.
- **Không thêm dependency chạy cùng ứng dụng** nếu thư viện chuẩn làm được. Mỗi dependency mới là thêm bề mặt bảo trì và rủi ro bảo mật. Công cụ lint, format, test chỉ được thêm khi đang thiết lập dự án mới chưa có toolchain. Dự án đã có thì thiếu công cụ phải báo, không tự thêm.
- **Không nuốt lỗi âm thầm.** Mọi lỗi phải được log kèm ngữ cảnh hoặc trả về qua kiểu dữ liệu xử lý lỗi rõ ràng, vì lỗi bị nuốt sẽ biến thành bug khó truy vết về sau. Không log secret, token hay mật khẩu.
- **Không hardcode secret** (API key, mật khẩu, token). Đọc từ biến môi trường hoặc file cấu hình nằm ngoài git.
- **Hỏi lại trước thay đổi lớn:** refactor diện rộng, đổi cấu trúc thư mục, đổi thư viện hoặc framework chính, thêm toolchain vào dự án đã có.
- **Chỉ ra phần thay đổi, không dán cả file.** Sửa file bằng công cụ sửa của agent, không dán nguyên file rồi coi như đã sửa. Sửa nhỏ: nêu file và khối đã đổi, kèm vài dòng ngữ cảnh. Sửa lớn hoặc rải nhiều chỗ: liệt kê file nào đổi và hành vi đổi ra sao.

# Comment và giải thích (người dùng thiên về vibe coding)

Người dùng có thể không tự đọc từng dòng code, nên cần hiểu dự án ở mức tổng thể mà không làm code rối.

- **Không comment từng dòng** và không comment lặp lại điều code đã nói rõ (`i += 1  # tăng i`).
- **Comment chỉ để giải thích "vì sao"**: lý do, ràng buộc hoặc đánh đổi không hiển nhiên (ví dụ giới hạn của API, workaround cho bug, lý do chọn thuật toán).
- **Docstring ngắn cho hàm hoặc module công khai khi tên và chữ ký chưa nói hết** làm gì, nhận gì, trả về gì. Dùng đúng dạng của ngôn ngữ (docstring Python, `///` Rust, JSDoc/TSDoc, doc comment Go...). Không viết docstring chỉ lặp lại tên hàm.
- **Đặt tên rõ nghĩa** thay vì tên ngắn kèm comment giải thích.
- **Sau khi hoàn thành một tính năng hoặc thay đổi đáng kể**, tóm tắt ngắn bằng ngôn ngữ đơn giản: code làm gì, luồng chạy ra sao, file nào đã đổi và vì sao. Không bỏ qua tóm tắt này; đó là cách người dùng nắm thay đổi mà không cần docstring trên mọi hàm. Tránh thuật ngữ khi không cần; nếu dùng thì giải thích ngắn.
- Với dự án mới, tạo hoặc cập nhật `README.md` (hoặc `NOTES.md`) ngắn: mục đích, cấu trúc thư mục, cách chạy/test, các quyết định thiết kế chính.
- Nếu người dùng yêu cầu giải thích sâu hơn cho một đoạn cụ thể, giải thích ngoài code (trong câu trả lời), không nhét vào file.

# Cấu trúc dự án

Tổ chức theo tính năng khi tính năng đã đủ lớn: **một thư mục cho tính năng đó**, trong đó chia file theo trách nhiệm, **không tách mỗi hàm một file**. Tính năng còn nhỏ thì một file là đủ. Ngưỡng cảnh báo: file khoảng 300 dòng, hàm khoảng 50 dòng. Đó là tín hiệu để báo, không phải lệnh phải tách. Ngưỡng tách và ngoại lệ nằm trong `references/project-structure.md`. Khi vượt, báo và đề xuất cách tách, không tự ý refactor. Đọc file đó khi tạo dự án mới, thêm tính năng mới, hoặc khi cần tách/gộp file.

# Quy trình làm việc

Áp dụng cho mọi ngôn ngữ: khi sửa lỗi, tìm nguyên nhân gốc rồi mới vá (không đoán mò); giữ đúng phạm vi yêu cầu, thấy vấn đề khác thì báo chứ không tự sửa; hỏi xác nhận trước thao tác khó hoàn tác (xóa hàng loạt, `reset --hard`, `DROP TABLE`...); dùng lockfile và `.gitignore` đúng. Đọc `references/workflow.md` khi sửa lỗi, khởi tạo dự án, hoặc sắp làm thao tác nguy hiểm.

# "Hoàn thành" nghĩa là gì

Chỉ coi là xong khi đã **chạy thật** lệnh kiểm tra của dự án và báo lại kết quả. Dự án chưa có lệnh nào thì dùng lệnh mặc định trong file tham chiếu của ngôn ngữ đang sửa. Không được tuyên bố "xong" khi chưa chạy. Không chạy được thì nói rõ lý do và đưa lệnh để người dùng tự chạy.

1. Lệnh kiểm tra không còn lỗi mới do thay đổi này. Không tự bật `-Werror` hay đổi bộ lint. Nếu lệnh đã fail từ trước khi sửa, không tự sửa các lỗi cũ. Lấy mức nền bằng cách chạy trên bản gốc, hoặc chỉ kiểm tra file vừa sửa, rồi báo riêng các lỗi có sẵn.
2. Phần code vừa sửa đã được format bằng formatter của dự án, nếu dự án có.
3. Hành vi mới hoặc lỗi vừa sửa có test nếu dự án đã có chỗ đặt test. Không dựng framework test mới và không thêm test giả chỉ để lệnh thoát 0, trừ khi người dùng yêu cầu thiết lập test.

**Ngoại lệ:** với script dùng một lần hoặc prototype, chỉ cần format nếu đã có formatter; test là tùy chọn.

# Chọn file theo ngôn ngữ

Chỉ đọc file của ngôn ngữ đang làm việc. Mọi đường dẫn trong skill tính từ thư mục chứa `SKILL.md`, không tính từ thư mục làm việc. Lệnh kiểm tra trong file tham chiếu là phương án dự phòng, không phải lệnh bắt buộc khi dự án đã có cách kiểm tra riêng.

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
