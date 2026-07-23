## Prompt 1

### AI tool name
* Claude (Sonnet 5)

### Date and time
* 2026-07-10 19:30:00

### Prompt

```text
tôi đang cần tìm hiểu toàn bộ về [API-Contract-Testing](https://github.com/Hoang105205/Group04-Topic06-API-Contract-Testing/tree/master), tôi chưa có kiến thức gì hết và đang phải thực hiện task này; hãy giúp tôi

hãy giảng lại toàn bộ chi tiết giúp tôi về kiến thức này
Đây là proposal nhóm t đã nộp # Tool Survey Proposal: T6 API Testing ## 1. Candidate Tools Classification **Traditional Tool:** Postman (Industry-standard suite for functional and integration API testing) <br> **AI-Augmented Tool:** Postbot (Postman's native AI assistant for automated test generation) <br> **Backup Tool:** Pact (Consumer-driven contract testing framework) ## 2. Comparison Matrix | Axis | Postman | Pact | Postbot (Postman AI) | | :--------------------- | :------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------- | | **License Cost** | **Free tier** (1 user). Paid: **$12-$59** per user/month for enterprise features. | **Free/Open Source** (Framework). Hosted Pactflow scales from **$42+** per user/month. | **Included in Postman** via tokens. Free: 50 credits/mo. Paid: Higher quotas/add-ons. | | **Learning Curve** | **Low.** Intuitive GUI; industry standard for API testing. | **High.** Requires explicit coding (Java/JS/.NET) for consumer-driven contracts. | **Very Low.** Uses natural language prompts; bypasses deep JavaScript/Chai.js knowledge. | | **Fit with eshop-sut** | **Excellent.** Ideal for E2E/Integration testing of E-commerce data flows (catalogs/orders). | **Poor fit.** eshop-sut is a monolith. Pact is for decoupled microservices; using it here is over-engineering. | **Excellent.** Rapidly generates validation suites for complex JSON payloads (e.g., cart checkouts). | | **AI Capability** | **Minimal.** Relies entirely on the Postbot add-on. | **None.** Deterministic, explicit code assertions only. | **High.** Contextual test generation, JSON parsing, and auto-assertion drafting. | | **Community** | **Massive (35M+ users).** Vast ecosystem of workspaces and templates. | **Medium.** Niche, but the definitive standard for Contract Testing. | **Growing.** Directly leverages Postman's massive user base. | ## 3. Recommended Pick: Postman + Postbot **1. Zero Learning Curve:** Eliminates complex framework syntax. The team leverages existing Postman familiarity combined with intuitive AI prompting.<br> **2. Rapid Test Coverage:** Auto-generates assertions for deeply nested E-commerce JSON structures (carts/orders) in seconds, eliminating tedious manual validation.<br> **3. No Tooling Fatigue:** Avoids the operational overhead of adopting new tools (e.g., Pact Broker setup). Keeps all testing within a single, unified ecosystem. Hãy giảng lại chi tiết cho tôi kiến thức này và cụ thể nhóm tôi đang làm gì
Đây là phần phải nghiên cứu (file đính kèm). Hãy giải thích chi tiết cho tôi là team cần làm gì, liên quan gì đến bài học, tôi (Phương) cần làm gì
Đây là cụ thể task tôi cần làm. giải thích chi tiết lại giúp tôi. Nói chung giờ tôi đang ko hiểu 1 chữ gì và bắt đầu từ số 0, giai thích và hướng dẫn chi tiết giúp tôii
```

### The AI output

