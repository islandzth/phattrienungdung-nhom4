# History – Lịch sử làm bài và cách tư duy

> File này ghi lại **đã làm gì, vì sao làm vậy, còn dở gì** để một mô hình AI khác (hoặc thành viên mới) đọc là tiếp tục được ngay, không phải dò lại từ đầu.
> Đọc file này trước, sau đó đọc `AGENTS.md`, rồi `REQUIREMENTS.md`.

---

## 0. Bối cảnh nhanh

| Mục | Giá trị |
|---|---|
| Học phần | Phát triển ứng dụng WEB/TMĐT – UEH – TS Nguyễn Mạnh Tuấn |
| Bài | Bài tập nhóm **Chương 1**: khởi tạo đồ án, thống nhất phạm vi (chưa cần code) |
| Đề bài gốc | `../DE_BAI/BaiTap_Nhom_Chuong1.pdf` (24 trang) |
| Mô tả dự án của nhóm | `../DE_BAI/mota_wtcharge.md` |
| Đề tài | **WT Charge** – sạc xe điện, quét QR trên trụ, thanh toán bằng ví trả trước |
| Repo nguồn kế thừa | Backend: `github.com/vinkent-dev/worldtravel-charging-backend` (nhánh `from-server`) · App: `github.com/islandzth/wt-charge-app` |
| Thư mục sản phẩm | `NHOM/wt-charge-project/` (sẽ push lên repo GitHub Public) |

---

## Phiên 1 – 2026-11-16 – Claude Opus 5.5 (Claude Code trong VS Code)

### Yêu cầu của người dùng
> "đọc NHOM/DE_BAI BaiTap_Nhom_Chương1.pdf và mota_wtcharge để làm bài"

### Bước 1 – Đọc đề, tách ra danh sách việc phải nộp
Đề có 8 nhiệm vụ, sản phẩm nộp (mục 11 của PDF):
`README.md`, `REQUIREMENTS.md`, `PROJECT_PLAN.md`, `AGENTS.md`, `AI_USAGE_LOG.md`, `docs/competitor-analysis.md`, `docs/business-workflow.md`, GitHub Issues. Cấu trúc thư mục mẫu có thêm `docs/user-roles.md`, `.gitignore`.

Một số điều đề bài nhấn mạnh, đã dùng làm nguyên tắc khi viết:
- Không xác định phạm vi quá rộng ("làm giống Shopee" là sai).
- Phân tích đối thủ **không chỉ liệt kê tính năng**: phải nói học gì, bỏ gì, vì sao.
- AI chỉ đề xuất và phản biện; **quyết định thuộc về nhóm**. Bắt buộc dùng **2 công cụ AI khác nhau** (Nhiệm vụ 3 và 4).
- Không đưa bí mật hoặc dữ liệu cá nhân lên repo Public.

### Bước 2 – Đọc mã nguồn kế thừa để mô tả cho đúng thực tế
Clone 2 repo vào thư mục tạm (scratchpad, không nằm trong dự án). Phát hiện:

**Kiến trúc backend (đọc từ README và CLAUDE.md của repo gốc):**
```
Trụ sạc --OCPP 1.6--> Central System (Go, :8081) <-> NestJS (:3000) <-> App mobile
                                │
                    Laravel 11 (:8080): trang quản trị, REST API, ví, VNPay/AppotaPay
                                │
                           PostgreSQL 14
```
- Luồng sạc: App gửi yêu cầu → `pending_charges` (có `idempotency_key`) → CS gửi `RemoteStartTransaction` → trụ → `MeterValues` → `StopTransaction` → tính tiền.
- Model chính: `ChargePoint`, `Transaction` (start_meter, end_meter, fee_per_kw, connector_id), `Wallet` (amount), `PendingCharge`, `Payment`, `PaymentTransactions`.
- API: `auth/*` (JWT), `payment-vnpay`, `payment-url` (AppotaPay), `ipn`, `profile/*`, `current-transaction`, `max/{user_id}`, `internal/pending-charges/*` (basic auth, dành cho CS).
- Có `ocpp-simulator/` (trụ sạc giả lập bằng Node.js) → **chìa khóa** để sinh viên kiểm thử mà không cần phần cứng.

