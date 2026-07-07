# Hướng Dẫn Chuẩn Bị Trước Seminar — T06 API & Contract Testing

> **Gửi cho audience ≥ 3 ngày trước buổi seminar.**
> **Mục tiêu:** Khi vào seminar, mọi thứ đã cài sẵn — chỉ cần mở laptop và thực hành ngay.

---

## 1. Cài đặt phần mềm

### Postman Desktop App (BẮT BUỘC)
1. Download tại: https://www.postman.com/downloads/
2. Cài đặt → mở Postman → **Sign up** tài khoản Free (dùng email cá nhân)
3. Tài khoản Free cho bạn: 1 workspace, 50 AI credits/tháng (dùng cho Postbot)

### Node.js (BẮT BUỘC)
1. Download tại: https://nodejs.org/ (chọn **LTS version**)
2. Cài đặt → mở terminal, verify:
```bash
node --version    # Phải hiện v18+ hoặc v20+
npm --version     # Phải hiện 9+ hoặc 10+
```

### Newman CLI (BẮT BUỘC)
```bash
npm install -g newman
npm install -g newman-reporter-html
```
Verify:
```bash
newman --version  # Phải hiện version number
```

---

## 2. Clone & chạy repo demo

### Bước 1: Clone repo
```bash
git clone https://github.com/[team-repo]/eshop-demo-api.git
cd eshop-demo-api
```

### Bước 2: Cài dependencies
```bash
npm install
```
> **QUAN TRỌNG:** Chạy `npm install` TRƯỚC buổi seminar. Trong lớp có thể wifi yếu, `npm install` mất 2–5 phút.

### Bước 3: Chạy server
```bash
npm start
```
Server sẽ chạy tại `http://localhost:3000`

### Bước 4: Verify server
Mở browser → truy cập `http://localhost:3000/api/products` → phải thấy JSON response với danh sách products.

---

## 3. Import Postman Collection & Environment

### Bước 1: Import Collection
1. Mở Postman → **Import** (góc trái trên)
2. Chọn file `postman/Activity_Collection.json` trong thư mục repo
3. Collection "Activity Collection" sẽ hiện ở sidebar

### Bước 2: Import Environment
1. **Import** → chọn file `postman/Activity_Environment.json`
2. Góc phải Postman → dropdown Environment → chọn **"Activity Env"**

### Bước 3: Verify kết nối
1. Trong Collection, mở request `GET /api/products`
2. Nhấn **Send**
3. Nếu thấy response JSON với products → **setup thành công** ✅
4. Nếu lỗi `ECONNREFUSED` → kiểm tra server đang chạy (`npm start`)

---

## 4. Thử nghiệm nhanh (5 phút)

Trước buổi seminar, thử gửi vài request để quen giao diện:

| Thử | Request | Expected |
|-----|---------|----------|
| Login | `POST /api/auth/login` với body `{"username":"alice","password":"password123"}` | Response có field `token` |
| Xem products | `GET /api/products` | Array products |
| Thêm vào cart | `POST /api/cart` với body `{"user_id":1,"product_id":1,"quantity":2}` | Response 200/201 |
| Redeem coupon | `POST /api/coupon/redeem` với body `{"coupon_code":"SAVE20","user_id":1}` | Response có `success: true` |

---

## 5. Chuẩn bị cho hoạt động trong lớp

### Bạn sẽ làm gì trong seminar?
- **20 phút thực hành:** Viết test cases cho endpoint `POST /api/coupon/redeem`
- **Nửa lớp:** Viết test thủ công (JavaScript trong Postman)
- **Nửa lớp:** Dùng Postbot/AI để sinh test tự động
- **Cuối:** So sánh kết quả 2 nhóm

### Cần biết trước
- Postman tab **Scripts** → **Post-response**: nơi viết JavaScript assertions
- Framework cơ bản:
```javascript
pm.test("Tên test", function () {
    pm.expect(pm.response.json().field).to.be.a('string');
});
```
- Tab **Tests** → **"Ask Postbot"**: AI sinh test tự động (cần tài khoản Free)

### Test accounts có sẵn
| Username | Password |
|----------|----------|
| alice | password123 |
| bob | password456 |
| charlie | password789 |

### Coupons có sẵn
| Code | Discount | Hết hạn? |
|------|----------|----------|
| SAVE20 | 20% | Chưa (2026-12-31) |
| HALF | 50% | Chưa (2026-12-31) |
| EXPIRED10 | 10% | **Đã hết hạn** (2024-01-01) |

---

## Checklist trước khi vào lớp

- [ ] Postman Desktop App đã cài + có tài khoản Free
- [ ] Node.js đã cài (`node --version` chạy được)
- [ ] Newman đã cài (`newman --version` chạy được)
- [ ] Repo đã clone + `npm install` đã chạy xong
- [ ] Server chạy được (`npm start` → truy cập `localhost:3000` thấy response)
- [ ] Postman đã import Collection + Environment
- [ ] Gửi thử `GET /api/products` thành công
- [ ] Mang laptop + sạc vào lớp

---

## Troubleshooting nhanh

| Vấn đề | Cách sửa |
|---------|----------|
| `npm install` lỗi | Xóa thư mục `node_modules` → chạy lại `npm install` |
| `ECONNREFUSED` khi gửi request | Mở terminal khác → chạy `npm start` → thử lại |
| Port 3000 bị chiếm | Sửa `PORT=3001` trong terminal: `PORT=3001 npm start` (Mac/Linux) hoặc `$env:PORT="3001"; npm start` (PowerShell) |
| Postman không import được file | Kiểm tra đúng file `.json`, không phải `.yaml` |
| `newman: command not found` | Chạy lại `npm install -g newman`. Trên Mac/Linux có thể cần `sudo`. |
| Postbot không hiện | Gửi request trước (Postbot cần response mẫu). Kiểm tra còn AI credits. |

---

## Liên hệ

Nếu gặp vấn đề setup không giải quyết được, liên hệ team T06 qua Moodle forum hoặc nhắn tin trực tiếp trước buổi seminar.
