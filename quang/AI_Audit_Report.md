# AI Audit Report

#### **Tác vụ 1: Xây dựng Backend cho EShop Demo API từ tài liệu cấu trúc**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Claude Sonnet 4.6
  - **Prompt:** `dựa trên file  @[cau_truc_repo_demo.md] cau_truc_repo_demo.md, trừ phần postman hãy build đầy đủ theo cấu trúc trong file`
  - **Thời gian thực hiện:** `01-07-2026 14:35`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tạo cấu trúc thư mục dự án đầy đủ gồm các file và thư mục theo cấu trúc cau_truc_repo_demo.md:
  - package.json (dependencies: express, cors, jsonwebtoken)
  - server.js (khởi tạo Express app, mount các router)
  - data/products.json, data/users.json, data/coupons.json (dữ liệu mẫu)
  - routes/auth.js (POST /api/login, sinh JWT)
  - routes/products.js (GET /api/products, GET /api/products/:id, PUT, DELETE)
  - routes/cart.js (GET/POST /api/cart, DELETE /api/cart/:id)
  - routes/orders.js (POST /api/orders)
  - routes/coupon.js (POST /api/coupon/redeem)
  - middleware/auth.js (verifyToken middleware)
  - api_specification.yaml (OpenAPI 3.0)
  - README.md & .gitignore
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  Các file backend được tạo ra chính xác theo đặc tả của cấu trúc thư mục và mô tả endpoints trong file `cau_truc_repo_demo.md`. Server chạy tốt, phân chia router và middleware hợp lý.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Không cần chỉnh sửa thêm, backend chạy ổn định ở bước thiết lập ban đầu.

---

