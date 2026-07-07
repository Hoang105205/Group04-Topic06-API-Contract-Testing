# Wiki Nghiên Cứu — T06 API & Contract Testing

> **Mục đích:** Đây là wiki chung của team. Mỗi thành viên đọc section mình phụ trách, nắm nội dung, và viết vào User_Guide.md tương ứng.
> **Công cụ:** Postman (traditional) + Postbot (AI-augmented)
> **Target API:** EShop SUT (giảng viên cấp) + repo demo tự dựng

---

## Mục lục & Phân công

| # | Nội dung | Người phụ trách | Viết vào User Guide |
|---|----------|-----------------|---------------------|
| 1 | Install & Setup | **Quang** | Section 2 |
| 2 | Basic API Requests (GET/POST/PUT/DELETE) | **Quang** | Section 3 |
| 3 | API Authentication Testing Patterns | **Hoàng** | Section 4 |
| 4 | Test Scripts + Negative Testing | **Huy** | Section 5.1 + 5.1b |
| 5 | Collection Runner + Newman + Data-Driven Testing | **Quang** | Section 5.2 |
| 6 | Postbot (AI-augmented) | **Phương** | Section 5.3 |
| 7 | Failure Modes (3 cách tool đánh lừa) | **Phương** | Section 6 |
| 8 | Troubleshooting | **Quang** | Section 7 |
| 9 | References + AI Disclosure | **Hoàng** | Section 8 |

**Thuận (Presenter):** Không phụ trách viết wiki. Đọc toàn bộ để hiểu, tập trung luyện demo live.

---

## 1. Install & Setup — Quang phụ trách

### Cài đặt

1. **Postman Desktop App** — download tại https://www.postman.com/downloads/ (Windows/macOS/Linux)
2. **Tạo tài khoản Free** — 1 user, 50 AI credits/tháng (đủ dùng cho Postbot)
3. **Newman CLI** — cài qua terminal:
```bash
npm install -g newman
npm install -g newman-reporter-html
```

### Setup Environment trong Postman

1. Tạo Environment mới: Settings (gear icon) → Add → đặt tên "EShop Env"
2. Thêm variables:

| Variable | Initial Value | Mô tả |
|----------|--------------|-------|
| `base_url` | `http://localhost:3000` | URL của EShop SUT |
| `username` | `alice` | User test |
| `password` | `password123` | Password test |
| `auth_token` | (để trống) | Sẽ fill tự động khi login |
| `product_id` | (để trống) | Sẽ fill từ response GET /api/products |

3. Chọn Environment "EShop Env" ở góc phải Postman
4. Import Collection: File → Import → chọn file `.json`

### Verify kết nối

Gửi `GET {{base_url}}/api/products` → nhận response JSON → setup thành công.

---

## 2. Basic API Requests (GET/POST/PUT/DELETE) — Quang phụ trách

Đây là nền tảng API testing — phải nắm vững trước khi đi vào scripting.

### 4 HTTP Method cơ bản

| Method | Endpoint | Mô tả | Body |
|--------|----------|-------|------|
| **GET** | `/api/products` | Lấy danh sách products | Không |
| **POST** | `/api/auth/login` | Đăng nhập, nhận token | `{ "username": "...", "password": "..." }` |
| **PUT** | `/api/products/:id` | Cập nhật product (nếu API hỗ trợ) | `{ "name": "...", "price": ... }` |
| **DELETE** | `/api/products/:id` | Xóa product (nếu API hỗ trợ) | Không |

### Status Codes cần biết

| Code | Ý nghĩa | Khi nào gặp |
|------|---------|-------------|
| **200** OK | Thành công | GET, PUT thành công |
| **201** Created | Tạo mới thành công | POST tạo resource mới |
| **400** Bad Request | Input sai | Thiếu field, sai kiểu dữ liệu |
| **401** Unauthorized | Chưa xác thực | Thiếu token hoặc token sai |
| **403** Forbidden | Không có quyền | Token đúng nhưng không đủ quyền |
| **404** Not Found | Không tìm thấy | Resource không tồn tại |
| **409** Conflict | Xung đột | VD: coupon đã được redeem |
| **500** Internal Server Error | Lỗi server | Bug phía server |