```text
Chào Phương! Mình sẽ giúp bạn hiểu từ số 0 — đọc kỹ hết 2 tài liệu bạn gửi (wiki chung + task plan cá nhân) và giải thích tuần tự theo 4 câu hỏi của bạn. Đọc chậm, không cần vội.
PHẦN 1: Kiến thức nền — API Testing & Contract Testing là gì?
API là gì, và tại sao phải "test" nó?
Hình dung EShop SUT (hệ thống bạn được cấp) là một cửa hàng online. Frontend (giao diện web) không tự lưu trữ dữ liệu — nó phải "hỏi" một server backend qua các cổng giao tiếp gọi là API (Application Programming Interface). Ví dụ:

Muốn xem danh sách sản phẩm → gọi GET /api/products
Muốn đăng nhập → gọi POST /api/auth/login
Muốn thêm vào giỏ hàng → gọi POST /api/cart

API testing = gửi các request này trực tiếp (không qua giao diện web) và kiểm tra xem server trả lời đúng không: đúng status code, đúng dữ liệu, đúng khi input sai (bảo mật, validation...).
Postman là gì?
Là công cụ (traditional tool) để gửi request thủ công, lưu lại thành "Collection" (bộ sưu tập request), viết script kiểm tra tự động (pm.test(...)), và chạy hàng loạt bằng Newman (CLI).
Postbot là gì?
Là trợ lý AI tích hợp trong Postman. Bạn gửi 1 request thật → Postbot nhìn vào response → tự sinh code kiểm tra (pm.test) giúp bạn, thay vì bạn phải tự gõ tay. Đây là phần "AI-augmented" mà nhóm bạn nghiên cứu.
Contract Testing / Pact là gì?
Đây là khái niệm khác với API testing thông thường:

API testing thông thường: gửi request thật tới server thật, kiểm tra response thật.
Contract testing: dùng khi có nhiều service nói chuyện với nhau (microservices). Bên "consumer" (bên gọi API) viết ra một bản "hợp đồng" — ví dụ "tôi mong API /products trả về mảng, mỗi phần tử có field name kiểu string". Bên "provider" (server) sẽ tự verify xem có đúng hợp đồng đó không, mà không cần consumer và provider chạy cùng lúc.
Công cụ phổ biến nhất: Pact.
Điểm quan trọng: EShop SUT là monolith (1 server duy nhất, không tách microservices) → Contract Testing/Pact không thực sự cần thiết ở đây, chỉ mang tính tham khảo lý thuyết trong bài (Section 9 của wiki, do Hoàng phụ trách, không demo live).


PHẦN 2: Giải thích Proposal nhóm bạn đã nộp
Proposal của nhóm bạn đang làm nhiệm vụ: so sánh 3 công cụ theo yêu cầu môn học (khảo sát công cụ — traditional vs AI-augmented vs backup):
Vai tròCông cụVì saoTraditional ToolPostmanCông cụ chuẩn công nghiệp, test API thủ công/scriptAI-Augmented ToolPostbotAI tích hợp sẵn trong Postman, tự sinh testBackup ToolPactCông cụ contract testing, nhưng nhóm đánh giá là không phù hợp với EShop SUT vì đây là monolith
Comparison Matrix so sánh 3 công cụ theo 5 trục: chi phí, độ khó học, độ phù hợp với EShop SUT, khả năng AI, cộng đồng hỗ trợ. Kết luận: Pact "over-engineering" (thừa thãi) cho hệ thống monolith này.
Recommended Pick: Nhóm chọn Postman + Postbot làm công cụ chính vì 3 lý do: học nhanh, sinh test coverage nhanh, không cần học thêm công cụ mới (Pact Broker...).
→ Điều này giải thích tại sao trong wiki, Pact/Contract Testing chỉ được nhắc sơ (Section 9, không demo), còn Postman + Postbot mới là trọng tâm cả nhóm phải làm sâu (Section 2–7).

PHẦN 3: Wiki chung — Cả nhóm đang làm gì, và Phương (bạn) ở đâu trong đó?
Đây là bảng phân công (đọc lại từ mục lục wiki):
#Nội dungNgười phụ trách1-2Install & Setup, Basic API RequestsQuang3Authentication Testing PatternsHoàng4Test Scripts + Negative TestingHuy5.2Collection Runner + Newman + Data-drivenQuang5.3Postbot (AI-augmented)Phương (bạn)6Failure Modes (3 cách tool đánh lừa người dùng)Phương (bạn)8TroubleshootingQuang9References + AI DisclosureHoàng—Presenter, không viết wikiThuận
Nhiệm vụ toàn nhóm: dựng lên 1 file User_Guide.md hoàn chỉnh, mỗi người viết 1-2 section, ghép lại. Ngoài ra có deliverable phụ: Activity_Worksheet.md (bài tập tay cho khán giả seminar tự làm trong ≤25 phút).
Bạn (Phương) phụ trách 2 mảng nội dung:
Section 5.3 — Postbot
Bạn cần hiểu và viết ra: Postbot là gì, cách dùng (nút "Ask Postbot" trong tab Tests), cách viết prompt tùy chỉnh, và đặc biệt — bảng so sánh Postbot sinh test gì vs. con người viết tay test gì. Chìa khóa: Postbot giỏi việc "thấy gì test nấy" (status code, kiểu dữ liệu, field tồn tại...) nhưng không hiểu business logic của EShop (ví dụ: không tự biết "coupon hết hạn thì phải từ chối").
Section 6 — Failure Modes (3 cách công cụ đánh lừa người dùng)
Đây là phần bắt buộc theo đề bài T06. 3 lỗi:

FM1 – Hallucinated Fields: Postbot bịa ra field không có thật trong response (ví dụ đoán tên field userId trong khi API thực trả user_id) → test đỏ oan, làm dev sửa sai chỗ.
FM2 – Chỉ tạo happy-path: Postbot chỉ test trường hợp thành công (200 OK), bỏ sót các trường hợp lỗi (token hết hạn, thiếu field, vượt giới hạn) → cảm giác an toàn giả (coverage ảo). Phần này liên kết trực tiếp với Section 5.1b của Huy (Negative Testing) — bạn cần phối hợp với Huy để tránh viết trùng.
FM3 – Postman/Collection Runner giấu nguyên nhân gốc: ví dụ biến {{auth_token}} bị undefined → Runner chỉ báo "1 fail (401)" chứ không nói rõ là do biến rỗng → phải mở Postman Console (Alt+Ctrl+C) mới thấy nguyên nhân thật.

Mỗi Failure Mode bạn phải viết đủ 4 phần: mô tả – ví dụ cụ thể (tự tay làm ra) – hậu quả – cách phòng tránh.

PHẦN 4: Giải thích làm gì cụ thể
1. Cài đặt + làm quen Postbot lần đầu
Setup (30 phút):
bashgit clone https://github.com/ttbhanh/eshop-sut.git
cd eshop-sut/backend
npm install
node server.js
Sau đó vào Postman, tạo Environment tên "EShop SUT Env", điền 4 biến: base_url, user_email, user_password, auth_token (để trống, sẽ tự điền sau khi login).
Thực hành với Postbot (2 tiếng):

Gửi POST /api/login → lấy token → lưu vào {{auth_token}} (dùng script pm.environment.set(...) — xem mẫu ở Section 4 của wiki, phần Huy viết).
Gửi GET /api/products → mở tab Tests → bấm "Ask Postbot" → chọn "Test for response" → quan sát: Postbot tự sinh ra những dòng test nào? Lưu output lại (copy paste vào note).
Lặp lại với GET /api/products/1 và POST /api/cart (nhớ gửi kèm token).
So sánh nhanh: nếu tự bạn viết test tay, bạn sẽ viết gì khác với Postbot?

2. Đào sâu Postbot + So sánh AI vs. thủ công
Test Postbot trên nhiều loại endpoint hơn: GET list, GET single, POST có auth, và đặc biệt endpoint POST /api/apply-coupon (business logic phức tạp: mã giảm giá, điều kiện đơn hàng tối thiểu...).
Bạn cũng thử prompt tùy chỉnh bằng tiếng Anh (lưu ý: Postbot không hỗ trợ tiếng Việt), ví dụ: "Generate tests for status code, response time, and validate that each product has a positive price".
Rồi thử tính năng debug: cố tình viết 1 đoạn test sai cú pháp → nhờ Postbot sửa → xem nó sửa đúng không.
Phần quan trọng nhất ngày này: với /api/apply-coupon, bạn tự viết tay 5 test case (coupon hợp lệ→200, coupon sai→404, hết hạn→400, dưới mức tối thiểu→400, vượt số lần dùng→400), sau đó nhờ Postbot sinh test cho cùng endpoint đó, rồi điền vào bảng so sánh trong task plan. Đây chính là dữ liệu thô bạn sẽ dùng để viết Section 5.3 sau này.
3. Nghiên cứu Failure Modes + rà soát Activity Worksheet
Bạn tự tay tạo ra ví dụ thật cho từng Failure Mode (không phải nói suông):

FM1: tìm 1 lần Postbot bịa field không có thật.
FM2: đếm lại bảng so sánh Ngày 2 — bao nhiêu test negative Postbot bỏ sót so với bạn viết tay.
FM3: cố tình để {{auth_token}} rỗng, chạy Collection Runner, xem lỗi mơ hồ thế nào, rồi bật Console (Alt+Ctrl+C) để thấy nguyên nhân thật.

Song song, bạn đọc file activity_worksheet.md đã có sẵn trong thư mục Planning (Huy phối hợp phần này), tự làm thử theo đúng hướng dẫn để kiểm tra xem người khác làm theo có clear không, có thiếu bước nào không.
4. Viết chính thức 2 section
Dùng toàn bộ dữ liệu thu thập Ngày 1–3 để viết:

Section 5.3: Postbot là gì, cách dùng, prompt tùy chỉnh, bảng so sánh AI vs thủ công, ưu/nhược điểm, giới hạn free tier (50 credit/tháng, chỉ tiếng Anh, cần có response mẫu trước mới hoạt động).
Section 6: viết đủ 3 Failure Mode, mỗi mode có mô tả — ví dụ thật — hậu quả — cách phòng tránh. Nhớ liên kết FM2 với phần Negative Testing của Huy (5.1b).

5. Review chéo + hoàn thiện
Đọc và góp ý phần của Huy (test script + negative testing) và Quang, đảm bảo nội dung không mâu thuẫn với phần bạn viết. Hoàn thiện, gửi cho Hoàng để tổng hợp vào User_Guide.md chung. Chốt Activity_Worksheet.md cùng Huy — yêu cầu bắt buộc: nhóm khác phải tự làm được trong ≤25 phút, không cần bạn hỗ trợ.
Final - Chuẩn bị seminar + nộp bài
Xác nhận phần của bạn đã có trong bản User_Guide.md cuối cùng. Trong buổi seminar, bạn là người hỗ trợ hoạt động 20 phút cho khán giả (họ sẽ tự thực hành Postbot theo Activity Worksheet, bạn giúp khi họ gặp khó). Sau seminar: nộp input cho AI Audit Report (Hoàng tổng hợp), ký AI Disclosure, viết 300 từ Reflective Statement cá nhân, và góp ý vào Final_Reflection.md chung.

Tóm gọn lại: bạn đang là người phụ trách "linh hồn" của phần AI trong đề tài này — chứng minh Postbot giỏi ở đâu, dở ở đâu, và cách nó có thể "lừa" người dùng nếu không cẩn thận. Nếu bạn muốn, mình có thể giúp bạn viết luôn bản nháp Section 5.3 hoặc Section 6 dựa trên khung này, hoặc dựng bảng so sánh mẫu để bạn điền số liệu thực tế vào.
```

## Prompt 2
### AI tool name
Claude (Sonnet 5)

### Date and time
* 2026-07-10 19:45:00

### Prompt

```text
giờ hãy hướng dẫn tôi chi tiết cho task ngày 1 và 2, đây là repo của bài test https://github.com/ttbhanh/eshop-sut
```

### The AI output