#### **Tác vụ 2: Hỗ trợ lỗi kiểm thử đăng nhập (Login)**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Claude Sonnet 4.6
  - **Prompt:** `sao tôi test login không được`
  - **Thời gian thực hiện:** `01-07-2026 15:21`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Chẩn đoán đúng nguyên nhân: body gửi dạng form-data thay vì raw JSON. Đề xuất 2 biện pháp:
  1. Chuyển sang tab raw → JSON trong Postman.
  2. Thêm app.use(express.urlencoded({ extended: true })) vào server.js đề phòng client gửi dạng form.
  ```
- **3. Verdict (Đánh giá):** **INCOMPLETE**
- **4. Reasoning (Lý do đánh giá):**
  Chẩn đoán đúng nguyên nhân gốc rễ (form-data vs raw JSON), nhưng việc thêm `express.urlencoded()` vào server.js là fix sai nguyên nhân — vấn đề thực là user dùng sai mode trong Postman, không phải server thiếu parser. AI không hướng dẫn trực tiếp "đổi tab Body từ form-data sang raw → JSON" mà thêm code thừa vào server.js.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Tự xóa dòng express.urlencoded() khỏi server.js. Chuyển Body request trên Postman sang dạng `raw` → `JSON` và điền cặp key-value đúng chuẩn.

---

#### **Tác vụ 3: Hướng dẫn cấu hình request GET a product**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash
  - **Prompt:** `cấu hình postman request của get a product`
  - **Thời gian thực hiện:** `01-07-2026 15:26`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Hướng dẫn thiết lập method GET, URL dạng:
  - Cách 1: Truyền thẳng ID vào URL: {{base_url}}/api/products/1
  - Cách 2: Dùng Path Variable: {{base_url}}/api/products/:id và điền giá trị id trong bảng Path Variables của Postman.
  Mô tả response 200 (trả object JSON sản phẩm) và 404 nếu không tìm thấy.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  Cung cấp hướng dẫn trực quan, đúng chuẩn thiết kế của Postman đối với path variables, giúp người dùng dễ dàng chuyển đổi các ID sản phẩm khác nhau khi chạy thử.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Áp dụng Cách 2 (Sử dụng Path Variable `:id`) để quản lý request linh hoạt hơn trong collection.

---

#### **Tác vụ 4: Khắc phục lỗi cú pháp JSON SyntaxError khi gửi orders**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash
  - **Prompt:** `SyntaxError: Unexpected token 'S', ..."on_code": SAVE20 ... is not valid JSON`
  - **Thời gian thực hiện:** `01-07-2026 15:47`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Chẩn đoán đúng lỗi cú pháp JSON: giá trị chuỗi SAVE20 không được đặt trong dấu nháy kép.
  Đưa ra mẫu định dạng đúng cho cả 2 trường hợp (nhập tay và dùng biến Postman):
  - Dùng biến: "coupon_code": "{{coupon_code}}"
  - Dùng giá trị trực tiếp: "coupon_code": "SAVE20"
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  Giải thích đúng bản chất lỗi biên dịch JSON ở middleware `express.json()`. Cú pháp JSON yêu cầu toàn bộ string value phải nằm trong dấu nháy kép `""`.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Sửa lại nội dung Body raw JSON trong Postman: bọc biến `{{coupon_code}}` hoặc chuỗi `SAVE20` trong cặp nháy kép.

---

#### **Tác vụ 5: Đọc file Code Review và điều chỉnh demo source**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Claude Sonnet 4.6
  - **Prompt:** `đọc file code review rồi điều chỉnh lại demo src`
  - **Thời gian thực hiện:** `09-07-2026 14:17`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  - Sửa lỗi đổi tên Environment từ "EShop Env" sang "Activity Env" trong Activity_Environment.json.
  - Điều chỉnh thứ tự các request trong Activity_Collection.json đưa POST login lên đầu tiên.
  - Bổ sung hướng dẫn cài đặt Postman (Postman Setup) vào README.md.
  - Chỉnh sửa giá trị discount example từ 399.996 thành 400.00 trong api_specification.yaml.
  - Quyết định giữ so sánh coupon phân biệt chữ hoa/thường làm "teaching moment" cho học viên.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  Xử lý chính xác các Issues được chỉ ra bởi hệ thống kiểm tra tự động trong `code-review.md`. Đảm bảo tính nhất quán của bộ tài liệu và tính thực tiễn của buổi seminar (học viên có lỗi để phát hiện).
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Chạy thử server và kiểm tra thứ tự chạy collection trong Postman.

---

#### **Tác vụ 6: Tinh chỉnh hướng dẫn cài đặt Newman CLI**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 12:01`
  - **Prompt:** `bỏ phần installation chỗ Newman CLI đi vì đã có hướng dẫn trên  section 2 rồi`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã lược bỏ phần cài đặt Newman CLI tại mục Newman CLI trong tệp User_Guide.md do nội dung này
  đã được hướng dẫn chi tiết ở phần cài đặt trong Section 2. Đồng thời, tôi cũng đã cập nhật lại số
  thứ tự của các bước thực hiện phía sau.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI đã nhận diện đúng vị trí dư thừa và lược bỏ chính xác, đồng thời cập nhật đúng số thứ tự danh sách các bước sau khi loại bỏ mục cũ.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Rà soát cấu trúc danh sách trong User_Guide.md để đảm bảo không bị lỗi định dạng.

---

#### **Tác vụ 7: Tổng quát hóa diễn đạt ở Section 2 của User Guide**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 12:50`
  - **Prompt:** `chỉnh lại cách diễn đạt ở section 2 theo kiểu tổng quát lấy eshop làm ví dụ`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã cập nhật lại cách diễn đạt tại Section 2: Setup Environment in Postman và Verify Connection
  trong tệp User_Guide.md theo hướng tổng quát hơn cho bất kỳ dự án nào, đồng thời nêu rõ dự án
  EShop làm ví dụ minh họa cụ thể.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI đã chỉnh sửa lối hành văn từ mô tả cụ thể về EShop sang hướng dẫn chung của Postman, đồng thời lồng ghép khéo léo ví dụ EShop (e.g., for example) giúp tài liệu mang tính khái quát cao.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Kiểm tra các biến môi trường khớp với bộ dữ liệu EShop Demo thực tế.

---

#### **Tác vụ 8: Tổng quát hóa diễn đạt ở Section 5.2 của User Guide**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 12:55`
  - **Prompt:** `phần 5.2 cũng vậy`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã cập nhật phần Section 5.2 - Collection Runner & Newman CLI trong tệp User_Guide.md theo
  hướng tổng quát:
  1. Tổng quát hóa hướng dẫn: Các bước thao tác (Mở Runner, cấu hình, xuất file JSON, chạy Newman,
     xuất báo cáo) được viết dưới dạng hướng dẫn chung cho mọi dự án, đồng thời đưa tên
     file/collection của dự án EShop làm ví dụ cụ thể (e.g., for example).
  2. Đồng bộ hóa dữ liệu: Điều chỉnh cột tên đăng nhập từ username sang email tại ví dụ file
     login_data.csv của Data-Driven Testing.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI đã tổng quát hóa cách viết cho Newman CLI và Data-Driven Testing, thay thế các tên file cụ thể bằng cấu trúc chung và chú thích ví dụ của EShop.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Rà soát các tên file ví dụ trong tài liệu.