### Tab Authorization trong Postman

1. Mở Collection (hoặc folder) → tab **Authorization**
2. Type: **Bearer Token** → Token: `{{auth_token}}`
3. Mọi request con trong Collection tự động có header `Authorization: Bearer <token>`
4. Không cần set header thủ công cho từng request

---

## 3. API Authentication Testing Patterns — Hoàng phụ trách

Tổng quan 4 cơ chế xác thực API phổ biến và cách test bằng Postman. Mục này **không demo live** — chỉ viết User Guide.

### 3.1 Bearer Token / JWT (phổ biến nhất)

- Client login → server trả JWT token → client gửi token trong header `Authorization: Bearer <token>`
- Token thường có thời hạn (expiry) — cần test cả trường hợp token hết hạn
- **Trong Postman:** Lưu token vào variable sau login, set Bearer Token ở Collection level
- **EShop SUT dùng JWT** — đây là pattern chính team cần test

### 3.2 API Key

- Server cấp API key (string dài) → client gửi qua header hoặc query param
- Thường dùng cho third-party integration (Google Maps API, OpenWeather API)
- **Trong Postman:** Set header `X-API-Key: <key>` hoặc thêm query param `?api_key=<key>`

### 3.3 Basic Auth

- Username + password encode Base64 → gửi trong header `Authorization: Basic <base64>`
- Đơn giản nhất nhưng kém an toàn (password gửi mỗi request)
- **Trong Postman:** Tab Authorization → Type: Basic Auth → điền username/password. Postman tự encode.

### 3.4 Session Cookie

- Client login → server set cookie `Set-Cookie: session=abc123` → browser tự gửi cookie trong các request sau
- Phổ biến trong web app truyền thống (không phải REST API thuần)
- **Trong Postman:** Postman tự quản lý cookies qua Cookie Manager (góc phải dưới). Có thể xem/set/xóa cookies.

### So sánh nhanh

| Cơ chế | Bảo mật | Phức tạp | Dùng khi nào |
|--------|---------|----------|-------------|
| JWT Bearer | Cao | Trung bình | REST API hiện đại, mobile app |
| API Key | Trung bình | Thấp | Third-party integration |
| Basic Auth | Thấp | Rất thấp | Internal tools, dev environment |
| Session Cookie | Cao | Cao | Web app truyền thống |

---

## 4. Test Scripts + Negative Testing — Huy phụ trách

### Pre-request Script

Chạy **trước** khi request được gửi. Dùng để tạo dữ liệu động:

```javascript
// Tạo timestamp
pm.environment.set("timestamp", Date.now());

// Tạo random email
const randomEmail = `user_${Math.floor(Math.random() * 10000)}@test.com`;
pm.environment.set("test_email", randomEmail);
```

### Variable Chaining

Lưu data từ response request trước → dùng cho request sau. Flow E-commerce:

```javascript
// POST /api/auth/login → Test Script:
const responseData = pm.response.json();
pm.environment.set("auth_token", responseData.token);
pm.environment.set("user_id", responseData.user.id);

// GET /api/products → Test Script:
const products = pm.response.json();
pm.environment.set("product_id", products[0].id);

// POST /api/cart → dùng {{product_id}} trong body
// POST /api/orders → dùng {{auth_token}} trong header
```

Request tiếp theo dùng: `Authorization: Bearer {{auth_token}}`

### Assertion nâng cao

