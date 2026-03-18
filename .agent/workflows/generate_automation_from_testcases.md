---
description: Convert manual test cases into automation scripts autonomously using the 6-step AI-RBT Framework via Antigravity Capabilities.
---

> **BẮT BUỘC:** Bạn (Agent Antigravity) PHẢI đóng vai là một Senior Automation Engineer và tuân thủ chặt chẽ nguyên tắc Clean Code và Page Object Model (POM). Tuyệt đối không được "đoán" locator khi chưa quét giao diện.

Khởi chạy luồng nạp và sinh kịch bản tự động hóa từ file Manual Test Cases do USER cung cấp. Với tư cách là Antigravity, bạn cấu hình các tools nội tại (như MCP Servers, File System Tools, Terminal) để **tự lực xử lý từ A-Z**.
⚠️ BẠN PHẢI TUÂN THỦ Rule E3: Cấm tuyệt đối hỏi USER khi test Failed, phải tự chạy đi chạy lại sửa vòng lặp (Auto-heal) tới khi PASS. Ghi chú quá trình chạy vào artifact `task.md`.

**1. Khởi tạo, Phân tích & Lên Kế Hoạch (Context & Analysis):**
- Đọc file Test Cases do USER cung cấp (bằng `view_file` hoặc `read_url_content`).
- Nhận diện các màn hình (Pages) sẽ đi qua.
- **Tạo artifact `task.md`** để USER theo dõi checklist tiến độ 6 bước công việc bạn chuẩn bị làm. Mở `task_boundary` để chạy luồng.

**2. Khảo sát UI tự động bằng MCP (Autonomous UI Recon via MCP):**
- Sử dụng công cụ MCP Server (Playwright/Selenium MCP) đã kết nối với bạn để **tự động mở trình duyệt**.
- Dùng các tính năng như `navigate`, `interact` để tự chạy các bước trong Test Case.
- Gọi tính năng `get_accessibility_tree` hoặc phân tích DOM trực tiếp để **thu thập chính xác 100%** các locator thực tế.
- TUYỆT ĐỐI KHÔNG SUY ĐOÁN selector. Nếu URL bị chặn hoặc thay đổi cấu trúc, dùng `notify_user` để hỏi xác nhận.

**3. Thiết kế POM (POM Architecture Design):**
- Dùng công cụ `write_to_file` để sinh ra các file Class Page Object Model (`XxxPage.java` hoặc `XxxPage.ts`) trực tiếp vào project structure của USER.
- Gắn các Locator đã thu thập thực tế từ Bước 2 vào các biến. Khai báo các Methods tương tác rõ nghĩa theo Context.

**4. Chuẩn bị Dữ liệu (Test Data Strategy):**
- Viết file Utils định nghĩa các hàm sinh dữ liệu ngẫu nhiên (sử dụng thư viện Faker hoặc hàm Random tương ứng với framework).
- Mọi dữ liệu (đặc biệt là ID, Name, Email) truyền vào kịch bản bắt buộc phải unique và Traceable (VD: `Auto_1234@email.com`).

**5. Sinh Script (Automation Scripting):**
- Viết file Script Test chính (Test class). Import các class POM và Utils ở trên.
- Đảm bảo logic Test rõ ràng 3 phần: Setup (Arrange), Execution (Act), và **Verification (Assert)**.
- Bắt buộc phải có code lệnh Assert để check DOM hiển thị trạng thái Thành công / Thất bại.

**6. Chạy thử nghiệm & Tự sửa lỗi (Autonomous Execution & Auto-Heal - RULE E3):**
- Dùng công cụ `run_command` để kích hoạt script test ngay trên terminal (ví dụ: `mvn test` hoặc `npx playwright test`).
- Theo dõi `command_status`.
   - Nếu test **Passed**: Dọn dẹp lại log console và cập nhật `task.md` chốt (Done).
   - Nếu test **Failed**: Đọc log exception. Tìm hiểu vì sao xịt. Dùng MCP soi lại DOM. Tự `multi_replace_file_content` sửa code và **chạy lại. BẮT BUỘC KHÔNG HỎI USER TRONG QUÁ TRÌNH FIX LỖI (Quy tắc E3)**. Chỉ hỏi xác nhận Business Logic/Mâu thuẫn requirements khi chạy hết mọi cách vẫn bí.