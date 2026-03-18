# GIAI ĐOẠN 2: XÂY DỰNG HẠ TẦNG (Core Implementation - Utils First)

**Mục tiêu:**
Khác với các phương pháp cũ thường lao vào xây dựng Page Object Model (POM) và Test Scripts ngay, Master Framework nhấn mạnh triết lý "Utils First" (Xây nền móng trước).

Các hàm kiểm tra validate như so sánh chuỗi String, hàm parse ngày tháng, hay đẻ Data ID giả (Fake data) thường nằm rải rác mỗi file một ít gây rối và khó dò. Do đó, bước này ép AI phải gom toàn bộ chúng thành các **Helpers/Utils Classes** chuyên biệt bên trong thư mục `utils/`.

Mọi Class Test sau này chỉ cần Import các Helper này ra dùng.

**Cách sử dụng:**
Chạy file `prompt_3_build_utils.txt` để AI đẻ ra các file Helpers cần thiết cho dự án.
