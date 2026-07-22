# Antigravity Testing Kit 🚀

👋 Chào mừng bạn đến với **Antigravity Testing Kit**!

Đây là bộ Kit được xây dựng và phát triển bởi **Anh Tester**, dành riêng cho **Cộng đồng Tester Việt Nam**. Mục tiêu của repo này là cung cấp sẵn các thiết lập, quy tắc hành vi (Rules), kỹ năng (Skills), và quy trình (Workflows) chuẩn theo docs của Antigravity để hỗ trợ sử dụng AI Agent trên phần mềm **Antigravity**.

Bộ Kit này **không chỉ dành riêng cho Automation** — mà được thiết kế toàn diện cho cả **Manual Testing** lẫn **Automation Testing**, bao phủ toàn bộ vòng đời kiểm thử phần mềm từ phân tích yêu cầu, thiết kế test cases cho đến thực thi và báo cáo kết quả.

Đặc biệt, mọi công đoạn đều được **tích hợp AI một cách có hệ thống**, tạo thành một **quy trình ứng dụng AI hoàn thiện (End-to-End AI Testing Workflow)** — giúp Tester làm việc thông minh hơn, nhanh hơn và hiệu quả hơn trong kỷ nguyên AI.

---

## 🌟 Tính Năng Nổi Bật

- **🔁 Quy Trình AI Hoàn Thiện (End-to-End):** Được xây dựng thành một quy trình ứng dụng AI khép kín — từ phân tích yêu cầu (Requirements), thiết kế test cases (Manual), đến viết script tự động (Automation), tích hợp CI/CD và báo cáo kết quả — tất cả đều có AI hỗ trợ.
- **📋 Hỗ Trợ Cả Manual & Automation Testing:** Không chỉ dừng lại ở Automation, Kit còn trang bị đầy đủ quy trình, skill và prompt cho **Manual Tester** — bao gồm phân tích rủi ro (RBT), thiết kế test cases chất lượng cao và quản lý kết quả kiểm thử.
- **🧠 Tối ưu cho QA/Tester:** Tất cả các prompt, rule và workflow đều được tinh chỉnh dựa trên tư duy và quy trình làm việc thực tế của cả **Manual Tester** lẫn **Automation Engineer**.
- **🌐 Hỗ trợ Đa Nền Tảng:** Tương thích với các framework phổ biến như Web (Playwright, Selenium), Mobile (Appium), và API (Playwright, REST Assured).
- **🛡️ Tuân thủ Tiêu Chuẩn Cao (Strict Rules):** Đảm bảo AI luôn đi theo cấu trúc Page Object Model (POM), viết code rõ ràng, không đoán bừa locator và tự động sửa lỗi (Self-fix).
- **🇻🇳 Giao Tiếp Bằng Tiếng Việt:** AI được cấu hình để trao đổi, giải thích và báo cáo hoàn toàn bằng Tiếng Việt, thân thiện với người dùng Việt Nam.

---

## 📂 Cấu Trúc Thư Mục Chính

```
antigravity-testing-kit/
├── .agent/
│   ├── rules/           # Quy tắc bắt buộc AI phải tuân theo
│   ├── skills/          # 10 kỹ năng chuyên biệt cho AI
│   └── workflows/       # 16 kịch bản thực thi step-by-step (slash commands)
├── plans/
│   ├── manual/          # Quy trình 6 bước sinh Manual Test Cases (AI-RBT)
│   ├── automation/      # Quy trình 6 bước sinh Automation Scripts
│   └── cross-module/    # Quy trình phân tích Cross-Module & Ma trận kết hợp
├── practices/
│   ├── requirements/    # Lưu trữ requirements đã sinh
│   └── testcases/       # Lưu trữ testcases đã sinh (hoặc testcases mẫu)
├── prompt_templates/    # Prompt mẫu dùng nhanh (copy → paste → gửi)
├── scripts/
│   ├── convert_excel/   # Chuyển đổi Markdown Test Cases sang Excel
│   └── integrations/    # Tích hợp công cụ bên ngoài
│       ├── jira/        # Jira & Xray integration (self-contained)
│       └── google_sheet/# Đọc/ghi dữ liệu với Google Sheets
├── GEMINI.md            # Rule chung cho AI Agent
├── RULE_GLOBAL.md       # Quy tắc toàn cục cho toàn bộ tác vụ
└── TIPS_QUOTA.md        # Cẩm nang tối ưu quota token
```

