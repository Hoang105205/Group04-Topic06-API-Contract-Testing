# Cấu Trúc Repo Demo — eshop-demo-api

> Repo Node.js/Express tối giản — tồn tại để audience có target chạy Postman trong activity.
> **Nguyên tắc:** Tối thiểu, chạy được, không cần impressive.
> **Người dựng:** Huy + Quang (đồng phụ trách)

---

## Cấu trúc thư mục

```
eshop-demo-api/
├── package.json                  # Dependencies: express, cors, jsonwebtoken. Scripts: start, dev
├── server.js                     # Entry point — Express app, mount routes, listen port 3000
├── data/
│   ├── products.json             # Dữ liệu mẫu: 5–10 products (id, name, price, stock)
│   ├── users.json                # Dữ liệu mẫu: 3 users (id, username, password, email)
│   └── coupons.json              # Dữ liệu mẫu: 3 coupons (code, discount_percent, expiry, redeemed_by[])
├── routes/
│   ├── auth.js                   # /api/auth/login — validate credentials, return JWT token
│   ├── products.js               # /api/products — return danh sách products; /api/products/:id
│   ├── cart.js                   # /api/cart — thêm product vào cart (in-memory); GET /api/cart
│   ├── orders.js                 # /api/orders — tạo order từ cart, validate stock; GET /api/orders
│   └── coupon.js                 # /api/coupon/redeem — validate & apply coupon
├── middleware/
│   └── auth.js                   # JWT verify middleware — decode token, gắn req.user
├── postman/
│   ├── Activity_Collection.json  # Postman Collection cho audience activity (import vào Postman)
│   └── Activity_Environment.json # Environment: base_url = http://localhost:3000, user credentials
├── api_specification.yaml        # OpenAPI 3.0 spec cho toàn bộ endpoints (cho audience đọc)
└── README.md                     # Hướng dẫn setup nhanh: npm install → npm start → import collection
```

---

## Mô tả từng file/thư mục

### `package.json`
Dependencies: `express`, `cors`, `jsonwebtoken`. Dev dependencies: `nodemon` (optional). Script `start`: `node server.js`.

### `server.js`
Express app chính. Middleware: `express.json()`, `cors()`. Mount 5 route files dưới prefix `/api/`. Listen port 3000. Không cần database — data load từ JSON files.
```javascript
// server.js
app.use('/api/auth', authRoutes);
app.use('/api/products', productRoutes);
app.use('/api/cart', cartRoutes);
app.use('/api/orders', orderRoutes);
app.use('/api/coupon', couponRoutes);
```

### `data/products.json`
```json
[
  { "id": 1, "name": "Laptop", "price": 999.99, "stock": 10, "category": "electronics" },
  { "id": 2, "name": "Mouse", "price": 29.99, "stock": 50, "category": "electronics" },
  { "id": 3, "name": "Keyboard", "price": 79.99, "stock": 30, "category": "electronics" },
  { "id": 4, "name": "Notebook", "price": 5.99, "stock": 100, "category": "stationery" },
  { "id": 5, "name": "Pen", "price": 1.99, "stock": 200, "category": "stationery" }
]
```

### `data/users.json`
```json
[
  { "id": 1, "username": "alice", "password": "password123", "email": "alice@test.com" },
  { "id": 2, "username": "bob", "password": "password456", "email": "bob@test.com" },
  { "id": 3, "username": "charlie", "password": "password789", "email": "charlie@test.com" }
]
```
> Mật khẩu plain-text — đây là demo repo, không phải production.

### `data/coupons.json`
```json
[
  { "code": "SAVE20", "discount_percent": 20, "expiry_date": "2026-12-31", "redeemed_by": [] },
  { "code": "HALF", "discount_percent": 50, "expiry_date": "2026-12-31", "redeemed_by": [] },
  { "code": "EXPIRED10", "discount_percent": 10, "expiry_date": "2024-01-01", "redeemed_by": [] }
]
```

### `routes/auth.js`
**POST /api/auth/login**
- Input: `{ username, password }`
- Validate từ `users.json`
- Success: return `{ token: JWT, user: { id, username, email } }`
- Fail: return 401 `{ error: "Invalid credentials" }`

