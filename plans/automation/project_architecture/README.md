# Master Framework for E2E Automation (Generalized)

**Workflow:** `/generate_automation_from_testcases` hoặc `/generate_automation_from_ui_flow`
**Skill:** `qa_automation_engineer` + `framework_architect`

---

## Mục tiêu

Xây dựng hệ thống Automation có khả năng mở rộng, dễ bảo trì và báo cáo chuyên nghiệp. Thay vì "viết test đơn lẻ", chúng ta xây dựng **Framework gốc** vững chãi từ đầu.

## Cách sử dụng

Cung cấp kiến trúc phù hợp cho AI **ở Bước 0/1** để AI biết nơi chứa file mã nguồn.
- **Project mới:** AI sử dụng template bên dưới để scaffold.
- **Project có sẵn:** Mô tả cấu trúc hiện tại, AI sẽ tuân theo.

---

## Supported Stacks

### 🌐 Web Automation

| Stack | Ngôn ngữ | Runner | Report | Build Tool |
|---|---|---|---|---|
| **Playwright + TypeScript** | TypeScript | Playwright Test | HTML Report, Allure | npm |
| **Playwright + Python** | Python | Pytest | Allure, pytest-html | pip |
| **Selenium + Java** | Java | TestNG | Allure, ExtentReports | Maven / Gradle |
| **Selenium + Python** | Python | Pytest | Allure, pytest-html | pip |

### 📱 Mobile Automation

| Stack | Ngôn ngữ | Runner | Report | Build Tool |
|---|---|---|---|---|
| **Appium + Java** | Java | TestNG | Allure, ExtentReports | Maven / Gradle |
| **Appium + Python** | Python | Pytest | Allure, pytest-html | pip |

### 🔌 API Automation

| Stack | Ngôn ngữ | Runner | Report |
|---|---|---|---|
| **REST Assured** | Java | TestNG | Allure |
| **Playwright API** | TypeScript | Playwright Test | HTML Report |
| **Requests + Pytest** | Python | Pytest | Allure |

---

## Kiến Trúc Chuẩn

### 1. Playwright + TypeScript

```text
project-root/
├── playwright.config.ts          # Playwright configuration
├── package.json                  # Dependencies + npm scripts
├── .env.example                  # Environment template (KHÔNG chứa credentials thật)
├── .gitignore
├── README.md
├── src/
│   ├── pages/                    # Page Object classes
│   │   ├── base.page.ts          # Base page — common methods (wait, click, type, screenshot)
│   │   ├── login.page.ts
│   │   └── dashboard.page.ts
│   ├── fixtures/                 # Custom fixtures
│   │   ├── auth.fixture.ts       # Authentication fixture
│   │   └── base.fixture.ts       # Extended test with all fixtures
│   ├── utils/                    # Helpers & utilities
│   │   ├── test-data.ts          # Data generators (unique + traceable)
│   │   ├── env.config.ts         # Environment config reader
│   │   └── helpers.ts            # Common helper functions
│   └── tests/                    # Test specs (grouped by feature)
│       ├── auth/
│       │   └── login.spec.ts
│       └── dashboard/
│           └── dashboard.spec.ts
├── test-data/                    # External test data (JSON/YAML)
│   └── users.json
└── .github/
    └── workflows/
        └── playwright.yml        # CI pipeline template
```

### 2. Playwright + Python (Pytest)

```text
project-root/
├── pyproject.toml                # Python project config
├── requirements.txt              # Dependencies
├── conftest.py                   # Root fixtures + browser setup
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── pages/
│   │   ├── base_page.py          # Base page — common methods
│   │   ├── login_page.py
│   │   └── dashboard_page.py
│   ├── utils/
│   │   ├── config.py             # Env config reader
│   │   ├── test_data.py          # Data generators
│   │   └── helpers.py
│   └── tests/
│       ├── conftest.py           # Test-level fixtures
│       ├── test_login.py
│       └── test_dashboard.py
├── test-data/
│   └── users.json
└── .github/
    └── workflows/
        └── playwright.yml
```

### 3. Selenium + Java (Maven + TestNG)

