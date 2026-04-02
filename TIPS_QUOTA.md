# 📖 CẨM NANG SỬ DỤNG ANTIGRAVITY TỐI ƯU QUOTA TRONG AUTOMATION TESTING

> **Antigravity** là AI Coding Agent tích hợp trong Gemini CLI / Google IDX, hỗ trợ lập trình viên tự động hóa kiểm thử phần mềm (Automation Testing). Tài liệu này hướng dẫn cách **tối ưu quota** (giới hạn token) để làm việc hiệu quả nhất mà không bị gián đoạn giữa chừng.

Chiến thuật cốt lõi: **"Chia Để Trị"** – dùng đúng Mode (chế độ) và đúng Model (mô hình AI) cho từng loại task.

---

## Bảng Tóm Tắt Nhanh

| Model | Độ mạnh | Dùng khi nào | Token |
|-------|---------|--------------|-------|
| Claude Sonnet / Opus | ⭐⭐⭐ | Logic khó, fix bug hóc búa, Shadow DOM, iFrame, Flaky test | 🔴 Tốn nhiều |
| Gemini Pro | ⭐⭐ | Code phổ thông, sửa locator, thêm assertions, refactor | 🟡 Vừa phải |
| Gemini Flash / Claude Haiku | ⭐ | Đọc log, sinh data, viết docs, phân tích HTML | 🟢 Rất rẻ |

| Mode | Dùng khi nào | Token |
|------|-------------|-------|
| Planning Mode | Khởi tạo framework, xây kiến trúc POM mới, viết E2E phức tạp | 🔴 Tốn nhiều |
| Fast Mode | Bổ sung test case, update locator, refactor cục bộ, fix lỗi rõ ràng | 🟢 Tiết kiệm |

---

## 1. Phân bổ công việc theo Chế Độ (Mode)

> **Lưu ý:** Planning Mode tiêu tốn token lớn nhất — nó đọc rộng, suy nghĩ nhiều bước và tự động lặp lại quy trình.

*   **Dùng PLANNING MODE cho Tác vụ Nền tảng (Big Tasks):**
    *   Khởi tạo một Automation Framework mới (setup thư mục, Base Test, Report).
    *   Xây dựng kiến trúc Page Object Model (POM) cho module chức năng hoàn toàn mới.
    *   Viết một luồng End-to-End (E2E) Test phức tạp chạy qua nhiều page và thay đổi nhiều trạng thái.
*   **Chuyển về FAST MODE cho Tác vụ Bảo trì (Daily Tasks):**
    *   Sau khi "dựng khung" xong ở Planning, hãy lập tức tắt nó đi.
    *   Dùng Fast Mode để chạy việc hàng ngày: bổ sung thêm test case, update locators bị đổi, tối ưu lại các hàm (methods) hoặc refactor code cục bộ.

---

## 2. Chiến thuật lựa chọn AI Models (Theo độ khó)

Code ưu tiên Claude, nhưng hãy biết cách phân bổ cho các Model khác để bảo vệ quota.

*   ⭐⭐⭐ **Claude Sonnet / Opus (Bộ óc Tinh nhuệ):**
    *   **Nhiệm vụ:** Giải quyết các logic Code khó và Fix Bug hóc búa.
    *   **Automation Testing:** Dùng để xử lý các bài toán như lấy element trong Shadow DOM, tương tác iFrame lồng nhau, xử lý bất đồng bộ (Async/await), kéo thả (Drag-and-Drop) hoặc các test case đang chạy chập chờn (Flaky). 
    *   *Lưu ý: Token của Claude rất nhanh cạn, chỉ dùng khi thật sự "kẹt".*
*   ⭐⭐ **Gemini Pro (Cỗ máy Bảo trì):**
    *   **Nhiệm vụ:** Cán đáng công việc code "phổ thông". 
    *   **Automation Testing:** Sửa lỗi locator sai ID/Class, thêm một vài bước verify/assertions đơn giản, thay đổi data cứng thành biến linh hoạt. Giải quyết cực tốt các lỗi rõ ràng và đã biết nguyên nhân.
