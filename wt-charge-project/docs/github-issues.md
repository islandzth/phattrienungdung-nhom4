# GitHub Issues ban đầu

Copy từng mục dưới đây vào **Issues → New issue** trên GitHub.

| Issue | Nội dung | Người phụ trách | Nhãn |
|---|---|---|---|
| #1 | Khảo sát 3 nền tảng sạc xe điện tương tự | TV2 | docs |
| #2 | Xác định yêu cầu chức năng và phạm vi | TV1 | docs |
| #3 | Mô tả quy trình nghiệp vụ sạc xe bằng QR | TV4 | docs |
| #4 | Phản biện yêu cầu bằng AI thứ hai | TV5 | docs, ai |
| #5 | Rà soát mã kế thừa, loại bỏ bí mật, chạy Docker + trụ giả lập | TV4 | setup, security |
| #6 | Xác định công nghệ và phân tích giao diện tham khảo cho wireframe | TV3 | design |

---

## #1 Khảo sát 3 nền tảng sạc xe điện tương tự

## Mục tiêu
Phân tích V-GREEN, EBOOST, VETC Trạm sạc để rút ra chức năng phù hợp cho WT Charge.

## Công việc
- Cài app hoặc mở web từng nền tảng, khảo sát trang chủ.
- Khảo sát cách tìm trạm và chi tiết trụ sạc.
- Khảo sát quy trình kích hoạt sạc và thanh toán.
- Chụp ảnh màn hình, lưu vào `docs/images/`.
- Sửa các ô đánh dấu ⚠ trong bảng so sánh.

## Kết quả cần nộp
docs/competitor-analysis.md

## Tiêu chí hoàn thành
- Có tối thiểu 3 nền tảng, có đường dẫn.
- Có bảng so sánh, không còn dấu ⚠.
- Có ít nhất 3 ảnh minh họa.
- Có đề xuất và lý do lựa chọn.

---

## #2 Xác định yêu cầu chức năng và phạm vi

## Mục tiêu
Chốt danh sách chức năng phiên bản 1 sau khi tổng hợp kết quả AI và ý kiến nhóm.

## Công việc
- Đọc bản nháp REQUIREMENTS.md.
- Họp nhóm, thống nhất các mục "cần xác minh".
- Cập nhật tiêu chí nghiệm thu cho từng chức năng Bắt buộc.

## Kết quả cần nộp
REQUIREMENTS.md, docs/user-roles.md

## Tiêu chí hoàn thành
- Mọi chức năng có mã, người dùng, ưu tiên, tiêu chí nghiệm thu.
- Có mục Ngoài phạm vi kèm lý do.
- Cả 5 thành viên đã đồng ý (approve PR).

---

## #3 Mô tả quy trình nghiệp vụ sạc xe bằng QR

## Mục tiêu
Mô tả đầy đủ luồng quét QR → sạc → trừ tiền, kèm các ngoại lệ.

## Công việc
- Đọc luồng trong mã kế thừa (`PendingChargeController`, `TransactionController`, Central System).
- Vẽ sequence diagram và state diagram.
- Liệt kê ngoại lệ (mất kết nối, bấm 2 lần, hết tiền).

## Kết quả cần nộp
docs/business-workflow.md

## Tiêu chí hoàn thành
- Có ít nhất 1 quy trình chính dạng danh sách bước + sơ đồ.
- Sơ đồ hiển thị đúng trên GitHub.

---

## #4 Phản biện yêu cầu bằng AI thứ hai

## Mục tiêu
Dùng một công cụ AI khác Claude để phản biện bản yêu cầu.

## Công việc
- Chạy Prompt 2 trong AI_USAGE_LOG.md, đính kèm tài liệu.
- Điền bảng tổng hợp quyết định.
- Trình bày tại buổi họp nhóm.

## Kết quả cần nộp
AI_USAGE_LOG.md (Lần sử dụng 2)

## Tiêu chí hoàn thành
- Ghi rõ tên công cụ, prompt, kết quả.
- Bảng quyết định có ít nhất 8 dòng, mỗi dòng có lý do.

---

## #5 Rà soát mã kế thừa, loại bỏ bí mật, chạy Docker + trụ giả lập

## Mục tiêu
Có môi trường chạy local an toàn để đưa lên repo Public.

## Công việc
- Sao chép mã cần dùng từ repo gốc, **loại bỏ** `.env*`, file backup, file chứa mật khẩu, `full_log.txt`.
- Tạo `.env.example` với giá trị giả.
- Chạy `docker-compose up` và `ocpp-simulator`, ghi hướng dẫn.

## Kết quả cần nộp
Thư mục mã nguồn + `docs/setup.md`

## Tiêu chí hoàn thành
- `git log` không chứa bí mật (TV5 kiểm tra lại).
- Một thành viên khác làm theo hướng dẫn và chạy được.

---

## #6 Xác định công nghệ và phân tích giao diện tham khảo

## Mục tiêu
Chốt thư viện frontend (Tailwind, Alpine.js, thư viện quét QR trên trình duyệt) và phong cách giao diện.

## Kết quả cần nộp
Mục 5 trong README.md, ghi chú trong docs/

## Tiêu chí hoàn thành
- Đã thử quét QR trên ít nhất 1 iPhone và 1 Android.
