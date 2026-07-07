# Bảng Phân Công Nhiệm Vụ — Team 5 (T06 API & Contract Testing)

## Bảng phân công chi tiết

| Thành viên | Vai trò | Nội dung nghiên cứu | Deliverable phụ trách | Ghi chú |
|------------|---------|-------------------|---------------------|---------|
| **Hoàng** | Team Lead / PM + Researcher nhẹ | **API Authentication Testing Patterns**: các cơ chế xác thực API phổ biến (JWT, Bearer token, session cookie, API key) và cách test từng loại bằng Postman. Hiểu tổng quan các topic để review và điều phối. | Setup repo Git, nộp Tool_Survey_Proposal.md (đã xong), điều phối timeline, review tất cả deliverable, viết mục **Introduction** + **Authentication Testing Patterns** + **References** trong User_Guide.md, tổng hợp AI Audit [AI-02]/[AI-03]/[AI-04], viết Final_Reflection.md | Nghiên cứu authentication patterns là task nhẹ — không cần demo live, chỉ viết User Guide. Phải tự chạy được Postman: import Collection, gửi request, xem kết quả test. |
| **Quang** | Researcher — Install & Setup + Collection Runner + Newman | **Install & Setup + Basic API Requests + Collection Runner + Newman CLI + Data-Driven Testing**: cài đặt Postman, tạo environment, demo nhanh các HTTP method cơ bản (GET, POST, PUT, DELETE). Collection Runner GUI, cài đặt và chạy Newman, export Collection JSON, generate HTML/JSON/JUnit report, iteration data (CSV/JSON) | Viết mục **Install & Setup**, **First Test** (basic API requests), **Advanced Usage (phần Collection Runner & Newman + Data-Driven Testing)**, và **Troubleshooting** trong User_Guide.md. Ghi hình Demo_Screencast.mp4 (cùng Thuận). | Phải tự chạy được Postman: cài đặt, gửi mọi loại request cơ bản, export Collection, chạy Newman CLI, đọc được report output. |
| **Huy** | Researcher — Test Scripts | **Test Scripts có logic thực sự**: Pre-request Scripts, Test Scripts, `pm.test()`, `pm.expect()`, variable chaining (auth token flow), assertion nâng cao (body fields, data types, response time), **Negative Testing & Error Handling** (test 401, 400, boundary values, error response structure) | Viết mục **Advanced Usage (phần Test Scripts + Negative Testing)** trong User_Guide.md. Hỗ trợ Phương verify Postbot output. | Phải tự chạy được Postman: viết được pre-request script + test script hoàn chỉnh cho flow login→products→cart→order, bao gồm cả negative test cases. |
| **Phương** | Researcher — Postbot (AI-augmented) | **Postbot (AI-augmented)**: cách Postbot sinh test assertions, so sánh output Postbot vs. manual tests, phân tích hallucinated fields, business rules gap, giới hạn free tier (50 credits/tháng) | Viết mục **Advanced Usage (phần Postbot)** và mục **Failure Modes** trong User_Guide.md. Chuẩn bị **Activity_Worksheet.md** (cùng Huy hỗ trợ). | Phải tự chạy được Postman: dùng Postbot sinh test cho ít nhất 3 endpoint, so sánh với manual test đã viết. |
| **Thuận** | **Presenter** | Hiểu tổng quan các topic ở mức demo được. Nghiên cứu sâu: **EShop SUT endpoints** — nắm rõ API specification của giảng viên để build Collection chính xác. | Chuẩn bị **Seminar_Slides.pptx** (≤15 slides). Build **EShop Postman Collection + Environment** cho live demo. Luyện demo ≥3 lần. Trình bày live demo 10 phút. Hỗ trợ Q&A. | **Scope research nhẹ hơn** — không cần viết sâu vào User Guide. Ưu tiên luyện tập demo mượt, không đọc slide. Phải tự chạy được Postman trên EShop SUT mà không cần hỗ trợ. |

