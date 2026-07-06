# TÀI LIỆU ĐẶC TẢ YÊU CẦU & KIẾN TRÚC HỆ THỐNG (PROJECT REQUIREMENTS)
**Tên dự án:** VaultManager - App Quản Lý Tài Khoản Trình Duyệt Đa Tầng  
**Mục đích tài liệu:** Ghi lại toàn bộ yêu cầu gốc, cấu trúc dữ liệu và hướng dẫn thiết kế để các lập trình viên hoặc người phát triển kế tiếp hiểu rõ bản chất dự án, không bị "lạc đường" trong quá trình bảo trì hay mở rộng.

---

## 1. MỤC TIÊU DỰ ÁN (OBJECTIVES)
- Xây dựng một ứng dụng quản lý tài khoản/mật khẩu hoạt động **trực tiếp trên trình duyệt web** (Browser-based Application).
- Đảm bảo **tính thẩm mỹ cao (UI/UX đẹp mắt, hiện đại)**, giao diện gọn gàng, bố cục rõ ràng, hỗ trợ chế độ Giao diện Sáng/Tối (Light & Dark Theme).
- **Hoạt động tự trị (Self-contained / Offline-first):** Toàn bộ mã nguồn, style CSS và SVG Icon được nhúng trực tiếp trong 1 file `index.html`. Không phụ thuộc vào thư viện bên ngoài hay API máy chủ để bảo đảm tuyệt đối quyền riêng tư dữ liệu cho người dùng.

---

## 2. CHI TIẾT CÁC YÊU CẦU NGHIỆP VỤ (BUSINESS REQUIREMENTS)

### 2.1. Quản lý thông tin tài khoản cốt lõi
Mỗi bản ghi tài khoản cần lưu trữ các trường dữ liệu bắt buộc và tùy chọn sau:
- **Tên đăng nhập / Email (`user`):** Định danh tài khoản.
- **Mật khẩu (`pass`):** Phải được ẩn mặc định (`••••••••••••`), có nút **👁️ Bật/Tắt hiển thị** và nút **📋 Copy nhanh**.
- **Mục nhỏ / Nhóm phụ (`subGroup`):** Nhãn phân loại bên trong web (Ví dụ: *Tài khoản chính, Clone/Phụ, VIP, Khách hàng...*).
- **Ghi chú (`note`):** Ghi chú các thông tin bổ trợ như câu hỏi bảo mật, số điện thoại khôi phục, mã PIN 2FA.
- **Lịch sử thời gian:**
  - **Ngày tháng khởi tạo (`createdAt`):** Ghi nhận tự động thời điểm thêm tài khoản (định dạng ISO).
  - **Ngày tháng chỉnh sửa (`updatedAt`):** Cập nhật tự động mỗi khi người dùng thay đổi thông tin.

### 2.2. Phân cấp dữ liệu hợp lý, dễ tra cứu (Mô hình 3 tầng)
Hệ thống phải được chia thành **3 tầng phân cấp rõ ràng**:
1. **Tầng 1 - Đề mục lớn (Categories):** Các nhóm lĩnh vực lớn như *Mạng xã hội, Công việc & Doanh nghiệp, Tài chính & Ngân hàng, Hạ tầng & Server...* hiển thị ở thanh điều hướng trái (Sidebar).
2. **Tầng 2 - Trang Web / Dịch vụ (Websites):** Các dịch vụ cụ thể thuộc Đề mục lớn (Ví dụ: *Facebook, Google, Vietcombank, AWS*). Các tài khoản chung 1 trang web phải được **gom chung vào một khung rõ ràng**.
3. **Tầng 3 - Tài khoản cụ thể (Accounts):** Các tài khoản thuộc trang web đó.

### 2.3. Phân chia mục nhỏ hơn khi số lượng tài khoản nhiều
- Khi một trang web có số lượng tài khoản lớn (vượt quá ngưỡng `5 tài khoản`), hệ thống phải **tự động kích hoạt cơ chế tổ chức nhỏ gọn**:
  - **Thanh lọc theo Tiểu mục (Sub-group Tabs):** Cho phép bấm chọn lọc xem riêng từng nhóm phụ (*Tài khoản chính*, *Clone*, *Khách hàng*...).
  - **Phân trang tự động (Pagination):** Chia nhỏ danh sách thành từng trang (5 tài khoản/trang) để tránh làm bảng dữ liệu bị dài vô tận.

