# Quy Tắc Kiểm Thử API Automation (API Testing Rules)

> Áp dụng cho tất cả các bài test API Automation trong dự án (REST Assured - Java, Playwright API - TypeScript, Requests - Python, Supertest - JavaScript/TypeScript).

---

## 1. Kiến Trúc API Framework

- Phân tách rõ ràng các tầng trách nhiệm:
  - **API Client / Service Layer:** Chứa logic khởi tạo HTTP Request (URL, Headers, Params, Auth) và gửi đi. KHÔNG chứa assertion.
  - **Model / DTO Layer:** Định nghĩa các lớp Request / Response Data Transfer Objects (POJO, Pydantic, TypeScript Interface).
  - **Test Layer:** Chứa kịch bản test, gọi API Client, và thực hiện Assertions.
  - **Test Data / Utility Layer:** Hàm sinh dữ liệu ngẫu nhiên, đọc config, parse JSON Schema.
- Áp dụng **API Object Pattern** hoặc **Builder Pattern** khi xây dựng Request Payloads phức tạp.

---

## 2. Quy Trắc Kiểm Tra Kết Quả (Assertions Standard)

Mỗi API Test Case **BẮT BUỘC** phải thực hiện các kiểm tra sau:

1. **HTTP Status Code:** Kiểm tra exact match với status code mong đợi (VD: `200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `409 Conflict`).
2. **Response Schema:** Xác thực Response Body đúng cấu trúc JSON Schema / Contract đã định nghĩa.
3. **Key Business Fields:** Kiểm tra các trường dữ liệu quan trọng trong Response Body bằng exact value hoặc pattern match (sử dụng Soft Assertions khi kiểm tra nhiều trường cùng lúc).
4. **Error Response Detail (Negative Cases):** Đối với kịch bản lỗi, kiểm tra chính xác `errorCode`, `message` hoặc `details` trả về, không chỉ kiểm tra mỗi Status Code.
5. **Response Time / SLA:** Kiểm tra thời gian phản hồi nằm trong ngưỡng cho phép (VD: `< 2000ms`) nếu có yêu cầu SLA.
6. **Headers Check:** Kiểm tra Content-Type (VD: `application/json`) và các Security Headers quan trọng khi cần.

---

## 3. Quản Lý Authentication & Bảo Mật

- **KHÔNG hardcode** passwords, secret keys, API tokens trực tiếp trong source code hoặc file test data.
- Đọc credentials và base URL từ biến môi trường (`.env`, config files, hoặc system environment variables).
- Quản lý **Token Lifecycle** hiệu quả:
  - Lấy Auth Token ở bước Setup (`beforeAll`, `@BeforeClass`, hoặc Shared Fixture).
  - Tái sử dụng token giữa các test cases để tối ưu thời gian chạy.
  - Xử lý kịch bản tự động refresh token khi token hết hạn.
- **Masking Sensitve Data:** Tự động ẩn hoặc làm mờ (masking) thông tin nhạy cảm (Password, Token, Credit Card) trong log file và test execution reports.

---

## 4. Quy Tắc Sinh Dữ Liệu (Test Data Rules)

- Dữ liệu tạo mới qua API (Username, Email, Code/ID) **BẮT BUỘC** phải là dữ liệu động, độc lập và **traceable**:
  ```
  Format: auto_api_[testName]_[timestamp]_[random]
  Ví dụ:  auto_api_createUser_1712049200_a8f2@test.com
  ```
- **Cleanup / Teardown:** Các test case tạo dữ liệu thử nghiệm (`POST`) nên có bước dọn dẹp (`DELETE`) ở phần Teardown (`afterEach`, `afterAll`, `@AfterClass`) để tránh làm bẩn database môi trường test.
- Sử dụng **Data-Driven Testing** (TestNG DataProvider, Pytest Parametrize, Playwright Parameterized) khi kiểm tra cùng 1 API với nhiều bộ dữ liệu (Input Validation Matrix).

---

## 5. Chuẩn Hóa Theo HTTP Methods (HTTP Standards & Idempotency)

| HTTP Method | Thao Tác | Expected Status | Idempotent | Lưu Ý Kiểm Thử |
|---|---|---|---|---|
| **GET** | Read | 200 OK | ✅ Có | Read-only. KHÔNG làm thay đổi dữ liệu server. |
| **POST** | Create | 201 Created / 200 OK | ❌ Không | Trả về ID hoặc thông tin resource mới tạo. Gọi 2 lần cùng payload tạo 2 record khác nhau. |
| **PUT** | Update (Replace) | 200 OK / 204 No Content | ✅ Có | Cập nhật toàn bộ object. Thiếu optional field có thể reset field đó. Gọi N lần cho kết quả giống nhau. |
| **PATCH** | Update (Partial) | 200 OK | ❌/✅ Tùy API | Chỉ cập nhật trường được truyền lên. Trường không truyền phải giữ nguyên value cũ. |
| **DELETE** | Remove | 200 OK / 204 No Content | ✅ Có | Xóa resource. Gọi `GET` lại resource đó sau khi xóa PHẢI trả về `404 Not Found`. |

---

## 6. Chạy Test & Tự Động Fix Lỗi (Auto-Heal Workflow)

Khi chạy API Automation test bằng command line:

1. **Phân tích Log lỗi cụ thể:**
   - **Status Code mismatch:** Kiểm tra lại Request Body payload, Headers, hoặc Auth.
   - **Schema Mismatch:** Kiểm tra xem API backend có thay đổi response structure không.
   - **401/403 Error:** Kiểm tra token lấy được có hợp lệ hoặc hết hạn không.
   - **404 Not Found:** Kiểm tra lại Path variables, Base URL, hoặc Resource ID đã bị xóa chưa.
   - **500 Internal Server Error:** Log lỗi phía Server Backend (cần báo với dev nếu data gửi lên đúng spec).
2. **Sửa code & Re-run:** Cập nhật code test/model, chạy lại test verification cho đến khi PASS ổn định.

---

## 7. OWASP API Security & Advanced Protocol Checklist (BẮT BUỘC BỔ SUNG)

Mọi bộ API Test Cases được sinh ra **PHẢI** áp dụng các quy tắc kiểm thử nâng cao sau:

| # | Quy Tắc Kiểm Thử | Mô Tả & Expectation | Status Code |
|---|---|---|---|
| 1 | **BOLA / IDOR (Broken Object Level Auth)** | User A gửi token của mình nhưng sửa `id` sang tài nguyên của User B trong URL/Body. | **403 Forbidden** |
| 2 | **Mass Assignment** | Gửi kèm các thuộc tính quản trị/hệ thống không được phép (`role: "ADMIN"`, `isAdmin: true`, `createdAt`, `id`). | **200/201** (nhưng bị loại bỏ) hoặc **400** |
| 3 | **Content Negotiation & Content-Type** | Gửi `Content-Type: text/plain` hoặc `application/xml` thay vì `application/json`. | **415 Unsupported Media Type** |
| 4 | **Malformed Payload** | Gửi Request Body JSON bị lỗi cú pháp syntax (`{ "name": }`). | **400 Bad Request** |
| 5 | **Oversized Payload** | Gửi Request Body có kích thước vượt quá giới hạn (VD: > 10MB JSON). | **413 Payload Too Large** |
| 6 | **Unrestricted Resource Consumption** | Spaming N requests liên tục trong thời gian cực ngắn. | **429 Too Many Requests** |
| 7 | **Race Condition / Concurrency** | Gửi 2 request song song tạo trùng unique data tại cùng 1 millisecond. | 1 **201 Created**, 1 **409 Conflict** |
| 8 | **ReDoS / Stress Payload** | Gửi string cực dài (VD: 10,000 chars) ở trường password/input để test chống treo server. | **400 / 422** |
| 9 | **Sensitive Data Exposure** | Verify Response Body **KHÔNG bao giờ** trả về `password`, `hash`, `secretKey` trong bất kỳ endpoint nào. | **200 OK** (No Secrets in Body) |