```text
Mình đã đọc trực tiếp repo thật (README, api_specification.md, setup_guide.md) để đảm bảo hướng dẫn đúng 100% với hệ thống thật — vì file task plan gốc của bạn ghi một số chi tiết hơi khác so với repo thật (ví dụ endpoint không phải /api/auth/login mà là /api/login). Dưới đây là hướng dẫn chi tiết, đi từng bước một.

⚠️ Lưu ý quan trọng trước khi bắt đầu
So với file Task Plan bạn có, thực tế repo khác một chút:
Trong Task Plan bạn ghiThực tế trong repogit clone .../eshop-sut.git → cd eshop-sut/backendĐúng — thư mục backend nằm ngay trong reponpm install → node server.jsThiếu 1 bước: phải chạy node database.js trước để tạo & seed database (chỉ cần 1 lần)Endpoint login: POST /api/auth/loginThực tế: POST /api/login (không có /auth/)Body login dùng usernameThực tế dùng email, không phải usernameTài khoản test: test@eshop.com / Test1234!✅ Đúng khớp
⚠️ Một điểm bạn nên tự kiểm chứng khi thực hành (đây cũng là dữ liệu hay cho phần Failure Mode của bạn): README ghi mật khẩu admin là Admin123!, nhưng setup_guide.md lại ghi admin123. Đây là ví dụ thật về tài liệu không nhất quán — bạn có thể note lại làm ví dụ minh họa.

1. Setup + Postbot lần đầu
Bước 1: Cài Postman Desktop App

Vào https://www.postman.com/downloads/ → tải bản cho hệ điều hành của bạn → cài đặt.
Mở app → Sign up tài khoản Free (email cá nhân là được).
Kiểm tra AI credits: Settings (icon bánh răng góc trên phải) → Resource usage → xem còn bao nhiêu credit (mặc định Free = 50/tháng).

Bước 2: Clone và chạy backend EShop SUT
Mở Terminal:
bashgit clone https://github.com/ttbhanh/eshop-sut.git
cd eshop-sut/backend
npm install
node database.js      # ⚠️ bước này Task Plan của bạn thiếu — bắt buộc chạy 1 lần để tạo DB + seed data
node server.js
Nếu thấy dòng Server is running on http://localhost:3000 → thành công. Để terminal này chạy xuyên suốt, đừng tắt.
Bước 3: Tạo Environment trong Postman

Trong Postman, bấm icon Environments (bên trái) → + để tạo mới → đặt tên EShop SUT Env.
Thêm các biến sau (cột Initial Value):

VariableInitial Valuebase_urlhttp://localhost:3000user_emailtest@eshop.comuser_passwordTest1234!auth_token(để trống)

Save. Sau đó chọn Environment này ở dropdown góc trên bên phải (chỗ ghi "No Environment").

Bước 4: Login lấy token

Tạo request mới: POST {{base_url}}/api/login
Tab Body → chọn raw → JSON → nhập:

json{
  "email": "{{user_email}}",
  "password": "{{user_password}}"
}

Bấm Send. Response trả về sẽ có dạng (theo api_specification.md): chứa token và user.
Vào tab Scripts (hoặc "Tests" tùy version) của request này → Post-response → viết:

javascriptconst res = pm.response.json();
pm.environment.set("auth_token", res.token);
console.log("Token saved:", res.token);

Save → Send lại lần nữa để script chạy → kiểm tra Environment (icon con mắt góc trên phải) xem auth_token đã có giá trị chưa.

Bước 5: Gửi GET /api/products và thử Postbot

Tạo request mới: GET {{base_url}}/api/products
Bấm Send → xem response (mảng sản phẩm).
Vào tab Tests (hoặc mục có icon AI/sao lấp lánh) → bấm "Ask Postbot".
Chọn gợi ý "Test for response" (hoặc tương tự) → Postbot sẽ tự đọc response và sinh code pm.test(...).
Quan sát và ghi lại: Postbot sinh ra bao nhiêu test? Test gì? (thường sẽ là: status 200, response là array, mỗi item có field gì kiểu gì...)
Bấm Save để lưu lại test đó.

Bước 6: Lặp lại với 2 endpoint nữa
GET {{base_url}}/api/products/1 (xem chi tiết 1 sản phẩm) → Ask Postbot → quan sát.
POST {{base_url}}/api/cart (cần token) — nhớ vào tab Authorization của request này → chọn Bearer Token → điền {{auth_token}}. Body:
json{
  "id": 1,
  "name": "Sản phẩm A",
  "price": 100000,
  "quantity": 2
}
→ Send → Ask Postbot → quan sát.
Bước 7: So sánh nhanh (viết ra giấy/note, không cần văn bản chính thức)
Tự hỏi: nếu là bạn viết tay test cho 3 endpoint trên, bạn sẽ viết gì khác? Ví dụ: bạn có thể muốn test "giá phải > 0" (business rule) — Postbot có tự nghĩ ra điều này không, hay chỉ test "giá là số"?

2. Đào sâu Postbot + So sánh AI vs Thủ công
Bước 1: Test Postbot trên endpoint phức tạp hơn — /api/apply-coupon
Theo api_specification.md thật, endpoint và body chính xác là:
POST {{base_url}}/api/apply-coupon
json{
  "code": "SAVE10",
  "total_amount": 500000,
  "user_id": 1
}
Repo có sẵn các mã coupon mẫu để bạn test nhiều tình huống (từ README, phần FR-09):
MãLoạiGiá trịNgưỡng tối thiểuHạn dùngSố lần/ngườiSAVE10percent10%300,000₫2099-12-311BIGBUYfixed50,000₫500,000₫2099-12-311VIP100fixed100,000₫300,000₫2099-12-312EXPIREDpercent20%100,000₫2020-01-01 (đã hết hạn)1
→ Gửi request với SAVE10 trước → Send → Ask Postbot → quan sát output.
Bước 2: Test prompt tùy chỉnh (tiếng Anh)
Trong ô prompt của Postbot, gõ (nhớ: chỉ tiếng Anh, Postbot không hỗ trợ tiếng Việt):

"Generate tests for status code, response time, and validate that discount_amount and final_amount are numbers"

Quan sát Postbot sinh ra gì khác so với lúc bạn chỉ bấm "Test for response" mặc định.
Bước 3: Test khả năng debug của Postbot
Cố tình viết 1 đoạn test sai cú pháp, ví dụ:
javascriptpm.test("broken test" function () {  // thiếu dấu phẩy sau "broken test"
    pm.response.to.have.status(200)
});
→ Nhờ Postbot debug (thường có nút "Fix" hoặc bạn hỏi "why is this test failing?") → xem nó có phát hiện đúng lỗi cú pháp không.
Bước 4: So sánh Manual vs AI — phần quan trọng nhất
Tự viết tay 5 test case cho /api/apply-coupon (dựa theo đúng 5 điều kiện C1–C5 trong README):
javascript// 1. Coupon hợp lệ, đủ điều kiện → 200
pm.test("Valid coupon returns 200", function () {
    pm.response.to.have.status(200);
});

// 2. Coupon không tồn tại → dùng code random, ví dụ "NOTEXIST"
pm.test("Invalid coupon code handled", function () {
    pm.expect(pm.response.code).to.be.oneOf([400, 404]);
});

// 3. Coupon EXPIRED → mong đợi 400 (đã hết hạn theo C2)
pm.test("Expired coupon rejected", function () {
    pm.response.to.have.status(400);
});

// 4. total_amount thấp hơn min_order_amount, ví dụ dùng BIGBUY với total_amount: 100000 (< 500,000)
pm.test("Below minimum order rejected", function () {
    pm.response.to.have.status(400);
});

// 5. Dùng coupon vượt quá max_uses_per_user (gọi lại API lần 2 với cùng user_id + coupon 1 lượt)
pm.test("Max uses exceeded rejected", function () {
    pm.response.to.have.status(400);
});
Sau đó nhờ Postbot sinh test cho cùng endpoint này, rồi điền vào bảng so sánh (từ Task Plan gốc):
Test CaseManual (bạn viết)Postbot (AI sinh)Nhận xétValid coupon → 200✅ có?Invalid coupon → 404✅ có? thường KHÔNG cóPostbot không tự nghĩ ra input saiExpired coupon → 400✅ có? thường KHÔNG cóĐây là business rule (C2), Postbot không biếtBelow min order → 400✅ có? thường KHÔNG cóBusiness rule (C3)Max uses exceeded → 400✅ có? thường KHÔNG cóBusiness rule (C5), cần gọi nhiều request liên tiếpBusiness rule: discount formula đúng✅ có thể viết? thường KHÔNG cóCần biết công thức total × discount_value/100Edge case: total_amount = 0✅ có? có thể có (kiểu dữ liệu) nhưng không hiểu ý nghĩa
(Dấu "?" là ô bạn sẽ tự điền dựa trên kết quả thật khi bạn chạy Postbot — mỗi lần Postbot generate có thể ra kết quả hơi khác nhau).
Bước 5: Ghi chú lại 2 điều

Postbot làm tốt ở đâu: thường là status code cơ bản, kiểu dữ liệu của field (number, string), field có tồn tại hay không, response time.
Postbot bỏ sót ở đâu: hầu như luôn bỏ sót phần "business logic" — tức là nó không tự biết luật nghiệp vụ của EShop (VD: coupon hết hạn, vượt ngưỡng, vượt lượt dùng) trừ khi bạn ghi rõ trong prompt.

Đây chính là dữ liệu cốt lõi để bạn viết Section 5.3 và cũng là bằng chứng thực tế cho FM2 (Failure Mode #2 — happy-path only) ở Section 6 sau này.

✅ Checklist cuối

 Backend EShop chạy được, login lấy token thành công
 Test Postbot trên ít nhất 3 endpoint (products list, product detail, cart hoặc apply-coupon)
 Thử ít nhất 1 prompt tùy chỉnh tiếng Anh
 Thử tính năng debug của Postbot với 1 test lỗi cú pháp
 Viết tay 5 test case cho apply-coupon + so sánh với Postbot → điền bảng
 Có ít nhất 1–2 screenshot lưu lại làm bằng chứng

Nếu muốn, mình có thể giúp bạn viết sẵn bộ test script hoàn chỉnh cho apply-coupon (cả 5 case) để bạn copy thẳng vào Postman, tiết kiệm thời gian ngày 2.
```


## Prompt 3
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-10 20:15:00
### Prompt

````text
Bước 4: Login lấy token

1. Tạo request mới: POST `{{base_url}}/api/login`
2. Tab Body → chọn raw → JSON → nhập:
json

```json
{
  "email": "{{user_email}}",
  "password": "{{user_password}}"
}
```
3. Bấm Send. Response trả về sẽ có dạng (theo api_specification.md): chứa `token` và `user`.
4. Vào tab Scripts (hoặc "Tests" tùy version) của request này → Post-response → viết:
javascript

```javascript
const res = pm.response.json();
pm.environment.set("auth_token", res.token);
console.log("Token saved:", res.token);
```

5. Save → Send lại lần nữa để script chạy → kiểm tra Environment (icon con mắt góc trên phải) xem `auth_token` đã có giá trị chưa.
Bước 5: Gửi GET /api/products và thử Postbot

