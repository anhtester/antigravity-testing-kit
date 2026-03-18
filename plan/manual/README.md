# AI-DRIVEN RISK-BASED TESTING FRAMEWORK (AI-RBT)

**Mục tiêu:** 
Tận dụng tốc độ của AI để thực thi chi tiết, kết hợp với tư duy chiến lược RBT (Risk-Based Testing) của con người để tối ưu hóa nguồn lực kiểm thử.

## 📌 Nguyên Tắc Cốt Lõi

1. **Human Strategy:** Con người xác định chiến lược, mức độ rủi ro và tiêu chuẩn.
2. **AI Execution:** AI thực hiện phân tích, viết TCs và rà soát lỗ hổng.
3. **Human Verification:** Con người kiểm tra lại kết quả của AI trước khi chốt.

---

## 🚀 Quy Trình Tổng Quan (6 Bước)

1. **Context & Role-play (Khởi tạo ngữ cảnh):** Khởi tạo tư duy chuyên gia QA/Tester cho AI bằng cách định hình vai trò và cung cấp bối cảnh dự án.
2. **Analysis & QnA (Phân tích yêu cầu):** AI đọc tài liệu và phân tích yêu cầu để làm rõ các điểm mờ (Ambiguity) trước khi viết kịch bản.
3. **Decomposition (Phân rã module):** Phân rã hệ thống thành các Module (Feature Mapping - FM) nhỏ hơn để dễ dàng đánh giá.
4. **Traceability (Đảm bảo độ bao phủ):** Thiết lập ma trận truy vết để đảm bảo độ bao phủ yêu cầu (Coverage).
   - *(Lưu ý: Sau bước 4 có thể có một bước Checkpoint Đánh giá rủi ro do con người thực hiện - Human Only).*
5. **RBT & TC Generation (Sinh Test Case chi tiết):** Áp dụng chiến lược Risk-Based Testing để AI sinh nội dung kịch bản kiểm thử (Logic & Scenario).
6. **Template Mapping (Chuẩn hóa Format):** Chuẩn hóa toàn bộ định dạng Test Case và điền vào file/bảng mẫu để dùng cho quản lý (ví dụ: Jira, Excel).

*(Mỗi bước trên tương ứng với 1 thư mục con trong thư mục này, bao gồm `README.md` hướng dẫn chi tiết và `prompt.txt` chứa câu lệnh mẫu cho AI).*

---

## ⚠️ Lưu ý Quan Trọng: Chiến Lược Thực Thi (Execution Strategy)

Đối với phần **Manual Testing** (Đi từ Yêu cầu chức năng / Figma), bạn **BẮT BUỘC phải chạy tuần tự thủ công từng prompt một** thay vì gộp chung thành một lệnh Workflow chạy tự động 100%.

**Lý do:**
1. **Điểm nghẽn ở Bước 2 (Analysis & QnA):** AI cần thời gian phân tích và đưa ra các câu hỏi (Ambiguities) về logic của Requirement để bạn giải đáp. Nếu chạy 1 lèo, AI sẽ tự đoán mò logic dẫn đến viết Test Case sai trầm trọng.
2. **Điểm chốt chặn Nhân sự (Human in the loop):** Ở Bước 4 và 5, chính con người (Tester) phải tự review đánh giá rủi ro (RBT) trước khi cho AI sinh kịch bản chi tiết.
3. **Chống Ảo giác (Hallucination):** Việc nạp 1 tài liệu dài và bắt AI nhả ra hàng trăm kịch bản cùng lúc sẽ khiến AI mất tập trung, bỏ sót luồng (Miss coverage). Việc xé nhỏ ra từng prompt sẽ mang lại chất lượng Test Case tuyệt đối nhất.
