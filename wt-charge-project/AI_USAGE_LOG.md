# AI Usage Log

## Lần sử dụng 1 – Phân tích yêu cầu (AI thứ nhất)

### Công cụ
Tên công cụ AI: **Claude (Anthropic) – Claude Code**, mô hình Claude Opus 5.5

### Mục đích
Đọc đề bài Chương 1, đọc mã nguồn 2 repository kế thừa (backend, app), từ đó phân tích nghiệp vụ và soạn bản nháp bộ tài liệu khởi tạo.

### Prompt
Yêu cầu ban đầu của nhóm: _"Đọc NHOM/DE_BAI BaiTap_Nhom_Chương1.pdf và mota_wtcharge để làm bài"_, kèm thông tin dự án theo mẫu Prompt 1 của đề:

```
Bạn là chuyên gia phân tích nghiệp vụ cho dự án thương mại điện tử.
Thông tin dự án:
- Tên dự án: WT Charge
- Sản phẩm hoặc dịch vụ: Dịch vụ sạc xe điện trả trước, quét QR trên trụ để sạc, trừ tiền ví theo kWh
- Khách hàng mục tiêu: Người dùng xe điện tại TP.HCM; chủ trạm sạc nhỏ (khách sạn, bãi xe)
- Vấn đề kinh doanh: Trạm nhỏ không có nhân viên, thu tiền thủ công/thẻ RFID; người dùng không biết trước chi phí
- Website tham khảo: vgreen.net, eboost.vn, vetc.com.vn
- Mã nguồn kế thừa: worldtravel-charging-backend, wt-charge-app
- Thời gian thực hiện: ~13 buổi
- Quy mô nhóm: 5 thành viên
Hãy đề xuất: (1) mục tiêu, phạm vi (2) nhóm người dùng, quyền hạn (3) danh sách chức năng
(4) quy trình nghiệp vụ chính (5) yêu cầu phi chức năng (6) chức năng phiên bản đầu
(7) chức năng nâng cao (8) rủi ro và cách giảm thiểu.
Phân biệt rõ: yêu cầu bắt buộc / có thể phát triển sau / cần nhóm xác minh thêm.
Không giả định nhóm có đủ thời gian hoặc năng lực để triển khai tất cả chức năng.
Hãy giải thích cơ sở của từng đề xuất.
```

### Kết quả đề xuất
- Xác định hệ thống kế thừa gồm: Central System Go (OCPP 1.6), backend Laravel (quản trị, ví, VNPay/AppotaPay), NestJS (API mobile), app React Native (QR, start/stop, nạp ví).
- Đề xuất phạm vi học phần: **cổng web khách hàng + trang quản trị**, không sửa Central System, không làm app di động, dùng `ocpp-simulator` thay trụ thật, VNPay sandbox.
- Soạn bản nháp: README, REQUIREMENTS (F01–F36, NF01–NF09), vai trò người dùng, 3 quy trình nghiệp vụ, phân tích 3 nền tảng, kế hoạch, rủi ro, AGENTS.md.
- Cảnh báo: repo gốc chứa file `.env.backup*` và một file có tên lẫn mật khẩu admin → không được đưa nguyên repo lên Public.

### Nội dung nhóm cần kiểm tra / điều chỉnh / loại bỏ
| Nội dung AI đề xuất | Đánh giá ban đầu của nhóm |
|---|---|
| Làm web thay vì app | _[Nhóm thảo luận – có phù hợp yêu cầu giảng viên?]_ |
| Mức số dư tối thiểu 10.000 đ để bắt đầu sạc | Cần xác minh trong mã (`max/{user_id}`) |
| Các ghi nhận về đối thủ (đánh dấu ⚠) | Phải tự kiểm tra trên app thật và chụp ảnh |
| Phí đỗ quá giờ, bản đồ, đặt chỗ | Đã đưa vào Ngoài phạm vi / Nâng cao |
| Tiến độ 13 buổi | Điều chỉnh theo lịch học thực tế |

### Phản biện
Xem Lần sử dụng 2.

