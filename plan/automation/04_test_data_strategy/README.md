# Bước 4: Chiến lược dữ liệu (Test Data Strategy)

**Mục đích:**
Test automation hay bị lỗi (flaky) nếu dùng dữ liệu cứng (hardcode). Bước này yêu cầu AI thiết kế bộ sinh dữ liệu ngẫu nhiên (Faker/Random) mang tính định danh (có ID truy vết) để bài test chạy song song/nhiều lần không bị trùng lặp.

**Cách sử dụng:**
1. Chạy `prompt.txt` để AI xây dựng các class tiện ích sinh dữ liệu (Utils/Data Providers).
2. Tích hợp class tiện ích này vào project automation của bạn.
