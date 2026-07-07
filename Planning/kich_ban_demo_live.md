# Kịch Bản Demo Live — 10 Phút (0:10–0:20)

> Presenter thao tác Postman thật trên **EShop SUT do giảng viên cấp** (không dùng slides).
> **Người thực hiện:** Thuận (Presenter)
> **Yêu cầu:** Bao phủ đủ 3 tính năng chính trong tối đa 4 bước.

---

## Kịch bản chi tiết

| Thời gian | Bước | Presenter làm gì | Tính năng thể hiện |
|-----------|------|------------------|-----------------|
| **0:10–0:13** (3 phút) | **Bước 1: Login + Variable Chaining** | Mở Postman, show Environment đã cấu hình (`base_url`, `username`, `password`). Gửi `POST /api/auth/login` với body JSON. Mở tab **Test Results** — chỉ cho audience thấy test pass: status 200, token được lưu vào `{{auth_token}}`. Mở tab **Scripts** (Post-response) — đọc to 3 dòng code quan trọng: parse JSON, lưu token, assert token là string. Gửi tiếp `GET /api/products` — header tự điền `Bearer {{auth_token}}`. Chỉ response body, highlight các field sẽ assert ở bước 2. | **Test Scripts** + Variable Chaining |
| **0:13–0:15½** (2½ phút) | **Bước 2: Assertion Nâng Cao** | Mở `GET /api/products` → tab Scripts → show test script đã viết sẵn: assert mỗi product có `name` (string), `price` (number, > 0), `id` (number). Thêm 1 assertion response time < 1000ms. Nhấn Send → tab Test Results → tất cả xanh. Nói nhanh: “Đây là assertion vượt xa status 200 — kiểm tra cấu trúc dữ liệu thật sự.” | **Test Scripts** — Assertion nâng cao |
| **0:15½–0:18** (2½ phút) | **Bước 3: Collection Runner + Newman** | Chuột phải Collection → **Run collection** → chọn 4 requests (login, products, cart, order) → nhấn **Run**. Kết quả hiện: 4 requests, X assertions, tất cả pass. Chuyển sang terminal (đã mở sẵn) → chạy: `newman run EShop_Collection.json -e EShop_Env.json -r cli,html` → show kết quả trên terminal → mở file `report.html` trong browser. Nói: “Đây là automation — chạy toàn bộ flow từ CLI, xuất report, tích hợp vào bất kỳ pipeline nào.” | **Collection Runner + Newman** |
| **0:18–0:20** (2 phút) | **Bước 4: Postbot AI** | Mở request `POST /api/orders` (hoặc 1 request có response phức tạp). Nhấn tab **Tests** → nhấn **“Ask Postbot”** → chọn “Test for response” hoặc gõ prompt: “Generate tests to validate order response fields.” Postbot sinh code → Presenter đọc nhanh 2–3 assertions Postbot tạo ra. Chỉ ra 1 assertion đúng (status code) và 1 gap (Postbot bỏ sót business rule: “order total phải bằng tổng price × quantity”). Nói: “AI tạo được starting point tốt, nhưng business rules vẫn cần con người.” | **Postbot (AI-augmented)** |

**Tổng thời gian: 10 phút chính xác.**

---

## Checklist chuẩn bị trước buổi Seminar

### Collection & Environment (cho EShop SUT)

| # | Item | Chi tiết | Người chuẩn bị |
|---|------|----------|----------------|
| 1 | **EShop Postman Collection** | Chứa ít nhất: POST /api/auth/login, GET /api/products, POST /api/cart, POST /api/orders. Mỗi request có Test Script hoàn chỉnh. Sắp xếp đúng thứ tự chạy. | Thuận |
| 2 | **EShop Environment file** | Variables: `base_url` (URL EShop SUT), `username`, `password`, `auth_token` (empty — sẽ fill khi login), `product_id` (empty), `cart_id` (empty). | Thuận |
| 3 | **Test data cố định** | Đảm bảo EShop SUT có sẵn products để GET. Nếu cần tạo product trước, thêm request "Setup — Create Product" trong Collection. | Thuận + Huy |
| 4 | **Export Collection JSON** | File → Export → v2.1 format. Để sẵn trong thư mục demo. | Thuận |
| 5 | **Export Environment JSON** | Tương tự. | Thuận |

### Terminal & Tools

| # | Item | Chi tiết |
|---|------|----------|
| 6 | **Newman cài sẵn** | `npm install -g newman` và `npm install -g newman-reporter-html` — test chạy được trước buổi |
| 7 | **HTML report pre-generated** | Chạy Newman 1 lần trước → có sẵn `report.html` để show nhanh nếu CLI chạy lỗi live |
| 8 | **Postman Console bật sẵn** | Alt+Ctrl+C — để debug nếu có lỗi live |

### Postbot

| # | Item | Chi tiết |
|---|------|----------|
| 9 | **AI credits kiểm tra** | Vào Settings → Resource usage → xác nhận còn ≥ 10 credits |
| 10 | **Request POST /api/orders có response phức tạp** | Postbot cần response mẫu để sinh test — gửi request trước 1 lần để Postman cache response |
| 11 | **Backup screenshot Postbot output** | Chụp ảnh Postbot output trước buổi — nếu Postbot lag/hết credits live, chuyển sang ảnh |

### Luyện tập

| # | Item | Chi tiết |
|---|------|----------|
| 12 | **Luyện ≥ 3 lần** | Chạy toàn bộ kịch bản từ đầu đến cuối ≥ 3 lần. Lần cuối bấm giờ — phải ≤ 10 phút |
| 13 | **Rehearsal với team** | Demo trước team ít nhất 1 lần — nhận feedback về tốc độ nói, clarity |
| 14 | **Kịch bản nói** | Chuẩn bị sẵn câu nói cho mỗi bước transition — tránh "uhm", "à" giữa các bước |

---

## Xử lý tình huống khẩn cấp

| Tình huống | Xử lý |
|------------|-------|
| EShop SUT không chạy / timeout | Nói rõ cho audience, chuyển sang dùng pre-recorded screencast (Demo_Screencast.mp4) |
| Postbot hết credits / lag | Show screenshot Postbot output đã chuẩn bị trước. Giải thích bằng lời. |
| Newman lỗi CLI | Show file `report.html` đã pre-generate. Giải thích CLI output trên terminal screenshot. |
| Assertion fail live | Đây là **cơ hội** — giải thích cho audience cách debug: bật Console, kiểm tra variable. Thể hiện kỹ năng troubleshooting thật. |