---

## Bảng Deliverable & Deadline

| # | Deliverable | Người chủ trách | Stage | Deadline tương đối |
|---|-------------|-----------------|-------|-------------------|
| 1 | **Tool_Survey_Proposal.md** | Hoàng (đã nộp) | S1 | Tuần 6 — **ĐÃ HOÀN THÀNH** |
| 2 | **User_Guide.md** (≥ 6 sections) | Hoàng + Quang + Huy + Phương (đồng chủ trì, Hoàng review) | S4 | Tuần 8–9 (trước pre-share ≥3 ngày) |
| 3 | **Demo_Screencast.mp4** (5–8 phút) | Quang (quay & edit) + Thuận (thuyết minh) | S4 | Tuần 8–9 (cùng User Guide) |
| 4 | **Activity_Worksheet.md** | Phương (chủ trì) + Huy (hỗ trợ) | S5 | ≥3 ngày trước seminar live |
| 5 | **Seminar_Slides.pptx** (≤ 15 slides) | Thuận (chủ trì) + Hoàng (review) | S6 | ≥3 ngày trước seminar live |
| 6 | **Audience_Feedback_Aggregated.md** | Hoàng (thu thập trong buổi seminar) | S7 | Ngay sau buổi seminar |
| 7 | **[AI-02] AI Audit Report** (5 sections, ≥600 words) | Hoàng (chủ trì, thu thập input từ cả team) | S8 | ≤5 ngày làm việc sau seminar |
| 8 | **[AI-03] AI Disclosure** (signed PDF per member) | Hoàng (phân phối template, mỗi người tự ký) | S8 | ≤5 ngày làm việc sau seminar |
| 9 | **[AI-04] Reflective Statement** (300 words) | Hoàng (viết) + cả team review | S8 | ≤5 ngày làm việc sau seminar |
| 10 | **Final_Reflection.md** | Hoàng (viết chính) + cả team đóng góp ý | S8 | ≤5 ngày làm việc sau seminar |
| 11 | **Peer_Review.md** (review 2 team bạn) | Huy + Quang (đồng chủ trì) | S8 | Theo phân công của giảng viên |

---

## Timeline tổng quan (5 người, part-time, 3–4 tuần)

```
Tuần 6:   S1 nộp Proposal ✓ → S2 nhận Approval
Tuần 6–7: S3 — Cả team cài Postman, luyện tập độc lập trên EShop SUT
          Hoàng: setup repo, review tiến độ, nghiên cứu API auth patterns
          Quang: sâu Install & Setup + Basic API Requests + Collection Runner + Newman + Data-Driven Testing
          Huy: sâu Test Scripts + Negative Testing
          Phương: sâu Postbot
          Thuận: build EShop Collection + luyện demo
Tuần 7–8: S4 — Viết User_Guide.md + quay Screencast
Tuần 8–9: S5 — Pre-share: push User Guide + Screencast + Activity Worksheet lên Moodle
Tuần 9–11: S6 — LIVE SEMINAR (tuần cụ thể do giảng viên xếp)
Sau seminar ≤5 ngày: S8 — AI Audit + Reflection + Peer Review
```

---

## Quy tắc làm việc

1. **Mọi thành viên phải tự chạy Postman độc lập** — không ai được "chỉ đọc tài liệu mà không thực hành." Checkpoint cuối tuần 7: mỗi người gửi screenshot chạy thành công flow login→products cho Hoàng.
2. **Review chéo**: Huy review phần viết của Quang và ngược lại. Phương review Failure Modes với Huy (vì liên quan Test Scripts).
3. **Không thành viên nào chịu quá 2 topic nghiên cứu chính** — bảng trên đã tuân thủ nguyên tắc này.
4. **Meeting tối thiểu**: 1 lần/tuần (30 phút) để sync tiến độ. Trước deadline 3 ngày: meeting daily standup (15 phút).
