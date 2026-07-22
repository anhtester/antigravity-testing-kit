---
description: Sinh API test cases và automation scripts từ Swagger/OpenAPI specification. Hỗ trợ 2 mode — SPEC (chỉ test cases) và FULL (test cases + automation scripts).
skills:
  - qa_automation_engineer
  - test_data_generator
---

# Workflow: Sinh API Tests từ Swagger/OpenAPI

> **BẮT BUỘC (MANDATORY SKILL & RULES):** Bạn PHẢI nạp và đọc kỹ nội dung của skill **`qa_automation_engineer`** (tại `.agent/skills/qa_automation_engineer/SKILL.md`) và quy tắc **`api_rules.md`** (tại `.agent/rules/api_rules.md`) trước khi bắt đầu. Ngoài ra, tham khảo thêm skill **`test_data_generator`** để sinh test data đúng chuẩn.

Workflow này giúp agent phân tích Swagger/OpenAPI specification, xác định các endpoints, sinh API test cases có cấu trúc, và (tùy mode) tự động sinh automation scripts hoàn chỉnh.

## ⚠️ Nguyên tắc thực thi

- **Tất cả output bằng Tiếng Việt**
- **KHÔNG đoán** schema/endpoint — phải đọc spec thực tế (JSON/YAML)
- **Phải chờ user xác nhận** scope tại Bước 2 trước khi sinh chi tiết
- Nếu user chưa cung cấp Swagger URL/file → hỏi trước khi bắt đầu
- ⚠️ **Rule E3:** Khi test FAIL → tự đọc log → phân tích → sửa → chạy lại. KHÔNG hỏi user trong quá trình fix lỗi

## 2 Chế độ (Mode)

| Mode | Khi nào sử dụng | Output |
|---|---|---|
| **SPEC** (mặc định) | User cần API test cases dưới dạng tài liệu | API Test Cases (Markdown) + Test Data Matrix |
| **FULL** | User yêu cầu cả automation scripts | Như SPEC + Automation Scripts + Project Structure |

> Nếu user nói "generate automation", "viết code test API", hoặc yêu cầu scripts → tự động chuyển sang **Mode FULL**.

## Các bước thực hiện

### Bước 1: Tiếp nhận & Phân tích Spec (Parse & Analyze)

1. **Thu thập API Specification / Input Data** từ user:
   - **URL Swagger/OpenAPI** (VD: `https://api.example.com/swagger.json`) → dùng `read_url_content` để fetch
   - **File local Swagger/OpenAPI** (JSON/YAML) → dùng `view_file` để đọc
   - **Swagger UI URL** → trích xuất URL spec gốc (thường là `/v2/api-docs` hoặc `/v3/api-docs`)
   - **Scalar API Reference URL** → inspect HTML để tìm `data-configuration` chứa URL spec (VD: `https://book.anhtester.com/swagger` → spec tại `/swagger/json`)
   - **Các dạng API Doc khác** (Redoc, Stoplight, RapiDoc) → tìm URL spec trong page source hoặc network requests
   - **Postman Collection File** (JSON format v2.0/v2.1) → parse danh sách items, requests, headers, body raw/form-data
   - **Danh sách cURL commands** → parse HTTP method, URL, headers (`-H`), body (`-d` / `--data-raw`)
   - **Tài liệu Requirement / API Spec Text** → trích xuất endpoints, parameters, expected response status & body
2. **Parse spec** và trích xuất thông tin:
   - Base URL, API version, authentication scheme (Bearer, API Key, OAuth2, Basic)
   - Danh sách tất cả endpoints: `method + path`
   - Request parameters: path, query, header, body (schema + required fields)
   - Response schemas: status codes, response body structure
   - Models/Definitions: reusable data models
3. **Phân loại endpoints** theo nhóm:
   - **CRUD operations** — Create, Read, Update, Delete
   - **Authentication** — Login, Register, Token refresh
   - **Business Logic** — Các API xử lý nghiệp vụ phức tạp
   - **Utility** — Health check, config, metadata

### Bước 2: Xác nhận Scope & Tech Stack (CHECKPOINT — ⏸️ DỪNG LẠI)