1. Tạo request mới: GET `{{base_url}}/api/products`
2. Bấm Send → xem response (mảng sản phẩm).
3. Vào tab Tests (hoặc mục có icon AI/sao lấp lánh) → bấm "Ask Postbot".
4. Chọn gợi ý "Test for response" (hoặc tương tự) → Postbot sẽ tự đọc response và sinh code `pm.test(...)`.
5. Quan sát và ghi lại: Postbot sinh ra bao nhiêu test? Test gì? (thường sẽ là: status 200, response là array, mỗi item có field gì kiểu gì...)
6. Bấm Save để lưu lại test đó.
Bước 6: Lặp lại với 2 endpoint nữa
GET `{{base_url}}/api/products/1` (xem chi tiết 1 sản phẩm) → Ask Postbot → quan sát.
POST `{{base_url}}/api/cart` (cần token) — nhớ vào tab Authorization của request này → chọn Bearer Token → điền `{{auth_token}}`. Body:
json

```json
{
  "id": 1,
  "name": "Sản phẩm A",
  "price": 100000,
  "quantity": 2
}
```

→ Send → Ask Postbot → quan sát. phần này để làm gì
````
### The AI output
````
Được, mình giải thích lại mục đích (tại sao làm, không chỉ là làm gì) của từng bước, vì đây là phần dễ gây rối nếu chỉ đọc thao tác mà không hiểu "vì sao".
Bước 4 — Login lấy token: đang làm gì và để làm gì?
Vấn đề cần giải quyết: Nhiều API của EShop (giỏ hàng, đơn hàng, coupon...) yêu cầu bạn phải đăng nhập mới cho dùng — giống như bạn phải có "thẻ ra vào" mới được bước vào phòng. Thẻ ra vào đó chính là JWT token.
Vấn đề là: token này không cố định, mỗi lần login nó sinh ra 1 chuỗi token khác nhau (dài, ngẫu nhiên). Nếu bạn phải tự tay copy-paste token vào từng request thì rất mất thời gian và dễ sai. Nên bước 4 là để tự động hóa việc lấy token và lưu lại, dùng cho mọi request sau này.
Diễn giải từng bước nhỏ:

Bạn gửi email + password thật (2 cái này bạn đã lưu sẵn trong Environment ở Bước 3, nên gõ {{user_email}} thay vì gõ tay test@eshop.com) → server EShop kiểm tra đúng thì trả về 1 token.
Server trả về JSON dạng đại khái:

json{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9....",
  "user": { "id": 2, "email": "test@eshop.com", ... }
}

Đoạn script bạn viết trong tab Scripts/Post-response chạy tự động ngay sau khi có response — nó lấy giá trị token trong response đó ra, rồi lưu vào biến auth_token trong Environment. Nói cách khác: bạn đang "dạy" Postman tự cầm lấy chiếc thẻ ra vào (token) sau khi login, rồi cất nó vào túi (Environment variable) để dùng dần, không cần bạn tự tay copy nữa.
Việc bấm Send lại lần 2 là để chắc chắn script đã chạy và lưu đúng — bạn "mở túi ra xem" (icon con mắt) để confirm token đã nằm trong đó.

→ Kết quả sau Bước 4: bạn có sẵn {{auth_token}} dùng được cho mọi request cần xác thực phía sau, không cần login lại thủ công mỗi lần.
Bước 5 — GET /api/products + Ask Postbot: đang làm gì?
Đây là bước thực hành nghiên cứu chính của bạn (vì bạn phụ trách Section 5.3 — Postbot).

GET /api/products là 1 API không cần token (ai cũng xem được danh sách sản phẩm) → dùng làm ví dụ đơn giản nhất để bắt đầu quan sát Postbot, chưa vướng vấn đề auth.
Khi bạn Send, Postman nhận về 1 response thật (mảng sản phẩm với các field như id, name, price...).
Khi bấm "Ask Postbot": Postbot nhìn vào chính response thật vừa nhận được đó rồi tự đoán và viết code kiểm tra (pm.test(...)) — ví dụ tự viết "kiểm tra status là 200", "kiểm tra response là 1 mảng", "kiểm tra mỗi phần tử có field price kiểu number"...

Mục đích thật sự của bước này không phải là để có bộ test hoàn chỉnh — mà là để bạn quan sát và ghi chép: Postbot tự nghĩ ra được những gì, dựa trên cái gì. Đây chính là dữ liệu nghiên cứu bạn cần cho câu hỏi lớn của đề tài: "AI-augmented testing tool làm được gì, giới hạn ở đâu?"
Bước 6 — Lặp lại với 2 endpoint khác: đang làm gì?
Mục đích: kiểm tra xem Postbot có phản ứng khác nhau với các loại endpoint khác nhau không — vì nếu chỉ thử 1 endpoint, bạn không đủ dữ liệu để kết luận gì cả (giống như chỉ hỏi 1 người rồi kết luận "cả nhóm nghĩ vậy" — không đủ căn cứ).

GET /api/products/1 — là GET lấy 1 object đơn lẻ (khác với GET danh sách/array ở Bước 5) → xem Postbot có sinh test khác đi không khi response không còn là mảng mà là 1 object.
POST /api/cart — đây là lần đầu bạn cho Postbot thấy 1 request có gửi kèm token (Authorization: Bearer) và có Body (dữ liệu gửi lên, không chỉ đọc dữ liệu về) → xem Postbot có tự phát hiện việc thiếu token sẽ gây lỗi 401 không, hay nó chỉ test happy-path (giả định lúc nào cũng gửi đúng, thành công).

Việc bạn phải tự set tab Authorization → Bearer Token → {{auth_token}} cho request /api/cart là bắt buộc, vì nếu không có, server sẽ trả về lỗi 401 (Unauthorized) — đây chính là lý do Bước 4 (lấy token) phải làm trước Bước 6.

Tóm gọn 3 bước này lại thành 1 câu: Bước 4 là "chuẩn bị chìa khóa" (token), Bước 5–6 là "dùng chìa khóa đó thử 3 loại cửa khác nhau (GET list, GET single, POST có auth) và ghi chép xem con AI (Postbot) tự mở khóa/kiểm tra được đến đâu, bỏ sót gì" — chính là nguyên liệu thô cho bài viết của bạn về Postbot sau này.
````

## Prompt 4
### AI tool name    
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 19:30:00
### Prompt

```text
ở bước 5 Bước 5: Gửi GET /api/products và thử Postbot

Tạo request mới: GET {{base_url}}/api/products
Bấm Send → xem response (mảng sản phẩm).
Vào tab Tests (hoặc mục có icon AI/sao lấp lánh) → bấm "Ask Postbot".
Chọn gợi ý "Test for response" (hoặc tương tự) → Postbot sẽ tự đọc response và sinh code pm.test(...).
Quan sát và ghi lại: Postbot sinh ra bao nhiêu test? Test gì? (thường sẽ là: status 200, response là array, mỗi item có field gì kiểu gì...)
Bấm Save để lưu lại test đó. postbot ra v nè ? là sao
```

### The AI output

```text
Ảnh này cho thấy 2 điều — 1 điều bình thường (đúng như mong đợi), và 1 điều có vẻ bị nhầm, mình giải thích rõ từng phần:
Phần bình thường: Postbot đã hoạt động đúng
Ở khung bên phải, Postbot hiển thị:

"I have added a test to check if the response status code is 200 and the response time is less than 200ms."

Và nó tự chèn code vào (dòng 5–7):
javascriptpm.test("Response status code is 200 and response time is less than 200ms", function () {
    pm.expect(pm.response).to.have.status(200);
    pm.expect(pm.response.responseTime).to.be.below(200);
});
→ Đây chính xác là kết quả bạn cần quan sát và ghi chép cho nghiên cứu Postbot (đúng như Bước 5 mục 4–5 yêu cầu). Postbot ở đây chỉ sinh ra 2 assertion: status code = 200 và response time < 200ms. Nó chưa kiểm tra gì về nội dung dữ liệu (không check response có phải array không, không check field price, name có tồn tại/kiểu đúng không).
→ Đây là dữ liệu quý cho bạn: ghi lại đúng như vậy vào note — "Lần thử 1 với GET /api/products, Postbot chỉ sinh 2 test: status 200 + response time. Không tự sinh test kiểm tra cấu trúc dữ liệu (schema) dù response là 1 mảng sản phẩm." Đây là ví dụ thực tế cho việc Postbot đôi khi sinh test rất tối thiểu, không toàn diện — liên quan trực tiếp đến FM2 (happy-path/coverage ảo) mà bạn sẽ viết ở Section 6.
Phần có vẻ bị nhầm: dòng 1–4
Nhìn kỹ 4 dòng đầu:
javascriptconst res = pm.response.json();
pm.environment.set("auth_token", res.token);
console.log("Token saved:", res.token);
Đây chính là đoạn script lưu token mà bạn viết ở Bước 4 cho request login (POST /api/login) — không phải thứ nên xuất hiện ở request GET /api/products. Vì /api/products không trả về token trong response, nên nếu code này chạy trên request này, res.token sẽ là undefined (không lỗi, nhưng vô nghĩa — vì sản phẩm không có field token).
Nguyên nhân thường gặp: khi bạn tạo request mới bằng cách Duplicate (nhân bản) request login cũ, Postman copy luôn cả nội dung tab Scripts của request cũ sang request mới, và bạn quên xóa đi trước khi thêm code mới.
Cách sửa: vào đúng request GET /api/products → tab Scripts → xóa 3 dòng đầu đó đi (chỉ giữ lại phần Postbot vừa sinh ra ở dòng 5–7), vì request này không cần lưu token, nó chỉ cần test response.

Tóm lại: Postbot làm đúng việc của nó (tab bạn đang thấy là "Scripts" — bản Postman mới đổi tên tab "Tests" thành "Scripts", không sao cả, vẫn là chỗ đúng). Chỉ có 4 dòng đầu là "rác" còn sót lại từ request khác, nên xóa đi cho sạch trước khi tiếp tục Bước 6.
```

