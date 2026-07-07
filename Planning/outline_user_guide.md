# Outline — User_Guide.md (Stage S4)

> Yêu cầu từ Seminar_Guide.docx: ≥ 6 sections, viết bằng tiếng Anh, các bước phải reproducible.
> Yêu cầu từ T06: mục "Failure Modes" riêng biệt, liệt kê 3 cách Postman/Postbot có thể đánh lừa người dùng.

---

## Section 1: Introduction
**Người viết:** Hoàng

Giới thiệu ngắn gọn Postman là gì, tại sao chọn Postman cho API testing, và Postbot là AI assistant tích hợp. Nêu phạm vi guide: tập trung vào Test Scripts, Collection Runner + Newman, và Postbot. Đề cập Postman miễn phí (Free tier, 50 AI credits/tháng) — phù hợp sinh viên. Ghi rõ EShop SUT là target API cho mọi ví dụ.

---

## Section 2: Install & Setup
**Người viết:** Quang

Hướng dẫn cài đặt Postman Desktop App (Windows/macOS/Linux), tạo tài khoản Free, cài Newman CLI qua `npm install -g newman`. Import EShop Postman Collection từ file JSON. Setup Environment (base_url, credentials). Chạy thử request `GET /api/health` hoặc `GET /api/products` để verify kết nối. Screenshot từng bước.

---

## Section 3: First Test — Basic API Requests
**Người viết:** Quang

Demo nhanh các HTTP method cơ bản trước khi đi sâu vào scripting:
- **GET** `/api/products` — lấy danh sách products, xem response body, headers, status code
- **POST** `/api/auth/login` — gửi body JSON, nhận token
- **PUT** `/api/products/:id` — cập nhật product (nếu EShop SUT hỗ trợ — không phải API nào cũng có đủ CRUD)
- **DELETE** `/api/products/:id` — xóa product (tương tự, chỉ test nếu API hỗ trợ)

Giải thích ý nghĩa status codes phổ biến: 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 404 Not Found. Giới thiệu tab Authorization trong Postman — cách set Bearer Token ở cấp Collection để mọi request con tự động có header. Đây là nền tảng trước khi đi vào Test Scripts ở section tiếp theo.

---

## Section 4: API Authentication Testing Patterns
**Người viết:** Hoàng

Tổng quan các cơ chế xác thực API phổ biến và cách test bằng Postman:
1. **Bearer Token / JWT** — cách Postman lưu token vào variable và inject vào header tự động
2. **API Key** — truyền qua header hoặc query param
3. **Basic Auth** — Postman có sẵn tab Authorization
4. **Session Cookie** — cách test API dùng cookie-based auth

Giải thích khi nào dùng cơ chế nào, ưu nhược điểm. Liên hệ với EShop SUT: API dùng JWT Bearer Token — đây là pattern phổ biến nhất cho REST API hiện đại. Mục này không demo trực tiếp tại seminar nhưng cung cấp nền tảng lý thuyết quan trọng cho người đọc.

---

## Section 5: Advanced Usage
**Người viết:** Huy (5.1) + Quang (5.2) + Phương (5.3)

### 5.1 — Test Scripts
**Huy viết**

Pre-request Scripts: tạo dữ liệu động (timestamp, random email). Variable chaining hoàn chỉnh: flow login → lưu token → inject vào products → lưu product_id → inject vào cart → lưu cart_id → inject vào orders. Assertion nâng cao: validate nested JSON objects, kiểm tra array length, response time threshold, JSON schema validation. Mỗi khái niệm đi kèm 1 đoạn code ngắn và giải thích.

**5.1b — Negative Testing & Error Handling**

Viết test cho các trường hợp lỗi — đây là thứ Postbot KHÔNG tạo được (liên kết với Failure Mode 6.2): gửi request không có token → assert 401; gửi body thiếu field bắt buộc → assert 400; gửi sai kiểu dữ liệu (string thay vì number) → assert 400; gửi giá trị biên (quantity = 0, price âm) → assert validation error. Kiểm tra error response structure nhất quán: mọi lỗi đều trả `{ error: string }`. Đây là kỹ năng phân biệt tester thật sự với người chỉ biết gọi API.

### 5.2 — Collection Runner & Newman CLI
**Quang viết**

Collection Runner GUI: chạy toàn bộ E-commerce flow 1 lần, đọc kết quả pass/fail, iterations & delay. Export Collection + Environment thành JSON. Cài Newman, chạy CLI: `newman run collection.json -e env.json -r cli,html`. Đọc hiểu HTML report: tổng assertion, thời gian trung bình, request fail.