1. **Trình bày tóm tắt** cho user review:
   - Tổng số endpoints phát hiện (phân nhóm)
   - Authentication method
   - Danh sách endpoint groups + số lượng API mỗi nhóm
   - Mode đề xuất (SPEC hay FULL)
2. **Hỏi user xác nhận:**
   - "Bạn muốn test tất cả endpoints hay chỉ tập trung vào nhóm nào?"
   - "Bạn muốn output là test cases (SPEC) hay cả automation scripts (FULL)?"
   - Nếu Mode FULL: "Tech stack mong muốn?" (mặc định theo bảng bên dưới)
3. **Chờ user xác nhận** scope trước khi sang Bước 3

**Tech Stack mặc định (Mode FULL):**

| Framework | Ngôn ngữ | Khi nào dùng |
|---|---|---|
| **REST Assured** | Java | Mặc định cho Java projects, TestNG runner |
| **Playwright API Testing** | TypeScript | Khi user dùng Playwright hoặc TypeScript stack |
| **Supertest + Jest** | TypeScript/JS | Khi user dùng Node.js backend |
| **Requests + Pytest** | Python | Khi user dùng Python stack |

### Bước 3: Sinh API Test Scenarios & Test Data

1. **Với mỗi endpoint** trong scope đã xác nhận, sinh test scenarios theo 7 loại:
   - **✅ Happy Path** — Request hợp lệ, response đúng schema + status code
   - **❌ Negative — Validation** — Thiếu required fields, sai data type, vượt max length
   - **❌ Negative — Auth** — Không có token, token hết hạn, token sai role
   - **🔲 Boundary** — Min/max values, empty string, null, special characters
   - **⚡ Edge Cases** — Concurrent requests, duplicate creation, large payload, unicode/emoji
   - **🔒 Security** — SQL injection, XSS, IDOR, sensitive data exposure (xem mục 5 bên dưới)
   - **📄 Pagination & Filtering** — Phân trang, sắp xếp, tìm kiếm (xem mục 6 bên dưới)

2. **Với mỗi scenario**, xác định rõ:
   - **Request:** Method, URL, Headers, Body/Params (giá trị cụ thể)
   - **Expected Response:** Status code, Response body structure, Error message
   - **Priority:** P1 (Critical) / P2 (High) / P3 (Medium) / P4 (Low)

3. **Sinh Test Data Matrix** (sử dụng skill `test_data_generator`):
   - Data valid cho Happy Path
   - Data invalid cho Negative cases (mỗi field 1 bộ negative)
   - Boundary values theo schema constraints (minLength, maxLength, min, max, pattern)
   - Data phải **unique + traceable** (VD: `auto_api_1712049200@test.com`)

4. **Field-Level Validation cho Request Body (BẮT BUỘC):**

   Với mỗi endpoint có request body (POST/PUT/PATCH), agent **PHẢI liệt kê từng field** trong body và sinh negative TCs riêng cho TỪNG field:

   | Loại Field | Validation cần test |
   |---|---|
   | **String (name, title...)** | Required → gửi thiếu · Empty string `""` · Chỉ whitespace `"   "` · Vượt maxLength · Dưới minLength · Ký tự đặc biệt `<>&"'` · Unicode/Emoji · XSS payload `<script>alert(1)</script>` · SQL injection `' OR 1=1--` |
   | **Email** | Format sai (thiếu `@`, thiếu domain) · Email trùng (nếu unique) · Max length · Case sensitivity |
   | **Number (age, price...)** | Kiểu string thay vì number · Số âm · Số 0 · Số thập phân (nếu integer) · Overflow (`999999999999`) · Min/Max value |
   | **Boolean** | Gửi string `"true"` thay vì boolean `true` · Gửi `null` · Gửi số `1`/`0` |
   | **Date/DateTime** | Format sai · Ngày không tồn tại (`2024-02-31`) · Ngày tương lai/quá khứ (tùy rule) · Timezone khác nhau |
   | **Enum** | Giá trị ngoài enum · Case sensitivity · Empty string |
   | **Array** | Mảng rỗng `[]` · Mảng quá nhiều phần tử · Phần tử sai type · Phần tử trùng lặp |
   | **Object (nested)** | Thiếu required sub-fields · Extra fields không có trong schema · Nested object `null` |
   | **File (multipart)** | Sai MIME type · Quá dung lượng · File rỗng (0 bytes) · Tên file ký tự đặc biệt |

   **Ví dụ — POST /api/customers `{ name, email, phone, age }`:**
   ```
   → Field "name" (string, required, maxLength: 100):
     TC: Gửi thiếu field name → 400 + error message
     TC: name = "" (empty) → 400
     TC: name = "   " (whitespace) → 400
     TC: name = 101 ký tự → 400 (vượt max)
     TC: name = "<script>alert(1)</script>" → 400 hoặc sanitized
   
   → Field "email" (string, required, format: email):
     TC: email = "invalid" → 400
     TC: email = "test@" → 400
     TC: email trùng user khác → 409 Conflict
   
   → Field "age" (integer, min: 0, max: 150):
     TC: age = "abc" → 400 (sai type)
     TC: age = -1 → 400 (dưới min)
     TC: age = 151 → 400 (vượt max)
     TC: age = 18.5 → 400 (decimal thay vì integer)
   ```

   > **Nguyên tắc:** Mỗi field có schema riêng → validation TCs riêng. KHÔNG gộp nhiều fields vào 1 TC. Ưu tiên test từng field độc lập trước, sau đó test kết hợp.