## Prompt 5
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 20:00:00
## Prompt

```text
sau đó r sao nữa, làm tiếp 2 endpoint tiếp theo hay test tiếp bước 5 ?
```
### The AI output

```text
Sau khi dọn sạch 4 dòng rác đó, bạn có 2 lựa chọn — mình khuyên làm theo thứ tự sau:
Việc cần làm ngay: Save lại request này

Xóa 3 dòng rác (const res = ..., pm.environment.set("auth_token"...), console.log(...)) → chỉ giữ lại đoạn pm.test(...) Postbot vừa sinh.
Bấm Save (Ctrl+S hoặc icon Save) để lưu request GET /api/products này lại — kể cả code, kể cả response mẫu.
Ghi chú lại kết quả vào note của bạn (bảng so sánh Ngày 2):

EndpointPostbot sinh gìGhi chúGET /api/productsstatus 200 + response time < 200msKhông test schema/field dù response là array
Sau đó: chuyển sang Bước 6 — 2 endpoint tiếp theo
Đúng, bạn chuyển sang 2 endpoint mới như task ban đầu đã định, không cần lặp lại Bước 5 nhiều lần trên cùng 1 endpoint (trừ khi bạn muốn thử prompt tùy chỉnh — nói ở dưới).
Endpoint tiếp theo — GET /api/products/1:

Tạo request mới → GET {{base_url}}/api/products/1
Send → xem response (giờ là 1 object đơn, không phải mảng)
Tab Scripts → Ask Postbot → chọn "Test for response"
Ghi lại: Postbot có sinh test khác với lúc trước không? (vì lần này response không phải array)

Endpoint tiếp theo — POST /api/cart:

Tạo request mới → POST {{base_url}}/api/cart
Tab Authorization → Bearer Token → {{auth_token}}
Body raw JSON:

json{
  "id": 1,
  "name": "Sản phẩm A",
  "price": 100000,
  "quantity": 2
}

Send → Ask Postbot → ghi lại. Đặc biệt chú ý: vì request này có gửi token + có Body, xem Postbot có tự nghĩ đến việc test "nếu thiếu token thì phải trả 401" không — thường là không, vì Postbot chỉ nhìn vào response của request bạn vừa gửi thành công, nó không tự tạo ra request lỗi để so sánh.

Gợi ý thêm (không bắt buộc, nhưng làm dữ liệu bạn phong phú hơn)
Nếu muốn, quay lại GET /api/products và thử thêm 1 prompt tùy chỉnh (đây là phần Bước 2 của Ngày 2, có thể làm sớm hơn cũng được):

Gõ vào ô prompt Postbot: "Generate tests to validate that each product in the response array has id, name, and price fields with correct types"

So sánh: prompt tùy chỉnh này có ép Postbot kiểm tra sâu hơn (đúng field, đúng kiểu) so với lúc bạn chỉ bấm nút "Test for response" mặc định không? → Đây là minh chứng cho việc "Postbot output phụ thuộc nhiều vào cách bạn hỏi" — cũng là 1 insight hay cho Section 5.3.
Tóm lại thứ tự làm: dọn sạch → Save → làm tiếp 2 endpoint mới (Bước 6) → nếu còn thời gian/credit, quay lại thử prompt tùy chỉnh trên endpoint cũ.
```

## Prompt 6
### AI tool name    
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 21:00:00
### Prompt

```text
hãy tiếp tục hướng dẫn tôi làm các phần còn lại
```

### The AI output

```text
Được, mình tiếp tục hướng dẫn từ chỗ bạn đang dừng — theo đúng thứ tự còn lại của Ngày 1 và Ngày 2.
Tiếp tục Ngày 1 — Bước 6 (2 endpoint còn lại)
6a. GET /api/products/1

Tạo request mới → chọn method GET → URL: {{base_url}}/api/products/1
Bấm Send → xem response. Response giờ sẽ là 1 object đơn (không phải mảng như lúc nãy), dạng:

json{ "id": 1, "name": "...", "price": ..., "description": "...", "category_id": ... }

Vào tab Scripts → bấm Ask Postbot → chọn "Test for response"
Ghi lại vào note của bạn:

EndpointPostbot sinh gìGhi chúGET /api/products/1? (điền sau khi test)So sánh: có khác gì với lúc test /api/products (mảng) không?

Save request lại.

6b. POST /api/cart

Tạo request mới → POST → URL: {{base_url}}/api/cart
Tab Authorization → Type: Bearer Token → Token: {{auth_token}} (token này đã có sẵn từ lúc bạn login ở Bước 4, không cần login lại)
Tab Body → raw → JSON:

json{
  "id": 1,
  "name": "Sản phẩm A",
  "price": 100000,
  "quantity": 2
}

Send. Nếu response trả về lỗi 401 → nghĩa là auth_token bị rỗng hoặc sai — quay lại kiểm tra Environment (icon con mắt) xem token có giá trị không. Nếu 200/201 → thành công.
Tab Scripts → Ask Postbot → ghi lại:

EndpointPostbot sinh gìGhi chúPOST /api/cart?Có tự nghĩ đến test 401-nếu-thiếu-token không? (thường KHÔNG)
✅ Checkpoint kết thúc Ngày 1
Chụp màn hình lúc Postbot sinh test cho ít nhất 1 trong 2 endpoint vừa làm → gửi vào nhóm chat báo tiến độ. Xong Ngày 1.

Ngày 2 — Đào sâu Postbot + So sánh Manual vs AI
Bước 1: Test endpoint phức tạp — POST /api/apply-coupon

Tạo request mới → POST → {{base_url}}/api/apply-coupon
Không cần token (endpoint này không yêu cầu Authorization header theo spec, chỉ cần user_id trong body)
Body raw JSON:

json{
  "code": "SAVE10",
  "total_amount": 500000,
  "user_id": 1
}

Send → xem response (sẽ có discount_amount và final_amount)
Ask Postbot → ghi lại kết quả.

Bước 2: Thử prompt tùy chỉnh (tiếng Anh)
Trên chính request /api/apply-coupon đó, xóa test cũ (hoặc thêm request mới), gõ vào ô prompt Postbot:

Generate tests for status code, response time, and validate that discount_amount and final_amount are numbers

So sánh với lần Ask Postbot mặc định trước đó — prompt tùy chỉnh này có ép Postbot kiểm tra sâu hơn không?
Bước 3: Test khả năng debug của Postbot
Trong tab Scripts, cố tình gõ 1 đoạn sai cú pháp, ví dụ:
javascriptpm.test("broken test" function () {
    pm.response.to.have.status(200)
});
(thiếu dấu phẩy sau "broken test") → Save → xem Postman báo lỗi gì → hỏi Postbot sửa (thường có nút gợi ý fix, hoặc bạn gõ "why is this broken?" vào ô prompt) → ghi lại nó có sửa đúng không.
Bước 4: Viết tay 5 test case cho apply-coupon (phần quan trọng nhất)
Dựa theo đúng 5 điều kiện C1–C5 trong đặc tả (README EShop), viết vào tab Scripts của request apply-coupon (hoặc tạo request riêng cho từng case để test độc lập):
javascript// Case 1: Coupon hợp lệ, đủ điều kiện (dùng SAVE10, total_amount 500000) → 200
pm.test("Valid coupon returns 200", function () {
    pm.response.to.have.status(200);
});

// Case 2: Coupon không tồn tại — đổi code thành "NOTEXIST"
pm.test("Invalid coupon code rejected", function () {
    pm.expect(pm.response.code).to.be.oneOf([400, 404]);
});

// Case 3: Coupon hết hạn — dùng code "EXPIRED"
pm.test("Expired coupon rejected", function () {
    pm.response.to.have.status(400);
});

// Case 4: Chưa đủ ngưỡng đơn hàng — dùng "BIGBUY" nhưng total_amount: 100000 (< 500,000 yêu cầu)
pm.test("Below minimum order amount rejected", function () {
    pm.response.to.have.status(400);
});

// Case 5: Vượt số lần dùng — gọi lại "SAVE10" lần thứ 2 cho cùng user_id (SAVE10 chỉ cho dùng 1 lần/người)
pm.test("Max uses per user exceeded rejected", function () {
    pm.response.to.have.status(400);
});
Cách chạy 5 case này: mỗi case cần đổi lại Body cho đúng tình huống (đổi code và total_amount) rồi Send riêng từng lần, xem response thật có đúng như bạn mong đợi không (đây cũng là lúc bạn có thể phát hiện bug thật của hệ thống — mục tiêu môn học).
Bước 5: Nhờ Postbot sinh test cho cùng endpoint, rồi điền bảng so sánh
Test CaseManual (bạn viết)Postbot (AI sinh)Nhận xétValid coupon → 200✅?Invalid coupon → 404✅?Expired coupon → 400✅?Below min order → 400✅?Max uses exceeded → 400✅?Business rule: discount formula đúng✅?Edge case: total_amount = 0✅?
Bạn tự điền cột Postbot dựa trên kết quả thật — nhưng theo kinh nghiệm chung, cột "Postbot" ở 5/7 dòng trên thường sẽ trống hoặc "không có", vì Postbot chỉ nhìn được 1 response thành công tại 1 thời điểm, nó không tự nghĩ ra các tình huống lỗi khác.
Bước 6: Note lại 2 kết luận cho Section 5.3

Postbot làm tốt: status code cơ bản, kiểu dữ liệu field, response time.
Postbot bỏ sót: toàn bộ business rule (C1–C5), test tình huống lỗi/negative case, và test phụ thuộc vào nhiều request liên tiếp (như case 5 — gọi 2 lần để test vượt lượt dùng).


✅ Checklist bạn đã hoàn thành sau khi làm xong phần này

 Login lấy token tự động
 Test Postbot trên 3+ endpoint (products, products/1, cart, apply-coupon)
 Thử prompt tùy chỉnh
 Thử debug với Postbot
 5 test case tay cho apply-coupon + bảng so sánh với Postbot

Bạn đã có đủ nguyên liệu thô để bước sang Ngày 3 (Failure Modes). Muốn mình hướng dẫn tiếp Ngày 3 luôn không?
```

