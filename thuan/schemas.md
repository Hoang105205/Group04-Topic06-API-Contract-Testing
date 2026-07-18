# JSON Schemas cho EShop SUT (schemas.md)

Tài liệu này chứa toàn bộ các bản thiết kế JSON Schemas tương ứng với 8 kịch bản phản hồi (response) chính của hệ thống **EShop SUT** được khảo sát, kèm theo hướng dẫn chi tiết cách cài đặt và sử dụng chúng trong Postman để phục vụ cho việc kiểm thử hợp đồng (Contract Testing).

---

## PHẦN I: DANH SÁCH 8 BẢN THIẾT KẾ JSON SCHEMAS

### 1. Schema Đăng nhập thành công (Auth Schema)
*   **API:** `POST {{base_url}}/api/login`
*   **Mô tả:** Xác thực phản hồi trả về khi đăng nhập thành công bao gồm thông điệp, mã token JWT và đối tượng người dùng (`user`) chứa các thông tin cơ bản.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["message", "token", "user"],
  "properties": {
    "message": { "type": "string" },
    "token": { "type": "string" },
    "user": {
      "type": "object",
      "required": ["id", "name", "email", "role"],
      "properties": {
        "id": { "type": "integer" },
        "name": { "type": "string" },
        "email": { "type": "string", "format": "email" },
        "password": { "type": "string" },
        "role": { "type": "string", "enum": ["user", "admin"] },
        "login_attempts": { "type": "integer" },
        "locked_until": { "type": ["string", "null"] },
        "reset_token": { "type": ["string", "null"] },
        "shipping_address": { "type": ["string", "null"] },
        "phone": { "type": ["string", "null"] }
      }
    }
  }
}
```

---

### 2. Schema Danh sách sản phẩm (Products List Schema)
*   **API:** `GET {{base_url}}/api/products`
*   **Mô tả:** Mảng chứa danh sách các sản phẩm đang hiển thị trong hệ thống.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "array",
  "items": {
    "type": "object",
    "required": ["id", "name", "price", "description", "imageUrl", "category_id"],
    "properties": {
      "id": { "type": "integer" },
      "name": { "type": "string" },
      "price": { "type": "number", "minimum": 0 },
      "description": { "type": "string" },
      "imageUrl": { "type": "string", "format": "uri" },
      "category_id": { "type": "integer" }
    }
  }
}
```

---

### 3. Schema Chi tiết sản phẩm (Single Product Schema)
*   **API:** `GET {{base_url}}/api/products/:id`
*   **Mô tả:** Trả về thông tin chi tiết của một sản phẩm dựa trên ID được yêu cầu.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["id", "name", "price", "description", "imageUrl", "category_id"],
  "properties": {
    "id": { "type": "integer" },
    "name": { "type": "string" },
    "price": { "type": "number", "minimum": 0 },
    "description": { "type": "string" },
    "imageUrl": { "type": "string", "format": "uri" },
    "category_id": { "type": "integer" }
  }
}
```

---

### 4. Schema Thông tin giỏ hàng (Cart Schema)
*   **API:** `GET {{base_url}}/api/cart`
*   **Mô tả:** Trả về danh sách các sản phẩm đang nằm trong giỏ hàng hiện tại của người dùng.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "array",
  "items": {
    "type": "object",
    "required": ["id", "name", "price", "quantity"],
    "properties": {
      "id": { "type": "integer" },
      "name": { "type": "string" },
      "price": { "type": "number", "minimum": 0 },
      "quantity": { "type": "integer", "minimum": 1 }
    }
  }
}
```

---

