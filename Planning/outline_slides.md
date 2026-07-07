# Outline — Seminar_Slides.pptx (Stage S6)

> Dùng cho Pitch 10 phút (0:00–0:10). ≤ 15 slides. Không chứa bước demo — demo là Postman thật.
> **Người phụ trách:** Thuận (chủ trì) + Hoàng (review)

---

## Slide 1: Title Slide
- **T06 — API & Contract Testing with Postman + Postbot**
- Tên team, tên thành viên, môn CS423/CSC15003
- Logo Postman + logo Postbot

## Slide 2: Why API Testing?
- API là "xương sống" của mọi ứng dụng hiện đại (web, mobile, microservices)
- Một API bug = hàng nghìn user bị ảnh hưởng trước khi UI kịp hiển thị lỗi
- API testing phát hiện bug sớm hơn, nhanh hơn, rẻ hơn GUI testing

## Slide 3: Topic Scope — T06 là gì?
- Test contract giữa client và server — không chỉ response code, mà cả cấu trúc data
- Kỹ thuật truyền thống: Postman (manual + scripted testing)
- Hướng AI: Postbot (AI sinh test), AI test gen từ OpenAPI spec
- Contract testing với Pact — đề cập ngắn gọn, không đi sâu

## Slide 4: Tại sao chọn Postman?
- 35M+ users — industry standard cho API testing
- Free tier đủ dùng cho sinh viên (1 user, 50 AI credits/tháng)
- GUI trực quan + scripting mạnh (JavaScript + Chai.js)
- Tích hợp sẵn Postbot — không cần tool AI riêng

## Slide 5: Các Tính Năng Chính
- **Test Scripts** — JavaScript assertions vượt xa `status === 200`
- **Collection Runner + Newman** — chạy hàng loạt, xuất report
- **Postbot** — AI sinh test assertions, so sánh phê phán

## Slide 6: Basic API Requests
- GET, POST, PUT, DELETE — 4 HTTP method cơ bản của API testing
- Ý nghĩa status codes: 200, 201, 400, 401, 404
- Tab Authorization: set Bearer Token ở cấp Collection → mọi request con tự động có header

## Slide 7: API Authentication Patterns
- 4 cơ chế phổ biến: Bearer Token/JWT, API Key, Basic Auth, Session Cookie
- Postman hỗ trợ sẵn tab Authorization cho mọi loại
- EShop SUT dùng JWT Bearer Token — pattern phổ biến nhất cho REST API

## Slide 8: Test Scripts
- Pre-request Scripts: tạo dữ liệu động trước khi gửi request
- Variable chaining: login → token → inject vào mọi request sau
- Assertion: body fields, data types, response time, schema validation
- **Không demo ở slide này — xem live demo**

## Slide 9: Collection Runner + Newman
- Collection Runner GUI: chạy toàn bộ E-commerce flow 1 lần bấm
- Newman CLI: chạy từ terminal, xuất HTML/JSON/JUnit report
- Thể hiện tư duy automation — không test từng request thủ công
- Newman chạy từ CLI phù hợp tích hợp vào bất kỳ pipeline nào

## Slide 10: Postbot (AI-Augmented)
- Tích hợp sẵn trong Postman — tab Tests → “Ask Postbot”
- Phân tích response → sinh JavaScript assertions tự động
- Hỗ trợ natural language prompt
- Free tier: 50 credits/tháng

## Slide 11: Postbot — Giới Hạn Cần Biết
- Chỉ tạo happy-path tests — bỏ sót negative tests, edge cases
- Có thể bịa field không tồn tại (hallucinated fields)
- Phụ thuộc response mẫu — không sinh test khi chưa có response
- Business rules phải do con người viết

## Slide 12: Failure Modes — 3 Cách Tool Đánh Lừa Bạn
1. Postbot bịa field → false negative → developer sửa sai hướng
2. Postbot chỉ cover happy path → coverage ảo → API crash khi nhận input lạ
3. Collection Runner giấu lỗi gốc → debug mất thời gian

## Slide 13: So Sánh Postman vs. Alternatives
- RestAssured (Java): mạnh nhưng cần code Java, learning curve cao hơn
- Karate (Gherkin): hay cho BDD nhưng setup phức tạp hơn
- **Pact (Contract Testing):** bắt buộc khi có nhiều microservices — đảm bảo provider và consumer không phá hợp đồng API. EShop monolith thì overkill, nhưng microservices thì không thể thiếu
- **Postman thắng ở: ease of use + AI tích hợp sẵn + community lớn**

## Slide 14: EShop SUT — Demo Preview
- EShop exposes Express REST API: auth, products, cart, orders
- Live demo sẽ chạy flow: Login → Browse Products → Add to Cart → Place Order
- Audience activity: thực hành trên repo demo riêng của team

## Slide 15: Key Takeaways + Q&A
- Postman: không chỉ “gọi API thủ công” — scripting + automation + AI
- Postbot: tốt cho starting point, nhưng **luôn review và bổ sung thủ công**
- API testing không thay thế security/performance testing — nhưng là nền tảng
- “Giờ chúng ta chuyển sang live demo — không dùng slides nữa”

---

## Ghi chú cho Thuận (Presenter)

- **Tổng: 15 slides** — vừa đủ giới hạn
- Thời lượng pitch: 10 phút = trung bình 40 giây/slide
- Slide 6 nói nhanh — basic API chỉ cần overview
- Slide 7 nói nhanh — auth patterns chỉ cần overview
- Slide 8, 9, 10 nói nhanh — nội dung chi tiết sẽ demo live
- Không đọc slide — nói tự nhiên, dùng slide làm visual support
- Slide 12 (Failure Modes) là slide quan trọng nhất — thể hiện tư duy phê phán