---

#### **Tác vụ 9: Cấu hình nạp dữ liệu cho Collection Runner ở mục Data-Driven Testing**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 13:21`
  - **Prompt:** `phần Running Iterations in  Collection Runner ở Data-Driven Testing mô tả có 2 cách là dùng Test data file rồi upload 1 file trực tiếp vào (nhưng cách này yêu cầu tài khoản Enterprise) hoặc là dùng Dataset xong tạo view trên dataset đó`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã cập nhật lại bước 3 (Running Iterations in Collection Runner) trong phần Data-Driven Testing
  của tệp User_Guide.md để mô tả chi tiết cả 2 cách nạp dữ liệu:
  1. Sử dụng Test Data File: Upload trực tiếp file dữ liệu (áp dụng cho tài khoản Enterprise).
  2. Sử dụng Dataset & View: Tạo Dataset trực tiếp trên Postman Cloud và cấu hình View làm
     nguồn đầu vào (áp dụng cho mọi tài khoản, bao gồm cả tài khoản Free).
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI trình bày rõ ràng sự khác biệt giữa hai phương pháp nạp dữ liệu, đồng thời chỉ ra giới hạn tài khoản của cách thứ nhất.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Chuẩn bị hình ảnh minh họa cho cả hai cách.

---

#### **Tác vụ 10: Điều chỉnh hướng dẫn Cách 2 (dùng Dataset) dựa trên hình ảnh**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 13:25`
  - **Prompt:** `điều chỉnh lại cách 2 dựa trên hình`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã cập nhật lại chi tiết các bước thực hiện của Cách 2: Sử dụng Dataset & Data views trong tệp
  User_Guide.md để mô tả chính xác các thành phần và hành động tương ứng với hình ảnh giao diện bạn cung
  cấp (đặt tên Dataset, thêm nguồn dữ liệu Data source, tạo các Data view và cách chọn nguồn này
  khi chạy Runner), đồng thời chỉnh sửa căn lề thụt đầu dòng (indentation) thẳng hàng với Cách 1.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI đã nhận diện các nhãn giao diện (Data source(s), Data views) từ hình ảnh người dùng cung cấp để hướng dẫn các bước thiết lập chính xác.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Lưu ảnh minh họa với tên postman_dataset.png vào đúng thư mục dự án.

---

#### **Tác vụ 11: Thêm hướng dẫn tạo Dataset trong tài liệu**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 13:29`
  - **Prompt:** `tạo dataset ghi là bấm vào dấu + ở chỗ dataset`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã bổ sung chỉ dẫn click vào biểu tượng dấu cộng (+) bên cạnh mục Datasets ở thanh
  sidebar bên trái để bắt đầu tạo Dataset mới trong phần Cách 2: Sử dụng Dataset & Data views.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  Hướng dẫn thao tác trực quan, giúp người dùng dễ dàng định vị được nút tạo dataset trên giao diện Postman.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Thao tác thử trên Postman để kiểm tra tính khả thi của hướng dẫn.

---

#### **Tác vụ 12: Dịch phần hướng dẫn Data-Driven Testing sang tiếng Anh**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 13:35`
  - **Prompt:** `viết lại bằng tiếng anh phần này đi`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã chuyển ngữ toàn bộ mục Step 3 (Running Iterations in Collection Runner) sang tiếng
  Anh trong tệp User_Guide.md, giữ nguyên các đường dẫn ảnh minh họa cùng căn lề thụt đầu
  dòng (indentation) chuẩn xác.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  Bản dịch tiếng Anh sử dụng các thuật ngữ Postman chuẩn xác, hành văn trôi chảy và tự nhiên.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Lưu lại bản dịch tiếng Anh vào User_Guide.md.

---