---

## 3. CHỨC NĂNG CHUYỂN ĐỔI & PHỤC HỒI DỮ LIỆU (CONVERTER & RECOVERY ENGINE)

Yêu cầu đặc biệt được bổ sung nhằm hỗ trợ người dùng chuyển đổi dữ liệu từ các file ghi nhớ cũ sang định dạng chuẩn:

### 3.1. Làm sạch dữ liệu tự động (Sanitation Engine)
- Dữ liệu copy từ tài liệu Rich Text hoặc Markdown thường bị dính cú pháp liên kết hoặc HTML entity lỗi. Trình chuyển đổi bắt buộc phải tự động xử lý:
  - **Markdown Links:** Chuyển đổi `[https://domain.com](https://domain.com)` thành `https://domain.com`.
  - **Mailto Links:** Chuyển đổi `[email@domain.com](mailto:email@domain.com)` thành `email@domain.com`.
  - **HTML Entities:** Chuyển đổi `&amp;` -> `&`, `&lt;` -> `<`, v.v.

### 3.2. Chuyển đổi từ Text thô (Unstructured Text Conversion)
- Hỗ trợ nhập danh sách dạng từng dòng theo quy tắc: `Tên Website | Username | Password | Ghi chú`.
- Hệ thống tự tạo Website và Tài khoản chuẩn cấu trúc JSON.

---

## 4. CẤU TRÚC DỮ LIỆU CHUẨN (STANDARD JSON SCHEMA)

Bất kỳ tính năng xuất file, sao lưu hay nhập dữ liệu nào trong tương lai đều phải tuân thủ nghiêm ngặt cấu trúc JSON `v1.0` sau:

```json
{
  "version": "1.0",
  "exportedAt": "2026-07-05T14:39:19.546Z",
  "categories": [
    {
      "id": "cat-1",
      "name": "Tên Đề Mục Lớn",
      "icon": "💬",
      "desc": "Mô tả đề mục"
    }
  ],
  "websites": [
    {
      "id": "web-1",
      "categoryId": "cat-1",
      "name": "Tên Trang Web",
      "icon": "📘",
      "url": "https://example.com",
      "note": "Ghi chú chung của web"
    }
  ],
  "accounts": [
    {
      "id": "acc-1",
      "webId": "web-1",
      "user": "username_or_email",
      "pass": "password_value",
      "subGroup": "Tài khoản chính",
      "note": "Ghi chú chi tiết",
      "createdAt": "2026-07-05T14:39:01.642Z",
      "updatedAt": "2026-07-05T14:39:01.642Z"
    }
  ]
}
```

---

## 5. HƯỚNG DẪN DÀNH CHO NHÀ PHÁT TRIỂN KẾ TIẾP (MAINTENANCE GUIDELINES)
1. **Lưu trữ tĩnh:** Không thay đổi tên khóa bộ nhớ `localStorage` (`vault_categories`, `vault_websites`, `vault_accounts`, `vault_theme`) để tránh làm mất dữ liệu của người dùng cũ khi cập nhật app.
2. **Thêm icon/hình ảnh:** Hãy sử dụng **inline SVG** hoặc **Unicode Emojis**. Tuyệt đối không dùng URL CDN ngoại tuyến vì người dùng có thể mở app trong môi trường không có kết nối Internet hoặc iFrame cách ly.
3. **Mã nguồn:** Toàn bộ logic ứng dụng hiện nằm gọn trong `index.html`. Nếu mở rộng lên hàng ngàn tài khoản, có thể cân nhắc tích hợp Virtual Scroll nhưng vẫn nên giữ nguyên triết lý **1 file duy nhất (Single-file HTML Application)** để người dùng dễ dàng mang theo trên USB hoặc lưu trữ cá nhân.
