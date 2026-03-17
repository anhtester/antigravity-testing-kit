---
description: Generate Requirements content from a provided website module
skills:
  - requirements_analyzer
---

# Workflow: Generate Requirements from Website Module

> **BẮT BUỘC (MANDATORY SKILL):** Bạn PHẢI nạp và đọc kỹ nội dung của skill **`requirements_analyzer`** (tại `.agent/skills/requirements_analyzer/SKILL.md`) để biết định dạng chuẩn của tài liệu Requirements trước khi bắt đầu thực hiện tác vụ này.

Workflow này giúp bạn phân tích một module hoặc trang web được cung cấp và sinh ra tài liệu Yêu cầu (Requirements) chi tiết, chuẩn xác, phục vụ cho quá trình kiểm thử hoặc phát triển. 

## Các bước thực hiện:

1. **Tiếp nhận thông tin (Information Gathering):**
   - Đọc kỹ hướng dẫn từ kỹ năng (skill) **`requirements_analyzer`** để nắm bắt chuẩn đầu ra.
   - Lấy thông tin URL của trang web, tên module, hoặc mô tả/hình ảnh mà người dùng cung cấp.
   - Nếu cần thiết, hỏi người dùng về thông tin đăng nhập hoặc các trạng thái đặc biệt cần lưu ý.

2. **Khảo sát hệ thống (Recon & Investigation):**
   - Sử dụng các công cụ duyệt web (Browser tools/MCP) hoặc `read_url_content` để truy cập vào module trang web được yêu cầu.
   - Inspect kỹ lưỡi cấu trúc HTML, DOM, các form nhập liệu, các nút tương tác (buttons, links), và các thông báo lỗi (validation messages).
   - *Lưu ý: Không tự đoán các trường thông tin nếu không nhìn thấy trên giao diện thực tế.*

3. **Phân tích chức năng và tương tác (Analyze UI & Interactions):**
   - Phân tích luồng thao tác (User Flows).
   - Ghi nhận các trường dữ liệu tĩnh và động (Ví dụ: TextBox, Dropdown, Checkbox).
   - Ghi nhận các quy tắc nghiệp vụ hiển thị ở giao diện (Business Rules) như: trường bắt buộc (mandatory fields), định dạng hợp lệ (email, số điện thoại), hoặc giới hạn ký tự.

4. **Biên soạn tài liệu Yêu cầu (Draft Requirements):**
   - Dựa trên dữ liệu thu thập được, tạo một tài liệu mô tả bao gồm:
     * **Tổng quan (Overview):** Mục đích của module/trang.
     * **Yêu cầu chức năng (Functional Requirements):** Danh sách các tính năng người dùng có thể thực hiện (vd: Đăng nhập, Thêm mới, Xóa...).
     * **Quy tắc trường dữ liệu (Field Specifications):** Bảng chi tiết về từng thành phần UI (Tên trường, Loại, Bắt buộc/Không, Ràng buộc dữ liệu).
     * **Luồng xử lý (Business/User Flows):** Các bước để hoàn thành một chức năng chính.
     * **Yêu cầu phi chức năng (Non-functional Requirements - Nếu có thể quan sát):** Tính tương thích, hiệu năng tĩnh.

5. **Trình bày và Cung cấp (Review & Delivery):**
   - Định dạng tài liệu bằng Markdown rõ ràng.
   - Trình bày toàn bộ nội dung bằng **Tiếng Việt** ngắn gọn, chuyên nghiệp và dễ hiểu.
   - Sử dụng tính năng Artifact nếu tài liệu dài để người dùng tiện lưu trữ hoặc xuất file.