**App React Native 0.73:** các màn hình Auth, QrScreen, ChargeScreen, PaymentScreen (mệnh giá nhanh 10k/30k/50k/100k/200k/500k, tối thiểu 5.000 đ, tối đa 1 tỷ), Dashboard, History. Nội dung QR = `clientId` + 1 ký tự cuối là số cổng.

**⚠ Vấn đề bảo mật phát hiện:** repo backend gốc có `.env.backup.*`, `backend/.env.bak.new`, `full_log.txt` và một file có tên lẫn mật khẩu admin (`backend/sword' => Hash::make(...)`). → **Không fork hoặc copy nguyên repo gốc lên repo Public.** Đã ghi vào rủi ro R2, Issue #5, AGENTS.md và `.gitignore`.

Mình **không** mở hoặc sao chép nội dung các file bí mật đó.

### Bước 3 – Các quyết định tư duy chính (và lý do)

| # | Quyết định | Lý do | Mức chắc chắn |
|---|---|---|---|
| D1 | Xếp đề tài vào **Nhóm B – Ứng dụng đặt/sử dụng dịch vụ** | Có trạng thái dịch vụ (cổng trống/đang sạc), thanh toán và lịch sử, giống nghiệp vụ đặt dịch vụ | Cao |
| D2 | Phạm vi học phần = **web khách hàng responsive + trang quản trị**; không làm app mobile | Môn học là WEB/TMĐT; app có sẵn chỉ dùng để tham chiếu nghiệp vụ | **Cần nhóm/giảng viên duyệt** |
| D3 | **Không sửa Central System (Go)**, coi như hộp đen | Giao thức OCPP và phần cứng vượt nội dung môn; giảm rủi ro hỏng hệ thống | Cao |
| D4 | Kiểm thử bằng `ocpp-simulator`, thanh toán bằng **VNPay sandbox** | Không có trụ thật, không được dùng tiền thật | Cao |
| D5 | Mô hình **ví trả trước** (không trả sau) | Chặn được việc sạc khi thiếu tiền và tự dừng khi hết tiền; backend gốc đã có `wallets` | Cao |
| D6 | Điểm khác biệt: **không cần cài app**, quét QR là mở web | Đối thủ (V-GREEN, EBOOST, VETC) đều bắt cài app; trạm nhỏ và khách vãng lai không muốn cài app | Trung bình – lập luận, chưa khảo sát người dùng |
| D7 | Ví chỉ được cộng qua **IPN** có chữ ký, không qua return URL; IPN gửi lặp không cộng hai lần | Lỗi bảo mật kinh điển khi tích hợp cổng thanh toán | Cao |
| D8 | Tiền lưu dạng số nguyên; cộng/trừ ví trong `DB::transaction` + `lockForUpdate` | Tránh sai số float và lỗi khi hai yêu cầu đến cùng lúc | Cao |
| D9 | Đơn giá chốt **tại thời điểm bắt đầu phiên** | Đổi giá giữa phiên sẽ gây tranh chấp với khách | Cao |
| D10 | Đưa vào Ngoài phạm vi / Nâng cao: bản đồ, đặt chỗ, phí đỗ quá giờ, chatbot, hoàn tiền về ngân hàng, hóa đơn VAT | Đúng tinh thần đề: không cam kết vượt năng lực 5 sinh viên trong ~13 buổi | Cao |
| D11 | Mức số dư tối thiểu để bắt đầu sạc = **10.000 đ** | Giả định tạm; backend có API `max/{user_id}` chưa đọc kỹ | **Thấp – cần xác minh** |
| D12 | Nhóm 5 người, vai trò theo mẫu của đề (BA, UI/UX, FE, BE, QA), khoảng 13 buổi | Đề dùng ví dụ 5 thành viên; chưa có thông tin thật | **Placeholder** |

### Bước 4 – Phân tích đối thủ
- Nhận định: lĩnh vực sạc xe điện ở Việt Nam hầu như **chỉ có app**, không có website giao dịch. Vì vậy chọn 3 nền tảng đại diện cho 3 mô hình:
  - **V-GREEN / VinFast**: mạng lưới lớn, gắn với một hãng xe.
  - **EBOOST Car**: nhà vận hành độc lập; tìm trạm bằng QR, hiện phí trước khi sạc; đã tích hợp vào Grab Driver từ 4/2026.
  - **VETC Trạm sạc**: siêu ứng dụng gom nhiều nhà vận hành.
