# Requirements – WT Charge

## 1. Mục tiêu dự án

### 1.1 Trả lời 6 câu hỏi xác định đề tài (Nhiệm vụ 1)

| # | Câu hỏi | Trả lời |
|---|---|---|
| 1 | Kinh doanh sản phẩm/dịch vụ gì? | **Dịch vụ sạc xe điện trả trước**: người dùng nạp tiền vào ví, quét QR trên trụ để sạc, hệ thống trừ tiền theo số kWh tiêu thụ. |
| 2 | Khách hàng mục tiêu? | (a) Người dùng xe máy/ô tô điện tại TP.HCM, 22–45 tuổi, quen thanh toán điện tử. (b) Chủ trạm sạc nhỏ: khách sạn, khu du lịch, bãi giữ xe. |
| 3 | Khách hàng gặp vấn đề gì? | Trạm nhỏ không có nhân viên → thu tiền thủ công, phải phát thẻ RFID. Người dùng không biết trước chi phí, không có lịch sử/hóa đơn, phải cài nhiều app khác nhau. |
| 4 | Website giải quyết thế nào? | Luồng tự phục vụ: quét QR → sạc → tự trừ ví → lưu lịch sử. Chủ trạm xem trạng thái trụ, phiên sạc và doanh thu trên trang quản trị. |
| 5 | Điểm khác biệt? | **Không cần cài app**: chạy trên trình duyệt điện thoại, quét QR là dùng được. Không cần thẻ RFID. Hiện đơn giá và chi phí tạm tính theo thời gian thực. Nhắm vào trạm nhỏ, độc lập, không thuộc hãng xe nào. |
| 6 | Chức năng bắt buộc của phiên bản đầu? | Đăng ký/đăng nhập, nạp ví, quét QR/nhập mã trụ, bắt đầu/dừng sạc, trừ tiền theo kWh, lịch sử; quản trị trụ sạc, đơn giá, người dùng, giao dịch. |

### 1.2 Mục tiêu kinh doanh

- Cho phép trạm sạc nhỏ **vận hành không cần nhân viên**, thu tiền 100% qua ví.
- Người dùng hoàn tất một phiên sạc (từ lúc quét QR đến lúc sạc) trong **dưới 60 giây**.
- Mọi phiên sạc đều có bản ghi: thời gian, kWh, đơn giá, số tiền.

## 2. Phạm vi

### 2.1 Trong phạm vi (học phần này)

- Cổng web khách hàng, responsive (ưu tiên màn hình điện thoại).
- Trang quản trị web cho chủ trạm.
- REST API Laravel phục vụ hai cổng trên.
- Tích hợp **VNPay sandbox** để nạp ví.
- Kiểm thử luồng sạc với **trụ sạc giả lập** (`ocpp-simulator`).

### 2.2 Ngoài phạm vi

| Hạng mục | Lý do |
|---|---|
| Sửa Central System (Go/OCPP 1.6) | Có sẵn, chạy ổn định; giao thức phần cứng vượt quá nội dung học phần |
| Phát triển ứng dụng di động React Native | Học phần là phát triển ứng dụng WEB; app chỉ dùng để tham chiếu nghiệp vụ |
| Kết nối trụ sạc thật, thanh toán thật | Cần phần cứng và hợp đồng cổng thanh toán; dùng giả lập và sandbox |
| Đặt chỗ trước (booking trụ theo giờ) | Cần xử lý giữ chỗ, hủy, phạt; để giai đoạn sau |
| Bản đồ trạm sạc theo GPS | Phụ thuộc API bản đồ có phí; để nâng cao |
| Hoàn tiền về tài khoản ngân hàng, xuất hóa đơn VAT điện tử | Nghiệp vụ pháp lý phức tạp |
| Chatbot AI, gợi ý trạm thông minh | Không giải quyết vấn đề cốt lõi |

## 3. Người dùng

| Vai trò | Mô tả | Quyền hạn |
|---|---|---|
| Khách vãng lai | Chưa đăng nhập | Xem trang giới thiệu, bảng giá, danh sách trạm công khai; quét QR sẽ được chuyển tới đăng nhập |
| Khách hàng | Đã đăng ký tài khoản | Quản lý hồ sơ, ví, phiên sạc và lịch sử **của chính mình** |
| Quản trị viên | Chủ trạm / người vận hành | Quản lý trụ sạc, đơn giá, người dùng, giao dịch; xem báo cáo |

Chi tiết: [docs/user-roles.md](docs/user-roles.md).

## 4. Yêu cầu chức năng

