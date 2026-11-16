# Phân tích các nền tảng tương tự

> **Trạng thái:** BẢN NHÁP. Các ô ghi nhận dưới đây lấy từ trang giới thiệu chính thức, trang ứng dụng trên App Store và báo chí (xem mục 4). Thành viên phụ trách **phải tự cài app/mở web, chụp ảnh màn hình và sửa lại** những ô có đánh dấu ⚠ trước khi nộp.

## 1. Lựa chọn đối tượng khảo sát

Trong lĩnh vực sạc xe điện ở Việt Nam, hầu hết nhà vận hành **dùng app di động, không có website giao dịch**. Đây chính là khoảng trống đề tài nhắm tới. Nhóm chọn 3 nền tảng đại diện cho 3 mô hình:

| # | Nền tảng | Mô hình | Đường dẫn |
|---|---|---|---|
| A | **V-GREEN / ứng dụng VinFast** | Mạng lưới lớn, gắn với một hãng xe | https://vgreen.net · app VinFast |
| B | **EBOOST Car** | Nhà vận hành độc lập, nhiều hãng xe; đã tích hợp vào Grab Driver | https://eboost.vn · [App Store](https://apps.apple.com/bt/app/eboost-car/id1609222766) |
| C | **VETC – Trạm sạc** | Siêu ứng dụng gom nhiều nhà vận hành vào một chỗ (tìm trạm, kích hoạt, thanh toán) | https://vetc.com.vn · app VETC |

## 2. Bảng so sánh

| Tiêu chí | A. V-GREEN / VinFast | B. EBOOST Car | C. VETC Trạm sạc | Đề xuất cho WT Charge |
|---|---|---|---|---|
| **Trang chủ** | Web giới thiệu doanh nghiệp, tin tức, chương trình nhượng quyền; giao dịch nằm trong app ⚠ | Web giới thiệu; trong app, màn hình chính là bản đồ trạm ⚠ | Trạm sạc là một tính năng trong app VETC đa dịch vụ ⚠ | Trang chủ web gồm 2 nút lớn: **Quét QR để sạc** và **Nạp ví**. Phần dưới hiển thị số dư và phiên đang chạy (nếu có). Không làm trang tin tức |
| **Danh mục (trạm/trụ)** | Phân loại theo loại xe (ô tô/xe máy) và công suất AC/DC ⚠ | Bản đồ, bộ lọc, hoặc tìm bằng mã QR | Bản đồ gom trạm của nhiều nhà vận hành ⚠ | Phiên bản 1: danh sách trạm (tên, địa chỉ, số cổng trống, đơn giá). **Chưa** làm bản đồ (F27 nâng cao) vì mạng lưới nhỏ và API bản đồ tốn phí |
| **Tìm kiếm và lọc** | Lọc theo vị trí và loại sạc ⚠ | Lọc trạm trên bản đồ | Tìm trạm gần, dẫn đường | Tìm theo tên/địa chỉ, lọc **còn cổng trống**. Đủ cho quy mô dưới 20 trạm |
| **Chi tiết (trụ sạc)** | Công suất, số cổng, trạng thái ⚠ | Hiện phí trước khi xác nhận sạc | Trạng thái và giá theo nhà vận hành ⚠ | Hiện **đơn giá/kWh trước khi bấm sạc**, trạng thái từng cổng và số dư ví. Đây là điểm người dùng quan tâm nhất (minh bạch chi phí) |
| **Đặt dịch vụ & thanh toán** | Kích hoạt trong app; có tính **phí đỗ quá giờ** sau khi sạc đầy (1.000 đ/phút tại trạm DC) | Kiểm tra phí → xác nhận → sạc → xác nhận thanh toán khi xong | Kích hoạt và thanh toán trong một app. Mô hình Grab: quét QR rồi thanh toán qua app ngân hàng, không cần nạp trước | **Ví trả trước** (nạp qua VNPay) → quét QR → sạc → tự trừ ví. Chọn ví vì tự dừng được khi hết tiền và đối soát đơn giản |
| **Điểm có thể cải thiện** | Gắn với hệ sinh thái VinFast; bắt buộc cài app | Bắt buộc cài app; mỗi nhà vận hành một app | Phụ thuộc nhà vận hành có tích hợp hay không; trạm nhỏ khó tham gia | **Không cần cài app**: quét QR mở web ngay. Hợp với trạm nhỏ độc lập, khách vãng lai, khách du lịch |

