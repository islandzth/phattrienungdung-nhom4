# Project Plan – WT Charge

## 1. Thành viên và vai trò

| # | Họ tên | Vai trò chính | Trách nhiệm | Tài liệu/sản phẩm phụ trách chính |
|---|---|---|---|---|
| TV1 | _[Họ tên]_ | Trưởng nhóm / BA | Quản lý tiến độ, yêu cầu, GitHub Issues, điều phối, review PR | README.md, REQUIREMENTS.md, PROJECT_PLAN.md |
| TV2 | _[Họ tên]_ | UI/UX | Khảo sát giao diện đối thủ, wireframe, mockup Figma, bộ màu/icon | docs/competitor-analysis.md, Figma |
| TV3 | _[Họ tên]_ | Frontend | Giao diện web khách hàng (HTML/CSS/JS, Blade, Tailwind), quét QR bằng camera | `resources/views/customer/*` |
| TV4 | _[Họ tên]_ | Backend / Database | Thiết kế CSDL, API Laravel, tích hợp VNPay sandbox, kết nối Central System | docs/business-workflow.md, migration, API |
| TV5 | _[Họ tên]_ | QA / Integration | Kế hoạch kiểm thử, chạy trụ giả lập, kiểm thử tích hợp, Docker, tài liệu | AGENTS.md, AI_USAGE_LOG.md, test plan |

Trang quản trị (F30–F35) do **TV3 + TV4** cùng làm trên nền giao diện quản trị có sẵn. Mỗi người là chủ chính của một số màn hình, ghi rõ trong Issue.

## 2. Các giai đoạn

| Giai đoạn | Nội dung | Buổi | Kết quả |
|---|---|---|---|
| 1 | Khảo sát và xác định yêu cầu | 1–2 | Bộ tài liệu khởi tạo, Issues, môi trường chạy được bằng Docker + trụ giả lập |
| 2 | Thiết kế giao diện | 3–4 | Wireframe và mockup 8 màn hình khách hàng, 5 màn hình quản trị |
| 3 | Xây dựng frontend | 5–7 | Giao diện tĩnh responsive, gọi API giả (mock) |
| 4 | Backend và cơ sở dữ liệu | 6–9 | API khách hàng, ví, VNPay sandbox, quản trị trụ/đơn giá |
| 5 | Tích hợp và kiểm thử | 10–11 | Luồng end-to-end chạy với `ocpp-simulator`; test case đạt ≥ 90% |
| 6 | Triển khai và hoàn thiện | 12–13 | Bản demo triển khai bằng Docker, slide và báo cáo cuối kỳ |

_Giai đoạn 3 và 4 chạy song song; frontend dùng dữ liệu mock cho tới khi API xong._

## 3. Công việc

Trạng thái: ⬜ Chưa làm · 🟨 Đang làm · ✅ Xong