## Prompt 7
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 21:30:00
### Prompt

```text
Test thêm endpoint POST /api/apply-coupon với payload mẫu đã cho. payload naof
```

### The AI output

```text
Payload mẫu là cái này (đã có sẵn trong file kế hoạch của bạn, ở phần "Postbot deep dive" Ngày 2):
json{ 
  "code": "SAVE10", 
  "total_amount": 350000, 
  "user_id": 2 
}
Cách dùng:

Tạo request mới: POST {{base_url}}/api/apply-coupon
Vào tab Body → chọn raw → JSON
Paste đúng payload trên vào
Nếu endpoint này cần xác thực (auth), nhớ thêm header Authorization: Bearer {{auth_token}} ở tab Headers hoặc Auth
Nhấn Send

Nếu bị lỗi 401/403 nghĩa là thiếu token — quay lại login lấy token mới trước (như hôm bạn tắt server, bạn đã login lại rồi thì dùng token đó luôn).
Sau khi có response thành công, bạn làm tiếp các việc của Ngày 2:

Vào tab Tests → Ask Postbot → xem nó generate ra assertion gì cho endpoint này
So sánh với 5 test case bạn tự viết tay (status 200, coupon sai → 400, hết hạn → 400, dưới ngưỡng đơn tối thiểu → 400, vượt số lần dùng → 400)
Điền vào bảng so sánh trong file kế hoạch
```

## Prompt 8
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 22:00:00
### Prompt

```text
Thử tính năng debug: cố tình viết sai 1 dòng test (VD: gõ sai tên field) → hỏi Postbot sửa → xem nó phát hiện lỗi thế nào.
Viết tay 5 test case cho /api/apply-coupon, rồi để Postbot generate cho cùng endpoint đó → điền vào bảng so sánh trong file kế hoạch. ko hieu hai buoc nay, buoc nay la lm j, muc dich gi, cu the buoc lm the nao
```

### The AI output

```text
Được, mình giải thích kỹ từng bước một, mục đích và cách làm cụ thể.
Bước 1: Thử tính năng debug của Postbot
Mục đích: Xem Postbot có khả năng "sửa lỗi" test script khi bạn viết sai không — đây là 1 tính năng khác với "generate test", và bạn cần ghi nhận nó trong User Guide (Section 5.3).
Cách làm cụ thể:

Vào 1 request bất kỳ đã có response thành công (VD GET /api/products)
Vào tab Tests
Cố tình viết 1 đoạn code test bị sai, ví dụ:

javascriptpm.test("Check status code", function () {
    pm.response.to.have.statuss(200);  // sai chính tả: "statuss" thay vì "status"
});
hoặc sai tên field không tồn tại:
javascriptpm.test("Check product name", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.productNam).to.exist;  // sai: field thật là "productName" hoặc "name"
});

Nhấn Send request → test sẽ báo lỗi/fail (vì code sai)
Bấm nút "Ask Postbot" (hoặc chọn đoạn code bị lỗi) → hỏi Postbot sửa, ví dụ gõ: "Fix this test script"
Quan sát: Postbot có tự phát hiện ra lỗi chính tả không? Nó sửa đúng không? Nó có giải thích lỗi là gì không?

Ghi lại: chụp screenshot trước/sau, note lại Postbot phát hiện được loại lỗi nào (lỗi cú pháp dễ, hay lỗi field sai tên khó hơn).

Bước 2: So sánh 5 test case tay vs Postbot cho /api/apply-coupon
Mục đích: Đây chính là số liệu quan trọng nhất cho toàn bộ nghiên cứu — chứng minh bằng dữ liệu thật là Postbot mạnh/yếu ở đâu so với con người.
Cách làm cụ thể:
2a. Viết tay 5 test case trước (không dùng AI), vào tab Tests của request POST /api/apply-coupon, tự code 5 test:
javascript// Test 1: Coupon hợp lệ → 200
pm.test("Valid coupon returns 200", function () {
    pm.response.to.have.status(200);
});

// Test 2: Coupon sai/không tồn tại → lỗi
pm.test("Invalid coupon code returns error", function () {
    pm.response.to.have.status(404); // hoặc mã lỗi thực tế của API
});

// Test 3: Coupon hết hạn → 400
pm.test("Expired coupon returns 400", function () {
    pm.response.to.have.status(400);
});

// Test 4: Đơn hàng dưới mức tối thiểu → 400
pm.test("Order below minimum returns 400", function () {
    pm.response.to.have.status(400);
});

// Test 5: Vượt số lần dùng coupon → 400
pm.test("Max uses exceeded returns 400", function () {
    pm.response.to.have.status(400);
});
Lưu ý: để test được đủ 5 case, bạn cần gửi request nhiều lần với payload khác nhau mỗi lần (VD lần 1 dùng code đúng "SAVE10", lần 2 dùng code sai "FAKE99", lần 3 dùng total_amount quá thấp...). Ghi lại kết quả thực tế của API cho mỗi case.
2b. Xóa hết code test tay đi (hoặc lưu riêng ra 1 chỗ), sau đó bấm Ask Postbot trên cùng endpoint này, để nó tự generate test.
2c. So sánh và điền bảng trong file kế hoạch:
Test CaseManualPostbotNotesValid coupon → 200✅ Có viết✅/❌ Postbot có tự nghĩ ra case này không?Invalid coupon → 404✅ Có viết✅/❌Expired coupon → 400✅ Có viết✅/❌Below min order → 400✅ Có viết✅/❌Max uses exceeded → 400✅ Có viết✅/❌Business rule: discount formula đúng✅ Có viết❌ (thường Postbot không tự kiểm tra logic tính toán)Edge case: total_amount = 0✅ Có viết❌
Kết quả bạn thường sẽ thấy: Postbot chỉ tự generate được test cho case "happy path" (status 200, kiểu dữ liệu đúng) chứ không tự nghĩ ra các case lỗi/negative như coupon sai, hết hạn, vượt giới hạn — vì nó chỉ dựa vào 1 response mẫu, không biết business logic. Đây chính là bằng chứng cho FM2 (Happy-path only) ở Ngày 3.
```

## Prompt 9
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 22:30:00
### Prompt

```text
Thử prompt tùy chỉnh, gõ thẳng vào ô chat Postbot: "Generate tests for status code, response time, and validate that each product has a positive price" → xem nó có hiểu đúng yêu cầu không. buoc nay lm xong r. ra dc postbot trong script r thi lm gi
```

### The AI output

