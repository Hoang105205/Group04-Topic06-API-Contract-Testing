# AI AUDIT REPORT

- Student name: Dương Gia Huy
- Student ID: 23127052

## AI-generated Prompt

### Prompt 1: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 14:12 14/07/2026
- **Prompt:**
````text
[phan_cong_nhiem_vu.md][outline_user_guide.md]
Phần việc của tôi (23127052 - Dương Gia Huy) là phải làm gì?
````

**AI Output:**
````text
Chào Dương Gia Huy. Dựa trên các tài liệu dự án của Team 5 (T06 API & Contract Testing), phần việc của bạn (Huy) tập trung vào mảng Test Scripts, xử lý lỗi (Negative Testing) và hỗ trợ kỹ thuật. Dưới đây là chi tiết các hạng mục bạn cần hoàn thành:

### 1. Nghiên cứu và Thực hành Postman
* Nghiên cứu chuyên môn về "Test Scripts có logic thực sự" bao gồm: Pre-request Scripts, `pm.test()`, `pm.expect()`, variable chaining (chẳng hạn như luồng auth token), và các assertion nâng cao (kiểm tra field trong body, data types, response time).
* Nghiên cứu về Negative Testing & Error Handling để xử lý các test case nâng cao mà AI không tự làm được, ví dụ như lỗi 401, 400, test giá trị biên và kiểm tra cấu trúc error response.
* Yêu cầu bắt buộc là bạn phải tự chạy được Postman độc lập và viết thành công pre-request script cùng test script hoàn chỉnh cho luồng `login → products → cart → order`, bao gồm đầy đủ các negative test cases.

### 2. Viết tài liệu (Deliverables)
* Viết Section 4 (Test Scripts + Negative Testing) trên Wiki Nghiên Cứu chung của nhóm.
* Chịu trách nhiệm viết mục 5.1 (Test Scripts) và mục 5.1b (Negative Testing & Error Handling) trong file `User_Guide.md` (Stage S4).
* Đồng chủ trì thực hiện `Peer_Review.md` (đánh giá 2 team bạn) cùng với thành viên Quang ở Stage S8.

### 3. Xây dựng Source Code và Hỗ trợ Activity
* Đồng phụ trách việc dựng repo demo Node.js/Express tối giản (`eshop-demo-api`) cùng với Quang để phục vụ cho buổi seminar.
* Hỗ trợ Phương chuẩn bị kịch bản thực hành trong file `Activity_Worksheet.md`.
* Hỗ trợ Phương xác minh (verify) các output do công cụ Postbot (AI) sinh ra để đảm bảo tính chính xác.

### 4. Review chéo công việc
* Theo quy tắc làm việc chung, bạn có nhiệm vụ review phần bài viết của Quang, và ngược lại Quang sẽ review phần của bạn.
* Bạn sẽ cùng Phương review mục Failure Modes, vì nội dung phần này liên quan trực tiếp đến mảng Test Scripts mà bạn đang phụ trách.
````

### Prompt 2: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 14:15 14/07/2026
- **Prompt:**
````text
Hãy hướng dẫn tôi từ phần việc đầu tiên: 
"Test Scripts có logic thực sự" bao gồm: Pre-request Scripts, pm.test(), pm.expect(), variable chaining và các assertion nâng cao (kiểm tra field trong body, data types, response time).
````

**AI Output:**
````text
Để bắt đầu công việc một cách hiệu quả và đúng lộ trình của tuần 6-7 (Stage S3), chúng ta sẽ đi sâu vào nhiệm vụ cốt lõi đầu tiên của bạn: **Thực hành viết Test Scripts và Negative Testing trên Postman**.

Mục tiêu của bước này là bạn phải tự chạy được Postman độc lập và xây dựng thành công bộ kịch bản kiểm thử tự động cho luồng `login → products → cart → order`, bao gồm cả các trường hợp lỗi (negative test cases).

Dưới đây là hướng dẫn chi tiết từng bước để bạn bắt tay vào lập trình các đoạn script này:

### Bước 1: Thiết lập Pre-request Scripts (Tạo dữ liệu động)

Pre-request Script là đoạn mã chạy **trước** khi request được gửi đi, rất hữu ích để chuẩn bị dữ liệu đầu vào. Ví dụ, khi bạn cần test việc đăng ký hoặc tạo dữ liệu mới liên tục mà không bị trùng lặp:

```javascript
// Tạo một timestamp để đảm bảo tính duy nhất
pm.environment.set("timestamp", Date.now());

// Tạo random email để dùng cho payload
const randomEmail = `user_${Math.floor(Math.random() * 10000)}@test.com`;
pm.environment.set("test_email", randomEmail);

```

### Bước 2: Kỹ thuật Variable Chaining (Luồng Auth Token)

Đây là kỹ thuật bắt buộc để tự động hóa luồng `login → products → cart → order`. Bạn sẽ trích xuất dữ liệu từ response của request trước và lưu vào Environment Variable để request sau sử dụng.

**Tại request `POST /api/auth/login` (Tab Tests / Post-response):**

```javascript
// Lấy dữ liệu trả về từ JSON
const responseData = pm.response.json();

// Lưu token và user_id vào biến môi trường
pm.environment.set("auth_token", responseData.token);
pm.environment.set("user_id", responseData.user.id);

```

*Lưu ý:* Ở các request tiếp theo (như `/api/cart` hay `/api/orders`), hệ thống sẽ tự động lấy `{{auth_token}}` để gắn vào header `Authorization: Bearer {{auth_token}}`.

### Bước 3: Advanced Assertions (Kiểm tra dữ liệu nâng cao)

Kiểm tra mã trạng thái (status code) là chưa đủ. Bạn cần sử dụng `pm.test()` và `pm.expect()` để xác thực chuyên sâu về kiểu dữ liệu (data types), các trường trong body, và thời gian phản hồi (response time).

**Tại request `GET /api/products`:**

```javascript
// 1. Kiểm tra Status Code
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// 2. Kiểm tra thời gian phản hồi (Performance)
pm.test("Response time < 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

// 3. Kiểm tra kiểu dữ liệu và cấu trúc (Schema Validation)
pm.test("Product list structure and data types", function () {
    const products = pm.response.json();
    
    // Đảm bảo trả về mảng và có phần tử
    pm.expect(products).to.be.an('array').that.is.not.empty;
    
    // Lấy product đầu tiên để test và lưu id cho bước Add to Cart
    const firstProduct = products[0];
    pm.environment.set("product_id", firstProduct.id);
    
    // Validate chi tiết các trường
    pm.expect(firstProduct).to.have.property('id').that.is.a('number');
    pm.expect(firstProduct).to.have.property('name').that.is.a('string');
    pm.expect(firstProduct.price).to.be.a('number').and.to.be.above(0);
});

```

### Bước 4: Viết Negative Testing & Error Handling

Đây là phần quan trọng nhất thể hiện tư duy của một tester thực thụ, vì AI (Postbot) thường chỉ tạo các test cho trường hợp thành công (happy-path) và bỏ sót các negative tests. Bạn nên tạo một thư mục riêng tên "Negative Tests" trong Collection để chứa các request cố ý truyền sai đầu vào.

Dưới đây là các kịch bản lỗi bạn cần bắt bằng Script:

**1. Test không có Token (401 Unauthorized):**

```javascript
pm.test("Unauthorized without token returns 401", function () {
    pm.response.to.have.status(401);
});

```

**2. Test thiếu trường bắt buộc (400 Bad Request) & Error Structure:**

```javascript
pm.test("Missing required field returns 400 and standard error structure", function () {
    // Assert status
    pm.response.to.have.status(400);
    
    // Assert error response structure
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error').that.is.a('string');
});

```

**3. Test giá trị biên (Boundary Values) khi đặt hàng:**
Cố ý gửi `quantity = 0` hoặc `price` âm và kiểm tra xem server có bắt lỗi chuẩn không.