5. **API Security Testing:**

   Sinh test cases bảo mật cho mỗi endpoint:

   | Loại | Test Scenarios |
   |---|---|
   | **Injection** | SQL injection trong query params (`?id=1 OR 1=1`) · SQL injection trong body fields · XSS trong input fields · Command injection (nếu API xử lý shell) |
   | **IDOR** | Truy cập resource của user khác bằng ID (`GET /users/999` khi user chỉ có quyền xem user 123) · Thay đổi ID trong PUT/DELETE để sửa/xóa resource không phải của mình |
   | **Auth Bypass** | Gọi API không có token → phải 401 · Token hết hạn → 401 · Token của role thấp gọi API role cao → 403 · Token bị tamper (sửa payload) → 401 |
   | **Sensitive Data** | Response không trả về password/secret · Response không leak internal IDs/stack traces · Headers không leak server info (`X-Powered-By`, `Server`) |
   | **Rate Limiting** | Gửi nhiều request liên tục → phải bị giới hạn (429) · Brute force login → lock account |
   | **CORS** | Kiểm tra `Access-Control-Allow-Origin` header · Kiểm tra preflight request (OPTIONS) |

6. **Pagination & Filtering Tests (cho GET List endpoints):**

   | Test Type | Scenarios |
   |---|---|
   | **Pagination** | Không truyền page/limit → dùng default · `page=1&limit=10` → đúng 10 items · `page=999` → trả mảng rỗng (không lỗi) · `limit=0` hoặc `limit=-1` → xử lý hợp lý · `limit=10000` (quá lớn) → giới hạn hoặc lỗi |
   | **Sorting** | `sort=name&order=asc` → kết quả đúng thứ tự · `sort=invalid_field` → lỗi hoặc ignore · Case sensitivity trong sort |
   | **Filtering** | Filter theo từng field hỗ trợ · Filter với giá trị không tồn tại → trả mảng rỗng · Filter với ký tự đặc biệt · Kết hợp nhiều filter cùng lúc |
   | **Search** | Tìm kiếm partial match · Tìm kiếm case-insensitive · Tìm kiếm với ký tự đặc biệt · Tìm kiếm empty string |

7. **Phân biệt PUT vs PATCH (nếu API có cả 2):**

   | Method | Test Focus |
   |---|---|
   | **PUT** | Gửi đầy đủ tất cả fields → cập nhật toàn bộ · Gửi thiếu optional field → field đó bị reset/null · Gửi thiếu required field → 400 |
   | **PATCH** | Gửi chỉ 1 field → chỉ field đó thay đổi, các field khác giữ nguyên · Gửi body rỗng `{}` → không thay đổi gì (hoặc 400) · Gửi field không tồn tại → ignore hoặc 400 |

### Bước 4: Đóng gói API Test Cases (Output — Mode SPEC)