### 5. Schema Thêm vào giỏ hàng thành công (Add to Cart Schema)
*   **API:** `POST {{base_url}}/api/cart`
*   **Mô tả:** Trả về khi thêm sản phẩm thành công vào giỏ hàng.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["message"],
  "properties": {
    "message": { "type": "string" }
  }
}
```

---

### 6. Schema Thanh toán đơn hàng thành công (Checkout Success Schema)
*   **API:** `POST {{base_url}}/api/checkout`
*   **Mô tả:** Xác thực kết quả phản hồi khi thanh toán đơn hàng thành công, bao gồm một thông báo và chi tiết của đơn hàng đã được tạo.

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["message", "orderId"],
  "properties": {
    "message": { "type": "string" },
    "orderId": { "type": "integer" }
  }
}

---

### 7. Schema Danh sách đơn hàng của tôi (Orders Schema)
*   **API:** `GET {{base_url}}/api/orders/my-orders`
*   **Mô tả:** Trả về lịch sử mua hàng gồm danh sách các đơn hàng cá nhân đã tạo của tài khoản đang đăng nhập.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "array",
  "items": {
    "type": "object",
    "required": ["id", "user_id", "total_amount", "status", "shipping_address", "created_at"],
    "properties": {
      "id": { "type": "integer" },
      "user_id": { "type": "integer" },
      "total_amount": { "type": "number", "minimum": 0 },
      "status": { "type": "string", "enum": ["pending", "confirmed", "shipping", "delivered", "cancelled"] },
      "shipping_address": { "type": "string" },
      "created_at": { "type": "string" }
    }
  }
}
```

---

### 8. Schema Áp dụng mã giảm giá thành công (Coupon Success Schema)
*   **API:** `POST {{base_url}}/api/apply-coupon`
*   **Mô tả:** Trả về khi người dùng áp dụng mã giảm giá hợp lệ thành công.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["success", "coupon_id", "discount_amount", "final_amount", "message"],
  "properties": {
    "success": { "type": "boolean" },
    "coupon_id": { "type": "integer" },
    "discount_amount": { "type": "number" },
    "final_amount": { "type": "number", "minimum": 0 },
    "message": { "type": "string" }
  }
}
```

---

### 9. Schema Phản hồi khi có lỗi xảy ra (Error Response Schema)
*   **API:** Áp dụng chung cho tất cả các kịch bản lỗi (ví dụ: `400 Bad Request`, `401 Unauthorized`, `404 Not Found`).
*   **Mô tả:** Cấu trúc lỗi đồng nhất phản hồi về phía client khi request không hợp lệ.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["error"],
  "properties": {
    "error": { "type": "string" }
  }
}
```

---

## PHẦN II: HƯỚNG DẪN SỬ DỤNG TRONG POSTMAN

Để sử dụng các schema trên nhằm tự động hóa việc xác thực hợp đồng API trong Postman, bạn hãy làm theo các bước dưới đây:

### Bước 1: Mở Tab "Tests" trong Request của Postman
1. Mở **Postman** và chọn Request bạn muốn kiểm thử (ví dụ: `api/login` hoặc `api/products`).
2. Nhấp chọn tab **Tests** (nằm bên dưới thanh nhập URL, cạnh tab *Headers* và *Body*).

### Bước 2: Viết Script Khai báo Schema và Thực hiện Assertion
Nhúng schema JSON tương ứng và sử dụng phương thức `pm.response.to.have.jsonSchema()` của Postman. Dưới đây là mã nguồn Postman Test Script cho cả 8 kịch bản để bạn copy-paste trực tiếp:

#### 👉 1. Test Script cho Đăng nhập thành công (`POST /api/login`)
```javascript
// 1. Declare the full JSON Schema for authSchema
const authSchema = {
  "type": "object",
  "required": ["message", "token", "user"],
  "properties": {
    "message": { "type": "string" },
    "token": { "type": "string" },
    "user": {
      "type": "object",
      "required": ["id", "name", "email", "role"],
      "properties": {
        "id": { "type": "integer" },
        "name": { "type": "string" },
        "email": { "type": "string", "format": "email" },
        "role": { "type": "string", "enum": ["user", "admin"] }
      }
    }
  }
};
// 2. Validate the response code is 200 OK.
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});
// 3. Validate the response body matches the expected JSON schema.
pm.test("Response matches Auth JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(authSchema);
});
// 4. Save Auth Token and User ID to environment variables if login is successful
if (pm.response.code === 200) {
    const responseData = pm.response.json();
    pm.environment.set("auth_token", responseData.token);
    pm.environment.set("user_id", responseData.user.id);
}
```