```text
Tốt, đến bước này bạn cần làm 3 việc: chạy thử, kiểm tra kết quả, và ghi lại nhận xét.
Bước 1: Review code Postbot vừa generate ra
Đọc kỹ đoạn script Postbot viết, xem nó có đúng 3 yêu cầu bạn đã prompt không:

Có test status code không?
Có test response time không? (thường Postbot viết dạng pm.expect(pm.response.responseTime).to.be.below(200))
Có test mỗi product có price dương không? (đây là phần khó nhất — cần loop qua mảng products và check từng price > 0)

Lưu ý: đây là endpoint GET /api/products (trả về danh sách), nên phần "mỗi product có positive price" đòi hỏi Postbot phải hiểu cấu trúc là 1 mảng và lặp qua từng phần tử — đây là điểm hay để test xem AI có "hiểu" cấu trúc dữ liệu không, hay chỉ check tên field hời hợt.
Bước 2: Chạy test (nhấn Send lại)

Nhấn Send để request chạy cùng với script test vừa được thêm vào tab Tests
Xem kết quả ở tab Test Results (cạnh tab Body/Cookies ở phần Response) — mỗi test sẽ hiện PASS (xanh) hoặc FAIL (đỏ)
Nếu có test nào FAIL, xem lý do tại sao — có thể do code Postbot viết sai, hoặc do data thực tế của API không đúng như kỳ vọng

Bước 3: Ghi chú đánh giá (quan trọng nhất cho báo cáo)
Trả lời các câu hỏi sau và note lại vào tài liệu nghiên cứu của bạn:

Postbot có generate đủ cả 3 phần mình yêu cầu không, hay chỉ làm được 1-2 phần rồi bỏ qua phần khó (thường là phần "loop qua từng product check price dương" sẽ khó hơn 2 phần kia)
Code có chạy đúng không (không bị lỗi cú pháp), hay cần bạn sửa lại thủ công
Nếu request thật sự có product với giá âm/0 — Postbot code có bắt được lỗi đó không, hay chỉ code cho có mà không thực sự validate đúng logic
So sánh: nếu bạn tự viết tay đoạn test tương tự, bạn có làm khác gì so với Postbot không?

Việc này giúp bạn có ví dụ cụ thể cho phần "Custom prompts: cách hướng dẫn Postbot bằng yêu cầu cụ thể" trong Section 5.3 — bạn cần 1 screenshot của prompt + code generate ra, và 1 câu nhận xét ngắn về độ chính xác của nó.
```

## Prompt 10
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 23:00:00
### Prompt

```text
```javascript
pm.environment.set("auth_token", pm.response.json().token);

pm.test("Response has required fields", function () {
    const responseData = pm.response.json();
    
    pm.expect(responseData).to.be.an('object');
    pm.expect(responseData).to.have.all.keys('success', 'coupon_id', 'discount_amount', 'final_amount', 'message');
});


pm.test("Coupon ID is a non-negative integer", function () {
    const responseData = pm.response.json();
    
    pm.expect(responseData).to.have.property('coupon_id');
    pm.expect(responseData.coupon_id).to.be.a('number').and.to.be.at.least(0);
});


pm.test("Discount amount is a non-negative number", function () {
    const responseData = pm.response.json();
    
    pm.expect(responseData).to.be.an('object');
    pm.expect(responseData.discount_amount).to.be.a('number').and.to.be.at.least(0, "Discount amount should be non-negative");
});


pm.test("Final amount is a non-negative number", function () {
    const responseData = pm.response.json();
    
    pm.expect(responseData).to.have.property('final_amount').that.is.a('number').and.is.at.least(0, "Final amount should be non-negative");
});


pm.test("Message must be a non-empty string", function () {
    const responseData = pm.response.json();
    
    pm.expect(responseData).to.be.an('object');
    pm.expect(responseData.message).to.exist.and.to.be.a('string').and.to.have.lengthOf.at.least(1, "Value should not be empty");
});

 day la phan script do postbot generate, gio lm sao
```

### The AI output

```text
Tốt, đây là output thật của Postbot rồi — giờ bạn phân tích và điền vào bảng so sánh.
Bước 1: Nhận xét nhanh về đoạn code này
Đọc kỹ, mình thấy vài điểm đáng chú ý (bạn cũng nên tự ghi lại tương tự):

Không có test status code — Postbot bỏ qua hoàn toàn việc check pm.response.to.have.status(200). Đây là thiếu sót lạ, vì thường status code là thứ cơ bản nhất.
Chỉ kiểm tra kiểu dữ liệu và field có tồn tại (schema validation) — không kiểm tra giá trị đúng về mặt logic. Ví dụ: nó check discount_amount là số không âm, nhưng không hề tính lại xem discount_amount có đúng công thức không (VD: SAVE10 = giảm 10%, thì 350000 phải ra discount = 35000 — Postbot không biết điều này).
Hoàn toàn không có test cho case lỗi (coupon sai, hết hạn, vượt giới hạn) — vì nó chỉ dựa vào 1 response mẫu (case thành công), không tự nghĩ ra được các trường hợp khác.

Bước 2: Chạy thử script này với nhiều tình huống khác nhau
Copy đoạn code Postbot này, rồi thử chạy lại request với các payload khác để xem nó có "vô tình" pass sai không:

Payload đúng (code: "SAVE10", total_amount: 350000, user_id: 2) → chạy xem tất cả test PASS chưa
Payload sai (code: "FAKE99") → xem response trả về là gì (có thể vẫn có field success: false nhưng vẫn có đủ 5 key, nên test Postbot vẫn PASS dù bản chất là lỗi!) — đây chính là điểm hay để ghi vào Failure Mode FM1/FM2

Bước 3: Điền bảng so sánh (dùng data thật bạn vừa có)
Test CaseManualPostbotNotesValid coupon → 200✅❌ (không check status code)Postbot bỏ sót check cơ bản nhấtInvalid coupon → 404/400✅❌Postbot không tự nghĩ ra case lỗiExpired coupon → 400✅❌như trênBelow min order → 400✅❌như trênMax uses exceeded → 400✅❌như trênBusiness rule: discount formula đúng✅❌Postbot chỉ check "là số không âm", không tính lại công thứcEdge case: total_amount = 0✅❌Postbot không test edge caseSchema: đủ 5 field đúng kiểu(có thể có)✅Đây là điểm Postbot làm tốt nhất
Bước 4: Viết nhận xét tổng kết (dùng luôn cho Section 5.3 và Section 6)
Ghi 2 câu kết luận rõ ràng, ví dụ:

"Postbot làm tốt việc validate schema và kiểu dữ liệu response, nhưng hoàn toàn không tự sinh ra được test cho status code, business logic (công thức tính discount), và các trường hợp lỗi/negative — vì nó chỉ dựa trên 1 response mẫu duy nhất."
Đây chính là bằng chứng cụ thể cho FM2 — Happy-path only ở Ngày 3: Postbot chỉ test được nhánh thành công, hoàn toàn bỏ qua các nhánh lỗi mà một tester thật sẽ luôn phải viết.

Bạn đã có đủ data cho phần so sánh này rồi — tiếp theo bạn có muốn mình giúp viết luôn đoạn "Document findings" (điểm mạnh/yếu của Postbot) dựa trên data này không?
```

## Prompt 11
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 23:30:00
### Prompt
```text
nhung neu lm dung logic thi se phai nho postbot viet phan script post res voi nhung logic minh tu dinh nghia chu ko de no tu doan bang json tra ve cua 1 body bat ki dung ko ? v minh co dg lm sai ko
```
### The AI output

```text
Bạn hiểu đúng vấn đề rồi — và không, bạn không làm sai gì cả. Thực ra bạn vừa tự phát hiện ra chính xác điều mà bài nghiên cứu này muốn bạn tìm ra. Để mình làm rõ 2 ý bạn đang nhập nhằng.
Có 2 cách dùng Postbot khác nhau — và bạn đang test đúng cả 2 cách
Cách 1 — Mặc định (auto), không cho thêm chỉ dẫn:

Bạn chỉ bấm "Ask Postbot" → chọn "Test for response" → không gõ thêm gì
Postbot chỉ nhìn vào 1 response mẫu đang có trên màn hình → đoán mò xem field nào, kiểu gì → viết test dựa hoàn toàn vào cái mẫu đó
Đây chính là cách bạn vừa làm, và nó cho ra kết quả sai (discount_amount below 0) vì mẫu nó nhìn thấy đang có bug
→ Đây là hành vi mặc định/tự nhiên nhất của Postbot, và nó chính là cái bạn cần vạch trần trong Section 6 (Failure Mode)

Cách 2 — Custom prompt, tự định nghĩa logic nghiệp vụ:

Bạn gõ thẳng vào ô chat Postbot, ví dụ: "Generate a test that verifies discount_amount equals total_amount multiplied by 10 divided by 100, and must be a positive number"
Lúc này Postbot sẽ viết test theo đúng công thức bạn yêu cầu, không đoán mò từ response nữa
Đây chính là phần "Test custom prompts" đã có sẵn trong task plan Ngày 2 của bạn (bạn đã thử prompt về price dương ở phần trước rồi)

Vậy bạn cần làm gì tiếp — thử luôn Cách 2 để so sánh
Đây là bước bổ sung rất đáng giá, làm cho Section 5.3 + Section 6 mạnh hơn hẳn:

Vào lại request /api/apply-coupon (đang có response bug)
Xóa hết code Cách 1 đi (hoặc giữ riêng ra 1 chỗ để so sánh)
Gõ prompt cụ thể vào ô chat Postbot, ví dụ:

"Write a test that calculates the expected discount as total_amount * 10 / 100 for a percent coupon, and asserts discount_amount matches this calculation exactly. Also assert final_amount = total_amount - discount_amount, and both values must be positive."