```text
project-root/
├── pom.xml                       # Maven config + dependencies
├── testng.xml                    # TestNG suite config
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── main/java/
│   │   └── com/project/
│   │       ├── pages/            # Page Object classes
│   │       │   ├── BasePage.java         # Base page — common methods
│   │       │   ├── LoginPage.java
│   │       │   └── DashboardPage.java
│   │       ├── drivers/          # Driver management
│   │       │   └── DriverFactory.java
│   │       ├── config/           # Configuration
│   │       │   └── ConfigReader.java
│   │       └── utils/            # Utilities
│   │           ├── WaitHelper.java
│   │           ├── ScreenshotUtil.java
│   │           └── TestDataGenerator.java
│   └── test/java/
│       └── com/project/
│           ├── base/
│           │   └── BaseTest.java         # Test lifecycle hooks
│           └── tests/
│               ├── LoginTest.java
│               └── DashboardTest.java
├── test-data/
│   └── users.json
└── .github/
    └── workflows/
        └── selenium.yml
```

### 4. Selenium + Python (Pytest)

```text
project-root/
├── pyproject.toml
├── requirements.txt
├── conftest.py                   # Root fixtures + driver setup
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── pages/
│   │   ├── base_page.py
│   │   ├── login_page.py
│   │   └── dashboard_page.py
│   ├── drivers/
│   │   └── driver_factory.py     # WebDriver factory
│   ├── utils/
│   │   ├── config.py
│   │   ├── test_data.py
│   │   ├── wait_helper.py
│   │   └── screenshot_util.py
│   └── tests/
│       ├── conftest.py
│       ├── test_login.py
│       └── test_dashboard.py
├── test-data/
│   └── users.json
└── .github/
    └── workflows/
        └── selenium.yml
```

### 5. Appium + Java (Maven + TestNG)

```text
project-root/
├── pom.xml
├── testng.xml
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── main/java/
│   │   └── com/project/
│   │       ├── screens/          # Screen Objects (tương đương Page Objects)
│   │       │   ├── BaseScreen.java       # Base screen — common mobile methods
│   │       │   ├── LoginScreen.java
│   │       │   └── HomeScreen.java
│   │       ├── drivers/          # Appium driver management
│   │       │   ├── AppiumDriverFactory.java
│   │       │   └── CapabilitiesManager.java
│   │       ├── config/
│   │       │   └── AppConfig.java
│   │       └── utils/
│   │           ├── MobileGestures.java   # Swipe, scroll, tap helpers
│   │           ├── ScreenshotUtil.java
│   │           └── TestDataGenerator.java
│   └── test/java/
│       └── com/project/
│           ├── base/
│           │   └── BaseTest.java
│           └── tests/
│               ├── LoginTest.java
│               └── HomeTest.java
├── apps/                         # APK/IPA files
│   └── .gitkeep
├── test-data/
│   └── users.json
└── .github/
    └── workflows/
        └── appium.yml
```

### 6. Appium + Python (Pytest)

```text
project-root/
├── pyproject.toml
├── requirements.txt
├── conftest.py                   # Root fixtures + Appium driver setup
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── screens/
│   │   ├── base_screen.py
│   │   ├── login_screen.py
│   │   └── home_screen.py
│   ├── drivers/
│   │   ├── appium_driver_factory.py
│   │   └── capabilities_manager.py
│   ├── utils/
│   │   ├── config.py
│   │   ├── test_data.py
│   │   ├── mobile_gestures.py
│   │   └── screenshot_util.py
│   └── tests/
│       ├── conftest.py
│       ├── test_login.py
│       └── test_home.py
├── apps/
│   └── .gitkeep
├── test-data/
│   └── users.json
└── .github/
    └── workflows/
        └── appium.yml
```

### 7. REST Assured (Java + TestNG)

```text
project-root/
├── pom.xml
├── testng.xml
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── main/java/
│   │   └── com/project/
│   │       ├── api/              # API client classes
│   │       │   ├── BaseApi.java          # Base API — common request methods
│   │       │   ├── AuthApi.java
│   │       │   └── UserApi.java
│   │       ├── models/           # POJO / DTO classes
│   │       │   ├── UserRequest.java
│   │       │   └── UserResponse.java
│   │       ├── config/
│   │       │   └── ApiConfig.java
│   │       └── utils/
│   │           ├── TestDataGenerator.java
│   │           └── JsonHelper.java
│   └── test/java/
│       └── com/project/
│           ├── base/
│           │   └── BaseApiTest.java
│           └── tests/
│               ├── AuthApiTest.java
│               └── UserApiTest.java
├── test-data/
│   ├── payloads/                 # Request body templates (JSON)
│   │   └── create_user.json
│   └── schemas/                  # JSON Schema validation
│       └── user_response_schema.json
└── .github/
    └── workflows/
        └── api-tests.yml
```

