# Project Instructions for AI

## 1. Project context

Đây là đồ án sinh viên học phần Phát triển ứng dụng WEB/TMĐT (UEH): **WT Charge**, nền tảng web cho phép khách hàng quét QR trên trụ sạc xe điện để sạc và thanh toán bằng ví trả trước.

Kiến trúc kế thừa:

| Thành phần | Công nghệ | Quyền của AI |
|---|---|---|
| `backend/`: Laravel 11 (web khách hàng, quản trị, REST API) | PHP 8.2, Blade, Tailwind, Alpine.js | **Được sửa** theo Issue |
| `ws/`: Central System OCPP 1.6 | Go | **Chỉ đọc**, không sửa |
| `appWs/`, `app-backend/` (NestJS) | Go, TypeScript | Chỉ đọc |
| `ocpp-simulator/`: trụ sạc giả lập | Node.js | Được sửa kịch bản kiểm thử |
| Cơ sở dữ liệu | PostgreSQL 14 | Chỉ thay đổi qua migration Laravel |

Tài liệu nghiệp vụ: `REQUIREMENTS.md`, `docs/business-workflow.md`, `docs/user-roles.md`.

## 2. Scope

- Chỉ triển khai các chức năng đã được nhóm thống nhất trong `REQUIREMENTS.md` (mã F01–F36).
- Ưu tiên chức năng mức **Bắt buộc**. Không tự thêm chức năng ngoài danh sách (bản đồ, chatbot, gợi ý, đặt chỗ...).
- Mỗi thay đổi phải gắn với một mã chức năng hoặc một GitHub Issue.

## 3. Coding principles

- Giữ mã nguồn dễ đọc và dễ bảo trì; theo chuẩn PSR-12 và cấu trúc thư mục mặc định của Laravel.
- Kiểm tra dữ liệu đầu vào bằng Form Request; không viết SQL nối chuỗi.
- Mọi thao tác cộng/trừ ví phải nằm trong `DB::transaction` có khóa dòng (`lockForUpdate`).
- Tiền lưu dạng số nguyên (đồng), không dùng float.
- Không thay đổi kiến trúc lớn khi chưa được nhóm đồng ý.
- Không tự ý xóa chức năng đang hoạt động.
- Ưu tiên thay đổi nhỏ, có thể kiểm tra.

## 4. Workflow

Trước khi sửa mã nguồn:
1. Đọc các tài liệu liên quan (REQUIREMENTS, workflow, Issue).
2. Phân tích hiện trạng mã nguồn.
3. Đề xuất phương án.
4. Chờ nhóm xác nhận.

Sau khi sửa mã nguồn:
1. Giải thích các thay đổi.
2. Nêu các file đã sửa.
3. Hướng dẫn chạy và kiểm thử (gồm cả kịch bản `ocpp-simulator` nếu liên quan đến sạc).
4. Báo cáo các giới hạn còn tồn tại.

## 5. Security

- Không đưa mật khẩu, API key, token, khóa bí mật VNPay vào mã nguồn; chỉ đọc từ `.env`. Chỉ commit `.env.example` với giá trị giả.
- Không tự ý vô hiệu hóa kiểm tra bảo mật (CSRF, middleware `jwt`, `basic_auth`, phân quyền admin).
- Không sử dụng dữ liệu cá nhân thật khi chưa được phép; dữ liệu demo dùng seeder với thông tin giả.
- Ví chỉ được cộng tiền qua IPN đã xác thực chữ ký, không bao giờ qua return URL.
- Nếu phát hiện bí mật trong mã kế thừa, báo ngay cho nhóm, không sao chép bí mật đó sang chỗ khác.

## 6. Communication

- Nếu yêu cầu chưa rõ, hãy hỏi lại trước khi triển khai.
- Trả lời bằng tiếng Việt; tên biến, hàm, commit message bằng tiếng Anh.
