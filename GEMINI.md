# GEMINI AI - GLOBAL AUTOMATION AGENT RULES

> **Scope:** Áp dụng cho mọi tác vụ Test Automation do Gemini (Antigravity) hoạt động trong dự án này.
> **Mục tiêu:** Sinh ra test scripts hiệu quả, ổn định – dễ debug – dễ scale – CI friendly.

---

## Git Pull Restriction Rule

* Tuyệt đối KHÔNG dùng lệnh GIT làm thay đổi trạng thái code (như `git pull`, `git checkout`, `git merge`, `git rebase`, `git reset`) để lấy code hoặc thay đổi nhánh.
* Vì có nhiều trường hợp code trên server chưa được cập nhật, việc pull code về sẽ ghi đè và làm sai toàn bộ phần code đang chỉnh sửa ở máy local.
* Luôn giữ nguyên trạng thái code local hiện tại để làm việc.
* Nếu cần file hoặc nội dung mới, hãy yêu cầu người dùng cung cấp thay vì tự ý dùng git.
* **Được phép** dùng lệnh read-only: `git status`, `git diff`, `git log` — để kiểm tra trạng thái mà không thay đổi code.

---

## Browser Rules (MANDATORY)

### 🖥️ Viewport & Mode

* Tất cả **UI debugging** phải chạy với **desktop viewport**: **`1920x1080`**
* Bắt buộc **mở browser thật** khi debug (headed mode)
* **Headless mode** chỉ được sử dụng **sau khi test đã debug PASS trên UI**
* CI/CD pipeline **được phép chạy headless mặc định**

### 🔄 Thứ Tự Debug Bắt Buộc (Playwright MCP)

Khi dùng Playwright MCP để debug UI, **LUÔN** tuân theo thứ tự:

```
navigate → resize(1920×1080) → wait_for(page_load) → snapshot → interact → screenshot(on_fail)
```

* **KHÔNG** gọi `browser_navigate` lại nếu đã đang ở đúng trang — tránh reload ngoài ý muốn
* **LUÔN** gọi `browser_resize(width=1920, height=1080)` ngay sau `browser_navigate`
* **LUÔN** verify page đã load xong trước khi lấy snapshot hoặc tương tác

### 📸 Screenshot & Snapshot

* Dùng **`snapshot`** để phân tích DOM và xác định locator
* Dùng **`screenshot`** để lưu bằng chứng khi test fail hoặc để báo cáo
* Chụp **screenshot ngay khi assertion fail** để hỗ trợ truy vết lỗi
* **KHÔNG** chụp screenshot tràn lan — chỉ khi cần thiết (fail / milestone quan trọng)

---

## Tools

### 🛠️ Ưu Tiên Sử Dụng

* Ưu tiên sử dụng **Playwright MCP** cho tất cả tác vụ debug UI
* Tham chiếu rule chi tiết: [Quy tắc Playwright](.agent/rules/playwright_rules.md)

### 🔍 Inspect & Debug

* Mở browser thật để debug (headed mode)
* Inspect **DOM / HTML thực tế** trên trình duyệt — **KHÔNG đoán locator**
* Execute và debug test trực tiếp trên UI trước khi sinh code
* **KHÔNG** generate code khi chưa inspect DOM

### ⚡ Nguyên Tắc

* Một locator phải được **verify chạy được** trên browser hiện tại trước khi đưa vào code
* Nếu locator lấy từ code cũ → **bắt buộc verify lại** trước khi dùng

---

## Cleanup & Delivery

### ✅ Điều kiện bàn giao (Definition of Done)

Test chỉ được coi là **hoàn thành** khi đáp ứng **toàn bộ** các tiêu chí sau:

#### 🧹 Code Cleanup

- [ ] Xoá toàn bộ `print()`, `console.log()`, debug log tạm thời
- [ ] Xoá locator không còn sử dụng
- [ ] Không để lại commented-out code
- [ ] Không có `waitForTimeout` / `Thread.sleep` hardcoded
- [ ] Không có test data hardcoded (email, username, ID phải random/traceable)