## 3. Đánh giá: học gì, không làm gì, vì sao

### Đáng học hỏi

1. **Hiện phí trước khi sạc** (EBOOST): giải quyết trực tiếp vấn đề "không biết trước chi phí" → đưa vào F21 và F22.
2. **Tìm trạm bằng mã QR** (EBOOST): QR là điểm vào tự nhiên nhất ngay tại trụ → trở thành luồng chính (F20) của WT Charge.
3. **Một nơi để tìm, kích hoạt và thanh toán** (VETC): khách không phải chuyển qua nhiều màn hình → toàn bộ luồng sạc nằm gọn trong 3 màn hình (Thông tin trụ → Đang sạc → Tóm tắt).

### Chưa phù hợp với đề tài

| Tính năng | Lý do không làm trong phiên bản 1 |
|---|---|
| Bản đồ toàn quốc, dẫn đường | Mạng lưới nhỏ; API bản đồ tốn phí; không phải vấn đề cốt lõi của trạm nhỏ |
| Phí đỗ quá giờ (V-GREEN) | Hợp lý với trạm DC đông khách, nhưng cần dữ liệu thời điểm rút súng sạc chính xác. Ghi vào danh sách nâng cao |
| Thanh toán trả sau qua app ngân hàng (mô hình Grab) | Không biết trước số tiền nên không tự dừng được khi khách thiếu tiền; rủi ro nợ xấu với trạm nhỏ |
| Tích hợp nền tảng bên thứ ba (Grab, VETC) | Cần hợp tác thương mại, vượt phạm vi học phần |

### Lý do chọn phương án

- **Web thay vì app:** khách vãng lai ở khách sạn hoặc khu du lịch thường không muốn cài app chỉ để sạc một lần. Quét QR là dùng được ngay, chi phí phát triển cũng thấp hơn làm 2 app iOS/Android.
- **Ví trả trước:** backend gốc đã có mô hình ví (`wallets`) và cổng thanh toán. Hệ thống có thể chặn sạc khi số dư không đủ và tự dừng khi hết tiền.

## 4. Nguồn tham khảo

- EBOOST Car – App Store: https://apps.apple.com/bt/app/eboost-car/id1609222766
- EBOOST nhận đầu tư chiến lược từ Grab – VnEconomy: https://vneconomy.vn/eboost-nhan-dau-tu-chien-luoc-tu-grab-thuc-day-mo-rong-mang-luoi-tram-sac-xe-dien-tai-viet-nam.htm
- Grab – Thanh toán phí sạc xe điện: https://www.grab.com/vn/blog/driver/vanhanh-thanhtoansac/
- V-Green điều chỉnh phí đỗ quá giờ – DNSE: https://www.dnse.com.vn/senses/tin-tuc/v-green-thong-bao-dieu-chinh-thoi-gian-tinh-phi-do-qua-gio-voi-tai-xe-xanh-sm-tai-tram-sac-xe-dien-vinfast-35117565
- Giá sạc ô tô điện tại Việt Nam – Báo Hà Tĩnh: https://baohatinh.vn/sac-oto-dien-tai-viet-nam-ton-bao-nhieu-tien-post290377.html
- Người dùng xe điện loay hoay với nhiều ứng dụng – Viettimes: https://viettimes.vn/ha-tang-sac-mo-rong-nguoi-dung-xe-dien-van-loay-hoay-voi-ung-dung-va-loi-ky-thuat-post193263.html
- VETC đưa trụ sạc về một mối – VnReview: https://vnreview.vn/threads/vetc-dua-350-tru-sac-o-to-dien-ve-mot-moi-sau-1-thang-ra-mat.88281/

## 5. Ảnh minh họa

_[Thành viên phụ trách chèn ảnh chụp màn hình vào `docs/images/` và tham chiếu tại đây: màn hình chính, màn hình chi tiết trụ, màn hình thanh toán của mỗi nền tảng.]_