| Mã | Công việc | Người phụ trách | Kết quả cần đạt | Thời hạn | Trạng thái |
|---|---|---|---|---|---|
| T01 | Tạo repo, cấu trúc thư mục, mời thành viên | TV1 | Repo Public, đủ file theo đề | Buổi 1 | 🟨 |
| T02 | Xác định vấn đề, yêu cầu chức năng | TV1 | REQUIREMENTS.md được nhóm duyệt | Buổi 1 | 🟨 |
| T03 | Khảo sát 3 nền tảng tương tự | TV2 | competitor-analysis.md có ảnh minh họa, bỏ hết dấu ⚠ | Buổi 1 | 🟨 |
| T04 | Mô tả quy trình nghiệp vụ | TV4 | business-workflow.md có sơ đồ | Buổi 1 | 🟨 |
| T05 | Phân tích yêu cầu bằng AI thứ hai (phản biện) | TV5 | AI_USAGE_LOG.md lần 2, bảng quyết định | Buổi 1 | ⬜ |
| T06 | Rà soát mã kế thừa, loại bỏ bí mật, chạy Docker + simulator | TV4, TV5 | Hướng dẫn chạy local; không còn file `.env*` thật | Buổi 2 | ⬜ |
| T07 | Wireframe màn hình khách hàng | TV2 | 8 màn hình: đăng nhập, trang chủ, trụ, đang sạc, tóm tắt, ví, lịch sử, hồ sơ | Buổi 3 | ⬜ |
| T08 | Wireframe màn hình quản trị | TV2 | 5 màn hình: dashboard, trụ, người dùng, giao dịch, báo cáo | Buổi 4 | ⬜ |
| T09 | Thiết kế CSDL bổ sung (đơn giá theo trụ, trạm, trạng thái phiên) | TV4 | ERD + migration | Buổi 4 | ⬜ |
| T10 | Giao diện đăng ký/đăng nhập/hồ sơ | TV3 | F01–F03 responsive | Buổi 5 | ⬜ |
| T11 | Giao diện quét QR, thông tin trụ, đang sạc | TV3 | F20–F23 chạy trên Chrome/Safari điện thoại | Buổi 6–7 | ⬜ |
| T12 | API ví và tích hợp VNPay sandbox (IPN) | TV4 | F10–F12; IPN gửi lặp không cộng tiền hai lần | Buổi 7–8 | ⬜ |
| T13 | API bắt đầu/dừng sạc, tính tiền, tự dừng khi hết tiền | TV4 | F22–F24 với trụ giả lập | Buổi 8–9 | ⬜ |
| T14 | Trang quản trị: dashboard, trụ sạc, đơn giá, in QR | TV3, TV4 | F31–F32 | Buổi 9 | ⬜ |
| T15 | Trang quản trị: người dùng, giao dịch | TV3 | F33–F34 | Buổi 9 | ⬜ |
| T16 | Kế hoạch và kịch bản kiểm thử | TV5 | Test case cho mọi chức năng Bắt buộc | Buổi 8 | ⬜ |
| T17 | Kiểm thử tích hợp end-to-end | TV5 | Báo cáo lỗi trên Issues; ≥ 90% test case đạt | Buổi 10–11 | ⬜ |
| T18 | Triển khai demo, slide, báo cáo | Cả nhóm | Link demo + báo cáo | Buổi 12–13 | ⬜ |

## 4. Rủi ro

| # | Rủi ro | Khả năng | Ảnh hưởng | Phương án xử lý |
|---|---|---|---|---|
| R1 | Mã kế thừa phức tạp (3 ngôn ngữ: PHP, Go, TypeScript), khó hiểu | Cao | Cao | Chỉ sửa phần Laravel; Central System dùng như "hộp đen"; buổi 2 dành riêng để đọc mã và vẽ sơ đồ |
| R2 | Lộ bí mật khi đưa repo lên Public (repo gốc có `.env.backup*`, file chứa mật khẩu) | Cao | Rất cao | **Không fork nguyên repo gốc.** Chỉ sao chép thư mục đã rà soát; `.gitignore` chặn `.env*`; TV5 kiểm tra trước mỗi lần merge |
| R3 | Không có trụ sạc thật | Chắc chắn | Trung bình | Dùng `ocpp-simulator`; viết sẵn kịch bản giả lập (start, meter, stop, mất kết nối) |
| R4 | Tích hợp VNPay sandbox mất nhiều thời gian (chữ ký, IPN cần URL công khai) | Trung bình | Cao | Bắt đầu sớm (buổi 7); dùng ngrok để nhận IPN khi chạy local; phương án dự phòng là "nạp tiền giả lập" do quản trị viên duyệt |
| R5 | Lỗi tính tiền / trừ ví hai lần khi có yêu cầu đồng thời | Trung bình | Cao | Dùng DB transaction + khóa dòng; idempotency key; có test case riêng |
| R6 | Camera trình duyệt không quét được QR (iOS Safari yêu cầu HTTPS) | Trung bình | Trung bình | Luôn có ô **nhập mã trụ thủ công**; dùng HTTPS khi demo |
| R7 | Thành viên trễ hạn, phân công không đều | Trung bình | Cao | Mỗi Issue có 1 người chịu trách nhiệm và hạn chót; họp 15 phút đầu mỗi buổi |
| R8 | Phụ thuộc AI sinh mã mà thành viên không hiểu | Trung bình | Cao | Tuân thủ AGENTS.md; mỗi PR phải được tác giả giải thích khi review |