Mức ưu tiên: **Bắt buộc** (phiên bản 1) · **Nên có** (làm nếu kịp) · **Nâng cao** (giai đoạn sau).

### 4.1 Tài khoản

| Mã | Chức năng | Mô tả | Người dùng | Ưu tiên | Tiêu chí nghiệm thu |
|---|---|---|---|---|---|
| F01 | Đăng ký | Đăng ký bằng email, mật khẩu, họ tên, số điện thoại | Khách vãng lai | Bắt buộc | Email trùng hoặc sai định dạng bị từ chối và báo lỗi; mật khẩu ≥ 8 ký tự; tạo tài khoản kèm ví số dư 0 đ |
| F02 | Đăng nhập / đăng xuất | Đăng nhập bằng email + mật khẩu, nhận JWT | Khách hàng, QTV | Bắt buộc | Sai thông tin báo lỗi chung (không nói rõ sai email hay mật khẩu); sau đăng xuất token không dùng được |
| F03 | Quản lý hồ sơ | Xem/sửa họ tên, SĐT; đổi mật khẩu | Khách hàng | Bắt buộc | Đổi mật khẩu yêu cầu nhập mật khẩu cũ đúng |
| F04 | Quên mật khẩu | Gửi link đặt lại qua email | Khách hàng | Nên có | Link hết hạn sau 60 phút |
| F05 | Xóa tài khoản | Người dùng tự yêu cầu xóa tài khoản | Khách hàng | Nên có | Chỉ cho phép khi không có phiên sạc đang chạy |

### 4.2 Ví và thanh toán

| Mã | Chức năng | Mô tả | Người dùng | Ưu tiên | Tiêu chí nghiệm thu |
|---|---|---|---|---|---|
| F10 | Xem số dư ví | Hiển thị số dư hiện tại | Khách hàng | Bắt buộc | Số dư khớp với tổng nạp − tổng đã trừ |
| F11 | Nạp tiền qua VNPay | Chọn mệnh giá nhanh (10k, 30k, 50k, 100k, 200k, 500k) hoặc nhập số tiền → chuyển tới VNPay sandbox | Khách hàng | Bắt buộc | Từ chối số tiền < 5.000 đ; ví chỉ được cộng khi **IPN** từ VNPay báo thành công và chữ ký hợp lệ; IPN gửi lặp không cộng tiền hai lần |
| F12 | Lịch sử nạp tiền | Danh sách giao dịch nạp: thời gian, số tiền, mã giao dịch, trạng thái | Khách hàng | Bắt buộc | Sắp xếp mới nhất trước; phân trang 20 dòng |

### 4.3 Sạc xe

| Mã | Chức năng | Mô tả | Người dùng | Ưu tiên | Tiêu chí nghiệm thu |
|---|---|---|---|---|---|
| F20 | Quét QR / nhập mã trụ | Quét QR bằng camera trình duyệt, hoặc nhập mã trụ thủ công. Mã QR = mã trụ + số cổng | Khách hàng | Bắt buộc | Mã không tồn tại hoặc trụ đang khóa → báo lỗi; mở được trên Chrome/Safari điện thoại |
| F21 | Xem thông tin trụ | Tên trạm, trạng thái từng cổng (Sẵn sàng / Đang sạc / Lỗi / Offline), đơn giá/kWh | Khách hàng | Bắt buộc | Trạng thái khớp với trụ giả lập trong vòng 10 giây |
| F22 | Bắt đầu sạc | Chọn cổng → kiểm tra số dư → gửi lệnh RemoteStart tới trụ | Khách hàng | Bắt buộc | Số dư < mức tối thiểu (_đề xuất 10.000 đ – cần nhóm chốt_) → không cho sạc; mỗi người chỉ có 1 phiên đang chạy; bấm 2 lần không tạo 2 phiên (idempotency key) |
| F23 | Theo dõi phiên sạc | Hiện kWh đã sạc, thời gian, chi phí tạm tính, cập nhật định kỳ | Khách hàng | Bắt buộc | Cập nhật tối thiểu 15 giây/lần (polling hoặc WebSocket) |
| F24 | Dừng sạc | Người dùng bấm dừng; hoặc hệ thống tự dừng khi chi phí chạm số dư | Khách hàng, hệ thống | Bắt buộc | Số tiền trừ = (end_meter − start_meter) × đơn giá, làm tròn đến đồng; số dư không bao giờ âm |
| F25 | Lịch sử sạc | Danh sách phiên: trạm, thời gian, kWh, đơn giá, số tiền | Khách hàng | Bắt buộc | Khớp với dữ liệu phiên trong CSDL |
| F26 | Danh sách trạm công khai | Danh sách trạm, địa chỉ, số cổng trống, đơn giá | Khách vãng lai | Nên có | Không lộ thông tin nội bộ (mã client, IP) |
| F27 | Bản đồ trạm sạc | Hiển thị trạm trên bản đồ | Khách vãng lai | Nâng cao | — |