### 8. Playwright API (TypeScript)

```text
project-root/
├── playwright.config.ts
├── package.json
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── api/                      # API client classes per resource
│   │   ├── base.api.ts           # Base API request context, default headers
│   │   ├── auth.api.ts
│   │   └── user.api.ts
│   ├── models/                   # TypeScript interfaces / DTOs
│   │   └── user.model.ts
│   ├── utils/                    # Helpers & data generators
│   │   ├── test-data.ts
│   │   └── schema-validator.ts
│   └── tests/                    # API test specs
│       ├── auth.api.spec.ts
│       └── user.api.spec.ts
└── test-data/
    ├── payloads/
    └── schemas/
```

### 9. Pytest + Requests (Python)

```text
project-root/
├── pyproject.toml
├── requirements.txt
├── pytest.ini
├── conftest.py                   # Root fixtures (base_url, auth_token, api_session)
├── .env.example
├── .gitignore
├── README.md
├── src/
│   ├── api/                      # API client classes
│   │   ├── base_api.py           # requests.Session wrapper
│   │   ├── auth_api.py
│   │   └── user_api.py
│   ├── models/                   # Pydantic models / dataclasses
│   │   └── user_model.py
│   ├── utils/                    # Helpers & data generators
│   │   ├── test_data.py
│   │   └── schema_validator.py
│   └── tests/                    # Pytest test files
│       ├── test_auth_api.py
│       └── test_user_api.py
└── test-data/
    ├── payloads/
    └── schemas/
```

---

## Component Checklist (Bắt Buộc)

Mỗi framework PHẢI bao gồm các thành phần sau:

| # | Component | Bắt buộc | Mô tả |
|---|-----------|----------|--------|
| 1 | **Project Structure** | ✅ | Thư mục rõ ràng, phân tách pages/tests/utils/config |
| 2 | **Config Management** | ✅ | Environment qua `.env` + config file — KHÔNG hardcode |
| 3 | **Browser / Driver Management** | ✅ | Factory pattern cho browser/driver setup |
| 4 | **Base Classes** | ✅ | BasePage/BaseScreen chứa common methods |
| 5 | **Page Object Model** | ✅ | Mỗi page → 1 class, locators khai báo ở đầu class |
| 6 | **Test Data Management** | ✅ | Data factory + external data (JSON/YAML) + unique/traceable |
| 7 | **Utilities** | ✅ | Wait helpers, screenshot utils, logger, string generators |
| 8 | **Reporting** | ✅ | Ít nhất 1 report tool + screenshot on failure |
| 9 | **CI/CD Pipeline** | 🟡 Khuyến khích | GitHub Actions / GitLab CI / Jenkins template |

---

## Design Principles

1. **DRY** — Mỗi logic chỉ viết 1 lần, tái sử dụng qua Base classes và Utils
2. **Single Responsibility** — Page chỉ chứa UI interaction, Test chỉ chứa test logic
3. **Open/Closed** — Dễ mở rộng (thêm page, thêm test) mà không sửa core
4. **Configuration over Code** — Env, browser, timeout quản lý qua config, không hardcode
5. **Fail Fast, Log Rich** — Screenshot on failure, structured logging, clear assertion messages

---

## Anti-Patterns (CẤM)

| ❌ Anti-Pattern | ✅ Đúng cách |
|---|---|
| Hardcode URL/credentials trong code | Đọc từ `.env` hoặc config file |
| Locator inline trong test | Khai báo trong Page class |
| `Thread.sleep()` / `waitForTimeout()` | Smart waits (`expect()`, `WebDriverWait`) |
| Global mutable state | Isolated fixtures/setup per test |
| Monolithic test file (1 file 500+ dòng) | Tách theo module/feature |
| `System.out.println()` / `console.log()` | Logger framework (Log4j, winston, logging) |
| Đoán locator không inspect DOM | Mở browser inspect DOM thực tế |

---

## Tham chiếu

- **Skill chi tiết:** `.agent/skills/framework_architect/SKILL.md`
- **Rules:** `.agent/rules/automation_rules.md`, `.agent/rules/locator_strategy.md`, `.agent/rules/api_rules.md`
- **Workflow scaffold:** `/generate_automation_framework`