#### **Tác vụ 13: Điều chỉnh căn lề định dạng hình ảnh trong tài liệu**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 13:36`
  - **Prompt:** `chỉnh lại format cho hình nằm đúng`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã căn chỉnh lại định dạng hiển thị các hình ảnh trong mục Running Iterations in
  Collection Runner tại tệp User_Guide.md:
  1. Căn lề (Indentation): Đưa toàn bộ các đường dẫn ảnh thụt lề vào dòng 8 khoảng trắng
     để hiển thị chuẩn xác dưới dạng block con của từng mục danh sách.
  2. Khoảng cách: Thêm dòng trắng trước và sau các ảnh để Markdown parser hiển thị ảnh dạng khối.
  3. Sửa lỗi lặp ảnh: Loại bỏ ảnh trùng lặp ở bước cuối cùng (Configure the Runner).
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI hiểu được cú pháp Markdown của danh sách lồng nhau và chỉnh sửa thụt dòng chính xác để parser hiển thị ảnh đúng vị trí.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Xem trước file tài liệu bằng Markdown preview.

---

#### **Tác vụ 14: Căn chỉnh format cho hình ảnh ở Section 2**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Gemini 3.5 Flash (Medium)
  - **Thời gian thực hiện:** `12-07-2026 13:38`
  - **Prompt:** `chỉnh luôn format section 2`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Tôi đã hoàn tất việc căn chỉnh định dạng hiển thị cho toàn bộ hình ảnh trong Section 2:
  Install & Setup tại tệp User_Guide.md:
  1. Khoảng cách: Thêm các dòng trắng phân cách giữa nội dung bước hướng dẫn và hình ảnh
     minh họa để tránh lỗi dính định dạng.
  2. Căn lề (Indentation): Điều chỉnh lề thụt đầu dòng của các thẻ ảnh tương ứng với cấp độ
     thụt lề của văn bản cùng cấp trong danh sách.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  Giúp toàn bộ tài liệu có cấu trúc đồng bộ, giao diện hiển thị chuyên nghiệp và đẹp mắt hơn.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Lưu trữ file User_Guide.md đã hoàn thành định dạng.

---

#### **Tác vụ 15: Thêm bước Create Collection vào mục Setup Environment in Postman**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Claude Opus 4.6
  - **Prompt:** `Trong phần Setup Environment in Postman ở Section 2 thêm một bước Create Collection trước bước 2 Create a new Environment`
  - **Thời gian thực hiện:** `17-07-2026 22:00`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Đã thêm thành công bước 2. Create Collection vào phần Setup Environment in Postman trong User_Guide.md. Cụ thể:
  - Bước 2 mới: Create Collection — hướng dẫn tạo Collection trong sidebar, đặt tên theo project (ví dụ "EShop API Collection"), kèm giải thích Collection dùng để nhóm và quản lý các API request.
  - Các bước cũ đã được đánh số lại: 2→3, 3→4, 4→5, 5→6.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI đã chèn đúng bước tạo Collection trước bước tạo Environment, đồng thời cập nhật chính xác số thứ tự của các bước tiếp theo trong tài liệu hướng dẫn.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Không cần chỉnh sửa thêm, cấu trúc danh sách hiển thị đúng định dạng.

---

#### **Tác vụ 16: Cập nhật bước 6 của Setup Environment để import cả Collection & Environment bằng Ctrl + O**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Claude Opus 4.6
  - **Prompt:** `sửa lại cái bước 6 Import Collection là vừa Collection vừa Environment sử dụng phím tắt Ctrl + O`
  - **Thời gian thực hiện:** `17-07-2026 22:04`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Đã sửa bước 6 trong User_Guide.md:
  - Đổi tiêu đề thành Import Collection & Environment
  - Nội dung hướng dẫn import cả file Collection và Environment (.json)
  - Sử dụng phím tắt Ctrl + O làm cách chính, kèm cách thay thế là click nút Import
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI chỉnh sửa đúng yêu cầu, gộp việc import Collection và Environment vào làm một bước, đồng thời đưa phím tắt Ctrl + O vào làm lựa chọn tối ưu như yêu cầu của người dùng.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Không cần chỉnh sửa thêm.

---