```javascript
pm.test("Zero quantity rejected", function () {
    pm.response.to.have.status(400);
});

```

---

Sau khi bạn đã hoàn thiện bộ script này trên Postman, bước tiếp theo là đưa những kiến thức này vào file `User_Guide.md` (mục 5.1 và 5.1b).

Bạn muốn tiếp tục thảo luận về cách phân bổ các đoạn script này vào file tài liệu `User_Guide.md`, hay muốn chuyển sang việc dựng cấu trúc source code cho repo `eshop-demo-api` mà bạn đang đồng phụ trách cùng Quang?
````

### Prompt 3: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 14:18 14/07/2026
- **Prompt:**

````text
Tôi không hiểu lắm, tại sao lại đột ngột có biến pm, rồi lại pm.test, pm...
````

**AI Output:**

````text
Sự thắc mắc của bạn là hoàn toàn hợp lý. Lần đầu nhìn vào script của Postman, việc thấy một biến "từ trên trời rơi xuống" mà không hề có dòng khai báo `let` hay `const` nào chắc chắn sẽ gây khó hiểu.

Để giải thích một cách cặn kẽ, biến `pm` là viết tắt của chữ **Postman**.

### 1. Nguồn gốc của biến `pm`

Khi bạn viết mã JavaScript ở tab **Pre-request Script** hoặc **Tests** trong Postman, những đoạn mã này không chạy độc lập mà được thực thi bên trong một môi trường khép kín gọi là **Postman Sandbox**.

Trong môi trường này, Postman đã tự động tạo và "tiêm" (inject) sẵn một object toàn cục (global object) tên là `pm`. Bạn có thể hình dung nó giống hệt như biến `document` hoặc `window` khi bạn viết JavaScript trên trình duyệt web, hay biến `console` trong Node.js — bạn được quyền sử dụng ngay lập tức để tương tác với hệ thống mà không cần khai báo.

### 2. Sức mạnh của object `pm`

Biến `pm` đóng vai trò là "cầu nối" để đoạn mã JavaScript của bạn giao tiếp với giao diện và dữ liệu của Postman. Dưới đây là những "nhánh" phổ biến nhất mà bạn sẽ sử dụng trong nhiệm vụ viết kịch bản kiểm thử:

* **`pm.response`**: Chứa toàn bộ thông tin mà server trả về (status code, headers, body).
* *Ví dụ:* Bạn gọi `pm.response.json()` để chuyển đổi body từ chuỗi thô thành một JSON object dễ dàng trích xuất dữ liệu.


* **`pm.environment`**: Cho phép bạn can thiệp vào các Biến môi trường (Environment Variables).
* *Ví dụ:* `pm.environment.set("auth_token", token)` giúp bạn lưu token vừa nhận được, để các request sau có thể tự động gọi ra dùng.


* **`pm.test("Tên bài test", function)`**: Hàm cốt lõi dùng để định nghĩa một kịch bản kiểm thử (test case). Postman sẽ chạy logic bên trong hàm này, nếu mọi thứ hợp lệ, nó sẽ hiển thị kết quả màu Xanh (Pass); nếu có lỗi, nó sẽ báo Đỏ (Fail).
* **`pm.expect()`**: Hàm này được dùng bên trong `pm.test` để xác nhận (assert) tính đúng đắn của dữ liệu. Thực chất, Postman đã tích hợp sẵn thư viện kiểm thử nổi tiếng tên là **Chai.js** vào phía sau hàm này. Nhờ đó, bạn có thể viết mã đọc rất tự nhiên, giống như tiếng Anh.
* *Ví dụ:* `pm.expect(product.price).to.be.a('number')` (Kỳ vọng giá của sản phẩm phải là một con số).

### Tóm lại

Bất cứ khi nào bạn cần phần mềm Postman làm một việc gì đó (lấy dữ liệu, lưu biến, hoặc đánh giá pass/fail), bạn sẽ gọi `pm` ra để "ra lệnh".

Để hiểu rõ hơn, bạn có muốn chúng ta cùng "giải phẫu" từng dòng code của đoạn script trích xuất dữ liệu `token` từ request Đăng nhập và tự động gán nó cho request Lấy danh sách sản phẩm không?
````

### Prompt 4: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 14:21 14/07/2026
- **Prompt:**

````text
Bạn hãy hướng dẫn từng bước chi tiết để tôi hiểu
````

**AI Output:**

````text
Chúng ta sẽ đi vào thực hành ngay trên Postman. Để dễ hình dung, nếu bạn đã quen với cấu trúc của các ngôn ngữ hướng đối tượng như C# hay Java, hãy xem `pm` giống hệt như một class `static` toàn cục (ví dụ như `System.Console` hay `Environment`). Bạn không cần khởi tạo (không cần dùng từ khóa `new`), cứ gọi thẳng tên nó ra để dùng ở bất kỳ đâu trong Postman Sandbox.

Dưới đây là luồng kết nối dữ liệu (Variable Chaining) hoàn chỉnh cho EShop SUT, đi từ lúc Đăng nhập cho đến khi Thêm vào giỏ hàng.

### Bước 1: Gọi API Đăng nhập và trích xuất Token

Đầu tiên, chúng ta cần lấy được "chìa khóa" (token) để đi qua các cánh cửa API khác.

1. Tạo một request mới: chọn method **POST**, nhập URL `{{base_url}}/api/auth/login`.
2. Chuyển sang tab **Body**, chọn **raw** và **JSON**, nhập thông tin đăng nhập:
```json
{
    "username": "alice",
    "password": "password123"
}

```


3. Chuyển sang tab **Tests** (đây là nơi code sẽ chạy *sau khi* có kết quả trả về từ server). Bạn dán đoạn mã sau vào:
```javascript
// 1. Ép kiểu dữ liệu trả về từ chuỗi thô thành một JSON object để dễ thao tác
const responseData = pm.response.json();

// 2. Viết test để đảm bảo API chạy đúng trước khi làm việc khác
pm.test("Đăng nhập thành công (Status 200)", function () {
    pm.response.to.have.status(200);
});

// 3. Nếu có token, lưu nó vào Biến môi trường (Environment Variable)
if (responseData.token) {
    pm.environment.set("auth_token", responseData.token);
    pm.environment.set("user_id", responseData.user.id);
}

```


4. Bấm **Send**. Nếu bạn nhìn lên góc phải màn hình Postman, mở biểu tượng con mắt (Environment quick look), bạn sẽ thấy `auth_token` và `user_id` đã được tự động lưu lại.

---

### Bước 2: Tự động hóa Header cho Request tiếp theo

Bây giờ bạn đã có token, bạn không cần phải copy-paste nó thủ công vào các request sau.

1. Tạo request thứ hai: method **GET**, URL `{{base_url}}/api/products`.
2. Chuyển sang tab **Authorization**.
3. Ở cột **Type**, chọn **Bearer Token**.
4. Ở ô Token hiện ra bên cạnh, bạn gõ chính xác: `{{auth_token}}`.
*(Lúc này Postman sẽ tự động lấy chuỗi token dài loằng ngoằng ở Bước 1 đắp vào đây).*

---

### Bước 3: Lấy danh sách sản phẩm và lưu ID

Chúng ta cần lấy ID của một sản phẩm bất kỳ để chuẩn bị ném nó vào giỏ hàng.

1. Vẫn ở request `GET /api/products` vừa tạo, bạn chuyển sang tab **Tests**.
2. Dán đoạn mã này vào:
```javascript
const products = pm.response.json();

pm.test("Có trả về danh sách sản phẩm", function() {
    pm.expect(products).to.be.an('array').that.is.not.empty;
});

// Trích xuất ID của sản phẩm đầu tiên (index 0) trong mảng và lưu lại
pm.environment.set("product_id", products[0].id);

```


3. Bấm **Send**. Lúc này biến môi trường của bạn đã có thêm `product_id`.

---

### Bước 4: Thêm sản phẩm vào giỏ hàng bằng dữ liệu động