### `.agent/` — Bộ não của AI Agent

| Thư mục | Vai trò |
|---------|--------|
| `rules/` | Quy tắc bắt buộc: POM, locator strategy, smart waits, Playwright/Selenium/Appium rules |
| `skills/` | 10 kỹ năng chuyên biệt: automation engineer, manual testing, UI debug, locator healer, test data generator, framework architect, jira integration... |
| `workflows/` | 16 slash commands: `/generate_automation_from_testcases`, `/generate_manual_testcases_rbt`, `/generate_cross_module_test_plan`, `/generate_combinatorial_test_data`... |
---

### `scripts/` — Công Cụ Bổ Trợ & Tích Hợp

Chứa các công cụ tiện ích và kịch bản kết nối hệ thống. Mỗi công cụ/integration là một **thư mục độc lập** (tự quản dependencies, config, README riêng).

| Công cụ / Tích hợp | Chức năng | Docs |
|--------------------|-----------|------|
| `convert_excel/` | Chuyển đổi file Markdown Test Cases sang Excel có layout tối ưu | [README](scripts/convert_excel/README.md) |
| `integrations/jira/` | Lấy Requirements từ Jira, xác thực Xray, đẩy kết quả test lên Xray | [README](scripts/integrations/jira/README.md) |
| `integrations/google_sheet/` | Đồng bộ, đọc/ghi dữ liệu test từ Google Sheets qua API | [README](scripts/integrations/google_sheet/README.md) |

```bash
# Cài đặt nhanh (ví dụ Jira)
cd scripts/integrations/jira
npm install
cp .env.example .env    # Điền credentials
```

---

### `plans/` — Quy Trình 6 Bước Chuyên Sâu

Dành cho các tác vụ phức tạp, cần thực hiện **tuần tự trong cùng 1 conversation**.

| Plan | Mô tả | Bắt đầu nhanh |
|------|-------|---------------|
| [`plans/manual/`](plans/manual/) | Sinh Manual Test Cases theo quy trình **AI-RBT 6 bước** (Risk-Based Testing) | Xem [`plans/manual/QUICK_START.md`](plans/manual/QUICK_START.md) |
| [`plans/automation/`](plans/automation/) | Sinh Automation Scripts theo **6 bước** từ context → review | Xem [`plans/automation/QUICK_START.md`](plans/automation/QUICK_START.md) |
| [`plans/cross-module/`](plans/cross-module/) | Phân tích tính năng **đa module** & sinh **ma trận kết hợp** (Pairwise/Cartesian) | Xem [`plans/cross-module/QUICK_START.md`](plans/cross-module/QUICK_START.md) |

**Cách dùng:** Mở `QUICK_START.md` → Làm theo từng bước → Gửi prompt mỗi bước vào Antigravity.

### `prompt_templates/` — Prompt Mẫu Dùng Nhanh

Dành cho tác vụ **đơn lẻ**, chỉ cần copy → thay `[...]` bằng dữ liệu thực → paste → gửi.

