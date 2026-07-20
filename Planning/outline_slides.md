# Outline — Seminar_Slides.pptx (Stage S6)

> Dùng cho bài trình bày 15 phút (bao gồm giới thiệu, lý thuyết và định hướng live demo). 
> Mục tiêu slide count: **16 slides** (giới hạn tối ưu từ 15-17 slides).
> **Người phụ trách:** Thuận (chủ trì) + Hoàng (review)

---

## Slide 1: Title Slide (Giới thiệu chung)
- **T06 — API & Contract Testing with Postman + Postbot**
- Tên nhóm (Group 04), thành viên thực hiện (23127047 - Hoàng, 23127052 - Huy, 23127104 - Phương, 23127125 - Thuận, 23127462 - Quang), môn học Kiểm thử phần mềm.
- Logo Postman + logo Postbot AI.

## Slide 2: Why API Testing? (Tại sao cần kiểm thử API?)
- API là "xương sống" kết nối Frontend - Backend và tích hợp hệ thống.
- Phát hiện lỗi sớm (Shift-Left Testing): Chi phí sửa lỗi ở tầng API rẻ hơn nhiều so với khi đã lên tầng UI/GUI.
- Tốc độ chạy test cực nhanh, dễ dàng tự động hóa so với kiểm thử giao diện.
- *Nhấn mạnh:* Một lỗi API nghiêm trọng có thể làm sập toàn bộ các client (web, mobile app) cùng lúc.

## Slide 3: Topic Scope — Phạm vi đề tài T06
- Trọng tâm đề tài:
  - **API Testing:** Viết Test Scripts tự động hóa luồng nghiệp vụ trên Postman.
  - **Contract Testing:** Xác thực giao diện dữ liệu (JSON Schema) giữa client & server.
  - **AI-Augmented Testing:** Ứng dụng Postbot (AI assistant) để sinh test tự động.
- Giới hạn: Đề cập ngắn gọn so sánh với Pact (CDC chuyên sâu cho microservices) nhưng không đi sâu thực hành.

## Slide 4: Tool Choice — Tại sao chọn Postman + Postbot?
- **Postman:** Tiêu chuẩn ngành (35M+ người dùng), trực quan, hỗ trợ JavaScript scripting mạnh mẽ.
- **Postbot:** Tích hợp AI trực tiếp trong Postman, sinh test code tự động bằng ngôn ngữ tự nhiên (Natural Language).
- **EShop SUT:** Hệ thống e-commerce monolithic dùng Express/Node.js lý tưởng cho Postman test suite.
- Chi phí: Free tier (50 AI credits/tháng) hoàn toàn đủ cho nhu cầu học tập và seminar.

## Slide 5: Tổng quan tính năng chính của Postman
- **Request Builder & Auth Inheritance:** Quản lý tập trung thông tin đăng nhập.
- **Variables & Variable Chaining:** Truyền dữ liệu động giữa các API để chạy E2E flow.
- **Newman CLI:** Đưa Postman test suite chạy độc lập trên terminal và tích hợp CI/CD.
- **Postbot:** Trợ lý AI tăng tốc độ viết assertion code.