- Nguồn: WebSearch (App Store, VnEconomy, Grab blog, DNSE, Viettimes, VnReview). Danh sách link nằm cuối `docs/competitor-analysis.md`.
- **Trung thực về độ tin cậy:** ô nào chưa kiểm chứng được thì đánh dấu ⚠. Nhóm phải tự cài app, chụp ảnh màn hình và sửa lại trước khi nộp.

### Bước 5 – Các file đã tạo

| File | Trạng thái |
|---|---|
| `README.md` | Xong bản nháp; thiếu họ tên, MSSV |
| `REQUIREMENTS.md` | Xong bản nháp: F01–F36 (26 chức năng), NF01–NF09, Ngoài phạm vi, giả định |
| `docs/user-roles.md` | Xong |
| `docs/business-workflow.md` | Xong: 3 quy trình, sơ đồ Mermaid (sequence + state), bảng ngoại lệ |
| `docs/competitor-analysis.md` | Bản nháp; còn các ô ⚠ và thiếu ảnh |
| `PROJECT_PLAN.md` | Xong: 6 giai đoạn, T01–T18, R1–R8 |
| `AGENTS.md` | Xong: quy định phần nào AI được sửa / chỉ được đọc |
| `AI_USAGE_LOG.md` | Lần 1 (Claude) đã điền; Lần 2 có sẵn prompt nhưng **chưa chạy** |
| `docs/github-issues.md` | 6 Issue mẫu, đủ Mục tiêu / Công việc / Kết quả / Tiêu chí hoàn thành |
| `.gitignore` | Chặn `.env*`, `*.bak*`, `*.backup*`, `full_log.txt` |
| `history.md` | File này |

### Bước 6 – Việc còn dở (TODO cho phiên sau)
- [ ] **Nhiệm vụ 4:** chạy Prompt 2 (trong `AI_USAGE_LOG.md`) trên **một AI khác Claude**, điền bảng quyết định, rồi cập nhật lại `REQUIREMENTS.md` theo quyết định của nhóm.
- [ ] Bỏ các dấu ⚠ trong `docs/competitor-analysis.md` và thêm ảnh vào `docs/images/`.
- [ ] Điền họ tên, MSSV, ngày bắt đầu và kết thúc học phần.
- [ ] Xác minh D11: đọc `WalletController::getMax` và cách Central System xử lý tự dừng sạc.
- [ ] Nhóm/giảng viên duyệt D2 (web thay vì app).
- [ ] Tạo repo Public, push, tạo 6 Issue, kiểm tra truy cập bằng cửa sổ ẩn danh.
- [ ] Issue #5: sao chép mã kế thừa **đã rà soát** (không kèm bí mật), viết `docs/setup.md`.

---

## Hướng dẫn cho AI tiếp theo

1. **Đọc theo thứ tự:** `history.md` → `AGENTS.md` → `REQUIREMENTS.md` → `docs/business-workflow.md`. Nếu cần đối chiếu đề thì đọc PDF trong `../DE_BAI/`.
2. **Không thay đổi** các quyết định D1–D10 khi người dùng chưa yêu cầu. D2, D11, D12 là các điểm còn mở, được phép đề xuất lại.
3. Nếu bạn được dùng làm **AI phản biện (Nhiệm vụ 4)**: đánh giá độc lập, không mặc định Claude đúng, không đổi toàn bộ đề tài. Ghi kết quả vào `AI_USAGE_LOG.md` – Lần sử dụng 2.
4. Không mở, trích dẫn hay sao chép các file `.env*` / backup / log trong repo gốc.
5. Viết tài liệu bằng tiếng Việt; tên biến, commit bằng tiếng Anh.
6. **Sau mỗi phiên làm việc, thêm một mục "Phiên N" vào cuối file này** theo cùng khung: Yêu cầu → Đã làm → Quyết định + lý do → File thay đổi → TODO.

---

<!-- Mẫu cho phiên tiếp theo:

## Phiên N – YYYY-MM-DD – [Tên công cụ AI / mô hình]

### Yêu cầu của người dùng
### Đã làm
### Quyết định (thêm Dxx mới hoặc ghi rõ sửa Dxx nào, vì sao)
### File thay đổi
### TODO còn lại
-->