```javascript
// Kiểm tra status code
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Kiểm tra field cụ thể trong body
pm.test("Product has valid price", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.price).to.be.a('number');
    pm.expect(jsonData.price).to.be.above(0);
});

// Kiểm tra kiểu dữ liệu + cấu trúc
pm.test("Response schema validation", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('id').that.is.a('number');
    pm.expect(jsonData).to.have.property('name').that.is.a('string');
    pm.expect(jsonData).to.have.property('email').that.includes('@');
});

// Kiểm tra response time
pm.test("Response time < 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

// Kiểm tra header
pm.test("Content-Type is application/json", function () {
    pm.response.to.have.header("Content-Type");
    pm.expect(pm.response.headers.get("Content-Type")).to.include("application/json");
});
```

### Thao tác UI

1. Mở request → Tab **Scripts** → Chọn **Pre-request** hoặc **Post-response**
2. Viết JavaScript → Nhấn **Save** → Nhấn **Send**
3. Kết quả test ở tab **Test Results** (xanh = pass, đỏ = fail)
4. Set script ở cấp **Collection** hoặc **Folder** → chạy cho mọi request con

### Negative Testing & Error Handling

Postbot KHÔNG tạo được negative tests — phải viết thủ công. Đây là kỹ năng phân biệt tester thật sự.

```javascript
// 1. Không có token → expect 401
pm.test("Unauthorized without token", function () {
    pm.response.to.have.status(401);
});

// 2. Thiếu field bắt buộc → expect 400
pm.test("Missing required field returns 400", function () {
    pm.response.to.have.status(400);
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
});

// 3. Sai kiểu dữ liệu (gửi string thay vì number) → expect 400
pm.test("Wrong data type returns 400", function () {
    pm.response.to.have.status(400);
});

// 4. Giá trị biên (quantity = 0, price âm) → expect validation error
pm.test("Zero quantity rejected", function () {
    pm.response.to.have.status(400);
});

// 5. Error response structure nhất quán
pm.test("Error response has consistent structure", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error').that.is.a('string');
});
```

**Cách tổ chức:** Tạo folder riêng "Negative Tests" trong Collection, chứa các request cố ý gửi input sai.

---

## 5. Collection Runner + Newman + Data-Driven Testing — Quang phụ trách

### Collection Runner (GUI)

1. Chuột phải Collection → **Run collection**
2. Chọn các request, số lần lặp (iterations), delay
3. Chọn environment file → Nhấn **Run**
4. Kết quả pass/fail theo từng request và từng assertion

### Newman CLI

```bash
# Cài đặt
npm install -g newman

# Chạy collection với environment
newman run "EShop_API_Collection.json" -e "EShop_Environment.json"

# Chạy với HTML report (external reporter)
npm install -g newman-reporter-html
newman run "EShop_API_Collection.json" -e "EShop_Environment.json" -r cli,html --reporter-html-export "report.html"

# Chạy với JSON report (built-in)
newman run "EShop_API_Collection.json" -e "EShop_Environment.json" -r cli,json --reporter-json-export "results.json"

# Chạy với JUnit XML report
newman run "EShop_API_Collection.json" -e "EShop_Environment.json" -r cli,junit --reporter-junit-export "results.xml"
```

### Report formats

| Format | Mô tả |
|--------|-------|
| **CLI** | Hiển thị trên terminal — bảng tóm tắt pass/fail, thời gian |
| **HTML** | File HTML mở bằng trình duyệt — chi tiết từng request |
| **JSON** | Dữ liệu thô — parse bằng script |
| **JUnit XML** | Format chuẩn — tương thích nhiều tool reporting |

### Export Collection

File → Export → Collection v2.1 (JSON) → Save. Environment export tương tự.

### Data-Driven Testing (Iteration Data)

Chạy cùng 1 request với nhiều bộ dữ liệu từ file CSV hoặc JSON.

**File CSV mẫu (`login_data.csv`):**
```csv
username,password,expected_status
alice,password123,200
bob,password456,200
alice,wrong_password,401
(empty),password123,400
charlie,(empty),400
```

