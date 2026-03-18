# GIAI ĐOẠN 1: THIẾT KẾ & CHUẨN HÓA DỮ LIỆU (Design & Data Mapping)

Giai đoạn này bao gồm 2 nhiệm vụ cốt lõi trước khi AI bắt tay vào code bất cứ thứ logic UI nào:
1. **Phân tích UI & Chiến lược Automation:** Đóng vai Kiến trúc sư Automation, xác định Page Object, chiến lược Đăng nhập (UI hay API - rất quan trọng để giảm thiểu Flaky test), và bóc tách các Element phức tạp.
2. **Chuyển đổi kịch bản BDD (Gherkin) sang JSON Data:** Tạo một file data JSON linh hoạt chứa các key được chuẩn hoá sang dạng `camelCase` có sử dụng các biến động (Dynamic values) như `{{RANDOM_ID}}` thay vì nạp dữ liệu cứng.

**Cách sử dụng:**
Sử dụng lần lượt 2 file `prompt_1_analyzing_ui.txt` và `prompt_2_bdd_to_json.txt` đính kèm trong thư mục này để yêu cầu AI xử lý.
