---
name: requirements_analyzer
description: Kỹ năng phân tích trang web/module và sinh ra tài liệu Yêu cầu (Requirements Document/User Stories) chuẩn mực.
---

# Kỹ năng Phân tích Yêu cầu (Requirements Analyzer)

Kỹ năng này cung cấp các hướng dẫn chi tiết để AI (Antigravity) có thể chuyển đổi giao diện UI hoặc cấu trúc DOM/HTML của một trang web thành các tài liệu Yêu cầu rõ ràng, chi tiết, phục vụ trực tiếp cho QA, Tester và Developer.

## 1. Mục tiêu cốt lõi
- Xây dựng tài liệu yêu cầu bám sát thực tế hệ thống đang chạy.
- Đảm bảo tính nhất quán, tính bao quát cho cả Happy Path và Edge Cases (Trường hợp ngoại lệ/báo lỗi).
- Định dạng xuất ra một cách chuyên nghiệp (Sử dụng cấu trúc Artifact).

## 2. Quy trình trích xuất thông tin
Khi được yêu cầu tạo Requirements từ một trang web:
1. **Phân tích Khung giao diện (Layout Analysis):** Xác định các phần Header, Footer, Sidebar, và Nội dung chính (Main Content).
2. **Thu thập Form & Inputs:**
   - Tìm tất cả các trường nhập liệu (`input`, `select`, `textarea`).
   - Ghi nhận thuộc tính `type` (text, email, password, number), `required`, `maxlength`, `minlength`, `pattern`.
3. **Thu thập Các nút tương tác (Buttons/Links/Actions):**
   - Xác định chức năng của từng nút (Save, Submit, Cancel, Delete, Edit).
   - Các cảnh báo, thông báo (Alerts, Toasts, Validation Messages) xuất hiện khi tương tác lỗi.
4. **Trích xuất Luồng công việc (Workflows):**
   - Sự phụ thuộc giữa các thành phần (VD: Nút Submit chỉ enable khi đã tích chọn Checkbox "Tôi đồng ý").

## 3. Cấu trúc Tài liệu Yêu cầu Đầu ra (Output Format)
Tài liệu cần được format theo Markdown chuyên nghiệp hoặc lưu dưới dạng Artifact (`requirements_spec.md`).

**Nội dung bắt buộc phải có:**

### 3.1. Tổng quan (Overview)
Mô tả tóm tắt tính năng và mục đích của trang web/module.

### 3.2. Yêu cầu Chức năng (Functional Requirements)
Chia thành các **User Stories** hoặc **Use Cases**:
- **Tên tính năng** (Ví dụ: Chức năng Đăng nhập)
- **Mô tả:** "Là một người dùng, tôi muốn... để có thể..."
- **Tiêu chí chấp nhận (Acceptance Criteria):** Ghi rõ các điều kiện cần thỏa mãn.

### 3.3. Đặc tả Trường Dữ liệu (Field Specifications)
Đây là phần cốt lõi dành cho Automation Tester:
* Dùng bảng Markdown (*Markdown Table*) để liệt kê:
  - Tên Trường (Label)
  - Loại (Type UI)
  - Validation Rules (Bắt buộc / Mặc định / Giới hạn độ dài).
  - Ghi chú (Notes).

### 3.4. Các luồng xử lý và Báo lỗi (Business Rules & Validations)
Liệt kê chi tiết các Validation Message mong đợi khi người dùng nhập sai dữ liệu.

## 4. Bắt buộc (Strict Rules)
- Luôn viết bằng **Tiếng Việt**.
- Không tự suy diễn các yêu cầu nghiệp vụ phức tạp nếu không có căn cứ từ UI. Nếu thiếu logic, hãy liệt kê chúng vào mục "Câu hỏi/Làm rõ với PO-User".
- Nếu có Playwright MCP, ưu tiên mở browser thật để screenshot/capture giao diện nếu cần.
