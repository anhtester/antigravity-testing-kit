# GEMINI AI - GLOBAL AUTOMATION AGENT RULES

> **Scope:** Áp dụng cho mọi tác vụ Test Automation do Gemini (Antigravity) hoạt động trong dự án này.
> **Mục tiêu:** Sinh ra test scripts hiệu quả, ổn định – dễ debug – dễ scale – CI friendly.

## Browser Rules (MANDATORY)

* Tất cả **UI debugging** phải chạy với **desktop viewport** (ví dụ: `1920x1080`)
* Bắt buộc **mở browser thật** khi debug (headed mode)
* **Headless mode** chỉ được sử dụng **sau khi test đã debug PASS trên UI**
* CI/CD pipeline **được phép chạy headless mặc định**

---

## Tools

* Ưu tiên sử dụng **Playwright MCP**
* Mở browser thật để debug
* Inspect **DOM / HTML thực tế** trên trình duyệt
* Execute và debug test trực tiếp trên UI
* **KHÔNG** generate code khi chưa inspect DOM

---

## Cleanup & Delivery

* Sau khi test **PASS**:

  * Remove toàn bộ debug log
  * Xoá code thừa, locator không dùng
  * Không để commented code
* Deliver source code:

  * Clean
  * Readable
  * Maintainable

---

## 1. Ngôn Ngữ & Giao Tiếp

- Luôn giao tiếp, giải thích ý tưởng và báo cáo bằng **Tiếng Việt**.
- Diễn giải **ngắn gọn, rõ ràng, dễ hiểu**.
- Tránh suy đoán lập trình hoặc giải thích mơ hồ về lỗi mà cần có căn cứ trực tiếp.

## 2. Quy Trình Làm Việc (Workflow)

- **Recon (Điều tra):** Luôn inspect giao diện thực tế hoặc DOM/HTML/XML trước khi viết automation. Tuyệt đối KHÔNG ĐOÁN locator.
- **Implementation:** Giữ vững mô hình **Page Object Model (POM)**. Phân tách rõ Page objects, Test execution và Utils/Test data.
- **Execution & Self-fix:** Chạy test ngay sau khi code xong. Nếu test FAIL → tự đọc log → phân tích root cause → sửa code → chạy lại → đến khi PASS ổn định. Chỉ hỏi User khi gặp business rule mâu thuẫn.
- **Cleanup:** Gỡ bỏ debug logs, code thừa, locator không dùng trước khi deliver.

## 3. Tech Stack Hỗ Trợ

| Loại | Công nghệ |
|------|-----------|
| Ngôn ngữ | Java, TypeScript |
| Web Automation | Playwright (TS/Java), Selenium WebDriver (Java) |
| Mobile Automation | Appium (Java) |
| API Automation | REST Assured |
| Test Framework | TestNG, Playwright Test |
| Build Tool | Maven, npm |

## 4. Tham Chiếu Rules Chi Tiết

Agent phải tham chiếu quy tắc chi tiết trong `.agent/rules/`:

- [Quy tắc chung Automation](.agent/rules/automation_rules.md) — POM, Test Data, Naming, Assertions
- [Chiến lược chọn Locator](.agent/rules/locator_strategy.md) — Thứ tự ưu tiên locator
- [Quy tắc Playwright](.agent/rules/playwright_rules.md) — Browser setup, locator semantic, wait strategy
- [Quy tắc Selenium](.agent/rules/selenium_rules.md) — WebDriverWait, TestNG structure
- [Quy tắc Appium](.agent/rules/appium_rules.md) — Mobile locator, scroll, permission

## 5. Tham Chiếu Skills

Agent sử dụng skills trong `.agent/skills/` tùy theo nhiệm vụ:

| Skill | Vai trò |
|-------|---------|
| `qa_automation_engineer` | Master skill cho automation — điều phối toàn bộ quy trình |
| `rbt_manual_testing` | Master skill cho manual testing — quy trình AI-RBT 6 bước |
| `requirements_analyzer` | Phân tích requirements từ website/tài liệu |
| `ui_debug_agent` | Inspect UI/DOM, thu thập locators |
| `smart_locator_agent` | Sinh locator mới ổn định |
| `locator_healer_agent` | Sửa locator hỏng |
| `test_data_generator` | Sinh test data unique, traceable |
| `flaky_test_analyzer` | Phân tích và khắc phục flaky tests |

