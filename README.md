# VaultManager — Hệ Thống Quản Lý Tài Khoản Đa Tầng

Ứng dụng quản lý tài khoản/mật khẩu chạy trực tiếp trên trình duyệt, hoạt động 100% offline, không phụ thuộc máy chủ hay thư viện bên ngoài. Toàn bộ mã nguồn nằm trong một file `index.html` duy nhất — dễ dàng lưu trên USB hoặc mở trong môi trường cách ly (Sandboxed iFrame).

## Tính năng chính

- **Kiến trúc quản lý 3 tầng:** Đề mục lớn (Categories) → Trang web/Dịch vụ (Websites) → Tài khoản chi tiết (Accounts).
- **Phân chia mục nhỏ tự động:** Khi một web có nhiều tài khoản (>5), hệ thống tự động bật thanh lọc theo tiểu mục (Sub-group Tabs) và phân trang (5 tài khoản/trang).
- **Bảo mật mật khẩu:** Ẩn/hiện mật khẩu bằng dấu sao, nút copy nhanh Username/Password vào clipboard kèm thông báo Toast.
- **Trình tạo mật khẩu mạnh:** Tùy chỉnh độ dài 8–32 ký tự, chọn chữ hoa/thường/số/ký tự đặc biệt.
- **Giao diện Sáng/Tối (Light/Dark):** Chuyển đổi tức thì, lưu tùy chọn vào localStorage.
- **Chuyển đổi & Phục hồi dữ liệu JSON:**
  - Làm sạch tự động cú pháp Markdown (`[url](url)` → `url`) và HTML entities (`&amp;` → `&`).
  - Chuyển đổi từ text thô (`Website | Username | Password | Ghi chú`) sang JSON chuẩn.
  - Phục hồi thay thế toàn bộ hoặc gộp thêm vào dữ liệu hiện có.
- **Sao lưu & Phục hồi:** Xuất/nhập file `.json` backup.
- **Xuất Notepad tự động:** Mỗi lần thêm/sửa tài khoản, tự động tải file `accounts_notepad.txt` chứa toàn bộ danh sách (chống trùng lặp theo ID).

## Cách sử dụng

1. Tải file `index.html` về máy.
2. Mở trực tiếp bằng trình duyệt (Chrome, Edge, Firefox...).
3. Dữ liệu được lưu tự động trong `localStorage` của trình duyệt.

### Phím tắt & thao tác

| Thao tác | Mô tả |
|----------|-------|
| `+ Thêm Tài Khoản` | Thêm tài khoản mới vào một trang web |
| `+ Thêm Web` | Thêm trang web/dịch vụ mới |
| `Thêm Đề Mục Lớn` | Thêm danh mục lớn (Sidebar) |
| `⚡ Tạo mật khẩu` | Mở trình tạo mật khẩu mạnh |
| `🔄 Chuyển đổi & Phục hồi JSON` | Mở bộ công cụ chuyển đổi/làm sạch dữ liệu |
| `Sao lưu` | Xuất file `.json` backup toàn bộ |
| `Tải file JSON` | Nhập file `.json` để phục hồi |

## Cấu trúc dữ liệu (JSON Schema v1.0)

```json
{
  "version": "1.0",
  "exportedAt": "2026-07-05T14:39:19.546Z",
  "categories": [
    { "id": "cat-1", "name": "Mạng Xã Hội", "icon": "💬", "desc": "Mô tả" }
  ],
  "websites": [
    { "id": "web-1", "categoryId": "cat-1", "name": "Facebook", "icon": "📘", "url": "https://...", "note": "Ghi chú" }
  ],
  "accounts": [
    { "id": "acc-1", "webId": "web-1", "user": "email@...", "pass": "*****", "subGroup": "Tài khoản chính", "note": "...", "createdAt": "...", "updatedAt": "..." }
  ]
}
```

## Cấu trúc thư mục

```
.
├── index.html              # File ứng dụng chính (Single-file HTML App)
├── CHANGELOG (1).md        # Lịch sử thay đổi theo phiên bản
├── REQUIREMENTS (1).md     # Tài liệu đặc tả yêu cầu & kiến trúc
├── README.md               # Tài liệu này
└── backup/                 # Bản sao lưu file HTML cũ
    ├── HỆ THỐNG QUẢN LÝ TÀI KHOẢN-index.html
    └── HỆ THỐNG QUẢN LÝ TÀI KHOẢN-indexv2.html
```

## Lưu ý cho nhà phát triển

- Không đổi tên khóa `localStorage` (`vault_categories`, `vault_websites`, `vault_accounts`, `vault_theme`) để tránh mất dữ liệu người dùng cũ.
- Sử dụng inline SVG hoặc Unicode Emoji, không dùng CDN ngoài.
- Giữ triết lý 1 file duy nhất (`index.html`) để dễ mang theo và lưu trữ.

## Phiên bản

- **v1.2.0** — Tích hợp bộ công cụ chuyển đổi & làm sạch dữ liệu JSON, xuất Notepad tự động.
- **v1.1.0** — Hoàn thiện kiến trúc đa tầng, phân trang và tiểu mục tự động.
- **v1.0.0** — Khởi tạo nền tảng ứng dụng.

## Giấy phép

Dự án cá nhân — sử dụng tự do.