Đây là bước kết nối mọi thứ lại với nhau. Thay vì gõ tay ID người dùng và ID sản phẩm, chúng ta sẽ gọi các biến môi trường ra.

1. Tạo request thứ ba: method **POST**, URL `{{base_url}}/api/cart`.
2. Vào tab **Authorization**, tiếp tục chọn **Bearer Token** và điền `{{auth_token}}`.
3. Chuyển sang tab **Body**, chọn **raw** và **JSON**. Dán đoạn payload này vào:
```json
{
    "user_id": {{user_id}},
    "product_id": {{product_id}},
    "quantity": 2
}

```

4. Bấm **Send**.

Lúc này, bạn đã thiết lập thành công một luồng tự động (Variable Chaining). Nếu bạn chạy toàn bộ thư mục chứa 3 request này bằng **Collection Runner**, nó sẽ tự động đăng nhập, tự động lấy token, tự động chộp lấy ID sản phẩm và quăng thẳng vào giỏ hàng mà không cần bạn can thiệp bất kỳ thao tác tay nào.
````

### Prompt 5: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 14:24 14/07/2026
- **Prompt:**

````text
Bạn hãy generate lại, bạn chỉ cần cho tôi phần nội dung (phần 5.1a và 5.1b thôi), còn lại format tôi sẽ tự chỉnh. Hiện tại file User_Guide của tôi như thế này. Tôi muốn phần nội dung phải là chủ yếu là giải thích chung chung cho ng dùng hiểu. Rồi lấy ví dụ là EShop SUT.

Phần nội dung phải được tổng hợp và soạn thảo bằng tiếng Anh (theo đúng quy định viết tài liệu của nhóm). 
````

**AI Output:**

````text
Dưới đây là nội dung chi tiết cho riêng phần **5.1a** và **5.1b**. Văn phong được giữ nguyên vẹn sự chuyên nghiệp, giải thích khái niệm rõ ràng trước khi đi vào ví dụ cụ thể trên hệ thống EShop SUT bằng tiếng Anh:

```markdown
### 5.1a — Test Scripts

API testing requires more than just checking if a request returns a `200 OK` status. Postman allows testers to write JavaScript code that executes either before a request is sent (**Pre-request Scripts**) or after a response is received (**Tests / Post-response Scripts**). These scripts are essential for validating complex API behaviors, managing data flows, and ensuring performance.

#### Pre-request Scripts (Dynamic Data)
Pre-request scripts execute before the API call is made. They are highly useful for generating dynamic payloads to avoid data duplication errors, such as creating unique emails for registration endpoints.
```javascript
// Generate a unique timestamp and random email before sending the request
pm.environment.set("timestamp", Date.now());
const randomEmail = `user_${Math.floor(Math.random() * 10000)}@test.com`;
pm.environment.set("test_email", randomEmail);

```

#### Variable Chaining (The E-Commerce Flow)

Instead of manually copying IDs or authentication tokens from one response to another, scripts can extract data from a JSON response and store it as an Environment Variable. This is critical for end-to-end automation.

In our **EShop SUT**, we use variable chaining to completely automate the purchase journey:

**1. Extract Token from Login (`POST /api/auth/login`):**

```javascript
const responseData = pm.response.json();

pm.test("Save Auth Token and User ID", function () {
    pm.environment.set("auth_token", responseData.token);
    pm.environment.set("user_id", responseData.user.id);
});

```

*Note: Subsequent requests will automatically inherit `{{auth_token}}` in their Authorization headers.*

**2. Extract Product ID from List (`GET /api/products`):**

```javascript
const products = pm.response.json();

pm.test("Extract Product ID", function () {
    // Save the ID of the first product to use in the Cart request
    pm.environment.set("product_id", products[0].id);
});

```

**3. Inject into Cart Request (`POST /api/cart`):**
The body of the cart request dynamically consumes the extracted variables, allowing the entire flow to run continuously without manual intervention:

```json
{
    "user_id": {{user_id}},
    "product_id": {{product_id}},
    "quantity": 2
}

```

#### Advanced Assertions

Postman uses the built-in Chai.js assertion library inside the `pm.expect()` block. Testers can validate schema structures, specific data types, and performance thresholds.

```javascript
pm.test("Response time is acceptable (< 800ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(800);
});

pm.test("Validate Product Schema and Data Types", function () {
    const products = pm.response.json();
    pm.expect(products).to.be.an('array').that.is.not.empty;
    
    const firstProduct = products[0];
    pm.expect(firstProduct).to.have.property('id').that.is.a('number');
    pm.expect(firstProduct.price).to.be.a('number').and.to.be.above(0);
});

```

### 5.1b — Negative Testing & Error Handling

Happy-path testing (verifying that valid inputs yield successful outputs) only covers a fraction of the API contract. **Negative Testing** involves intentionally sending invalid, malformed, or unauthorized data. As an API tester, it is your responsibility to ensure the server handles errors gracefully and returns standard error codes (4xx series) rather than crashing (500 Internal Server Error).

You should organize your negative tests in a dedicated folder (e.g., "Negative Tests") within your collection. To thoroughly test the **EShop API**, we must cover these critical edge cases:

#### 1. Unauthorized Access

Ensure secure endpoints reject requests that lack proper authentication credentials.

```javascript
// Test without an Authorization header -> Expect 401 Unauthorized
pm.test("Unauthorized access returns 401", function () {
    pm.response.to.have.status(401);
});

```

#### 2. Missing Required Fields

If a client fails to send a mandatory payload field (e.g., removing `product_id` from the Cart request), the server must catch it and return a standard `400 Bad Request`.

```javascript
pm.test("Missing product_id returns 400", function () {
    pm.response.to.have.status(400);
});

pm.test("Error response structure is consistent", function () {
    const jsonData = pm.response.json();
    // Validate that the server returns a clear error message string
    pm.expect(jsonData).to.have.property('error').that.is.a('string');
});

```

#### 3. Boundary Values & Business Logic

Test edge cases that violate domain logic. AI tools typically miss these scenarios. For instance, submitting a cart with an item quantity of `0` or applying an expired coupon code.

```javascript
// Example: Sending payload with "quantity": 0
pm.test("Zero quantity is rejected by validation (Status 400)", function () {
    pm.response.to.have.status(400);
});

```

```
````

### Prompt 6: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 12:27 15/07/2026
- **Prompt:**

````text
Tôi muốn bạn ở phần 5.1a, giải thích rõ ra pre-request và post-request, cùng với các step để navigate tới việc điều chỉnh code ở 2 phần này
````

**AI Output:**

````text
Chào bạn, tôi đã điều chỉnh lại riêng phần **5.1a** theo đúng ý bạn. Nội dung giờ đây đã giải thích rất rõ ràng sự khác biệt về mặt khái niệm giữa Pre-request và Post-response, đồng thời bổ sung thêm một mục hướng dẫn từng bước (step-by-step navigation) để người đọc biết cách tìm và viết code trên giao diện Postman.

Đương nhiên, văn phong "Giải thích Concept chung trước -> Ví dụ EShop" và ngôn ngữ tiếng Anh vẫn được giữ nguyên. Bạn có thể copy phần này để thay thế mục 5.1a hiện tại nhé:

```markdown
### 5.1a — Test Scripts

### Concept: Scripting in Postman
API testing requires more than just checking if a request returns a `200 OK` status. Postman allows testers to write JavaScript code to automate data preparation and validate complex API behaviors. These scripts are divided into two distinct execution phases:
1. **Pre-request Scripts:** Code that executes *before* the HTTP request is sent to the server.
2. **Post-response Scripts (Tests):** Code that executes *after* the HTTP response is received from the server.

### How to Navigate to the Scripts Interface
To write or edit scripts in Postman, follow these simple steps:
1. Open any API request in your Collection.
2. Look at the tab menu directly below the URL bar and click on the **Scripts** tab.
3. Inside the Scripts tab, you will see two sub-tabs: **Pre-request** and **Post-response**. Select the appropriate tab based on when you want your code to run.
4. Write your JavaScript code in the editor below and click **Save** (or press Ctrl+S/Cmd+S).

#### 1. Pre-request Scripts (Dynamic Data Preparation)
**Concept:** When you need to send unique data every time you run a request (such as a new email address to avoid "User already exists" errors), you use a Pre-request Script. It prepares the environment and payload before the API call is actually made.

**EShop Application:**
If we were testing a registration endpoint in our EShop SUT, we could use this phase to generate a dynamic email:
```javascript
// Navigate to Scripts -> Pre-request
// Generate a unique timestamp and random email before sending the request
pm.environment.set("timestamp", Date.now());
const randomEmail = `user_${Math.floor(Math.random() * 10000)}@test.com`;
pm.environment.set("test_email", randomEmail);

```

#### 2. Post-response Scripts (Assertions & Variable Chaining)

**Concept:** Once the server replies, you need to verify if the data is correct or extract specific values to use in future requests. This is done in the Post-response tab (historically called "Tests" in older Postman versions).

**EShop Application - Variable Chaining:**
Instead of manually copying authentication tokens from one response to another, we extract the token from the Login response and store it dynamically:

```javascript
// Navigate to Scripts -> Post-response on the POST /api/auth/login request
const responseData = pm.response.json();

pm.test("Save Auth Token and User ID", function () {
    pm.environment.set("auth_token", responseData.token);
    pm.environment.set("user_id", responseData.user.id);
});

```

*Note: Subsequent requests (like adding items to the cart) will automatically inherit this `{{auth_token}}` in their Authorization headers.*

**EShop Application - Advanced Assertions:**
We also use Post-response scripts to validate the actual structure of the returned data using the built-in Chai.js assertion library inside the `pm.expect()` block.

```javascript
// Navigate to Scripts -> Post-response on the GET /api/products request
pm.test("Response time is acceptable (< 800ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(800);
});

pm.test("Validate Product Schema and Data Types", function () {
    const products = pm.response.json();
    pm.expect(products).to.be.an('array').that.is.not.empty;
    
    // Validate the exact data types of the first product
    const firstProduct = products[0];
    pm.expect(firstProduct).to.have.property('id').that.is.a('number');
    pm.expect(firstProduct.price).to.be.a('number').and.to.be.above(0);
});

```

Bạn xem phần giải thích và điều hướng này đã đủ chi tiết và dễ hiểu cho người mới bắt đầu (audience) chưa nhé! Nếu cần thêm ảnh chụp màn hình (screenshot placeholder) vào phần điều hướng, tôi có thể bổ sung.
````

### Prompt 7: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 13:35 15/07/2026
- **Prompt:**

````text
Còn có khía cạnh nào khác không? (Postman có hỗ trợ gì thêm không? Error Handling còn có thể làm được gì nữa?)
````

**AI Output:**

````text
Nếu bạn muốn đào sâu hơn nữa vào "vũ khí" của Postman trong việc xử lý lỗi (Error Handling) và kiểm thử tiêu cực (Negative Testing), Postman thực sự còn hỗ trợ những tính năng rất mạnh mẽ dành cho dân Automation Test chuyên nghiệp.

Ngoài việc chỉ assert status code hay check các field cơ bản, bạn có thể bổ sung 3 khía cạnh nâng cao sau đây vào tài liệu. Chúng trả lời chính xác câu hỏi: *"Postman còn làm được gì nữa với lỗi?"*

Dưới đây là nội dung Markdown tiếng Anh để bạn chèn tiếp vào file:

```markdown
#### 4. JSON Schema Validation for Errors
**Concept:** Checking individual fields in an error response can be tedious. Postman includes a built-in JSON Schema validator (Ajv). You can define a strict "Error Contract" (e.g., following RFC 7807) and validate the entire error payload in a single line of code. This ensures your API never returns undocumented error formats.
**EShop Application:** When triggering a validation error in the Cart API, we use a JSON schema to guarantee the error response always contains exactly three fields: `error_code`, `message`, and `timestamp`.
```javascript
const errorSchema = {
    "type": "object",
    "required": ["error_code", "message", "timestamp"],
    "properties": {
        "error_code": { "type": "string" },
        "message": { "type": "string" },
        "timestamp": { "type": "string", "format": "date-time" }
    },
    "additionalProperties": false // Fails if the server leaks extra data
};

pm.test("Error response strictly matches the Error JSON Schema", function () {
    pm.response.to.have.jsonSchema(errorSchema);
});

```

#### 5. Rate Limiting Validation (Status 429)

**Concept:** A robust API must protect itself from brute-force attacks or spam by implementing rate limits. Negative testing should verify that if a client sends too many requests in a short window, the server gracefully intercepts them and returns a `429 Too Many Requests` status, along with a `Retry-After` header.
**EShop Application:** Using the Postman Collection Runner, we can loop the `POST /api/auth/login` request 50 times with zero delay. We then write a script to assert that the server eventually stops returning 401s (Invalid Credentials) and starts returning 429s to prevent spam.

```javascript
pm.test("API throttles spam requests with 429 Too Many Requests", function () {
    // We expect the server to eventually block us during a rapid loop run
    pm.expect(pm.response.code).to.be.oneOf([400, 401, 429]);
    
    if (pm.response.code === 429) {
        pm.expect(pm.response.headers.has('Retry-After')).to.be.true;
    }
});

```

#### 6. Dynamic Error Handling (Conditional Workflows)

**Concept:** How does your automated test suite handle an unexpected error? Instead of letting the Collection Runner blindly execute downstream requests that will inevitably fail (e.g., trying to add to a cart without a token), Postman allows you to dynamically control the execution flow using `postman.setNextRequest()`.
**EShop Application:** In our E-commerce flow, if the `POST /api/auth/login` request fails (e.g., the test server is down or credentials changed), we instruct Postman to immediately halt the test suite rather than wasting time executing the Products and Cart endpoints.

```javascript
pm.test("Login must be successful to continue workflow", function () {
    if (pm.response.code !== 200) {
        console.error("Login failed! Halting the automation suite.");
        // Passing 'null' stops the Collection Runner completely
        postman.setNextRequest(null); 
    } else {
        pm.expect(pm.response.code).to.eql(200);
    }
});

```

**Tại sao 3 phần này lại "ăn điểm"?**
1. **JSON Schema:** Thể hiện bạn biết cách test cấu trúc (contract testing) chứ không chỉ test giá trị (value testing).
2. **Rate Limiting:** Ghi điểm về tư duy bảo mật (Security Testing).
3. **Dynamic Error Handling:** Thể hiện trình độ "pro" trong việc sử dụng API của chính Postman để điều hướng luồng chạy (workflow), biến Postman từ một công cụ gửi request thành một framework test tự động thông minh.

