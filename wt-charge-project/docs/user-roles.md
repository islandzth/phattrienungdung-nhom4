# Vai trò người dùng và chức năng

## 1. Bảng vai trò

| Vai trò | Nhiệm vụ chính | Quyền hạn dự kiến |
|---|---|---|
| **Khách vãng lai** | Tìm hiểu dịch vụ, xem bảng giá và danh sách trạm | Chỉ xem thông tin công khai. Quét QR sẽ được chuyển tới trang đăng nhập rồi quay lại đúng trụ |
| **Khách hàng** | Nạp ví, sạc xe, theo dõi chi phí và lịch sử | Đọc/ghi dữ liệu **của chính mình**: hồ sơ, ví, phiên sạc, giao dịch nạp tiền. Không xem được dữ liệu người khác |
| **Quản trị viên (chủ trạm)** | Vận hành trạm: quản lý trụ, giá, người dùng, giao dịch, báo cáo | Toàn quyền nghiệp vụ quản trị. Không xem được mật khẩu người dùng. Điều chỉnh số dư (nếu làm) phải ghi lý do |
| **Hệ thống / Central System** (tác nhân kỹ thuật) | Chuyển lệnh tới trụ sạc, báo trạng thái và chỉ số công tơ | Chỉ gọi được nhóm API `internal/*`, dùng thông tin xác thực riêng |

> Có thể thêm sau: **Nhân viên vận hành** (chỉ xem trạng thái trụ và hỗ trợ khách, không xem doanh thu). Chưa làm trong phiên bản 1 để giữ phân quyền đơn giản.

## 2. Danh sách chức năng theo vai trò

| Mã | Chức năng | Người sử dụng | Mức độ ưu tiên |
|---|---|---|---|
| F01 | Đăng ký | Khách vãng lai | Bắt buộc |
| F02 | Đăng nhập / đăng xuất | Khách hàng, QTV | Bắt buộc |
| F03 | Quản lý hồ sơ, đổi mật khẩu | Khách hàng | Bắt buộc |
| F04 | Quên mật khẩu | Khách hàng | Nên có |
| F05 | Xóa tài khoản | Khách hàng | Nên có |
| F10 | Xem số dư ví | Khách hàng | Bắt buộc |
| F11 | Nạp tiền qua VNPay sandbox | Khách hàng | Bắt buộc |
| F12 | Lịch sử nạp tiền | Khách hàng | Bắt buộc |
| F20 | Quét QR / nhập mã trụ | Khách hàng | Bắt buộc |
| F21 | Xem thông tin trụ và đơn giá | Khách hàng | Bắt buộc |
| F22 | Bắt đầu sạc | Khách hàng | Bắt buộc |
| F23 | Theo dõi phiên sạc | Khách hàng | Bắt buộc |
| F24 | Dừng sạc, trừ tiền | Khách hàng, Hệ thống | Bắt buộc |
| F25 | Lịch sử sạc | Khách hàng | Bắt buộc |
| F26 | Danh sách trạm công khai | Khách vãng lai | Nên có |
| F27 | Bản đồ trạm sạc | Khách vãng lai | Nâng cao |
| F30 | Đăng nhập quản trị | QTV | Bắt buộc |
| F31 | Dashboard | QTV | Bắt buộc |
| F32 | Quản lý trụ sạc, đơn giá, in QR | QTV | Bắt buộc |
| F33 | Quản lý người dùng | QTV | Bắt buộc |
| F34 | Quản lý giao dịch | QTV | Bắt buộc |
| F35 | Xuất báo cáo CSV | QTV | Nên có |
| F36 | Điều chỉnh số dư thủ công | QTV | Nâng cao |

## 3. Ma trận phân quyền tóm tắt

| Tài nguyên | Khách vãng lai | Khách hàng | Quản trị viên |
|---|---|---|---|
| Trang giới thiệu, bảng giá, danh sách trạm | Xem | Xem | Xem |
| Hồ sơ cá nhân | — | Xem/sửa của mình | Xem tất cả, khóa/mở |
| Ví | — | Xem, nạp của mình | Xem tất cả |
| Phiên sạc | — | Tạo, dừng, xem của mình | Xem tất cả |
| Trụ sạc | Xem công khai | Xem trạng thái, đơn giá | Thêm/sửa/khóa |
| Báo cáo doanh thu | — | — | Xem, xuất |
