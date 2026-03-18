# Bước 2: Phân tích & Điều tra UI (Analysis & UI Recon)

**Mục đích:**
Thay vì con người phải thủ công mở Inspect/DOM, bước này giao nhiệm vụ cho AI trực tiếp đóng vai trò như một tác nhân tự dùng công cụ có sẵn (như Playwright/Selenium MCP của Antigravity) để mở trình duyệt, điều hướng web và trích xuất ra đúng Locator thực sự thay vì viết mò. 

**Cách sử dụng:**
1. Tiến hành nhập URL/Domain môi trường chạy trong file `prompt.txt`.
2. Truyền kịch bản kiểm thử (Test Cases manual).
3. Gửi cho AI và yêu cầu AI tự động mở trình duyệt, phân tích hệ thống (gọi DOM hoặc Accessibility Tree) và gom nhặt toàn bộ locator.