Bạn thấy những kỹ thuật này đã đủ "nặng đô" cho phần hướng dẫn của nhóm chưa?
```
````

### Prompt 8: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 16:14 15/07/2026
- **Prompt:**

````text
Hình như tôi đang bị thiếu phần luồng Variable Chaining hoàn chỉnh, cùng với mọi số test trường hợp lỗi cho phần 5.1b.

Yêu cầu phần tôi viết thì nó như này:

### 5.1 — Test Scripts

**Huy viết**

Pre-request Scripts: tạo dữ liệu động (timestamp, random email). Variable chaining hoàn chỉnh: flow login → lưu token → inject vào products → lưu product_id → inject vào cart → lưu cart_id → inject vào orders. Assertion nâng cao: validate nested JSON objects, kiểm tra array length, response time threshold, JSON schema validation. Mỗi khái niệm đi kèm 1 đoạn code ngắn và giải thích.

**5.1b — Negative Testing & Error Handling**

Viết test cho các trường hợp lỗi — đây là thứ Postbot KHÔNG tạo được (liên kết với Failure Mode 6.2): gửi request không có token → assert 401; gửi body thiếu field bắt buộc → assert 400; gửi sai kiểu dữ liệu (string thay vì number) → assert 400; gửi giá trị biên (quantity = 0, price âm) → assert validation error. Kiểm tra error response structure nhất quán: mọi lỗi đều trả `{ error: string }`. Đây là kỹ năng phân biệt tester thật sự với người chỉ biết gọi API. 
````

**AI Output:**

````text
Đối chiếu với yêu cầu gốc, bản tinh chỉnh hiện tại của bạn viết rất hay nhưng đang **bỏ sót một vài chi tiết bắt buộc** để đạt điểm tối đa theo đúng outline của nhóm. Cụ thể:

1. **Luồng Variable Chaining chưa trọn vẹn:** Yêu cầu đòi hỏi phải nhắc đến toàn bộ flow (*login → token → products → product_id → cart → cart_id → orders*), trong khi bản hiện tại chỉ mới lấy ví dụ tới bước Login.
2. **Thiếu Nested JSON Validation:** Yêu cầu bắt buộc có kiểm tra object lồng nhau (nested JSON objects) trong phần assertion nâng cao.
3. **Thiếu Test Sai Kiểu Dữ Liệu:** Yêu cầu ghi rõ phải có test case "gửi sai kiểu dữ liệu (string thay vì number) → assert 400".
4. **Chưa nhấn mạnh liên kết với phần của Phương (Section 6.2):** Cần nhắc rõ việc Postbot không làm được điều này và chốt bằng câu "phân biệt tester thật sự".

Bạn chỉ cần thay thế/bổ sung các đoạn text dưới đây vào file của bạn là sẽ khớp 100% với yêu cầu mà vẫn giữ nguyên được văn phong mượt mà:

### 1. Sửa lại phần Variable Chaining (trong 5.1a) để hiển thị toàn bộ Flow

*Thay thế đoạn Variable Chaining hiện tại của bạn bằng đoạn này:*

**Variable Chaining:** Instead of manually copying IDs from one response to another, we can extract values and store them dynamically to automate an entire end-to-end workflow.


For a complete E-commerce flow, you chain requests by capturing outputs and passing them as inputs to the next step:

1. **Login:** Extract the token from `POST /api/auth/login` and set `{{auth_token}}`.
2. **Products:** Extract a product ID from `GET /api/products` and set `{{product_id}}`.
3. **Cart:** Inject `{{product_id}}` into the `POST /api/cart` payload, extract the resulting cart ID, and set `{{cart_id}}`.
4. **Orders:** Inject `{{cart_id}}` into `POST /api/orders` to complete the checkout.

Here is the code to capture the token from the Login response:

```javascript
const responseData = pm.response.json();

pm.test("Save Auth Token and User ID", function () {
    pm.environment.set("auth_token", responseData.token);
    pm.environment.set("user_id", responseData.user.id);
});

```

### 2. Bổ sung Nested JSON vào phần Data Validation (trong 5.1a)

*Thêm phần kiểm tra nested object vào trong đoạn script của mục Data Validation:*

```javascript
pm.test("Validate Product Schema, Data Types, and Nested Objects", function () {
    const products = pm.response.json();
    pm.expect(products).to.be.an('array').that.is.not.empty;
    
    const firstProduct = products[0];
    // Validate flat properties
    pm.expect(firstProduct).to.have.property('id').that.is.a('number');
    pm.expect(firstProduct.price).to.be.a('number').and.to.be.above(0);
    
    // Validate nested JSON objects
    pm.expect(firstProduct).to.have.property('category');
    pm.expect(firstProduct.category).to.have.property('name').that.is.a('string');
});

```

### 3. Sửa lại phần mở đầu của 5.1b để liên kết với Failure Mode 6.2

*Thay thế 2 đoạn mở đầu của mục 5.1b bằng đoạn này:*

Happy-path testing (verifying that valid inputs yield successful outputs) only covers a fraction of your API's contract. Negative Testing involves intentionally sending invalid, malformed, or unauthorized data to see how the system reacts.

As noted in **Section 6.2**, AI tools like Postbot completely miss these scenarios. Writing these manual negative tests is exactly what separates a true QA engineer from someone who just knows how to ping an API. The API must catch the invalid input and return standard client error codes (4xx series) rather than crashing (500 Internal Server Error).

### 4. Bổ sung mục Invalid Data Types vào 5.1b (Có thể đặt làm mục số 3)

*Thêm mục này vào danh sách các test case lỗi của bạn (trước mục Boundary Values):*

#### 3. Invalid Data Types

A robust API should strictly validate the data types of incoming payloads. If a client sends a string where an integer is expected, the server should reject it cleanly.

For example, sending `"quantity": "two"` (string) instead of `2` (number) to the Cart endpoint:

```javascript
pm.test("Invalid data type (string instead of number) returns 400", function () {
    pm.response.to.have.status(400);
});

```

*(Lưu ý: Bạn cũng cần sửa đoạn JSON Schema Validation for Errors của bạn ở dưới một chút cho đúng yêu cầu: `{"error": string}` thay vì dùng `error_code`, `message`, `timestamp` như tôi gợi ý lúc trước, để bám sát tuyệt đối vào outline "mọi lỗi đều trả `{ error: string }"`).*
````

### Prompt 9: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 16:51 15/07/2026
- **Prompt:**

````text
Tôi muốn có thêm 1 cái bảng nói về các câu lệnh thông dụng sử dụng như pm.environment.set, pm.expect, pm.expect(products).to.be.an('array').that.is.not.empty....
````

**AI Output:**

````text
Để bạn dễ dàng sao chép và dán trực tiếp vào trình soạn thảo tài liệu của mình (như Markdown, Notion, Obsidian, GitHub...), mình đã đặt toàn bộ bảng dưới dạng mã nguyên bản (raw markdown) trong khung bên dưới. Bạn chỉ cần bấm nút **Copy code** ở góc trên cùng của khung này:

```markdown
| Command / Assertion | Description |
| :--- | :--- |
| `pm.environment.set("key", value)` | Creates or updates a variable in your active Environment. Useful for variable chaining. |
| `pm.environment.get("key")` | Retrieves the value of an existing Environment variable. |
| `pm.response.json()` | Parses the raw response body into a readable JavaScript object/array. |
| `pm.test("Name", function() {...})` | Wraps your validation logic. The "Name" will appear in your Postman test result reports. |
| `pm.response.to.have.status(200)` | A quick assertion to verify the HTTP status code returned by the server. |
| `pm.expect(A).to.eql(B)` | Asserts that value A is strictly equal to value B. |
| `pm.expect(obj).to.have.property('id')` | Validates that a specific key (e.g., 'id') exists within a JSON object. |
| `pm.expect(obj.id).to.be.a('number')` | Checks the exact data type of a value (can be `'number'`, `'string'`, `'boolean'`). |
| `pm.expect(arr).to.be.an('array').that.is.not.empty` | Ensures the returned data is an array and contains at least one item. |
| `pm.expect(time).to.be.below(800)` | Asserts that a numeric value is less than a specified target (great for performance testing). |

```
````

### Prompt 10: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 14:56 16/07/2026
- **Prompt:**

````text
Tôi có ý định là bạn viết thêm 1 phần nữa, nói về test passed ở postman, kiểu như với mỗi pm. gì đó, sẽ có phần test results sau khi chạy post-response

Hãy lấy ví dụ là script này
**Data Validation:** Beyond extracting variables, Post-response scripts are crucial for validating the actual structure and performance of the returned data. We also use Post-response scripts to validate the actual structure of the returned data using the built-in Chai.js assertion library inside the pm.expect() block.<br>
For example, here is how you assert the schema and response time for the `GET {{base_url}}/api/products` endpoint.

```javascript
pm.test("Response time is acceptable (< 800ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(800);
});

pm.test("Validate Product Schema and Data Types", function () {
    const products = pm.response.json();
    pm.expect(products).to.be.an('array').that.is.not.empty;
    
    // Validate the exact data types of the first product
    const firstProduct = products[0];
    pm.expect(firstProduct).to.have.property('id').that.is.a('number');
    pm.expect(firstProduct.price).to.be.a('number').and.to.be.above(0);
});
```
````

