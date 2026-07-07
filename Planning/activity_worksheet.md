# Activity Worksheet — Spec-to-Tests Sprint

> **Thời gian:** 20 phút (0:20–0:40) — Audience hands-on
> **Chạy trên:** Repo demo do team tự dựng (Node.js/Express)
> **Người phụ trách:** Phương (chủ trì) + Huy (hỗ trợ)
> **Yêu cầu T06:** "Activity worksheet MUST be reproducible by a peer team in ≤ 25 minutes without your help."

---

## Phần A: Setup (2 phút — Facilitator verify nhanh)

> **Lưu ý:** Audience đã nhận `audience_setup_guide.md` trước ≥ 3 ngày và cài đặt sẵn. Facilitator chỉ cần verify nhanh.

### Verify nhanh (Facilitator hướng dẫn):
1. “Mọi người đã chạy `npm start` chưa?” — Server phải chạy tại `http://localhost:3000`
2. “Mở Postman, chọn Environment **Activity Env**, gửi thử `GET /api/products`” — phải thấy JSON response
3. Nếu ai gặp lỗi → Facilitator hỗ trợ nhanh (xem Troubleshooting trong setup guide)

### Chia nhóm:
- **Nửa trái lớp (Nhóm A):** Viết test thủ công — KHÔNG dùng Postbot hay AI
- **Nửa phải lớp (Nhóm B):** Dùng Postbot/AI để sinh test

---

## Phần B: OpenAPI Snippet cho Activity

Endpoint test: **POST /api/coupon/redeem**

```yaml
paths:
  /api/coupon/redeem:
    post:
      summary: Redeem a discount coupon
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - coupon_code
                - user_id
              properties:
                coupon_code:
                  type: string
                  example: "SAVE20"
                user_id:
                  type: integer
                  example: 1
      responses:
        '200':
          description: Coupon applied successfully
          content:
            application/json:
              schema:
                type: object
                properties:
                  success:
                    type: boolean
                  discount_percent:
                    type: number
                  message:
                    type: string
        '400':
          description: Invalid coupon code
          content:
            application/json:
              schema:
                type: object
                properties:
                  error:
                    type: string
        '409':
          description: Coupon already redeemed
          content:
            application/json:
              schema:
                type: object
                properties:
                  error:
                    type: string
```

**Business rules (KHÔNG có trong spec — audience phải tự nghĩ ra hoặc AI phải gợi ý):**
- Mỗi user chỉ redeem coupon 1 lần
- Coupon hết hạn không redeem được
- Coupon code không phân biệt hoa/thường
- Discount không vượt quá 100%

---

## Phase 1: Viết Tests (10 phút — 0:23 đến 0:33)

### Nhóm A — Viết thủ công

Nhiệm vụ: Mở request `POST /api/coupon/redeem` trong Collection → tab **Scripts** → viết **5 test cases** bằng JavaScript Postman.

Gợi ý framework:
```javascript
pm.test("Tên test", function () {
    // assertion
});
```

Viết vào worksheet (giấy hoặc file text):
| # | Tên Test Case | Input | Expected Output | Assertion Code (nếu viết được) |
|---|--------------|-------|-----------------|-------------------------------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |

### Nhóm B — Dùng Postbot / AI

Nhiệm vụ: Mở request `POST /api/coupon/redeem` → gửi request 1 lần → tab **Tests** → **“Ask Postbot”** → gõ prompt:
> "Generate tests for this coupon redeem endpoint. Cover status codes, response body validation, and edge cases."

Nếu Postbot hết credits: dùng ChatGPT/Claude với prompt tương tự + paste OpenAPI snippet ở trên.

Ghi lại output AI vào worksheet:
| # | Tên Test Case do AI sinh | Assertion Code AI tạo | Bạn có review/sửa không? |
|---|------------------------|----------------------|------------------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

---

## Phase 2: So Sánh Manual vs. AI (5 phút — 0:33 đến 0:38)

**Facilitator:** Ghép 1 người Nhóm A với 1 người Nhóm B. Cùng điền bảng so sánh:

| Tiêu chí | Nhóm A (Thủ công) | Nhóm B (AI/Postbot) |
|----------|-------------------|---------------------|
| Số lượng test cases tạo được | | |
| Có test status code 200 không? | ☐ Có ☐ Không | ☐ Có ☐ Không |
| Có test status code 400 (invalid coupon) không? | ☐ Có ☐ Không | ☐ Có ☐ Không |
| Có test coupon đã redeem (409) không? | ☐ Có ☐ Không | ☐ Có ☐ Không |
| Có test business rule "coupon hết hạn" không? | ☐ Có ☐ Không | ☐ Có ☐ Không |
| Có test business rule "discount ≤ 100%" không? | ☐ Có ☐ Không | ☐ Có ☐ Không |
| Có assertion nào tham chiếu field KHÔNG có trong spec? | ☐ Có ☐ Không | ☐ Có ☐ Không |
| Thời gian hoàn thành (phút) | | |
| Có cần sửa code AI sinh ra không? | N/A | ☐ Có ☐ Không |

---

## Phase 3: Xác Định Gap (5 phút — 0:38 đến 0:43)

**Mỗi cặp trả lời 3 câu hỏi:**

### Câu 1: AI bỏ sót business rule nào?
> _Ghi lại business rule mà AI KHÔNG tạo test cho:_
>
> ___________________________________________________________________

### Câu 2: AI có bịa field không?
> _Kiểm tra code AI sinh — có assertion nào tham chiếu field không tồn tại trong OpenAPI spec không?_
>
> ___________________________________________________________________

### Câu 3: Kết luận — Khi nào nên dùng AI, khi nào nên viết tay?
> _Viết 1–2 câu kết luận:_
>
> ___________________________________________________________________

---

## Answer Key (dành cho Facilitator Team)

### 5 Test Cases tối thiểu cần có:

| # | Test Case | Status Code | Assertion chính |
|---|-----------|-------------|-----------------|
| 1 | Redeem coupon hợp lệ | 200 | `success === true`, `discount_percent` là number > 0 |
| 2 | Coupon code không tồn tại | 400 | `error` field là string, chứa "invalid" |
| 3 | Redeem coupon đã dùng | 409 | `error` field tồn tại |
| 4 | Coupon hết hạn (business rule) | 400 hoặc 409 | Response chứa "expired" |
| 5 | Thiếu field bắt buộc (coupon_code) | 400 | Response trả lỗi validation |

### Expected gaps của AI:

| Gap | Mô tả |
|-----|-------|
| **Business rule: coupon hết hạn** | OpenAPI spec không có field `expiry_date` → AI không biết coupon có hạn → không sinh test |
| **Business rule: redeem 1 lần** | Spec có response 409 nhưng AI thường không tự sinh test cho trường hợp này |
| **Race condition** | 2 request redeem cùng lúc → AI không bao giờ gợi ý |
| **Coupon code case-insensitive** | Spec không ghi → AI assume case-sensitive |
| **Hallucinated fields** | AI có thể assert `responseJson.user_name` (không có trong spec) hoặc `responseJson.expires_at` |

### Kết luận mong đợi:

> "AI tạo test nhanh cho schema validation (status code, field types) nhưng bỏ sót business rules không có trong spec. Negative tests và edge cases cần con người suy nghĩ từ domain knowledge."

---

## Rubric chấm Facilitator Team

> Thiết kế dựa trên tiêu chí từ Seminar_Guide.docx Section 6 (rubric) — mục "In-class hands-on activity" (20%).

| Tiêu chí | Điểm tối đa | Mô tả mức đạt |
|----------|-------------|---------------|
| **Tính khả thi** | 4đ | Activity hoàn thành trong ≤ 25 phút. Audience không cần hỗ trợ ngoài worksheet. |
| **Reproducibility** | 4đ | Peer team khác đọc worksheet và tự chạy được mà không cần team T06 giải thích. Repo demo chạy ổn định, Collection import thành công. |
| **So sánh Manual vs. AI** | 4đ | Bảng so sánh Phase 2 rõ ràng, có checkbox cụ thể. Audience thấy được sự khác biệt. |
| **Gap identification** | 4đ | Phase 3 buộc audience suy nghĩ phê phán. Có ≥ 3 business rules AI bỏ sót. |
| **Rubric cho peers** | 2đ | Có rubric hoặc tiêu chí rõ ràng để audience teams tự đánh giá kết quả. |
| **Engagement** | 2đ | Audience tích cực tham gia, không chỉ ngồi nghe thụ động. Facilitator di chuyển giữa các nhóm. |
| **Tổng** | **20đ** | |
