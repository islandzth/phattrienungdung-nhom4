# WT Charge – Nền tảng sạc xe điện thanh toán qua mã QR

> Đồ án nhóm – Học phần Phát triển ứng dụng WEB/TMĐT (UEH)
> Nhóm đề tài: **B – Ứng dụng đặt/sử dụng dịch vụ** (dịch vụ sạc xe điện trả trước qua ví điện tử)

## 1. Giới thiệu

WT Charge là hệ thống cho phép người dùng xe điện **quét mã QR trên trụ sạc → bắt đầu sạc → theo dõi lượng điện → tự động trừ tiền từ ví** mà không cần thẻ RFID hay nhân viên tại trạm. Chủ trạm quản lý trụ sạc, giá điện, người dùng và doanh thu qua trang quản trị web.

**Vấn đề kinh doanh cần giải quyết:**

- Trạm sạc tư nhân/nhỏ lẻ (khách sạn, quán cà phê, bãi giữ xe) thường không có nhân viên trực, thu tiền thủ công hoặc phải phát thẻ RFID → tốn chi phí vận hành, khó kiểm soát doanh thu.
- Người dùng xe điện phải cài nhiều app của nhiều hãng, không biết trước chi phí, không có hóa đơn/lịch sử rõ ràng.

**Giải pháp:** một luồng tự phục vụ duy nhất – *đăng nhập → nạp ví → quét QR → sạc → trừ tiền theo kWh → xem lịch sử* – cộng với trang quản trị giúp chủ trạm theo dõi trạng thái trụ và doanh thu theo thời gian thực.

Dự án kế thừa mã nguồn có sẵn:

| Thành phần | Repository | Công nghệ |
|---|---|---|
| Backend & trang quản trị | [worldtravel-charging-backend](https://github.com/vinkent-dev/worldtravel-charging-backend) | Laravel 11, Go (OCPP 1.6), NestJS, PostgreSQL |
| Ứng dụng di động | [wt-charge-app](https://github.com/islandzth/wt-charge-app) | React Native 0.73, Redux Toolkit |

Trong học phần, nhóm tập trung vào **phần WEB/App Cơ bản**: cổng web khách hàng (responsive, dùng được trên điện thoại) và trang quản trị. Ứng dụng di động là tài liệu tham chiếu nghiệp vụ, không phải phạm vi phát triển chính. Xem [REQUIREMENTS.md](REQUIREMENTS.md).

## 2. Khách hàng mục tiêu

| Nhóm | Mô tả | Nhu cầu chính |
|---|---|---|
| **Người dùng xe điện** (khách hàng chính) | Chủ xe máy/ô tô điện 22–45 tuổi tại TP.HCM, dùng điện thoại thông minh, quen thanh toán ví điện tử | Tìm được trụ trống, sạc nhanh không cần thẻ, biết trước chi phí, có lịch sử minh bạch |
| **Chủ trạm sạc nhỏ** (khách hàng B2B) | Khách sạn, khu du lịch, bãi đỗ xe có 1–20 trụ sạc | Thu tiền tự động, theo dõi doanh thu và trạng thái trụ từ xa |

## 3. Chức năng chính (phiên bản 1)

**Khách hàng (web):**
- Đăng ký, đăng nhập, quản lý hồ sơ
- Nạp tiền vào ví (VNPay sandbox), xem số dư
- Quét QR / nhập mã trụ → chọn cổng sạc → bắt đầu sạc
- Theo dõi phiên sạc (kWh, thời gian, chi phí tạm tính) → dừng sạc
- Xem lịch sử sạc và lịch sử nạp tiền

**Quản trị viên (web):**
- Dashboard: số trụ, trạng thái trụ, doanh thu, số phiên sạc
- Quản lý trụ sạc (thêm, sửa, khóa; cấu hình đơn giá/kWh)
- Quản lý người dùng, giao dịch sạc, giao dịch nạp tiền

Danh sách đầy đủ và tiêu chí nghiệm thu: [REQUIREMENTS.md](REQUIREMENTS.md).

## 4. Thành viên

| # | Họ tên | MSSV | Vai trò |
|---|---|---|---|
| 1 | Trương Anh Quân | 88241020359 | Trưởng nhóm / BA |
| 2 | Mai Hải Sơn | 87243020286 | UI/UX |
| 3 | Lê Văn Anh Tài | 88242020351 | Frontend |
| 4 | Nguyễn Thị Xuân Mai | 88242020349 | Backend / Database |
| 5 | Đinh Thị Yến Nhi | 88242020348 | QA / Integration |
| 6 | Vũ Thị Ngọc Ánh | 87243020289 | QA / Integration |

## 5. Công nghệ dự kiến

| Lớp | Công nghệ | Ghi chú |
|---|---|---|
| Frontend web | HTML, CSS (Tailwind), JavaScript, Blade (Laravel) + Alpine.js | Kế thừa giao diện quản trị có sẵn |
| Backend | PHP 8.2 + Laravel 11 (REST API, JWT) | Kế thừa |
| Giao tiếp trụ sạc | Go + OCPP 1.6 (Central System) | Kế thừa, **không sửa** trong học phần |
| Cơ sở dữ liệu | PostgreSQL 14 | |
| Thanh toán | VNPay **sandbox** | Chỉ môi trường thử nghiệm |
| Kiểm thử trụ sạc | `ocpp-simulator` (Node.js) – trụ sạc giả lập | Không cần phần cứng thật |
| Triển khai | Docker Compose | |
| Quản lý mã nguồn | GitHub (Issues, Pull Request) | |

## 6. Kế hoạch thực hiện

| Giai đoạn | Nội dung | Buổi |
|---|---|---|
| 1 | Khảo sát, xác định yêu cầu | 1–2 |
| 2 | Thiết kế giao diện (wireframe, mockup) | 3–4 |
| 3 | Xây dựng frontend | 5–7 |
| 4 | Backend & cơ sở dữ liệu | 6–9 |
| 5 | Tích hợp & kiểm thử (với trụ giả lập) | 10–11 |
| 6 | Triển khai & hoàn thiện | 12–13 |

Chi tiết: [PROJECT_PLAN.md](PROJECT_PLAN.md).

## 7. Liên kết

- Bảng yêu cầu: [REQUIREMENTS.md](REQUIREMENTS.md)
- Kế hoạch: [PROJECT_PLAN.md](PROJECT_PLAN.md)
- Quy tắc cho AI: [AGENTS.md](AGENTS.md)
- Nhật ký sử dụng AI: [AI_USAGE_LOG.md](AI_USAGE_LOG.md)
- Phân tích website: [docs/competitor-analysis.md](docs/competitor-analysis.md)
- Vai trò người dùng: [docs/user-roles.md](docs/user-roles.md)
- Quy trình nghiệp vụ: [docs/business-workflow.md](docs/business-workflow.md)
- Danh sách GitHub Issues: [docs/github-issues.md](docs/github-issues.md)