| # | Prompt | Mục đích |
|---|--------|----------|
| 01 | [`prompt_01_generate_requirements.txt`](prompt_templates/prompt_01_generate_requirements.txt) | Phân tích website sinh Requirements |
| 02 | [`prompt_02_generate_test_cases.txt`](prompt_templates/prompt_02_generate_test_cases.txt) | Sinh test cases từ requirements |
| 03 | [`prompt_03_create_framework_playwright.txt`](prompt_templates/prompt_03_create_framework_playwright.txt) | Dựng framework Playwright TS |
| 03 | [`prompt_03_create_framework_selenium.txt`](prompt_templates/prompt_03_create_framework_selenium.txt) | Dựng framework Selenium Java |
| 04 | [`prompt_04_generate_script_playwright.txt`](prompt_templates/prompt_04_generate_script_playwright.txt) | Viết test script Playwright TS |
| 04 | [`prompt_04_generate_script_selenium.txt`](prompt_templates/prompt_04_generate_script_selenium.txt) | Viết test script Selenium Java |
| 05 | [`prompt_05_convert_manual_to_automation.txt`](prompt_templates/prompt_05_convert_manual_to_automation.txt) | Chuyển manual TC sang automation |
| 06 | [`prompt_06_review_automation_code.txt`](prompt_templates/prompt_06_review_automation_code.txt) | Review code automation |
| 07 | [`prompt_07_generate_test_data.txt`](prompt_templates/prompt_07_generate_test_data.txt) | Sinh test data có cấu trúc |
| 08 | [`prompt_08_analyze_flaky_tests.txt`](prompt_templates/prompt_08_analyze_flaky_tests.txt) | Phân tích test không ổn định |
| 09 | [`prompt_09_generate_api_tests.txt`](prompt_templates/prompt_09_generate_api_tests.txt) | Viết test API từ Swagger |

> 💡 Thư mục [`prompt_templates/prompt_workflow_template/`](prompt_templates/prompt_workflow_template/) chứa phiên bản prompt ngắn gọn hơn, tối ưu cho workflow.

---

## ✳️ Hướng Dẫn Sử Dụng Trong Antigravity

1. **Clone Repo này về máy:**
   Hoặc bạn có thể copy trực tiếp thư mục `.agent` từ repo này.
   
2. **Tích hợp vào dự án của bạn:**
   Copy thư mục `.agent` vào thư mục gốc (root directory) của dự án Automation hoặc Manual Test mà bạn đang làm việc.

3. **Bắt đầu trò chuyện với AI trên Antigravity:**
   Khi mở dự án lên Antigravity, AI tự động nhận diện thư mục `.agent` và sẽ áp dụng ngay các Rule, Skill, Workflow của **Anh Tester** đã thiết lập sẵn.

4. **(Tùy chọn) Sử dụng Plan hoặc Prompt Template:**
    - Tác vụ phức tạp (1 module) → Mở [`plans/manual/QUICK_START.md`](plans/manual/QUICK_START.md) hoặc [`plans/automation/QUICK_START.md`](plans/automation/QUICK_START.md)
    - Tác vụ đa module (ma trận kết hợp) → Mở [`plans/cross-module/QUICK_START.md`](plans/cross-module/QUICK_START.md)
    - Tác vụ nhanh → Copy prompt từ [`prompt_templates/`](prompt_templates/) → paste vào chat

---

## 🤝 Hỗ Trợ & Đóng Góp

- Nếu bạn gặp khó khăn trong quá trình sử dụng hoặc muốn đóng góp để bộ công cụ này hoàn thiện hơn, đừng ngần ngại tạo **Issue** hoặc **Pull Request**.
- Tham gia cộng đồng **Anh Tester** để cùng trao đổi, học hỏi thêm nhiều kiến thức bổ ích về Automation Testing!
  - 📘 **Fanpage Facebook:** [Anh Tester](https://www.facebook.com/anhtester)
  - 👥 **Group Facebook Automation:** [Cộng đồng Automation Testing](https://www.facebook.com/groups/automationtest)
  - 👥 **Group Facebook Manual:** [Cộng đồng Manual Testing](https://www.facebook.com/groups/manualtest)
  - ✈️ **Telegram Automation:** [Cộng đồng Automation Testing](https://t.me/+kSUGJ3pVvxkyZWU1)
  - ✈️ **Telegram Manual:** [Cộng đồng Manual Testing](https://t.me/+8eChRz7OVqliZWRl)

---

## 📄 License

Dự án này được phân phối dưới giấy phép nguồn mở **[MIT License](LICENSE)**.

---
Anh Tester Automation Testing 🎯
https://anhtester.com