**Collection Runner:** Run → tab **Data** → Select File → chọn CSV → Postman tự chạy mỗi dòng = 1 iteration

**Trong Test Script, truy cập data:**
```javascript
const expectedStatus = pm.iterationData.get("expected_status");
pm.test("Status matches expected", function () {
    pm.response.to.have.status(parseInt(expectedStatus));
});
```

**Newman:**
```bash
newman run collection.json -e env.json -d login_data.csv --iteration-count 5
```

---

## 6. Postbot (AI-augmented) — Phương phụ trách

### Postbot là gì

AI assistant tích hợp sẵn trong Postman (GA từ 2024). Nhận diện cấu trúc request/response → sinh test scripts JavaScript tự động.

### Khả năng

| Khả năng | Chi tiết |
|----------|----------|
| **Sinh test assertions** | Phân tích response → tạo `pm.test()` cho status, body, types |
| **Prompt tùy chỉnh** | Gõ natural language: "test that price is positive" → sinh code |
| **Debug test scripts** | Phát hiện lỗi cú pháp, gợi ý sửa |
| **Sinh documentation** | Tạo mô tả request params, response examples |
| **Visualize response** | Hiển thị data dạng bảng, biểu đồ |

### Cách sử dụng

1. Gửi request → nhận response
2. Tab **Tests** → nhấn **"Ask Postbot"** (hoặc icon AI)
3. Chọn "Test for response" hoặc gõ prompt tiếng Anh
4. Review code Postbot sinh → nhấn **Save**

### Giới hạn Free tier (2026)

| Giới hạn | Chi tiết |
|----------|----------|
| **AI credits** | **50 credits/tháng** (Free plan). Mỗi hoạt động = 1 credit |
| **Solo plan** | 400 credits/tháng ($9/tháng) |
| **Không có API** | Chỉ dùng trong GUI, không gọi từ CLI/script |
| **Tiếng Việt** | Không hỗ trợ — prompt phải viết tiếng Anh |
| **Phụ thuộc response** | Cần response thật để sinh test — không tạo test khi chưa gửi request |

### Postbot tạo được vs. KHÔNG tạo được

| Tạo được ✅ | KHÔNG tạo được ❌ |
|-------------|-------------------|
| Status code assertions (200, 201, 404) | **Business rules** ("không đặt hàng quá số lượng tồn kho") |
| Field existence check | **Edge cases** domain logic ("coupon hết hạn vẫn redeem") |
| Data type validation (string, number) | **Negative tests** cho auth/authorization |
| Response time assertions | **Race condition** / concurrency |
| Schema validation cơ bản | **Cross-request validation** (assertion phụ thuộc request trước) |
| Header existence check | Tests cho WebSocket / streaming |

---

## 7. Failure Modes — Phương phụ trách

> **Bắt buộc theo T06:** User Guide phải có mục "failure modes" — 3 cách Postman/Postbot đánh lừa người dùng.

### FM1: Postbot bịa field không tồn tại (Hallucinated Fields)

**Mô tả:** Postbot sinh assertion cho field nó "nghĩ" phải có — nhưng field không tồn tại hoặc tên sai (VD: `userId` thay vì `user_id`).

**Hậu quả:** False negative — test đỏ → developer sửa API theo hướng sai.

**Phòng tránh:** Luôn diff Postbot output với API spec gốc. Xóa assertion tham chiếu field không có trong spec.

### FM2: Postbot chỉ tạo happy-path, bỏ sót negative tests

**Mô tả:** Postbot chỉ sinh assertions cho trường hợp thành công (status 200). KHÔNG sinh tests cho: token hết hạn, thiếu field, giá trị vượt giới hạn.

**Hậu quả:** Coverage ảo — cảm giác "test hết rồi" nhưng chỉ cover happy path.

**Phòng tránh:** Viết negative tests thủ công (xem Section 4). Dùng AI khác (ChatGPT/Claude) để sinh adversarial payloads.

### FM3: Postman giấu lỗi gốc trong Collection Runner