#### 🏗️ Cấu trúc & POM

- [ ] Tuân thủ mô hình **Page Object Model** — tách biệt Page class, Test class, Utils
- [ ] Locator được định nghĩa trong Page class, không viết inline trong test
- [ ] Tên file, class, method đặt theo convention rõ ràng và nhất quán
- [ ] Import không còn thừa (unused imports)

#### ✔️ Chất lượng Test

- [ ] Test **PASS ổn định** ít nhất **2 lần liên tiếp** trên UI (headed mode)
- [ ] Assertion có message rõ ràng, dễ debug khi fail
- [ ] Mỗi test case độc lập — không phụ thuộc thứ tự chạy
- [ ] Test data được sinh động (timestamp/random) và traceable

#### 📁 File Output

- [ ] Source code được lưu đúng vị trí trong project structure
- [ ] Không có file tạm, file test thừa trong thư mục source
- [ ] File cấu hình (config, .env) không chứa credentials thật

#### 📋 Báo Cáo Kết Quả

- [ ] Tóm tắt kết quả: số test PASS / FAIL / SKIP
- [ ] Nêu rõ các TC đã implement và TC nào bị skip (kèm lý do)
- [ ] Ghi chú các known issues hoặc limitation nếu có

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

| Loại             | Công nghệ                                     |
| ----------------- | ----------------------------------------------- |
| Ngôn ngữ        | Java, TypeScript                                |
| Web Automation    | Playwright (TS/Java), Selenium WebDriver (Java) |
| Mobile Automation | Appium (Java)                                   |
| API Automation    | REST Assured                                    |
| Test Framework    | TestNG, Playwright Test                         |
| Build Tool        | Maven, npm                                      |

## 4. Tham Chiếu Rules Chi Tiết

Agent phải tham chiếu quy tắc chi tiết trong `.agent/rules/`:

- [Quy tắc chung Automation](.agent/rules/automation_rules.md) — POM, Test Data, Naming, Assertions
- [Chiến lược chọn Locator](.agent/rules/locator_strategy.md) — Thứ tự ưu tiên locator
- [Quy tắc Playwright](.agent/rules/playwright_rules.md) — Browser setup, locator semantic, wait strategy
- [Quy tắc Selenium](.agent/rules/selenium_rules.md) — WebDriverWait, TestNG structure
- [Quy tắc Appium](.agent/rules/appium_rules.md) — Mobile locator, scroll, permission
- [Quy tắc API Testing](.agent/rules/api_rules.md) — Standard HTTP assertions, Schema validation, Auth lifecycle

## 5. Tham Chiếu Skills

Agent sử dụng skills trong `.agent/skills/` tùy theo nhiệm vụ:

| Skill                      | Vai trò                                                                                  |
| -------------------------- | ----------------------------------------------------------------------------------------- |
| `qa_automation_engineer` | Master skill cho automation — điều phối toàn bộ quy trình                          |
| `rbt_manual_testing`     | Master skill cho manual testing — 2 modes: QUICK (sinh TC nhanh) và FULL RBT (6 bước) |
| `requirements_analyzer`  | Phân tích requirements từ website/tài liệu                                           |
| `ui_debug_agent`         | Inspect UI/DOM, thu thập locators                                                        |
| `smart_locator_agent`    | Sinh locator mới ổn định                                                              |
| `locator_healer_agent`   | Sửa locator hỏng                                                                        |
| `test_data_generator`    | Sinh test data unique, traceable — hỗ trợ multi-step pipeline & combinatorial data       |
| `flaky_test_analyzer`    | Phân tích và khắc phục flaky tests                                                   |
| `jira_integration`       | Tích hợp Jira/Xray — lấy requirements, đẩy test results                             |

## 6. Kế Hoạch Kiểm Thử (Plan Templates)

Các bộ prompt template sẵn dùng trong `plans/`:

- **`plans/manual/`** — Quy trình sinh Manual Test Cases (2 modes: QUICK + FULL RBT)

  - Xem `plans/manual/QUICK_START.md` để bắt đầu nhanh
  - Workflow QUICK: `/generate_testcases_from_requirements`
  - Workflow FULL RBT: `/generate_manual_testcases_rbt`