Xem Postbot sinh ra code gì lần này
Chạy lại (Send) → xem test này có FAIL đúng không (vì bug thật sự đang có) — nếu FAIL đúng thì chứng tỏ khi được "mớm" đúng logic, Postbot mới bắt được bug chính xác

Kết luận quan trọng bạn sẽ viết vào báo cáo (đây là insight có giá trị nhất bạn tìm ra)

"Ở chế độ mặc định (không có chỉ dẫn), Postbot chỉ suy luận test từ 1 response mẫu duy nhất — nếu mẫu đó đang chứa bug, Postbot sẽ viết test xác nhận bug là hành vi đúng, khiến bug bị che giấu thay vì phát hiện. Chỉ khi được cung cấp prompt tùy chỉnh nêu rõ công thức nghiệp vụ, Postbot mới có thể viết test đúng để phát hiện ra sai lệch. Điều này cho thấy Postbot không tự có khả năng hiểu nghiệp vụ (business logic) — nó cần con người 'dạy' nó logic đúng trước, bản thân AI không tự suy ra được."

Đây chính là điểm khác biệt cốt lõi giữa "AI tự động hoàn toàn" và "AI cần con người dẫn dắt" — rất đáng để nhấn mạnh trong cả Section 5.3 (phần custom prompt) lẫn Section 6 (Failure Mode).
Bạn thử làm bước prompt tùy chỉnh này rồi gửi kết quả cho mình nhé, sẽ có thêm 1 bằng chứng rất mạnh.v baay gio xong buoc 3 roi thi lm gi tiep thepCharted progression pathway through remaining procedural stepsCharted progression pathway through remaining procedural stepsXong Bước 3 rồi (cả 2 cách: Postbot tự đoán + Postbot với custom prompt), giờ bạn tiếp tục làm Bước 4, 5, 6, 7 như kế hoạch — nhưng lưu ý 1 điểm quan trọng: từ giờ dùng bộ test ở Cách 2 (custom prompt, đúng công thức) để chạy tiếp, vì bộ Cách 1 đã biết là sai logic rồi.
Bước 4: Test coupon hết hạn (EXPIRED)

Đổi Body thành:

json{ "code": "EXPIRED", "total_amount": 200000, "user_id": 1 }

Nhấn Send
Xem 2 thứ:

Response body trả về gì (copy gửi cho mình)
Test Results của bộ test Cách 2 (custom prompt) — có FAIL không? (phải FAIL hoặc báo lỗi hợp lý, vì EXPIRED đã hết hạn từ 2020, không được áp dụng)



Bước 5: Test coupon không tồn tại

Đổi Body:

json{ "code": "FAKE99", "total_amount": 500000, "user_id": 1 }

Nhấn Send → copy response gửi cho mình

Bước 6: Test chưa đủ ngưỡng đơn hàng

Đổi Body:

json{ "code": "BIGBUY", "total_amount": 100000, "user_id": 1 }

BIGBUY cần tối thiểu 500,000₫, ở đây chỉ gửi 100,000₫ → phải bị từ chối
Nhấn Send → copy response gửi cho mình

Bước 7: Test vượt số lần dùng

Gửi lại đúng Body ở Bước 2 (SAVE10, total_amount 500000, user_id 1) — vì bạn đã dùng SAVE10 1 lần rồi (giới hạn 1 lần/người), lần này gửi lại phải bị từ chối
Nhấn Send → copy response gửi cho mình


Cách làm nhanh nhất: cứ đổi Body → Send → copy nguyên response JSON → dán vào đây, mình đọc và nói cho bạn biết đúng/sai theo spec, đồng thời note luôn để điền bảng so sánh. Làm xong 4 bước này là bạn có đủ dữ liệu để hoàn thành toàn bộ Ngày 2 và bắt đầu chuyển sang Ngày 3 (Failure Modes + Worksheet).
Bạn gửi kết quả Bước 4 (EXPIRED) trước nhé.
```

## Prompt 12
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-11 23:45:00
### Prompt

```text
hay giup toi viet custom prompt dung de tiep tuc test {{base_url}}/api/apply-coupon, toi chua test custom prompt
```
### The AI output

```text
Được, mình viết sẵn cho bạn 1 prompt cụ thể, bám sát đúng công thức nghiệp vụ trong tài liệu spec của repo. Bạn chỉ cần copy nguyên văn, dán vào ô chat Postbot.
Cách thực hiện

Vào request POST {{base_url}}/api/apply-coupon
Đảm bảo Body đang là case hợp lệ (SAVE10, total_amount 500000, user_id 1) và đã Send ít nhất 1 lần để có response mẫu
Vào tab Tests → mở ô chat Postbot (nút "Ask Postbot")
Xóa/thay prompt mặc định, copy đoạn dưới đây dán vào:

Write Postman tests for this coupon API response. The business rule is: for a "percent" type coupon, discount_amount must equal total_amount multiplied by discount_value divided by 100. For SAVE10, discount_value is 10, so with total_amount = 500000, the correct discount_amount should be exactly 50000, and final_amount should be exactly total_amount minus discount_amount (450000). Write assertions that:
1. Check status code is 200
2. Check discount_amount is a positive number, not negative
3. Check discount_amount equals exactly 50000 for this specific request
4. Check final_amount equals exactly 450000
5. Check final_amount is less than total_amount (a discount should reduce the total, not increase it)

Nhấn gửi prompt cho Postbot → xem nó sinh ra code gì
Nhấn Send lại request → xem Test Results: các dòng test mới này có FAIL không (nếu FAIL đúng nghĩa là nó bắt được bug thật)

Sau khi có code, gửi lại cho mình để mình đọc kết quả
Copy đoạn code Postbot vừa sinh (giống cách bạn đã làm mấy lần trước) dán vào đây, kèm theo Test Results (PASS/FAIL từng dòng) — mình sẽ:

Đọc xem prompt của bạn có "dạy" đúng Postbot không
Xác nhận nó có bắt được bug discount_amount = -4500000 không
Giúp bạn viết note so sánh giữa "Postbot tự đoán" (Bước 3 lần 1) vs "Postbot có custom prompt" (lần này) — đây chính là bằng chứng mạnh nhất cho Section 5.3 phần "Custom prompts"

Sau bước này xong, bạn quay lại làm tiếp Bước 4-7 (EXPIRED, FAKE99, BIGBUY, dùng lại SAVE10 lần 2) như kế hoạch trước.
```

## Prompt 13
### AI tool name
* Claude (Sonnet 5)
### Date and time
* 2026-07-12 00:00:00
### Prompt

```text
là postbot chỉ viết được test dựa trên json kết quả trả về hoặc prompt nghiệp vụ của tester, chứ ko tự suy luận được hả
```

### The AI output
````
Đúng, bạn tóm tắt rất chính xác — đó chính là bản chất cốt lõi rút ra được từ toàn bộ quá trình test của bạn. Để mình làm rõ thêm 1 chút cho đầy đủ.
Postbot chỉ có 2 nguồn thông tin để dựa vào

Response JSON đang hiển thị (mặc định) — nó nhìn vào cấu trúc dữ liệu (field nào, kiểu gì, giá trị hiện tại là bao nhiêu) rồi "bắt chước" lại thành assertion. Nó không biết giá trị đó có đúng về mặt nghiệp vụ hay không — nó chỉ biết "đây là giá trị đang có, tôi viết test xác nhận y hệt vậy".
Prompt tester tự gõ — nếu tester chủ động mô tả công thức/quy tắc nghiệp vụ, Postbot mới có thể viết test đối chiếu đúng, vì lúc này nó có "đáp án chuẩn" để so sánh, không cần đoán mò từ response nữa.

Nó không tự suy luận được vì thiếu 2 thứ

Không có kiến thức về domain/spec của hệ thống bạn đang test — nó không đọc api_specification.md, không biết SAVE10 là percent 10%, không biết quy tắc C1-C5. Nó chỉ thấy 1 con số JSON trước mắt.
Không có bộ nhớ về các lần gọi trước — mỗi lần bấm "Ask Postbot" là 1 lần độc lập, nhìn 1 response duy nhất. Đây chính là lý do nó không phát hiện được bug giới hạn lượt dùng coupon (Bước 7) — vì để biết "đây là lần 2" thì cần nhớ lại lần 1, mà nó không có khả năng đó.

Vì sao điều này quan trọng — nói ngắn gọn thành 1 câu cho báo cáo

"Postbot không có khả năng suy luận nghiệp vụ độc lập — nó chỉ phản chiếu lại (mirror) đúng những gì đang có trong response mẫu, hoặc làm theo đúng những gì tester mô tả qua prompt. Nói cách khác, độ chính xác của test do Postbot sinh ra phụ thuộc hoàn toàn vào (1) response mẫu có đúng hay không, và (2) tester có cung cấp đủ ngữ cảnh nghiệp vụ hay không — bản thân AI không tự bù đắp được phần thiếu sót này."

Câu này bạn dùng được thẳng vào phần kết luận của Section 5.3 hoặc câu mở đầu cho Section 6 — nó chính là "luận điểm trung tâm" (thesis statement) xuyên suốt cả 3 Failure Mode bạn đã tìm ra.
````
