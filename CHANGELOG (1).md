# LỊCH SỬ THAY ĐỔI & CẬP NHẬT (CHANGELOG)
**Dự án:** VaultManager - Hệ Thống Quản Lý Tài Khoản Đa Tầng Trên Trình Duyệt  
**Phiên bản hiện tại:** v1.2.0  
**Ngày cập nhật gần nhất:** 05/07/2026  

Tài liệu này ghi nhận toàn bộ tiến trình xây dựng, chỉnh sửa và bổ sung tính năng cho ứng dụng theo từng phiên bản phát triển.

---

## [v1.2.0] - 2026-07-05 (Cập nhật Bộ Công Cụ Chuyển Đổi & Làm Sạch Dữ Liệu)

### ✨ Tính năng bổ sung mới (New Features)
- **Tích hợp Trình Chuyển Đổi & Phục Hồi Dữ Liệu (Data Converter & Recovery Suite):**
  - Thêm nút thao tác nhanh `🔄 Chuyển đổi & Phục hồi JSON` tại Sidebar và `⚡ Chuyển đổi JSON` tại Header.
  - Hỗ trợ xử lý chuỗi JSON cũ hoặc văn bản thô (chứa thông tin tài khoản) dán trực tiếp vào ứng dụng.
- **Tính năng Tự động Chuẩn hóa & Làm sạch (Auto-Sanitation Engine):**
  - Tự động nhận dạng và loại bỏ các cú pháp Markdown bị mã hóa lỗi trong quá trình copy/paste từ tài liệu cũ (ví dụ: bóc tách `[https://facebook.com](https://facebook.com)` thành `https://facebook.com`, bóc tách `[mail@domain.com](mailto:...)` thành `mail@domain.com`).
  - Tự động chuyển đổi các thực thể HTML (HTML entities) như `&amp;` thành `&`, `&lt;` thành `<`, v.v.
- **Chuyển đổi từ định dạng Văn bản thô (Text/CSV to JSON):**
  - Cho phép dán danh sách tài khoản dạng từng dòng (`Website | Username | Password | Ghi chú`).
  - Tự động phân tích, tự tạo Đề mục và Website tương ứng rồi chuyển đổi thành cấu trúc chuẩn JSON `version 1.0`.
- **Chế độ Phục hồi linh hoạt:**
  - **Phục hồi thay thế toàn bộ (Replace All):** Xóa dữ liệu hiện tại và khôi phục 100% từ JSON vừa chuyển đổi.
  - **Gộp dữ liệu (Merge Data):** Bổ sung tài khoản mới từ JSON vào danh sách hiện có mà không làm mất tài khoản cũ.
  - **Tải về file sạch (.json):** Xuất file JSON chuẩn đã được làm sạch để lưu trữ ngoại tuyến.

### 🛠 Cải tiến UI/UX (Improvements)
- Thêm nút Nạp mẫu nhanh dữ liệu JSON vào modal chuyển đổi để kiểm tra tính năng làm sạch ngay lập tức.
- Thống kê chi tiết số lượng (Đề mục, Website, Tài khoản) ngay sau khi phân tích JSON thành công.

---

## [v1.1.0] - 2026-07-05 (Hoàn thiện Kiến trúc Đa tầng & Phân chia mục nhỏ)

### ✨ Tính năng cốt lõi đã hoàn thành
- **Kiến trúc Quản lý 3 Tầng Khoa học:**
  - *Tầng 1 (Đề mục lớn):* Phân loại theo ngành/lĩnh vực (Mạng xã hội, Công việc, Tài chính, Hạ tầng server...).
  - *Tầng 2 (Trang web/Dịch vụ):* Gom nhóm các tài khoản chung một dịch vụ (kèm Logo, URL, Ghi chú web).
  - *Tầng 3 (Tài khoản chi tiết):* Quản lý chuyên sâu Username, Password, SubGroup, Ghi chú và Lịch sử thời gian.
- **Thuật toán Phân chia mục nhỏ tự động (Sub-grouping & Pagination):**
  - Tự động kích hoạt thanh phân loại con (Tiểu mục như *Tài khoản chính, Clone, VIP*) khi số lượng tài khoản trong 1 web vượt quá ngưỡng.
  - Tự động chia trang (Pagination 5 tài khoản/trang) giúp giao diện luôn gọn gàng khi quản lý hàng chục tài khoản trên cùng 1 web.
- **Bảo mật Mật khẩu & Thao tác nhanh:**
  - Ẩn/hiện mật khẩu bằng dấu sao (`••••••••••••`).
  - Nút Copy nhanh (Username, Password) vào clipboard hiển thị thông báo Toast xác nhận.
- **Bộ tạo mật khẩu mạnh (Password Generator):**
  - Cho phép tùy chỉnh độ dài từ 8 đến 32 ký tự, chọn chữ hoa, chữ thường, số và ký tự đặc biệt.

---

## [v1.0.0] - Khởi tạo nền tảng ứng dụng

### ✨ Cấu trúc hệ thống ban đầu
- Ứng dụng Single-Page Application (SPA) chạy thuần trên trình duyệt với HTML5/CSS3/JavaScript (ES6).
- Hoạt động 100% offline không phụ thuộc máy chủ, không tải thư viện external CDN để bảo đảm an toàn dữ liệu tuyệt đối và khả năng tương thích với môi trường Sandboxed iFrame.
- Giao diện tùy biến hai chế độ Sáng / Tối (Light & Dark Mode).
- Cơ chế lưu trữ tự động qua `LocalStorage` của trình duyệt.