### Quyết định của nhóm
_[Ghi sau buổi thảo luận nhóm]_

### Người kiểm tra
_[TV1 – Trưởng nhóm]_

---

## Lần sử dụng 2 – Phản biện yêu cầu (AI thứ hai)

### Công cụ
Tên công cụ AI: _[Ví dụ: ChatGPT / Gemini / Copilot – phải khác công cụ ở Lần 1]_

### Mục đích
Đánh giá độc lập bản yêu cầu do AI thứ nhất đề xuất.

### Prompt
Dán nguyên văn prompt dưới đây, đính kèm nội dung `REQUIREMENTS.md`, `docs/competitor-analysis.md`, `docs/business-workflow.md`:

```
Bạn là người phản biện độc lập cho một dự án website thương mại điện tử
do nhóm sinh viên thực hiện trong một học phần.

Tôi sẽ cung cấp:
1. Mô tả đề tài: WT Charge – web cho phép quét QR trên trụ sạc xe điện để sạc, trả tiền bằng ví trả trước (nạp qua VNPay sandbox). Kế thừa backend Laravel + Central System OCPP có sẵn; dùng trụ giả lập.
2. Vấn đề kinh doanh và khách hàng mục tiêu: (đính kèm REQUIREMENTS.md mục 1)
3. Kết quả khảo sát ba nền tảng: V-GREEN, EBOOST, VETC (đính kèm competitor-analysis.md)
4. Danh sách yêu cầu do một công cụ AI khác đề xuất: (đính kèm REQUIREMENTS.md mục 2–6)

Hãy phản biện các yêu cầu theo những khía cạnh:
- Có thực sự giải quyết vấn đề kinh doanh không?
- Có yêu cầu nào trùng lặp hoặc không cần thiết không?
- Có chức năng nào vượt quá phạm vi của nhóm 5 sinh viên trong ~13 buổi không?
- Có thiếu chức năng cơ bản hoặc quy trình nghiệp vụ nào không?
- Có rủi ro về dữ liệu, bảo mật hoặc quyền riêng tư không?
- Có yêu cầu nào chưa đủ rõ để lập trình và kiểm thử không?

Với mỗi vấn đề, hãy nêu lý do và đề xuất điều chỉnh.
Không mặc định rằng phương án của công cụ AI trước là đúng.
Không tự ý thay đổi toàn bộ đề tài.
```

### Kết quả phản biện
_[Tóm tắt kết quả AI thứ hai]_

### Bảng tổng hợp quyết định

| Nội dung | AI thứ nhất đề xuất | AI thứ hai phản biện | Quyết định của nhóm |
|---|---|---|---|
| Web responsive thay vì app di động | Có – phù hợp học phần WEB, không cần cài app | _[...]_ | _[...]_ |
| Quét QR / nhập mã trụ (F20) | Có – Bắt buộc | _[...]_ | _[...]_ |
| Ví trả trước + VNPay sandbox (F10–F12) | Có – Bắt buộc, cộng ví chỉ qua IPN | _[...]_ | _[...]_ |
| Bắt đầu/dừng sạc, tự dừng khi hết tiền (F22–F24) | Có – Bắt buộc | _[...]_ | _[...]_ |
| Theo dõi phiên thời gian thực (F23) | Có – polling 15 giây/lần | _[...]_ | _[...]_ |
| Quản trị trụ sạc, đơn giá, in QR (F32) | Có – Bắt buộc | _[...]_ | _[...]_ |
| Bản đồ trạm sạc (F27) | Nâng cao | _[...]_ | _[...]_ |
| Phí đỗ quá giờ | Ngoài phạm vi | _[...]_ | _[...]_ |
| Điều chỉnh số dư thủ công (F36) | Nâng cao | _[...]_ | _[...]_ |
| Không sửa Central System Go | Có | _[...]_ | _[...]_ |

### Quyết định của nhóm
_[Nội dung được giữ lại, điều chỉnh hoặc loại bỏ, kèm lý do. Sau khi thống nhất, cập nhật REQUIREMENTS.md.]_

### Người kiểm tra
_[TV5 – QA]_
