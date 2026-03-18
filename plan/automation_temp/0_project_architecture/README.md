# Master Framework for E2E Automation (Generalized)

**Mục tiêu:** Xây dựng hệ thống Automation có khả năng mở rộng, dễ bảo trì và báo cáo chuyên nghiệp cho mọi bài toán Web.

Bản thân hệ thống này được thiết kế để khắc phục các điểm yếu của việc "Viết Test Case đơn lẻ", thay vào đó là **"Xây dựng 1 Framework gốc"** vững chãi từ đầu.

Dưới đây là sơ đồ kiến trúc gốc mà mọi dự án Automation áp dụng bộ Prompts này cần hướng tới:

```text
Project/
├── tests/         # Test Runner (Spec files)
├── pages/         # Page Objects (UI Logic & Locators)
├── utils/         # Core Utilities (Data Helpers, File Generators)
├── test_data/     # Dữ liệu kịch bản (JSON) & Templates
├── temp/          # Chứa file sinh ra khi chạy test (Tự động dọn dẹp)
├── temp_reports/  # Chứa kết quả trung gian để gom báo cáo tổng
└── reports/       # Báo cáo cuối (HTML, Trace, Evidence)
```

Bạn hãy cung cấp cấu trúc này cho AI ở những bước đầu tiên để AI có cái nhìn tổng quan về nơi chứa file mã nguồn.