**Data-Driven Testing:** chạy cùng 1 request với nhiều bộ dữ liệu khác nhau bằng file CSV/JSON (iteration data). Ví dụ: test `POST /api/auth/login` với 5 username/password từ file CSV — mỗi dòng là 1 iteration, tự verify đúng/sai credentials. Newman hỗ trợ `--iteration-data data.csv`. Đây là cách test hàng loạt nhanh mà không cần viết request trùng lặp.

### 5.3 — Postbot: AI-Generated Tests
**Phương viết**

Cách bật Postbot, gửi request, mở tab Tests → "Ask Postbot" → chọn "Test for response" hoặc gõ prompt tùy chỉnh. Ví dụ prompt: "Generate tests for status code, response time, and validate that each product has a positive price." So sánh output Postbot vs. manual tests: table so sánh side-by-side. Phân tích: Postbot tạo đúng gì (schema validation, status code), bỏ sót gì (business rules, edge cases, negative tests). Giới hạn free tier: 50 credits/tháng = ~50 lần generate test.

**Cuối Section 5:** Đề cập ngắn gọn (1–2 câu mỗi feature): Mock Servers tạo endpoint giả, Monitors chạy Collection theo lịch, Postman Flows visual workflow, OpenAPI import. Ghi rõ: "Các feature này không nằm trong phạm vi chính của guide — tham khảo Postman docs để biết thêm."

---

## Section 6: Failure Modes — 3 Cách Postman/Postbot Đánh Lừa Bạn
**Người viết:** Phương

> **BẮT BUỘC theo T06 topic brief** — "User guide MUST include a 'failure modes' section listing 3 real ways the tool can mislead users."

### 6.1 — Postbot bịa field không tồn tại (Hallucinated Fields)
Mô tả, ví dụ cụ thể, hậu quả, cách phòng tránh.

### 6.2 — Postbot chỉ tạo happy-path, bỏ sót hoàn toàn negative tests
Mô tả, ví dụ cụ thể, hậu quả, cách phòng tránh.

### 6.3 — Postman giấu lỗi assertion gốc trong Collection Runner
Mô tả, ví dụ cụ thể, hậu quả, cách phòng tránh.

(Chi tiết nội dung lấy từ file `nghien_cuu_postman_postbot.md`)

---

## Section 7: Troubleshooting
**Người viết:** Quang

Tổng hợp các lỗi thường gặp khi dùng Postman + Newman:
- `ECONNREFUSED` — server chưa chạy hoặc sai base_url trong Environment
- Biến `{{auth_token}}` undefined — quên lưu token từ request trước, hoặc chạy sai thứ tự request trong Collection
- Postbot không hiện nút "Ask Postbot" — cần gửi request trước (Postbot cần response mẫu), kiểm tra AI credits còn không
- Newman báo `collection not found` — đường dẫn file JSON sai, hoặc quên export Collection từ Postman
- Assertion fail không rõ nguyên nhân — bật Postman Console (Alt+Ctrl+C) để xem log chi tiết
- HTML report không generate — quên cài `newman-reporter-html` (`npm install -g newman-reporter-html`)

---

## Section 8: References
**Người viết:** Hoàng

- Postman Learning Center: https://learning.postman.com
- Newman GitHub: https://github.com/postmanlabs/newman
- Postman JavaScript Sandbox Reference: https://learning.postman.com/docs/tests-and-scripts/write-scripts/postman-sandbox-reference
- Postbot documentation: https://learning.postman.com/docs/getting-started/postbot
- ESshop API Specification: `/api_specification.md` (trong EShop SUT repo)
- Mark Winteringham — "API Testing in Action" (Chapters 4 + 9)
- Pact documentation: https://pact.io — Contract Testing (consumer-driven contract tests) đảm bảo provider và consumer API không phá hợp đồng. Thiết yếu cho microservices architecture. Tham khảo thêm khi team cần test API contract giữa nhiều services.

### AI Disclosure (theo Seminar_Guide.docx Section 7 — AI Policy)

Hướng dẫn team tuân thủ AI Policy:
- Mọi output Postbot phải được review và chỉnh sửa bởi người trước khi đưa vào deliverable — không copy-paste thẳng
- Ghi rõ tool AI đã dùng (Postbot, ChatGPT, Claude), prompt tóm tắt, và output nhận được trong [AI-02]
- Không paste API keys, student ID, hay thông tin nhạy cảm vào AI tools
- Mọi factual claim trong slides/screencast phải cite nguồn gốc, không cite AI
- [AI-03] signed PDF: mỗi thành viên tự ký disclosure