## Slide 6: Basic API Requests (4 HTTP Methods cơ bản)
- GET (Lấy dữ liệu), POST (Tạo mới), PUT (Cập nhật), DELETE (Xóa).
- Ý nghĩa các HTTP Status Codes phổ biến cần kiểm tra: `200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `404 Not Found`.
- *Mẹo Postman:* Thiết lập Authorization (Bearer Token JWT) ở cấp Collection để tất cả request con tự động kế thừa (Auth Inheritance).

## Slide 7: API Authentication Patterns (Cơ chế Xác thực)
- Các cơ chế xác thực REST API phổ biến: Bearer Token (JWT), API Key, Basic Auth, Session Cookie.
- EShop SUT sử dụng **JWT Bearer Token** (Cơ chế phổ biến nhất hiện nay).
- *Tự động hóa:* Sử dụng Post-response script trên Request Login để tự động lưu Token vào biến môi trường `auth_token`, loại bỏ hoàn toàn việc copy-paste thủ công.

## Slide 8: Scripting in Postman (Pre-request vs. Post-response)
- **Pre-request Scripts:** Chạy trước khi request đi (Ví dụ: sinh email ngẫu nhiên để tránh lỗi trùng lặp khi test API Register).
- **Post-response Scripts (Tests):** Chạy sau khi nhận response (Dùng Chai.js để viết assertions).
- **Variable Chaining (Chuỗi biến thế):** Lấy dữ liệu từ API này làm đầu vào cho API kế tiếp (Ví dụ: `Login Token → Add to Cart → Checkout`).

## Slide 9: Contract Testing với Postman (Xác thực Hợp đồng)
- Bản chất: Đảm bảo giao diện dữ liệu (data structure) không bị phá vỡ giữa Frontend và Backend.
- **JSON Schema Validation:** Sử dụng công cụ **Ajv Validator (v6.12.5)** tích hợp sẵn trong Postman để xác thực cấu trúc qua lệnh `pm.response.to.have.jsonSchema(schema)`.
- Kiểm tra chặt chẽ: Kiểu dữ liệu (`integer`, `string`), các trường bắt buộc (`required`), và định dạng (`format: email`).
- **Living Contracts:** Lưu trữ các ví dụ phản hồi (Saved Examples) trên Postman để tạo Mock Servers.

## Slide 10: Collection Runner & Newman CLI (Tự động hóa chạy loạt)
- **Collection Runner GUI:** Chạy toàn bộ E-commerce flow tự động với 1 nút bấm trên Postman.
- **Newman CLI:** Chạy bộ test suite trực tiếp từ terminal (`newman run collection.json -e env.json`).
- **Data-Driven Testing (DDT):** Nạp dữ liệu ngoài từ file **CSV/JSON** để chạy lặp kiểm thử (Mục 5.2 trong User Guide) - chạy nhiều tài khoản đăng nhập để kiểm tra validation.
- Xuất báo cáo kiểm thử phong phú (HTML Report, JUnit XML cho CI/CD).

## Slide 11: Ứng dụng Postbot AI trong sinh Test tự động
- Truy cập dễ dàng: Nút "Ask Postbot" tại tab Tests.
- Phân tích cấu trúc Response trả về thực tế để tự động tạo mã JavaScript assertions.
- Sử dụng câu lệnh ngôn ngữ tự nhiên để chỉ định logic kiểm thử phức tạp (ví dụ: *"Write a test that checks if discount_amount is 10% of total_amount"*).

## Slide 12: Giới hạn của Postbot AI
- **Chỉ tập trung Happy-path:** Bỏ sót các ca kiểm thử lỗi (negative tests) và biên (edge cases).
- **Phụ thuộc vào Response mẫu:** Nếu API chưa chạy hoặc server sập, Postbot không thể hoạt động.
- **Thiếu kiểm tra logic nghiệp vụ:** Chỉ kiểm tra cấu trúc bề mặt, không biết tính đúng đắn của dữ liệu nghiệp vụ nếu không được hướng dẫn bằng prompt chi tiết.

## Slide 13: Failure Modes — 3 Cách Tool Đánh Lừa Bạn (Quan trọng nhất!)
- **FM1 — Anchoring on a Buggy Response (Học theo bug):** Postbot tự động viết assertion pass cho một giá trị lỗi (ví dụ: discount_amount âm `-4,500,000₫` trong EShop).
- **FM2 — Superficial Error Tests (Kiểm thử lỗi hời hợt):** Postbot chỉ check xem có trường `error` kiểu string không, bỏ qua việc kiểm tra đúng loại lỗi nghiệp vụ hoặc các lỗi ràng buộc trạng thái (stateful - ví dụ: mã giảm giá quá giới hạn sử dụng).
- **FM3 — Runner Hides Environment Mismatch (Lỗi chọn sai môi trường):** Collection Runner có bộ chọn môi trường độc lập, dễ dẫn đến việc các biến `{{auth_token}}` không phân giải được nhưng runner không cảnh báo rõ ràng.

## Slide 14: So sánh Postman vs. Pact (Contract Testing)
- **Pact (Dedicated CDC):** Consumer-driven chuyên sâu cho microservices, phát hiện phá vỡ hợp đồng trong CI/CD build. Cực kỳ phức tạp và tốn chi phí hạ tầng (Pact Broker).
- **Postman (Lightweight):** Rất nhẹ, dễ thiết lập qua JSON Schema validation + Mock Server.
- *Kết luận:* Với kiến trúc Monolith như EShop SUT, dùng **Postman là tối ưu nhất** về mặt chi phí và hiệu quả học tập.

## Slide 15: Live Demo Preview — Kịch bản chạy thực tế
- Trình diễn chạy trực tiếp bộ collection trên Postman đối với **EShop SUT**:
  - Chạy chuỗi workflow thành công (Login → Product Detail → Cart → Checkout).
  - Trình diễn phát hiện **1 bug nghiệp vụ** của SUT thông qua kịch bản test lỗi (Negative Contract Tests): `GET /api/products/999` (Product Not Found) trả về `200 OK` thay vì `404 Not Found`
- Audience activity: thực hành trên repo demo riêng của team

## Slide 16: Key Takeaways & Q&A
- Postman không chỉ để gửi request thủ công: Scripting giúp tự động hóa và AI (Postbot) giúp tăng tốc độ.
- AI tốt cho cấu trúc cơ bản, nhưng con người vẫn giữ vai trò quyết định thiết lập logic nghiệp vụ và ca kiểm thử lỗi (negative tests).
- API Testing là nền tảng cốt lõi trước khi kiểm thử tích hợp UI.
- *Transition:* *"Bây giờ chúng ta sẽ chuyển sang phần Live Demo thực tế..."*

---

## 💡 Lưu ý cốt lõi cho buổi thuyết trình (15 phút)

1.  **Phân bổ thời gian (Time Boxing):**
    *   *Slide 1-5 (Giới thiệu, lý thuyết nền tảng):* **3 phút**.
    *   *Slide 6-10 (Cách viết Script, Authentication, Contract Testing, Newman, DDT):* **5 phút**.
    *   *Slide 11-14 (Postbot AI, Giới hạn, Failure Modes, so sánh Pact):* **4 phút**.
    *   *Slide 15-16 (Giới thiệu Demo & Kết luận):* **2 phút**.
    *   *Thời gian còn lại:* Dành cho Live Demo và Q&A.
2.  **Điểm nhấn mạnh (Core Highlights):**
    *   Hãy nhấn mạnh **Slide 13 (Failure Modes)**. Đây là phần đắt giá nhất chứng minh nhóm có sự nghiên cứu phê phán sâu sắc về AI (Postbot), chỉ rõ AI có thể sinh code xác thực cho bug của dev như thế nào.
    *   Nhấn mạnh tính năng **DDT (Data-Driven Testing)** ở Slide 10 vì đây là kỹ thuật nâng cao thể hiện tư duy automation tốt.
3.  **Tính đồng bộ:**
    *   Đảm bảo lúc demo sẽ chạy chính xác các thư mục `Contract Testing` trong bộ sưu tập mà nhóm đã thiết kế.