#### 👉 2. Test Script cho Danh sách sản phẩm (`GET /api/products`)
```javascript
// 1. Declare the full JSON schema for the products array
const productsSchema = {
  "type": "array",
  "items": {
    "type": "object",
    "required": ["id", "name", "price", "description", "imageUrl", "category_id"],
    "properties": {
      "id": { "type": "integer" },
      "name": { "type": "string" },
      "price": { "type": "number", "minimum": 0 },
      "description": { "type": "string" },
      "imageUrl": { "type": "string" },
      "category_id": { "type": "integer" }
    }
  }
};

// 2. Validate the response code is 200 OK
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 3. Validate the response body against the JSON Schema contract
pm.test("Products list matches JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(productsSchema);
});
```

#### 👉 3. Test Script cho Chi tiết sản phẩm (`GET /api/products/:id`)
```javascript
// 1. Declare the JSON schema for a single product
const singleProductSchema = {
  "type": "object",
  "required": ["id", "name", "price", "description", "imageUrl", "category_id"],
  "properties": {
    "id": { "type": "integer" },
    "name": { "type": "string" },
    "price": { "type": "number", "minimum": 0 },
    "description": { "type": "string" },
    "imageUrl": { "type": "string" },
    "category_id": { "type": "integer" }
  }
};

// 2. Validate the response status code is 200 OK
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 3. Validate the response body matches the JSON schema contract
pm.test("Product detail matches JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(singleProductSchema);
});
```

#### 👉 4. Test Script cho Thông tin giỏ hàng (`GET /api/cart`)
```javascript
// 1. Declare the expected JSON schema for the cart items
const cartSchema = {
  "type": "array",
  "items": {
    "type": "object",
    "required": ["id", "name", "price", "quantity"],
    "properties": {
      "id": { "type": "integer" },
      "name": { "type": "string" },
      "price": { "type": "number", "minimum": 0 },
      "quantity": { "type": "integer", "minimum": 1 }
    }
  }
};

// 2. Validate the response status code is 200 OK
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 3. Validate the response body against the expected JSON schema
pm.test("Cart items match JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(cartSchema);
});
```

#### 👉 5. Test Script cho Thêm vào giỏ hàng thành công (`POST /api/cart`)
```javascript
// 1. Declare the expected JSON schema for adding an item to the cart
const addToCartSchema = {
  "type": "object",
  "required": ["message"],
  "properties": {
    "message": { "type": "string" }
  }
};

// 2. Validate the response status code is 200 OK (based on practical observation)
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 3. Validate the response body against the expected JSON schema
pm.test("Add to cart response matches JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(addToCartSchema);
});
```

#### 👉 6. Test Script cho Thanh toán đơn hàng thành công (`POST /api/checkout`)
```javascript
const checkoutSchema = {
  "type": "object",
  "required": ["message", "orderId"],
  "properties": {
    "message": { "type": "string" },
    "orderId": { "type": "integer" }
  }
};

// 2. Validate the response status code is 200 OK
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 3. Validate the response body against the expected JSON schema
pm.test("Checkout response matches JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(checkoutSchema);
});
```