- **`plans/automation/`** — Quy trình 6 bước sinh Automation Scripts

  - Xem `plans/automation/QUICK_START.md` để bắt đầu nhanh
  - One-click: Copy `plans/automation/prompt_automation.txt`
  - Workflow: `/generate_automation_from_testcases`
- **`plans/cross-module/`** — Quy trình phân tích Cross-Module & Ma trận kết hợp

  - Xem `plans/cross-module/QUICK_START.md` để bắt đầu nhanh
  - Workflow phân tích: `/generate_cross_module_test_plan`
  - Workflow sinh data: `/generate_combinatorial_test_data`

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

| Framework  | Smart Wait                                                           |
| ---------- | -------------------------------------------------------------------- |
| Playwright | `expect().toBeVisible()`, `expect().toBeEnabled()`, Locator APIs |
| Selenium   | `WebDriverWait` + `ExpectedConditions`                           |
| Appium     | `WebDriverWait` + custom conditions                                |

- Hạn chế `waitForSelector` nếu `expect()` đáp ứng được.
- Mọi assertion phải có **timeout rõ ràng** hoặc dùng default timeout hợp lý.

## 9. Anti-Patterns (FORBIDDEN)

| ❌ Anti-Pattern                                   | ✅ Thay thế đúng                            |
| ------------------------------------------------- | ---------------------------------------------- |
| Guess selector / đoán locator                   | Inspect DOM thực tế trước khi code         |
| Hard sleep (`waitForTimeout`, `Thread.sleep`) | Smart waits (`expect()`, `WebDriverWait`)  |
| Copy selector từ code cũ không verify          | Luôn verify selector trên browser hiện tại |
| Viết test không chạy ngay                      | Chạy test ngay sau khi implement              |
| Commit test FAIL                                  | Chỉ commit khi test PASS ổn định           |
| Để debug log / commented code khi deliver       | Cleanup trước khi deliver                    |
| Dùng test data hardcoded trùng lặp             | Sinh data random + traceable                   |

## 10. Tham Chiếu Workflows

Agent sử dụng workflows trong `.agent/workflows/` qua slash commands:

| Workflow                                  | Mô tả                                                     |
| ----------------------------------------- | ----------------------------------------------------------- |
| `/generate_requirements_from_website`   | Sinh requirements từ website/module                        |
| `/analyze_requirement_document`         | Phân tích requirement document (Jira/.doc) — sinh tài liệu phân tích, KHÔNG sinh TC |
| `/generate_manual_testcases_rbt`        | Sinh manual test cases theo AI-RBT 6 bước (FULL RBT mode) |
| `/generate_testcases_from_requirements` | Sinh test cases nhanh từ requirements (QUICK mode)         |
| `/generate_automation_from_testcases`   | Chuyển manual test cases → automation scripts             |
| `/generate_automation_from_ui_flow`     | Sinh automation từ UI flow trực tiếp                     |
| `/generate_application_test_plan`       | Khám phá app, sinh test plan (Mode PLAN) hoặc full suite (Mode FULL) |
| `/generate_automation_framework`        | Thiết kế automation framework                             |
| `/generate_locator`                     | Sinh locator ổn định cho UI element                      |
| `/generate_test_data`                   | Sinh test data có cấu trúc                               |
| `/generate_cross_module_test_plan`    | Phân tích cross-module (2 modes: DOCUMENT/BROWSER), sinh ma trận kết hợp bằng script pairwise |
| `/generate_combinatorial_test_data`   | Sinh test data cho ma trận kết hợp — offline hoặc pipeline qua browser          |
| `/generate_api_tests_from_swagger`      | Sinh API tests từ Swagger spec                             |
| `/analyze_flaky_tests`                  | Phân tích và khắc phục flaky tests                     |
| `/fetch_jira_requirements`              | Lấy requirements/user stories từ Jira                     |
| `/import_test_results_xray`             | Đẩy kết quả test lên Xray                              |
