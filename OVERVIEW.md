# ScopeWeaver - Tổng quan Sản phẩm & Hướng dẫn Sử dụng

*Ngày cập nhật: 15/10/2026*

---

## 1. ScopeWeaver là gì?

**ScopeWeaver** là một hệ thống trung tâm điều khiển và nghiên cứu an ninh mạng tự động hóa tích hợp Trí tuệ Nhân tạo (AI). 

Sản phẩm được thiết kế nhằm giúp các chuyên gia an ninh mạng, nhóm kiểm thử xâm nhập (Penetration Testers) và các nhà quản lý hệ thống tự động hóa việc phát hiện, khai thác và quản lý rủi ro an ninh cho các ứng dụng web và hạ tầng công nghệ thông tin.

---

## 2. Các Chức năng chính

- **Tự động Lập Kế hoạch & Rà soát Lỗ hổng (Autonomous AI Security Testing)**:
  - Cho phép người dùng nhập địa chỉ trang web hoặc hệ thống mục tiêu.
  - Trí tuệ nhân tạo sẽ tự động phân tích cấu trúc trang web, lên danh sách các điểm cần kiểm tra và tự thực hiện các bước rà soát lỗ hổng an ninh mạng.

- **Quản lý & Phân loại Phát hiện An ninh (Findings Management)**:
  - Tự động ghi nhận các lỗ hổng tìm thấy kèm bằng chứng minh họa cụ thể (PoC).
  - Phân loại mức độ nghiêm trọng (Nghiêm trọng, Cao, Trung bình, Thấp).
  - Cung cấp tính năng **Đào sâu (Deepen)** để yêu cầu AI tiếp tục kiểm thử nâng cao và tính năng **Tái kiểm thử (Retest)** để xác minh lỗ hổng đã được sửa hay chưa.

- **Soi & Ghi nhận Lưu lượng Mạng (Traffic Inspection Proxy)**:
  - Tích hợp trạm trung gian bắt giữ và ghi lại toàn bộ dữ liệu trao đổi giữa công cụ kiểm thử và trang web mục tiêu.
  - Cho phép người dùng trực tiếp kiểm tra nội dung dữ liệu gửi đi/nhận về và đính kèm vào báo cáo lỗ hổng.

- **Kiểm soát Phạm vi An toàn (Asset & Scope Guard)**:
  - Quản lý danh sách các tên miền, địa chỉ IP và ứng dụng được phép kiểm thử.
  - Đảm bảo AI chỉ thực hiện rà soát trong đúng phạm vi cho phép, tuyệt đối không quét các mục tiêu nằm ngoài hệ thống.

- **Tương tác & Chỉ đạo Thời gian thực (Live Agent Chat & Hints)**:
  - Cung cấp khung trò chuyện trực tiếp để người dùng có thể gửi thêm gợi ý, hướng dẫn hoặc thay đổi mục tiêu kiểm thử trong khi AI đang chạy.

- **Xuất Báo cáo Tự động (Automated Reporting)**:
  - Tổng hợp toàn bộ danh sách lỗ hổng, mức độ rủi ro và các khuyến nghị khắc phục thành báo cáo hoàn chỉnh.

---

## 3. Đối tượng Sử dụng chính

1. **Chuyên viên An ninh mạng / Penetration Testers**: Tự động hóa các bước rà soát ban đầu để tập trung vào các kịch bản tấn công phức tạp.
2. **Đội ngũ Phát triển & Quản trị Hệ thống (DevSecOps / SysAdmins)**: Đánh giá an ninh cho ứng dụng nội bộ trước khi ra mắt.
3. **Quản lý An toàn Thông tin**: Cần cái nhìn tổng quan về số lượng lỗ hổng, mức độ rủi ro và tiến độ khắc phục.

---

## 4. Luồng Sử dụng Thực tế

1. **Khai báo Mục tiêu Kiểm thử**:
   Người dùng tạo một mục tiêu mới (ví dụ: `http://localhost:9999`) và đăng ký vào phạm vi cho phép kiểm thử.

2. **Khởi chạy Tác vụ Rà soát**:
   Tạo một Tác vụ (Task) mới và giao mục tiêu cho AI. Hệ thống AI sẽ tự động phân phân tích ứng dụng và thực hiện các thử nghiệm an ninh.

3. **Theo dõi Tiến độ & Xem Lỗ hổng**:
   Người dùng theo dõi tiến trình kiểm thử trực tiếp trên đồ thị hình cây hoặc nhật ký hoạt động. Khi phát hiện lỗ hổng (như lỗi chèn tệp LFI hay SQL Injection), thông tin chi tiết kèm bằng chứng sẽ xuất hiện ngay trong mục Phát hiện (Findings).

4. **Tái kiểm thử & Xuất Báo cáo**:
   Sau khi lập trình viên sửa lỗi, người dùng nhấn nút Tái kiểm thử (Retest) để AI xác nhận lại, sau đó xuất báo cáo tổng hợp kết quả.

---

## 5. Giới hạn & Lưu ý khi Sử dụng

- Hệ thống cần được kết nối với dịch vụ AI (Gemini, OpenAI hoặc Anthropic) thông qua API Key để thực hiện suy luận.
- Tính năng kiểm thử tự động yêu cầu mục tiêu kiểm thử phải hoạt động và có thể kết nối mạng được từ máy chủ chạy ScopeWeaver.