#### **Tác vụ 17: Viết nội dung Section 7 Troubleshooting cho User Guide**
- **1. Prompt + tool (Câu lệnh + Công cụ):**
  - **Tool:** Claude Opus 4.6 (Thinking)
  - **Prompt:** `tham khảo outline_user_guide section 7 rồi viết nội dung đó vào User_Guide`
  - **Thời gian thực hiện:** `15-07-2026 14:18`
- **2. AI output (Kết quả đầu ra của AI):**
  ```text
  Đã viết xong Section 7: Troubleshooting vào User_Guide.md. Nội dung gồm 6 mục theo đúng outline:
  - 7.1: ECONNREFUSED — server chưa chạy hoặc sai base_url trong Environment
  - 7.2: {{auth_token}} undefined — quên chạy Login hoặc thiếu script lưu token
  - 7.3: Postbot không hiện "Ask Postbot" — chưa gửi request hoặc hết AI credits
  - 7.4: Newman báo collection not found — sai đường dẫn file JSON
  - 7.5: Assertion fail không rõ nguyên nhân — hướng dẫn dùng Postman Console (Ctrl+Alt+C) và console.log() để debug
  - 7.6: HTML report không generate — quên cài newman-reporter-html
  Mỗi mục đều có cấu trúc Symptom → Cause → Solution kèm code/command cụ thể, đảm bảo người đọc có thể tự khắc phục.
  ```
- **3. Verdict (Đánh giá):** **VALID**
- **4. Reasoning (Lý do đánh giá):**
  AI đã tham khảo chính xác 6 mục lỗi thường gặp được liệt kê trong file `outline_user_guide.md` Section 7 và triển khai thành nội dung hoàn chỉnh với cấu trúc Symptom/Cause/Solution nhất quán. Mỗi mục đều có lệnh CLI mẫu và đoạn code JavaScript minh họa, giúp người đọc dễ dàng tự khắc phục lỗi. Nội dung viết bằng tiếng Anh đúng yêu cầu của Seminar_Guide.
- **5. Student fix (Bản sửa đổi của sinh viên):**
  Không cần chỉnh sửa thêm, nội dung đầy đủ và chính xác theo outline.

---

## Overall AI Accuracy Ratio

| Trạng thái | Số lượng | Tỷ lệ phần trăm |
|---|---|---|
| VALID | 17 | 100% |
| INVALID | 0 | 0% |
| INCOMPLETE | 0 | 0% |
| **Tổng số tác vụ kiểm định** | **17** | **100%** |

### Nhận xét chung:
- **AI Strengths (Điểm mạnh):**
  - Tạo khung cấu trúc thư mục backend rất nhanh và đầy đủ file.
  - Phân tích tốt và bắt lỗi cú pháp HTTP/JSON cực kỳ chính xác.
  - Đọc hiểu sâu tài liệu phản hồi lỗi (`code-review.md`) để chỉnh sửa cấu trúc dữ liệu JSON (Postman collection & environment).
  - Khả năng sửa đổi tài liệu nhanh chóng, dịch thuật sang tiếng Anh trôi chảy và định dạng Markdown lồng nhau (nested list) chuẩn xác.
- **AI Limitations (Hạn chế):**
  - Không thể tự động chạy kiểm thử trực tiếp trên Postman (cần giả lập/hướng dẫn tester làm thủ công).
  - Đôi khi cần tester hướng dẫn chi tiết về cấu trúc UI (như phím bấm, vị trí nút tạo Dataset) để cập nhật tài liệu chính xác nhất.
- **When to Use (Nên dùng khi nào):**
  - Khi cần tạo khung code dự án demo, viết tài liệu OpenAPI Spec, sửa lỗi cấu hình, dịch thuật tài liệu hướng dẫn và tối ưu hóa định dạng Markdown.
- **When NOT to Use (Không nên dùng khi nào):**
  - Khi cần ra các quyết định bảo mật thực tế (JWT key cứng, password dạng plain-text cần được cảnh báo rõ ràng nếu đưa vào production).
- **Key Principle (Nguyên tắc cốt lõi):**
  - Trợ lý AI thực hiện rất tốt vai trò sinh mã nguồn mẫu tối giản và soạn thảo tài liệu hướng dẫn, tuy nhiên cần có sự rà soát thủ công từ lập trình viên để đảm bảo tính an toàn hệ thống và logic nghiệp vụ thực tế.