1. Tạo **artifact** `api_test_cases.md` với cấu trúc:
   - **Tổng quan API** — Base URL, Version, Auth method, Tổng endpoints
   - **Endpoint Catalog** — Bảng: `| # | Method | Path | Mô tả | Số Test Cases |`
   - **Test Cases chi tiết** — Theo từng endpoint:

   ```
   | TC ID | Endpoint | Scenario | Request | Expected Response | Priority | Loại |
   ```

   - **Test Data Matrix** — Bảng data valid/invalid/boundary cho mỗi model
   - **Dependencies & Execution Order** — Thứ tự chạy test (VD: phải tạo user trước khi test get user)

2. Nếu user chọn **Mode SPEC** → **KẾT THÚC** tại đây

### Bước 5: Sinh Automation Scripts (Mode FULL)

> Chỉ thực hiện khi ở **Mode FULL**

1. **Thiết kế project structure** phù hợp với framework:

   **REST Assured (Java):**
   ```
   src/test/java/
   ├── api/                    # API client classes (per resource)
   │   ├── BaseApi.java        # Base config: baseURI, auth, logging
   │   ├── UserApi.java        # Methods: createUser(), getUser(), ...
   │   └── AuthApi.java        # Methods: login(), refreshToken(), ...
   ├── models/                 # POJO/DTO classes (from Swagger models)
   │   ├── UserRequest.java
   │   └── UserResponse.java
   ├── tests/                  # Test classes (TestNG)
   │   ├── UserApiTest.java
   │   └── AuthApiTest.java
   ├── utils/                  # Helpers
   │   ├── TestDataGenerator.java
   │   └── AssertionHelper.java
   └── testdata/               # Test data files (JSON/YAML)
   ```

   **Playwright API (TypeScript):**
   ```
   tests/
   ├── api/
   │   ├── helpers/
   │   │   ├── base-api.ts     # Base request context, auth
   │   │   ├── user-api.ts     # API methods per resource
   │   │   └── test-data.ts    # Data generators
   │   ├── user.api.spec.ts    # Test file per resource
   │   └── auth.api.spec.ts
   └── fixtures/
       └── api-fixtures.ts     # Shared fixtures (auth tokens, etc.)
   ```

   **Requests + Pytest (Python):**
   ```
   tests/
   ├── api/
   │   ├── conftest.py          # Fixtures: base_url, auth_token, api_client
   │   ├── helpers/
   │   │   ├── base_api.py      # Base API client (requests.Session)
   │   │   ├── user_api.py      # API methods per resource
   │   │   └── test_data.py     # Data generators
   │   ├── models/
   │   │   ├── user_model.py    # Pydantic/dataclass models
   │   │   └── response_model.py
   │   ├── test_user_api.py     # Test file per resource
   │   └── test_auth_api.py
   └── pytest.ini               # Pytest config
   ```

   **Supertest + Jest (TypeScript):**
   ```
   tests/
   ├── api/
   │   ├── helpers/
   │   │   ├── base-api.ts      # Supertest agent config
   │   │   ├── user-api.ts      # API methods per resource
   │   │   └── test-data.ts     # Data generators
   │   ├── models/
   │   │   └── user.model.ts    # TypeScript interfaces
   │   ├── user.api.test.ts     # Test file per resource
   │   └── auth.api.test.ts
   ├── jest.config.ts            # Jest config
   └── setup.ts                  # Global setup (auth token)
   ```

2. **Sinh code** theo thứ tự:
   - **Base API class** — Config baseURL, default headers, auth interceptor, request/response logging
   - **Model/DTO classes** — Từ Swagger definitions/schemas
   - **API client classes** — Methods cho mỗi endpoint (return typed response)
   - **Test Data generators** — Data factory với random + traceable values
   - **Test classes** — Gọi API client, assert status code + response body + schema

3. **Assertions bắt buộc** cho mỗi test:
   - ✅ HTTP Status Code (exact match)
   - ✅ Response body — key fields matching expected values
   - ✅ Response time < threshold (nếu có SLA)
   - ✅ Response schema validation (đúng cấu trúc)
   - ✅ Error message chính xác cho negative cases
   - ✅ Headers check (Content-Type, CORS nếu relevant)

