# AI-DRIVEN AUTOMATION TESTING FRAMEWORK

**Mục tiêu:** 
Sử dụng AI để tự động hóa quá trình xây dựng framework kiểm thử (Automation Framework), thiết kế Page Object Model (POM) và sinh ra mã nguồn (Clean Code) chất lượng cao, dễ bảo trì và tích hợp CI/CD.

## 📌 Nguyên Tắc Cốt Lõi (Dành cho Automation)

1. **AI DOM Recon First:** Luôn yêu cầu AI sử dụng các công cụ nội tại (ví dụ: Playwright/Selenium MCP) để tự khởi chạy trình duyệt hoặc điều tra tính tương tác của UI/DOM thật trên hệ thống. TUYỆT ĐỐI không cho phép AI tự động đoán bừa locator.
2. **POM Architecture:** Mọi script phải tuân thủ mô hình Page Object Model, phân bổ rõ ràng giữa class khai báo/tương tác giao diện (Pages) và file chạy luận lý kiểm thử (Tests).
3. **Smart Waits & Stability:** Không sử dụng hard sleep (`Thread.sleep`, `waitForTimeout`). Sử dụng chờ đợi thông minh (auto-waiting).
4. **Deterministic Data:** Dữ liệu kiểm thử sinh động phải unique (không hardcode) nhưng phải có tính truy gốc (traceable) dễ dàng phục vụ debug.

---

## 🚀 Quy Trình Tổng Quan (6 Bước)

1. **Context & Role-play (Khởi tạo ngữ cảnh):** Thiết lập vai trò chuyên gia Automation (ví dụ: Playwright/Java TestNG) và bối cảnh dự án.
2. **Analysis & UI Recon (Phân tích & Điều tra UI):** Cung cấp Test Cases, URL hệ thống và yêu cầu AI dùng công cụ (browser MCP) khởi tạo trình duyệt tự chạy truy xuất và lấy Locators thật từ DOM giao diện.
3. **POM Design (Thiết kế kiến trúc POM):** Dựa trên danh sách Locator mà AI tự động thu thập từ UI trực tiếp ở Bước 2, AI phác thảo cấu trúc các file mô hình class Page và hàm tương tác cho bạn kiểm duyệt.
4. **Test Data Strategy (Chiến lược dữ liệu):** Lên phương án khởi tạo tập dữ liệu ngẫu nhiên có truy vết để nhồi vào cho test layout.
5. **Script Generation (Sinh mã Automation):** AI kết hợp POM (từ bước 3) và Data Strategy (từ B4) để sinh đoạn script thực thi kiểm thử hoàn thiện.
6. **Review & Refactoring (Kiểm duyệt & Tối ưu):** Tối ưu hóa code, xóa log thừa, locator rác và review CI-ready guidelines.

*(Mỗi bước trên tương ứng với 1 thư mục con trong thư mục này, bao gồm `README.md` hướng dẫn chi tiết và `prompt.txt` chứa câu lệnh mẫu cho AI).*

---

## 🎯 Lưu ý Chuyên Sâu: Chiến Lược Thực Thi Kép (Execution Strategy)

Với công tác Automation Test, vì đầu vào của chúng ta đã là Kịch bản thủ công (Manual Test Cases) cực kỳ rõ ràng, nên rủi ro AI phải "đoán mò" nghiệp vụ là rất thấp. Do đó, bạn có **2 chiến lược** để sử dụng tùy theo nhu cầu:

### Cách 1: Chạy Tuần Tự (Manual Control)
Bạn sử dụng thủ công từng file `prompt.txt` trong 6 thư mục con (01 đến 06) để yêu cầu AI.
- **Phù hợp khi:** Bạn chỉ cần AI làm giúp 1 bước cụ thể (Ví dụ: "Chỉ sinh khung POM cho tao, không cần viết file chạy Script"). Hoặc khi Project quá lớn, bạn muốn AI tập trung nặn từng module một cách tỉ mỉ nhất dưới sự kiểm soát của bạn.

### Cách 2: Quy Trình 1 Chạm (One-Click Auto Workflow - Đề Xuất)
Khác với nhánh `plan/manual` bắt buộc phải chạy từng bước, nhánh Automation có thể tận dụng toàn bộ sức mạnh của AI Agent để tự trị hoàn toàn.
- **Cách dùng:** Bạn gọi Workflow `/generate_automation_from_testcases` kèm theo file Test Cases.
- **Ưu điểm vượt trội:** Agent sẽ tự đọc Test Case > Tự mở Browser lên bấm nút soi DOM lấy Locator > Tự đẻ Script > Tự xài Terminal để Run Test > Tự đọc Log Console để vá Bug > Chạy khi nào Xanh thì nghỉ. Tất cả diễn ra ngầm sau 1 nút bấm (Xem chi tiết nguyên lý ở Workflow `.agent/workflows/...`).