### `routes/products.js`
**GET /api/products** — return toàn bộ products từ `products.json`
**GET /api/products/:id** — return 1 product, 404 nếu không tìm thấy

### `routes/cart.js`
**POST /api/cart** — Input: `{ user_id, product_id, quantity }`. Validate product tồn tại, stock đủ. Lưu cart in-memory (array).
**GET /api/cart/:user_id** — return cart items của user

### `routes/orders.js`
**POST /api/orders** — Input: `{ user_id, coupon_code? }`. Tạo order từ cart của user. Nếu có coupon → validate và apply discount. Giảm stock. Xóa cart. Return `{ order_id, items, total, discount, final_total }`.
**GET /api/orders** — return danh sách orders (in-memory).

### `routes/coupon.js`
**POST /api/coupon/redeem** — Input: `{ coupon_code, user_id }`. Validate:
- Coupon tồn tại → 400 nếu không
- Coupon chưa hết hạn (`expiry_date > today`) → 400 nếu hết hạn
- User chưa redeem coupon này → 409 nếu đã redeem
- Success: thêm `user_id` vào `redeemed_by[]`, return `{ success: true, discount_percent, message }`

### `middleware/auth.js`
Decode JWT từ header `Authorization: Bearer <token>`. Gắn `req.user = { id, username }`. Return 401 nếu token invalid/missing. Apply cho routes: `/api/cart`, `/api/orders` (không apply cho `/api/auth/login` và `/api/products`).

### `postman/Activity_Collection.json`
Postman Collection v2.1 chứa 6 requests:
1. POST /api/auth/login
2. GET /api/products
3. GET /api/products/:id
4. POST /api/cart
5. POST /api/orders
6. POST /api/coupon/redeem

Mỗi request có pre-configured URL, headers, body template. **KHÔNG có test scripts sẵn** — audience tự viết.

### `postman/Activity_Environment.json`
Variables:
- `base_url`: `http://localhost:3000`
- `username`: `alice`
- `password`: `password123`
- `auth_token`: (empty — sẽ fill khi login)
- `coupon_code`: `SAVE20`
- `user_id`: `1`

### `api_specification.yaml`
OpenAPI 3.0 spec — mô tả đầy đủ 6 endpoints với request/response schemas. Audience dùng file này để biết cấu trúc data khi viết test. File này cũng dùng cho Phase 1 của activity (đặc biệt phần `POST /api/coupon/redeem`).

### `README.md`
```
# EShop Demo API

## Quick Start
1. npm install
2. npm start
3. Server runs at http://localhost:3000
4. Import postman/Activity_Collection.json into Postman
5. Import postman/Activity_Environment.json into Postman
6. Select "Activity Env" environment
7. Start testing!

## Endpoints
- POST /api/auth/login
- GET /api/products
- GET /api/products/:id
- POST /api/cart
- POST /api/orders
- POST /api/coupon/redeem

## Test Users
- alice / password123
- bob / password456
- charlie / password789
```

---

## Endpoints tối thiểu (theo yêu cầu đề bài)

| Endpoint | Method | Auth Required | Mục đích trong Activity |
|----------|--------|---------------|------------------------|
| `/api/auth/login` | POST | ❌ | Login, nhận token — bước đầu tiên |
| `/api/products` | GET | ✅ | Xem danh sách products |
| `/api/cart` | POST | ✅ | Thêm product vào cart |
| `/api/orders` | POST | ✅ | Đặt hàng — endpoint chính cho variable chaining demo |
| `/api/coupon/redeem` | POST | ✅ | **Endpoint chính cho activity** — audience viết test cho endpoint này |

---

## Ghi chú kỹ thuật

- **Không dùng database** — mọi dữ liệu lưu in-memory hoặc load từ JSON files. Server restart = data reset. Điều này OK cho demo.
- **JWT secret** hardcode trong code: `JWT_SECRET = "demo-secret-key"`. Không dùng cho production.
- **Port 3000** — nếu audience bị conflict, README hướng dẫn đổi qua env variable `PORT`.
- **CORS enabled** — phòng trường hợp audience test từ browser hoặc tool khác.
- Repo nên host trên GitHub public để audience clone nhanh trong buổi seminar.