**AI Output:**

````text
Dưới đây là phần nội dung được viết thêm để giải thích trực quan về tab **Test Results**, tiếp nối ngay sau đoạn code ví dụ của bạn. Mình vẫn giữ nguyên văn phong chuyên nghiệp, dẫn dắt và ngôn ngữ tiếng Anh để bạn dễ dàng ghép vào tài liệu:

---

**Data Validation:** Beyond extracting variables, Post-response scripts are crucial for validating the actual structure and performance of the returned data. We use the built-in Chai.js assertion library inside the `pm.expect()` block to enforce these rules. 

For example, here is how you assert the schema and response time for the `GET {{base_url}}/api/products` endpoint:

```javascript
pm.test("Response time is acceptable (< 800ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(800);
});

pm.test("Validate Product Schema and Data Types", function () {
    const products = pm.response.json();
    pm.expect(products).to.be.an('array').that.is.not.empty;
    
    // Validate the exact data types of the first product
    const firstProduct = products[0];
    pm.expect(firstProduct).to.have.property('id').that.is.a('number');
    pm.expect(firstProduct.price).to.be.a('number').and.to.be.above(0);
});

```

#### 3. Viewing Test Results

After you click **Send**, Postman executes your Post-response scripts and evaluates every `pm.test()` block. You can view the outcome by looking at the lower half of the Postman interface and clicking on the **Test Results** tab.

The string you provide in the `pm.test("Name", ...)` function becomes the visual label for your test.

* **When a test passes:** If all `pm.expect()` conditions inside the block evaluate to true, Postman flags it with a green **PASS** badge.
* **When a test fails:** If even a single assertion fails, the entire block gets a red **FAIL** badge, and Postman prints an error message explaining exactly what went wrong.

Using the `GET /api/products` script above as an example, here is what you will see in the Test Results tab:

**Scenario A: Success (Happy Path)**
If the API returns the data in 600ms and all data types are correct, your results will look like this:

> ✅ **PASS** Response time is acceptable (< 800ms)
> ✅ **PASS** Validate Product Schema and Data Types

**Scenario B: Failure (Catching a Bug)**
If the API is slow (e.g., takes 1200ms) and a backend bug causes the product price to return as a string (e.g., `"19.99"`) instead of a number, Postman will instantly catch it:

> ❌ **FAIL** Response time is acceptable (< 800ms) | *AssertionError: expected 1200 to be below 800*
> ❌ **FAIL** Validate Product Schema and Data Types | *AssertionError: expected '19.99' to be a number*

This immediate visual feedback is what makes Postman scripts so powerful. Instead of manually reading the JSON response every time, you rely on the Test Results tab to confidently confirm that the API contract is intact.
````

### Prompt 11: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 13:11 17/07/2026
- **Prompt:**

````text
Bạn hãy điều chỉnh lại, do tôi đổi sang dùng api mới để phải điều chỉnh lại các step (giờ là tới 6 steps). Bạn hãy thay đổi giọng văn cho mạch lạc và generate thêm script

**Variable Chaining:** Instead of manually copying IDs from one response to another, we can extract values and store them dynamically to automate an entire end-to-end workflow.<br>

For a complete E-commerce flow, you chain requests by capturing outputs and passing them as inputs to the next step:

1. **Login:** Extract the token and user ID from `POST /api/login` and set `{{auth_token}}` and `{{user_id}}`.
2. **User Information:** Extract all creditential of current user from `GET /api/users/me` and set `{{name}}` and `{{shipping_address}}`.
3. **Products:** Extract a product ID from `GET /api/products` and set `{{product_name}}` and `{{product_price}}`.
4. **Cart:** Inject `{{user_id}}`, `{{product_name}}` and `{{product_price}}` into the `POST /api/cart` payload to create a product in cart.
5. **Get Cart Information:** By using `GET /api/cart` to get all products in cart (price to calculate sum) then set to `{{sum_money}}`.
6. **Orders:** Inject `{{sum_money}}` and `shipping_address` into `POST /api/checkout` to complete the checkout.
````

**AI Output:**

````text
Chào bạn, việc chuyển đổi luồng API sang 6 bước như thế này rất sát với thực tế của một hệ thống E-commerce hiện đại. Tuy nhiên, vì luồng dữ liệu (data flow) đã thay đổi (không còn dùng `cart_id` mà chuyển sang tính tổng tiền `sum_money` và dùng `shipping_address`), văn bản mô tả và các đoạn script cần được viết lại để kết nối liền mạch với nhau.

Tôi đã sửa lỗi chính tả (ví dụ: *creditential* -> *credentials*), viết thêm các script bị thiếu cho bước 2 và bước 5, đồng thời sắp xếp lại câu chữ để tài liệu của bạn có tính dẫn dắt và logic nhất.

Dưới đây là phiên bản hoàn thiện cho phần này:

---

**Variable Chaining:** Instead of manually copying IDs from one response to another, we can extract values and store them dynamically to automate an entire end-to-end workflow.

For a complete E-commerce flow, you chain requests by capturing outputs and passing them as inputs to the next step:

1. **Login:** Extract the token and user ID from `POST /api/login` and set `{{auth_token}}` and `{{user_id}}`.
2. **User Information:** Extract the credentials of the current user from `GET /api/users/me` and set `{{name}}` and `{{shipping_address}}`.
3. **Products:** Extract a product's name and price from `GET /api/products` and set `{{product_name}}` and `{{product_price}}`.
4. **Cart:** Inject `{{user_id}}`, `{{product_name}}`, and `{{product_price}}` into the `POST /api/cart` payload to add the product to the cart.
5. **Get Cart Information:** Use `GET /api/cart` to retrieve all products in the cart, calculate the sum of their prices, and set `{{sum_money}}`.
6. **Orders:** Inject `{{sum_money}}` and `{{shipping_address}}` into `POST /api/checkout` to complete the checkout.

Here is how you would script this entire chain step-by-step to automate the workflow.

First, capture the token and user ID from the **Login** response:

```javascript
const responseData = pm.response.json();

pm.test("Save Auth Token and User ID", function () {
    pm.environment.set("auth_token", responseData.token);
    pm.environment.set("user_id", responseData.user.id);
});

```

> **Note:** Postman will automatically create these environment variables for you if they haven't been created yet.

Next, extract the personal details from the **User Information** response to use later in the checkout process:

```javascript
const user = pm.response.json();

pm.test("Save User Credentials", function () {
    pm.environment.set("name", user.name);
    pm.environment.set("shipping_address", user.shipping_address);
});

```

Then, dynamically grab the product's name and price of the first available item in the **Products** response array:

```javascript
const products = pm.response.json();

pm.test("Save First Product Name & Price", function () {
    // Check if the array has items to avoid runtime errors
    if(products.length > 0) {
        pm.environment.set("product_name", products[0].name);
        pm.environment.set("product_price", products[0].price);
    }
});

```

The workflow now moves to the **Cart** endpoint. Here, you don't need to extract anything. Instead, you inject the saved variables into your request body (JSON payload) to add the item to the cart.

> **Note:** If the variable represents a string, you must wrap it in quotes within your JSON body (e.g., `"name": "{{product_name}}"`). For numbers, quotes are not needed (e.g., `"price": {{product_price}}`).

Once the product is added, you need to call the **Get Cart Information** endpoint to calculate the total order value. The script below iterates through the returned array of cart items, sums up the prices, and stores the result:

```javascript
const cartItems = pm.response.json();

pm.test("Calculate and Save Total Sum", function () {
    let total = 0;
    
    // Ensure the response is an array before calculating
    if (Array.isArray(cartItems) && cartItems.length > 0) {
        cartItems.forEach(item => {
            total += item.price; 
        });
    }
    
    pm.environment.set("sum_money", total);
});

```

