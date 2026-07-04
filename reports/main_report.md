# Domain Testing 

## FR-06: Xem chi tiết sản phẩm

### 1. Input Variables

| Biến | Nguồn | Ràng buộc theo spec | Ghi chú (nếu phải tự suy luận) |
|---|---|---|---|
| `id` | Path param (`GET /api/products/:id`) và Body field (`POST /api/cart`) | Không có ràng buộc tường minh | (Suy luận) ID là số nguyên dương tự tăng (>= 1). Tồn tại trong CSDL. |
| `quantity` | Body field (`POST /api/cart`) | Số nguyên dương, tối thiểu là 1 | Spec không nêu giới hạn trên (max). (Suy luận) Phải là số nguyên. |

### 2. Equivalence Partitioning

**Biến `id`:**

| Lớp | Mô tả | Giá trị đại diện | Loại | Căn cứ từ spec |
|---|---|---|---|---|
| Valid ID | Số nguyên dương, tồn tại | 1 | Valid | (Suy luận) ID thông thường bắt đầu từ 1 |
| ID không tồn tại | Số nguyên dương nhưng không có trong DB | 999999 | Invalid | (Suy luận) Lỗi 404 hoặc lỗi validation liên kết |
| ID âm / bằng 0 | Số nguyên <= 0 | 0, -1 | Invalid | (Suy luận) ID thường là số nguyên dương tự tăng |
| ID thập phân | Số có phần thập phân | 1.5 | Invalid | (Suy luận) Sai kiểu, ID là số nguyên |
| Sai kiểu dữ liệu | Chuỗi, boolean | "abc", true | Invalid | (Suy luận) Phải là số |
| Rỗng / Null | Không truyền param hoặc truyền null | null | Invalid | (Suy luận) Thiếu field bắt buộc |


**Biến `quantity`:**

| Lớp | Mô tả | Giá trị đại diện | Loại | Căn cứ từ spec |
|---|---|---|---|---|
| Valid quantity | Số nguyên dương >= 1 | 5 | Valid | "số nguyên dương, tối thiểu là 1" |
| Dưới min | Giá trị < 1 | 0, -3 | Invalid | "tối thiểu là 1" |
| Số thập phân | Số không nguyên | 1.5 | Invalid | "số nguyên dương" |
| Sai kiểu dữ liệu | Chuỗi, boolean, object | "abc", true | Invalid | (Suy luận) Phải là kiểu số |
| Rỗng / Null | Không gửi quantity | null | Invalid | (Suy luận) Field bắt buộc |

### 3. Boundary Value Analysis

**Biến `id`:** (Domain số nguyên, min = 1)
*Lưu ý: Không có biên trên rõ ràng trong spec.*

| Test | Giá trị | Vị trí biên |
|---|---|---|
| Min - 1 | 0 | min - 1 |
| Min | 1 | min |
| Min + 1 | 2 | min + 1 |
| Robustness (INT_MAX) | 2147483647 | (Suy luận) Max giới hạn hệ thống |
| Robustness (INT_MAX + 1) | 2147483648 | (Suy luận) Overflow thăm dò |

**Biến `quantity`:** (Domain số nguyên, min = 1)
*Spec không nêu giới hạn trên — đây là spec gap. Đề xuất test case robustness (giá trị rất lớn, giá trị gây tràn số nguyên INT32_MAX) để thăm dò hành vi thực tế, kết quả Expected cần đánh dấu 'chưa xác định — cần thực nghiệm'.*

| Test | Giá trị | Vị trí biên |
|---|---|---|
| Min - 1 | 0 | min - 1 |
| Min | 1 | min |
| Min + 1 | 2 | min + 1 |
| Max (Spec gap) | 2147483647 | (Suy luận) Tràn số / Khối lượng cực lớn |
| Max + 1 | 2147483648 | (Suy luận) Vượt giới hạn số nguyên |

### 4. Test Case Table

| Test ID | API | Input | Expected Result (theo spec) | Actual | Pass/Fail |
|---|---|---|---|---|---|
| TC-FR06-01 | `GET /api/products/:id` | `id=1` | Trả về thông tin chi tiết sản phẩm |Trả về thông tin sản phảm |Passed |
| TC-FR06-02 | `GET /api/products/:id` | `id=999999` | Lỗi, không tìm thấy sản phẩm | Trả về `{}` (status: 200 ), không có message lỗi | Failed |
| TC-FR06-03 | `GET /api/products/:id` | `id=0` | (Suy luận) Hệ thống nên trả lỗi 400/404 cho ID không hợp lệ | Trả về `{}` (status: 200), không có message lỗi | Failed |
| TC-FR06-04 | `GET /api/products/:id` | `id="abc"` | Lỗi sai kiểu dữ liệu |Trả về {} (status: 200), không có message lỗi |Failed |
| TC-FR06-05 | `POST /api/cart` | `id=1, quantity=5` | Thêm vào giỏ thành công |Status : 200, message : "added to cart" | Passed |
| TC-FR06-06 | `POST /api/cart` | `id=1, quantity=0` | Lỗi "tối thiểu là 1" |Status : 200, message : "added to cart" |Failed |
| TC-FR06-07 | `POST /api/cart` | `id=1, quantity=-3` | Lỗi "số nguyên dương, tối thiểu là 1" |Status : 200, message : "added to cart" | Failed |
| TC-FR06-08 | `POST /api/cart` | `id=1, quantity=1.5` | Lỗi, chỉ nhận số nguyên |Status : 200, message : "added to cart" | Faile |
| TC-FR06-09 | `POST /api/cart` | `id=1, quantity="abc"` | Lỗi sai kiểu dữ liệu quantity |Status : 200, message : "added to cart" | Failed |
| TC-FR06-10 | `POST /api/cart` | `id=1, quantity=null` | Lỗi thiếu quantity bắt buộc |Status : 200, message : "added to cart" | Failed |
| TC-FR06-11 | `POST /api/cart` | `id=1, quantity=1` (min) | Thêm vào giỏ thành công |Status : 200, message : "added to cart" | Passed |
| TC-FR06-12 | `POST /api/cart` | `id=1, quantity=2147483647` | Theo spec vẫn là số nguyên dương hợp lệ → nên chấp nhận | Status 200, "added to cart" | Passed  |
| TC-FR06-13 | `POST /api/cart` | `id=1, quantity=2147483648` | Phải có thông báo vượt quá mặt hàng trong kho | Status : 200, message : "added to cart" | Failed |
| TC-FR06-14 | `GET /api/products/:id` | `id=2` (min+1) | Trả về thông tin chi tiết sản phẩm thứ 2 | Trả về thông tin chi tiết(nếu như có tồn tại trong CSDL) | Passed |
| TC-FR06-15 | `GET /api/products/:id` | `id=2147483647` | Nên trả 404 vì ID không tồn tại | Status 200, `{}` | Failed |
| TC-FR06-16 | `GET /api/products/:id` | `id=2147483648` | Nên trả 404 hoặc 400 (tràn số) | Status 200, `{}` | Failed |
| TC-FR06-17 | `POST /api/cart` | `id=1, quantity=2` (min+1) | Thêm vào giỏ thành công, đúng 2 sản phẩm |Thêm vào giỏ hàng đúng 2 sản phẩm | Passed |
| TC-FR06-19 | `GET /api/products/` | (id rỗng) | Khớp route danh sách sản phẩm (không có `:id`) — hành vi routing đúng, không phải lỗi | Trả về danh sách đầy đủ sản phẩm | Passed |