**Mô tả:** Biến `auth_token` undefined → request gửi header rỗng → server trả 401 → Runner báo "1 fail" nhưng không chỉ rõ nguyên nhân là variable undefined.

**Hậu quả:** Debug mất thời gian vì triệu chứng (401) che lấp nguyên nhân (variable chaining hỏng).

**Phòng tránh:** Bật **Postman Console** (Alt+Ctrl+C) trước khi chạy Collection Runner. Console hiển thị biến nào undefined.

---

## 8. Troubleshooting — Quang phụ trách

| Lỗi | Nguyên nhân | Cách sửa |
|-----|-------------|----------|
| `ECONNREFUSED` | Server chưa chạy hoặc sai `base_url` | Kiểm tra server đang chạy, verify URL trong Environment |
| `{{auth_token}}` undefined | Quên lưu token hoặc chạy sai thứ tự request | Kiểm tra Test Script của login request, chạy Collection đúng thứ tự |
| Postbot không hiện "Ask Postbot" | Chưa gửi request (Postbot cần response mẫu) | Gửi request trước, kiểm tra còn AI credits |
| Newman: `collection not found` | Đường dẫn file JSON sai | Verify đường dẫn, kiểm tra đã export Collection chưa |
| Assertion fail không rõ nguyên nhân | Runner chỉ hiện pass/fail, không giải thích | Bật Postman Console (Alt+Ctrl+C) để xem log chi tiết |
| HTML report không generate | Thiếu `newman-reporter-html` | `npm install -g newman-reporter-html` |
| Postman crash / lag | Collection quá lớn (>1000 requests) | Chia nhỏ Collection thành folders |

---

## 9. Contract Testing / Pact — Tham khảo (Hoàng tổng hợp)

> Không demo trực tiếp. Đề cập trong slides và User Guide phần References.

**Contract Testing là gì?** Đảm bảo provider (server) và consumer (client) đồng ý về format API. Khi provider thay đổi response → contract test fail → phát hiện breaking change trước khi deploy.

**Pact** là framework contract testing phổ biến nhất:
Consumer viết “expectation” (VD: “GET /api/products phải trả array có field `name` kiểu string”)
- Provider verify expectation đó
- Pact Broker lưu trữ và quản lý contracts

**Khi nào cần:**
- Microservices architecture — nhiều services gọi nhau
- Team muốn phát hiện breaking change sớm hơn end-to-end test

**Khi nào KHÔNG cần:**
- Monolith (như EShop SUT) — chỉ có 1 server, không có consumer-provider split
- Overkill cho seminar demo

**Tham khảo:** https://pact.io

---

## Các feature khác của Postman (đề cập 1 câu trong User Guide)

| Feature | Mô tả |
|---------|-------|
| **Mock Servers** | Tạo endpoint giả lập response khi backend chưa sẵn sàng |
| **Monitors** | Chạy Collection theo lịch trình trên cloud, alert khi fail |
| **Postman Flows** | Visual workflow builder — nối request bằng kéo thả, không cần code |
| **OpenAPI Import** | Import file OpenAPI/Swagger → tự sinh Collection |
| **Workspaces** | Chia sẻ Collection giữa team members qua Postman cloud |
| **Package Library** | Lưu và tái sử dụng scripts chung giữa Collections |
| **Visualizer** | Render response data thành bảng/biểu đồ HTML |

---

## Ghi chú chung

- **Cần xác minh:** Postman Free plan 2026 còn giới hạn 1 user/workspace không (pricing thay đổi tháng 3/2026)
- **Cần xác minh:** Postbot có hỗ trợ GraphQL request không
- **Khuyến nghị:** Cả team cài Postman Desktop App (không dùng web version) cho đầy đủ tính năng
- **AI Policy:** Mọi output Postbot phải review + chỉnh sửa trước khi đưa vào deliverable. Ghi disclosure trong [AI-02].