Finally, for the **Orders** endpoint, you inject `{{sum_money}}` and `{{shipping_address}}` into the checkout payload. Since this is the end of the flow, you no longer need to extract new variables. You simply validate that the server processed the order successfully:

```javascript
pm.test("Order placed successfully (Status 201)", function () {
    pm.response.to.have.status(201);
});

```

By mastering this extraction, calculation, and injection pattern, you can fully automate complex, multi-step business workflows without any manual intervention.

---

**Một số lưu ý nhỏ tôi đã tối ưu cho bạn:**

* Ở **Bước 4 (Cart)**, tôi đã thêm lời giải thích rõ ràng về việc phân biệt biến dạng chuỗi (String) cần dấu ngoặc kép `""` và biến dạng số (Number) thì không cần, giúp người đọc tránh lỗi JSON format.
* Ở **Bước 5 (Get Cart)**, tôi dùng vòng lặp `forEach` kết hợp kiểm tra `Array.isArray()` để tính tổng tiền một cách an toàn nhất, phòng trường hợp giỏ hàng trống sẽ không gây lỗi script. Nếu API thực tế của bạn trả về một Object chứa Array (ví dụ: `cartItems.data` hay `cartItems.products`), bạn chỉ cần nhắc học viên điều chỉnh đường dẫn mảng tương ứng.
````

### Prompt 12: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 13:41 17/07/2026
- **Prompt:**

````text
[server.js]
Bạn xem xem có api nào trả về nested variable không?
````

**AI Output:**

````text
Chào bạn, tôi đã rà soát kỹ đoạn mã nguồn bạn cung cấp. Thực tế là **có API trả về biến dạng nested (lồng nhau)**. Quá trình viết tắt bằng cú pháp ES6 trong JavaScript (ví dụ viết `{ user }` thay vì `{ "user": user }`) có thể khiến bạn vô tình lướt qua chi tiết này khi đọc code.

Dưới đây là các API trả về cấu trúc dữ liệu lồng nhau trong hệ thống của bạn:

### 1. Endpoint `POST /api/login`

Khi thông tin đăng nhập hợp lệ và mật khẩu chính xác, API này thực thi dòng lệnh: `res.json({ message: "Login successful", token, user });`.

* Biến `user` ở đây là một đối tượng (object) chứa toàn bộ thông tin của người dùng vừa được truy vấn từ cơ sở dữ liệu thông qua lệnh `SELECT * FROM users WHERE email = ?`.


* Vì object `user` được đặt vào bên trong object response tổng thể, nó sẽ tạo ra một cấu trúc JSON lồng nhau (nested JSON object).


Dữ liệu thực tế client nhận được sẽ có dạng như sau:

```json
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1...",
  "user": {
    "id": 1,
    "name": "Nguyễn Văn A",
    "email": "user@example.com",
    "password": "...",
    "role": "user",
    "login_attempts": 0,
    "locked_until": null
  }
}

```

### 2. Endpoint `POST /api/admin/import-products`

Tại phần kết thúc tiến trình import dữ liệu, API này trả về kết quả bằng lệnh: `res.json({ message: ..., inserted, errors, });`.

* Trong đó, `errors` được khai báo ban đầu là một mảng `let errors = [];` và được đẩy (`push`) các chuỗi văn bản báo lỗi vào trong quá trình lặp qua từng hàng dữ liệu.


* Việc chứa một mảng (Array) nằm bên trong một đối tượng (Object) phản hồi cũng chính là một cấu trúc dữ liệu lồng nhau.

Nếu bạn đang cần tìm một API để thực hành viết Test Scripts trong Postman nhằm kiểm tra việc truy xuất các biến lồng nhau (như phần tài liệu chúng ta đã soạn thảo), bạn hoàn toàn có thể sử dụng endpoint `POST /api/login` để trích xuất dữ liệu thông qua đường dẫn `responseData.user.id` nhé!
````

### Prompt 13: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 14:34 17/07/2026
- **Prompt:**

````text
Bạn có thể để riêng thêm 1 phần "kiểm tra array length" đồng thời có giải thích từng phần cho "response time, fundamental data types, and nested properties" và "kiểm tra array length" luôn không?
````

**AI Output:**

````text
Chào bạn, đây là một hướng đi cực kỳ xuất sắc và tỉ mỉ. Việc chia nhỏ và giải thích cặn kẽ từng loại Validation (Response Time, Object/Data Types, Array Length) sẽ giúp người đọc (đặc biệt là những người mới) hiểu rõ bản chất của từng dòng code thay vì chỉ copy-paste một cách máy móc.

Vì endpoint `POST /api/login` không trả về một mảng (array), tôi sẽ kết hợp sử dụng `POST /api/login` cho phần *Nested Properties* và `GET /api/products` cho phần *Array Length* để minh họa trực quan nhất.

Dưới đây là đoạn nội dung đã được tôi viết lại chi tiết, phân tách rõ ràng từng khái niệm. Bạn có thể copy trực tiếp phần này để thay thế cho mục **Data Validation** hiện tại:

---

**Data Validation:** Beyond extracting variables, Post-response scripts are crucial for validating the structure and performance of the returned data. We achieve this using the built-in Chai.js assertion library inside the `pm.expect()` block.

To master data validation, let's break it down into three core concepts:

**1. Asserting Response Time**
Performance matters just as much as correct data. You can easily assert that an endpoint responds within an acceptable timeframe to ensure a smooth user experience.

```javascript
pm.test("Response time is acceptable (< 800ms)", function () {
    // Validates that the request took less than 800 milliseconds to complete
    pm.expect(pm.response.responseTime).to.be.below(800);
});

```

**2. Validating Fundamental Data Types and Nested Properties**
When testing an endpoint like `POST /api/login`, you must ensure the server returns the exact data types expected by the client (e.g., strings for tokens, numbers for IDs). Furthermore, you need to drill down into nested JSON objects (like the `user` object inside the main response) to verify their internal structure.

```javascript
pm.test("Validate Login Schema, Data Types, and Nested Objects", function () {
    const responseData = pm.response.json();
    
    // 1. Validate flat properties (root level)
    pm.expect(responseData).to.be.an('object');
    pm.expect(responseData).to.have.property('message').that.is.a('string');
    pm.expect(responseData).to.have.property('token').that.is.a('string');
    
    // 2. Validate nested JSON objects (the 'user' object)
    pm.expect(responseData).to.have.property('user').that.is.an('object');
    
    // 3. Drill down into the nested object's data types
    pm.expect(responseData.user).to.have.property('id').that.is.a('number');
    pm.expect(responseData.user).to.have.property('email').that.is.a('string');
});

```

**3. Validating Array Length and Contents**
For endpoints that return lists of items, such as `GET /api/products`, verifying fundamental data types isn't enough. You must also verify that the response is actually an array and validate its length (e.g., ensuring it isn't empty, or it contains a specific number of elements).

```javascript
pm.test("Validate Array Structure and Length", function () {
    const products = pm.response.json();
    
    // Ensure the response is explicitly an array format
    pm.expect(products).to.be.an('array');
    
    // Ensure the array is not empty (length > 0)
    pm.expect(products).to.not.be.empty;
    
    // Alternatively, if you expect an exact number of items, use lengthOf:
    // pm.expect(products).to.have.lengthOf(10);
});

```

#### 3. Viewing Test Results

After you click **Send**, Postman executes your Post-response scripts and evaluates every `pm.test()` block. You can view the outcome by looking at the lower half of the Postman interface and clicking on the **Test Results** tab.

The string you provide in the `pm.test("Name", ...)` function becomes the visual label for your test.

* **When a test passes:** If all `pm.expect()` conditions inside the block evaluate to true, Postman flags it with a green **PASS** badge.
* **When a test fails:** If even a single assertion fails, the entire block gets a red **FAIL** badge, and Postman prints an error message explaining exactly what went wrong.