## 6. Kế Hoạch Kiểm Thử (Plan Templates)

Các bộ prompt template sẵn dùng trong `plan/`:

- **`plan/manual/`** — Quy trình 6 bước sinh Manual Test Cases (AI-RBT)
  - Xem `plan/manual/QUICK_START.md` để bắt đầu nhanh
  - Workflow: `/generate_manual_testcases_rbt`

- **`plan/automation/`** — Quy trình 6 bước sinh Automation Scripts
  - Xem `plan/automation/QUICK_START.md` để bắt đầu nhanh
  - One-click: Copy `plan/automation/prompt_automation.txt`
  - Workflow: `/generate_automation_from_testcases`

## 7. Test Data

- Tất cả field yêu cầu **unique** (Email, Username, Code/ID): **BẮT BUỘC** dùng dữ liệu random.
- Dữ liệu random phải **traceable / deterministic** — có thể truy ngược test gây lỗi.
- Format khuyến nghị: kết hợp `test name + timestamp + prefix`.

Ví dụ:

```
email:    test_login_1712049200@auto.test
username: auto_user_1712049200
code:     TC_LOGIN_1712049200
```

## 8. Code Quality (Smart Waits)

- **KHÔNG** dùng hard sleep (`waitForTimeout`, `Thread.sleep`, fixed delay).
- Chỉ sử dụng **smart waits** / auto-waiting:

| Framework | Smart Wait |
|-----------|-----------|
| Playwright | `expect().toBeVisible()`, `expect().toBeEnabled()`, Locator APIs |
| Selenium | `WebDriverWait` + `ExpectedConditions` |
| Appium | `WebDriverWait` + custom conditions |

- Hạn chế `waitForSelector` nếu `expect()` đáp ứng được.
- Mọi assertion phải có **timeout rõ ràng** hoặc dùng default timeout hợp lý.

## 9. Anti-Patterns (FORBIDDEN)

| ❌ Anti-Pattern | ✅ Thay thế đúng |
|----------------|-----------------|
| Guess selector / đoán locator | Inspect DOM thực tế trước khi code |
| Hard sleep (`waitForTimeout`, `Thread.sleep`) | Smart waits (`expect()`, `WebDriverWait`) |
| Copy selector từ code cũ không verify | Luôn verify selector trên browser hiện tại |
| Viết test không chạy ngay | Chạy test ngay sau khi implement |
| Commit test FAIL | Chỉ commit khi test PASS ổn định |
| Để debug log / commented code khi deliver | Cleanup trước khi deliver |
| Dùng test data hardcoded trùng lặp | Sinh data random + traceable |

## 10. Tham Chiếu Workflows

Agent sử dụng workflows trong `.agent/workflows/` qua slash commands:

| Workflow | Mô tả |
|----------|-------|
| `/generate_requirements_from_website` | Sinh requirements từ website/module |
| `/generate_manual_testcases_rbt` | Sinh manual test cases theo AI-RBT 6 bước |
| `/generate_testcases_from_requirements` | Sinh test cases từ requirements document |
| `/generate_automation_from_testcases` | Chuyển manual test cases → automation scripts |
| `/generate_automation_from_ui_flow` | Sinh automation từ UI flow trực tiếp |
| `/generate_full_automation_suite` | Khám phá app và sinh full automation suite |
| `/generate_automation_framework` | Thiết kế automation framework |
| `/generate_locator` | Sinh locator ổn định cho UI element |
| `/generate_test_data` | Sinh test data có cấu trúc |
| `/generate_api_tests_from_swagger` | Sinh API tests từ Swagger spec |
| `/generate_regression_suite` | Sinh regression test suite |
| `/generate_application_test_plan` | Sinh test plan cho application |
| `/analyze_flaky_tests` | Phân tích và khắc phục flaky tests |