#### 👉 7. Test Script cho Danh sách đơn hàng của tôi (`GET /api/orders/my-orders`)
```javascript
// 1. Declare the expected JSON schema for the list of orders
const ordersSchema = {
  "type": "array",
  "items": {
    "type": "object",
    "required": ["id", "user_id", "total_amount", "status", "shipping_address", "created_at"],
    "properties": {
      "id": { "type": "integer" },
      "user_id": { "type": "integer" },
      "total_amount": { "type": "number", "minimum": 0 },
      "status": { "type": "string", "enum": ["pending", "confirmed", "shipping", "delivered", "cancelled"] },
      "shipping_address": { "type": "string" },
      "created_at": { "type": "string" }
    }
  }
};

// 2. Validate the response status code is 200 OK
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 3. Validate the response body against the expected JSON schema
pm.test("Orders list matches JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(ordersSchema);
});
```

#### 👉 8. Test Script cho Áp dụng mã giảm giá thành công (`POST /api/apply-coupon`)
```javascript
// 1. Declare the expected JSON schema for applying a coupon successfully
const couponSchema = {
  "type": "object",
  "required": ["success", "coupon_id", "discount_amount", "final_amount", "message"],
  "properties": {
    "success": { "type": "boolean" },
    "coupon_id": { "type": "integer" },
    "discount_amount": { "type": "number" },
    "final_amount": { "type": "number", "minimum": 0 },
    "message": { "type": "string" }
  }
};

// 2. Validate the response status code is 200 OK (based on practical observation)
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 3. Validate the response body matches the expected JSON schema
pm.test("Coupon validation matches JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(couponSchema);
});
```

#### 👉 9. Test Script cho Phản hồi khi có lỗi xảy ra (ví dụ: `400 Bad Request`)
```javascript
// 1. Define the JSON schema for error responses
const errSchema = {
  "type": "object",
  "required": ["error"],
  "properties": {
    "error": { "type": "string" }
  }
};

// 2. Validate the response status code is 400 Bad Request (commonly used for errors)
pm.test("Status code is 400 Bad Request", function () {
    pm.response.to.have.status(400);
});

// 3. Validate the response body against the error JSON schema
pm.test("Error response matches Error JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(errSchema);
});
```

#### 👉 10. Test Script cho Lỗi sản phẩm không tồn tại (`GET /api/products/999` - 404 Not Found)
```javascript
// 1. Define the JSON schema for error responses
const errSchema = {
  "type": "object",
  "required": ["error"],
  "properties": {
    "error": { "type": "string" }
  }
};

// 2. Validate the response status code is 404 Not Found
pm.test("Status code is 404 Not Found", function () {
    pm.response.to.have.status(404);
});

// 3. Validate the response body against the error JSON schema
pm.test("Error response matches Error JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(errSchema);
});
```

#### 👉 11. Test Script cho Lỗi thêm sản phẩm không hợp lệ vào giỏ hàng (`POST /api/cart` - 400 Bad Request)
```javascript
// 1. Define the JSON schema for error responses
const errSchema = {
  "type": "object",
  "required": ["error"],
  "properties": {
    "error": { "type": "string" }
  }
};

// 2. Validate the response status code is 400 Bad Request
pm.test("Status code is 400 Bad Request", function () {
    pm.response.to.have.status(400);
});

// 3. Validate the response body against the error JSON schema
pm.test("Error response matches Error JSON Schema Contract", function () {
    pm.response.to.have.jsonSchema(errSchema);
});
```

---

### Bước 3: Chạy Request và Xem Kết quả Test
1. Nhấn nút **Send** để gửi request lên SUT.
2. Sau khi nhận được phản hồi, chọn tab **Test Results** ở khung kết quả (phía dưới cùng) để xem trạng thái:
   *   Nếu khớp hoàn toàn với thiết kế hợp đồng: Trạng thái hiển thị sẽ là `PASS` màu xanh lá cây.
   *   Nếu backend thay đổi cấu trúc phản hồi (thiếu trường bắt buộc hoặc sai kiểu dữ liệu): Trạng thái hiển thị là `FAIL` màu đỏ kèm theo nguyên nhân lỗi chi tiết (ví dụ: `data.user should have required property 'role'`).