4. **Best practices trong code:**
   - Dùng **Builder pattern hoặc Factory** cho test data
   - **Chaining requests** khi cần setup data (VD: create → get → update → delete)
   - **Soft assertions** khi cần check nhiều fields cùng lúc
   - **Parameterized tests** cho data-driven scenarios
   - **Cleanup/teardown** — xóa test data đã tạo sau khi test xong
   - Không hardcode token/credentials — đọc từ env hoặc config

### Bước 6: Chạy thử nghiệm & Tự sửa lỗi (Execution & Auto-Heal)

> Chỉ thực hiện khi ở **Mode FULL**

1. **Chạy test** bằng `run_command`:
   - REST Assured: `mvn test -Dtest=<TestClass>`
   - Playwright: `npx playwright test tests/api/`
   - Pytest: `python -m pytest tests/api/`

2. **Theo dõi** kết quả qua `command_status`:
   - Nếu **PASS** → cập nhật artifact, báo cáo kết quả
   - Nếu **FAIL** → áp dụng vòng lặp Auto-Heal:

   ```
   WHILE test FAIL:
     1. Đọc error log → xác định root cause
     2. Phân loại lỗi:
        - Schema mismatch → cập nhật model/assertion
        - Auth failure → kiểm tra token flow
        - 404/405 → kiểm tra lại endpoint path/method
        - Timeout → tăng timeout hoặc kiểm tra server
        - Data conflict → thay test data unique mới
     3. Sửa code bằng `replace_file_content` hoặc `multi_replace_file_content`
     4. Chạy lại test
     5. Lặp cho đến khi PASS (tối đa 5 vòng)
   ```

3. **⚠️ Rule E3:** KHÔNG hỏi user trong quá trình fix lỗi. Chỉ hỏi khi:
   - Server API không response (down/blocked)
   - Business rule mâu thuẫn với spec
   - Đã hết 5 vòng auto-heal mà vẫn fail

## Xử lý lỗi thường gặp

| Lỗi | Nguyên nhân | Cách xử lý |
|---|---|---|
| Swagger URL trả về HTML | URL là Swagger UI, không phải spec | Tìm URL spec gốc (`/v2/api-docs`, `/v3/api-docs`, `/swagger.json`) |
| Scalar URL trả về HTML | URL là Scalar API Reference UI | Inspect HTML tìm `data-configuration` → lấy field `url` → nối với base URL. Thử: `/swagger/json`, `/reference/json` |
| Spec rỗng hoặc không parse được | File corrupt hoặc format không chuẩn | Hỏi user cung cấp lại, thử convert YAML ↔ JSON |
| Auth endpoint không rõ | Spec không document auth flow | Hỏi user về auth method và cách lấy token |
| Response schema khác spec | API thực tế không khớp với document | Note là known issue, adjust test theo response thực tế |
| Rate limiting | API có giới hạn request/phút | Thêm delay giữa các test hoặc dùng retry logic |
| CORS blocked | Browser chặn cross-origin request | Kiểm tra `Access-Control-Allow-Origin` header, test trực tiếp bằng HTTP client (không qua browser) |
| 405 Method Not Allowed | Gọi sai HTTP method | Kiểm tra lại spec — xác nhận method đúng (PUT vs PATCH, POST vs PUT) |
| SSL/TLS Certificate Error | Self-signed cert hoặc expired | Thêm option skip SSL verification trong test config (chỉ cho test environment) |

## Output

### Mode SPEC
- Artifact `api_test_cases.md`:
  - API Overview (Base URL, Version, Auth)
  - Endpoint Catalog (bảng tổng hợp)
  - Test Cases chi tiết (phân nhóm theo endpoint)
  - Test Data Matrix
  - Execution Order & Dependencies

### Mode FULL
- Tất cả output của Mode SPEC, cộng thêm:
  - Project structure (phù hợp với framework)
  - Base API class + Auth helper
  - Model/DTO classes
  - API client classes (per resource)
  - Test classes với assertions đầy đủ
  - Test data generators
  - Kết quả chạy test (PASS/FAIL report)