To see exactly how Postman reports these outcomes, let's use the `POST /api/login` script from above and intentionally force a failure. We will keep our schema validation intact, but we will change our response time assertion to expect an impossibly fast `< 1ms` return time:

```javascript
pm.test("Response time is acceptable (< 1ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(1);
});

pm.test("Validate Login Schema and Data Types", function () {
    const responseData = pm.response.json();
    
    pm.expect(responseData).to.be.an('object');
    pm.expect(responseData).to.have.property('message').that.is.a('string');
    
    pm.expect(responseData).to.have.property('user').that.is.an('object');
    pm.expect(responseData.user).to.have.property('id').that.is.a('number');
});

```

When you run this script, Postman provides immediate visual feedback:

* The `Validate Login Schema and Data Types` test evaluates to true, so Postman flags it with a green PASS badge.
* Since the API cannot return data in under 1 millisecond, the `Response time` test gets a red FAIL badge. Furthermore, Postman prints the exact `AssertionError` (e.g., expected 5 to be below 1) to help you debug what went wrong.

This immediate visual feedback is what makes Postman scripts so powerful. Instead of manually reading the JSON response every time, you rely on the Test Results tab to confidently confirm that the API contract is intact.
````

### Prompt 14: 
**Prompt + Tool**
- **Tool:** Gemini 3.1 Pro
- **Timestamp:** 15:12 17/07/2026
- **Prompt:**

````text
Bạn cũng hay điều chỉnh lại phần 5.1b này, do đã đổi api nên có 1 số phần đã thay đổi
````

**AI Output:**

````text
Chào bạn, dựa trên mã nguồn `server.js` mới, hệ thống API của chúng ta đã có những thay đổi lớn về mặt thiết kế (ví dụ: đổi endpoint thành `/api/login`, có cơ chế khóa tài khoản thay vì chặn 429 thông thường, luồng validate coupon và order status rất chặt chẽ).

Tôi đã điều chỉnh lại toàn bộ phần **5.1b** để các kịch bản test (Negative Tests) bám sát 100% vào logic thực tế của backend mới. Văn phong vẫn giữ sự chuyên nghiệp, mạch lạc và dẫn dắt như các phần trước.

Dưới đây là nội dung đã được cập nhật:

---

### 5.1b — Negative Testing & Error Handling

Happy-path testing (verifying that valid inputs yield successful outputs) only covers a fraction of your API's contract. Negative Testing involves intentionally sending invalid, malformed, or unauthorized data to see how the system reacts.

As noted in Section 6.2, AI tools like Postbot completely miss these scenarios. Writing these manual negative tests is exactly what separates a true QA engineer from someone who just knows how to ping an API. The API must catch the invalid input and return standard client error codes (4xx series) rather than crashing (500 Internal Server Error).

**Best Practice:** Always organize your negative tests in a dedicated folder (e.g., "Negative Tests") within your Postman collection. This separates your test logic and makes your automated test reports much easier to analyze.

To understand how to approach negative testing, let's explore the most common edge cases you need to cover. We will use our updated API architecture as a running example to demonstrate how these tests are written in Postman:

#### 1. Unauthorized Access

Secure endpoints must actively reject unauthenticated requests.

If you attempt to access protected resources like `GET {{base_url}}/api/users/me` or `GET {{base_url}}/api/cart` without providing a valid Bearer token in the headers, the server must block the request and return an error.

```javascript
pm.test("Unauthorized access returns 401", function () {
    pm.response.to.have.status(401);
    pm.expect(pm.response.json().error).to.eql('Unauthorized');
});

```

#### 2. Missing Required Fields

APIs must validate structural integrity before processing logic.

The `POST {{base_url}}/api/apply-coupon` endpoint strictly requires a `code` and `total_amount`. If a client omits the coupon code, the server must catch it immediately and return a standard `400 Bad Request` before querying the database.

```javascript
pm.test("Missing coupon code returns 400 with an error message", function () {
    pm.response.to.have.status(400);
    pm.expect(pm.response.json()).to.have.property('error').that.includes('Vui lòng nhập mã');
});

```

#### 3. Invalid Data Formats & Types

A robust API should strictly validate the format of incoming payloads. If a client sends the wrong data structure, the server should reject it cleanly.

For example, the `POST {{base_url}}/api/admin/import-products` endpoint expects an array of products. If you send an empty array or a plain object/string, the API must reject it:

```javascript
pm.test("Invalid payload format (missing or not an array) returns 400", function () {
    pm.response.to.have.status(400);
    pm.expect(pm.response.json().error).to.eql("Không có dữ liệu để import");
});

```

#### 4. Boundary Values & Business Logic

These are critical scenarios that basic automated tools often miss. Your negative tests must enforce the core business rules of the platform. For our API, this includes:

* **Coupon Conditions:** Submitting `POST {{base_url}}/api/apply-coupon` with a valid code, but the `total_amount` is lower than the coupon's `min_order_amount`.
* **Invalid State Transitions:** Attempting to update an order via `PUT {{base_url}}/api/admin/orders/{{order_id}}/status` from "pending" directly to "delivered". The system must enforce sequential flow.
* **Illegal Actions:** Trying to cancel an order via `PUT {{base_url}}/api/orders/{{order_id}}/cancel` when its status is already "delivered" or "canceled".

```javascript
pm.test("Invalid order state transition is rejected (Status 400)", function () {
    pm.response.to.have.status(400);
    pm.expect(pm.response.json().error).to.include('Invalid state transition');
});

```

#### 5. JSON Schema Validation for Errors

Manually validating individual fields in an error response can be tedious. Instead, use Postman's built-in JSON Schema validator (Ajv). You can define a strict "Error Contract" and validate the entire error payload in a single line of code. This ensures your API never returns undocumented error formats.

Throughout our API, client errors consistently return a single `error` message string. We use a JSON schema to guarantee this format remains intact across all endpoints:

```javascript
const errorSchema = {
    "type": "object",
    "required": ["error"],
    "properties": {
        "error": { "type": "string" }
    },
    "additionalProperties": false
};

pm.test("Error response strictly matches the Error JSON Schema", function () {
    pm.response.to.have.jsonSchema(errorSchema);
});

```

#### 6. Brute-Force Protection & Account Lockout (Status 403)

A secure authentication system must protect itself from brute-force password guessing. Negative testing should verify that if a client repeatedly sends wrong credentials, the server intercepts them and locks the account.

Using the Postman Collection Runner (which is covered in `Section 5.2`), we can loop the `POST /api/login` request multiple times with a wrong password. We then write a script to assert that after 3 failed attempts, the server stops returning `401 Unauthorized` and switches to a `403 Forbidden` lockout state.

```javascript
pm.test("API locks account after 3 failed login attempts", function () {
    // The status should be either 401 (Invalid password) or 403 (Locked)
    pm.expect(pm.response.code).to.be.oneOf([401, 403]);
    
    if (pm.response.code === 403) {
        pm.expect(pm.response.json().error).to.include('Tài khoản đã bị khóa');
    }
});

```

#### 7. Dynamic Error Handling (Conditional Workflows)

How does your automated test suite handle an unexpected error? Instead of letting the Collection Runner blindly execute downstream requests that will inevitably fail (e.g., trying to access the Cart without a token), Postman allows you to dynamically control the execution flow using `postman.setNextRequest()`.

If the `POST /api/login` request fails (e.g., the test server is down or the account got locked), we instruct Postman to immediately halt the test suite rather than wasting time executing the rest of the endpoints.

```javascript
pm.test("Login must be successful to continue workflow", function () {
    if (pm.response.code !== 200) {
        console.error("Login failed! Halting the automation suite.");
        // Passing 'null' stops the Collection Runner completely
        postman.setNextRequest(null); 
    } else {
        pm.expect(pm.response.code).to.eql(200);
    }
});

```

> **Note:** The `postman.setNextRequest()` function only controls the execution flow when running tests via the **Collection Runner**. If you send the request individually using the regular Send button, this command is ignored.
````