*   ⭐ **Gemini Flash / Claude Haiku (Sát thủ Tốc độ & Tiết kiệm):**
    *   **Nhiệm vụ:** Xử lý văn bản, Data và đọc Log (nhanh, Context dài nhưng cực kỳ rẻ). 
    *   **Automation Testing:** 
        *   Phân tích cục Log CI/CD ngàn dòng để chỉ ra duy nhất 1 dòng gây lỗi `AssertionError`.
        *   Đọc HTML lớn và bóc tách cấu trúc để xuất ra Table locators.
        *   Sinh hàng trăm dòng Mock Data (JSON, CSV, Tài khoản giả) cho Data-Driven Testing.
        *   Tạo tài liệu (Test Document), viết Git Commit messages, soạn Javadoc / Docstrings.

---

## 3. Các Tip "Nhỏ mà Có Võ" để hạn chế tốn Token

### Khi gửi yêu cầu

*   **Tránh đưa rác vào Context:** Thay vì copy toàn bộ thẻ `<body>` của giao diện web đổ vào chat, hãy dùng Inspect trong trình duyệt và chỉ copy đúng thẻ `<div>` biểu diễn khối thông tin (Table / Form / Dropdown) đang cần xử lý.
*   **Định vị không gian làm việc:** Luôn `@mention` chỉ đúng file Test và file Page Object đang làm dở khi nhờ AI fix code (ví dụ: gõ `@LoginTest` và `@LoginPage`). Việc này ngăn AI đọc cả project mất thì giờ.
*   **Mô tả yêu cầu đầy đủ ngay lần đầu:** Tránh gửi yêu cầu mơ hồ rồi bổ sung dần qua 5-6 lượt chat. Mỗi lượt hỏi thêm, AI phải đọc lại toàn bộ context trước đó — rất tốn token. Hãy cung cấp đủ: mục tiêu, tech stack, file liên quan, kết quả mong muốn ngay trong câu hỏi đầu.
*   **Hạn chế gửi screenshot khi có thể dùng text:** Ảnh chụp màn hình tốn token gấp nhiều lần so với text. Nếu chỉ cần chỉ ra 1 lỗi trên UI, hãy copy text lỗi hoặc HTML element thay vì chụp ảnh.

### Khi debug lỗi

*   **Hỏi cùng Stack Trace:** Khi Script lỗi (ví dụ: `ElementNotVisible`), đừng quăng nguyên file code lên hỏi mù. Hãy copy dòng lỗi thực tế in ra ở Terminal ghép cùng đoạn Code bị hỏng thả vào Fast Mode — AI sẽ đánh đúng trọng tâm ngay lập tức.
*   **Giới hạn vòng lặp self-fix:** Nếu AI tự fix liên tục 3 lần mà test vẫn FAIL, hãy dừng lại và can thiệp thủ công — đọc log, xác định root cause rồi chỉ rõ cho AI. Để AI chạy loop vô hạn sẽ đốt token cực nhanh mà không giải quyết được gốc vấn đề.

### Khi quản lý conversation

*   **Tách conversation nhỏ theo task:** Mỗi conversation dài tích lũy context — càng dài, mỗi lượt chat tiếp theo AI càng phải đọc lại nhiều, token tăng cấp số nhân. Nên mở conversation mới cho mỗi task độc lập (ví dụ: 1 conversation cho Login Page, 1 conversation khác cho Dashboard Page).
*   **Tận dụng Workflows (Slash Commands) có sẵn:** Thay vì mô tả lại quy trình dài dòng, dùng trực tiếp slash commands đã được define sẵn (ví dụ: `/generate_automation_from_testcases`). Workflow đã chứa prompt tối ưu, giúp AI hiểu đúng yêu cầu ngay lần đầu mà không cần giải thích thêm.