### 4.4 Quản trị

| Mã | Chức năng | Mô tả | Người dùng | Ưu tiên | Tiêu chí nghiệm thu |
|---|---|---|---|---|---|
| F30 | Đăng nhập quản trị | Tài khoản quản trị tách riêng với khách hàng | QTV | Bắt buộc | Khách hàng không truy cập được bất kỳ URL `/admin` nào (trả 403) |
| F31 | Dashboard | Tổng số trụ, số trụ online, số phiên và doanh thu hôm nay/tháng này | QTV | Bắt buộc | Số liệu khớp với truy vấn CSDL |
| F32 | Quản lý trụ sạc | Thêm/sửa/khóa trụ; tên, địa chỉ, số cổng, đơn giá/kWh; in mã QR | QTV | Bắt buộc | Đổi đơn giá chỉ áp dụng cho phiên **bắt đầu sau** thời điểm đổi |
| F33 | Quản lý người dùng | Danh sách, tìm kiếm, xem chi tiết, khóa/mở khóa | QTV | Bắt buộc | Người dùng bị khóa không đăng nhập được |
| F34 | Quản lý giao dịch | Xem phiên sạc và giao dịch nạp tiền; lọc theo ngày, trụ, người dùng | QTV | Bắt buộc | Lọc đúng; xuất được danh sách |
| F35 | Xuất báo cáo CSV | Xuất doanh thu theo khoảng ngày | QTV | Nên có | File mở được bằng Excel, tiếng Việt không lỗi font |
| F36 | Điều chỉnh số dư thủ công | QTV cộng/trừ ví khi có khiếu nại, bắt buộc ghi lý do | QTV | Nâng cao | Mọi điều chỉnh được ghi log người thực hiện |

## 5. Yêu cầu phi chức năng

| Mã | Yêu cầu |
|---|---|
| NF01 | Giao diện responsive: dùng tốt ở chiều rộng 360px trở lên; ưu tiên thiết kế cho điện thoại trước |
| NF02 | Mọi dữ liệu đầu vào được kiểm tra ở **cả** frontend và backend (Laravel Form Request) |
| NF03 | API khách hàng yêu cầu JWT; API nội bộ giữa Central System và backend dùng xác thực riêng; trang quản trị dùng session và kiểm tra quyền |
| NF04 | Mật khẩu băm bằng bcrypt; không log mật khẩu, token |
| NF05 | **Không** commit `.env`, mật khẩu, API key, khóa VNPay vào repository; chỉ commit `.env.example` |
| NF06 | Cộng/trừ ví thực hiện trong **database transaction**, có khóa dòng, để tránh lỗi khi hai yêu cầu đến cùng lúc |
| NF07 | Thời gian phản hồi API thông thường < 1 giây trên môi trường demo |
| NF08 | Mã nguồn theo chuẩn PSR-12 (PHP) và ESLint (JS); mỗi Pull Request có ít nhất 1 người review |
| NF09 | Dữ liệu demo là dữ liệu mẫu, không dùng thông tin cá nhân thật |

## 6. Giới hạn và giả định

- **Thời gian:** khoảng 13 buổi học (một học kỳ). _[Nhóm điền ngày bắt đầu – kết thúc]_
- **Nhân lực:** 5 thành viên, mỗi người khoảng 6–8 giờ/tuần.
- **Phần cứng:** không có trụ sạc thật → dùng `ocpp-simulator` để giả lập trụ, cổng và chỉ số công tơ.
- **Thanh toán:** chỉ dùng VNPay sandbox; không xử lý tiền thật.
- **Kế thừa:** Central System (Go) và cấu trúc CSDL hiện có được giữ nguyên; nhóm chỉ thêm bảng/cột khi cần, và mọi thay đổi đều qua migration.
- **Bảo mật repo:** repo gốc có file `.env.backup*` và file lẫn mật khẩu → **không** sao chép nguyên repo gốc sang repo công khai của nhóm. Chỉ đưa mã đã rà soát.
- **Cần xác minh:** mức số dư tối thiểu để bắt đầu sạc; cơ chế tự dừng khi hết tiền (backend hiện có API `max/{user_id}` – cần đọc kỹ cách tính); việc Central System gọi lại backend khi dừng sạc.
