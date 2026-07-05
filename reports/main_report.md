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
| TC-FR06-13 | `POST /api/cart` | `id=1, quantity=2147483648` | (Spec gap) Giá trị vượt INT32_MAX — nên có validate để tránh tràn số | Status : 200, message : "added to cart" | Failed (gộp BUG-06-02) |
| TC-FR06-14 | `GET /api/products/:id` | `id=2` (min+1) | Trả về thông tin chi tiết sản phẩm thứ 2 | Trả về thông tin chi tiết(nếu như có tồn tại trong CSDL) | Passed |
| TC-FR06-15 | `GET /api/products/:id` | `id=2147483647` | Nên trả 404 vì ID không tồn tại | Status 200, `{}` | Failed |
| TC-FR06-16 | `GET /api/products/:id` | `id=2147483648` | Nên trả 404 hoặc 400 (tràn số) | Status 200, `{}` | Failed |
| TC-FR06-17 | `POST /api/cart` | `id=1, quantity=2` (min+1) | Thêm vào giỏ thành công, đúng 2 sản phẩm |Thêm vào giỏ hàng đúng 2 sản phẩm | Passed |
| TC-FR06-19 | `GET /api/products/` | (id rỗng) | Khớp route danh sách sản phẩm (không có `:id`) — hành vi routing đúng, không phải lỗi | Trả về danh sách đầy đủ sản phẩm | Passed |

### 5. Functional/Event-based Testing bổ sung (Ngoài phạm vi Domain Testing)

> **Ghi chú phương pháp luận:** Test case dưới đây không thuộc kỹ thuật Equivalence Partitioning/BVA vì không phân hoạch một miền giá trị dữ liệu cụ thể, mà kiểm thử hành vi phản hồi của hệ thống đối với một **sự kiện tương tác UI** (button click). Được thực thi qua trình duyệt (không qua Postman) vì bản chất sự kiện này chỉ tồn tại ở tầng giao diện.

| Test ID | Đối tượng kiểm thử | Hành động | Kết quả kỳ vọng (Expected Result) | Kết quả thực tế (Actual) | Pass/Fail |
|---|---|---|---|---|---|
| TC_06_FUNC_01 | Nút "Thêm vào giỏ hàng" (Giao diện) | Click 1 lần vào nút, quan sát biểu tượng giỏ hàng trên Header | Hệ thống tiếp nhận sự kiện ngay lập tức, gọi API và cập nhật số lượng sản phẩm vào giỏ hàng chỉ sau 1 lần bấm | Giỏ hàng không cập nhật sau lần click đầu tiên; phải click thêm lần 2 hệ thống mới ghi nhận | Failed |

**Bug liên quan:** BUG-06-03 (xem `bug_report.md`) 
### 6. AI Gap Analysis

Trong quá trình áp dụng Domain Testing/BVA cho FR-06 với sự hỗ trợ của AI (Gemini, Antigravity), chúng tôi ghi nhận các khoảng trống (gap) sau:

1. **AI tự mâu thuẫn giữa các mục trong cùng 1 tài liệu (Inconsistency):** Ở mục Boundary Value Analysis (mục 3), AI xác định đúng rằng biến `quantity` không có upper bound trong spec và gắn nhãn "spec gap" một cách hợp lý. Tuy nhiên, khi tổng hợp sang Test Case Table (mục 4), AI lại tự ý quay lại giả định cũ "Phải có thông báo vượt quá mặt hàng trong kho" cho TC-FR06-12/13 — một business rule hoàn toàn không tồn tại trong spec lẫn trong response thực tế của API. Đây là lỗi AI không tự kiểm tra tính nhất quán giữa các phần nó vừa tạo ra.

2. **Giới hạn cố hữu: AI không thể tự thực thi HTTP request thật:** Toàn bộ các bug quan trọng nhất (status 200 kèm body rỗng, chấp nhận quantity âm/tràn số) chỉ phát hiện được sau khi em tự gọi Postman. AI chỉ dự đoán Expected Result trên lý thuyết, không thể biết hành vi thực tế đang chạy.

Nguyên nhân chung: sự phức tạp của việc duy trì tính nhất quán xuyên suốt 1 tài liệu dài, và giới hạn  của AI khi không có khả năng thực thi mã/gọi API thật.

### 7. AI Critique

> Trong quá trình làm FR-06, AI hỗ trợ rất tốt trong giai đoạn đầu khi cần liệt kê các phân hoạch tương đương chuẩn cho các biến số. Giúp tiết kiệm thời gian đáng kể so với việc tự làm thủ công ngay từ đầu. Bên cạnh đó có những sai sót đáng lưu ý như : AI không bảo đảm nhất quán tuyệt đối giữa các mục trong bài làm hoặc cùng tài liệu. Dù đã đúng khi gắn nhãn "spec gap" cho biên trên của quantity ở phần Boundary Value Analysis, AI lại quay về giả định tồn kho tự bịa ("vượt quá hàng tồn kho") khi viết Expected Result cho Test Case Table — một business rule không hề tồn tại trong spec. 
==> Chính vì thế quan trọng là bản thân phải chủ động đọc lại toàn bộ tài liệu và kiểm tra tính nhất quán, đúng đắn của từng phần riêng lẻ. Ngoài ra AI cũng không thể thay thế con người trong việc thực thi thật. Do đó bản thân người dùng phải là người cuối cùng kiểm tra và quyết định tính đúng đắn.

## FR-07: Giỏ hàng (Shopping Cart)

### 1. Input Variables

| Biến | Nguồn | Ràng buộc theo spec | Ghi chú (nếu phải tự suy luận) |
|---|---|---|---|
| `Authorization` (Token) | Header (`GET /api/cart`) | Yêu cầu phải có Token hợp lệ | (Suy luận) JWT token hợp lệ của user đã đăng nhập. |
> **Phát hiện ban đầu:** Business spec FR-07 yêu cầu "Số lượng (có nút +/- để chỉnh)", nhưng qua kiểm tra UI thực tế, trang Giỏ hàng chỉ có nút Xóa, không có nút +/-. Ghi nhận là **BUG-07-01** (xem `bug_report.md`). <br>
> **Kết luận sau thực nghiệm :** Ban đầu dự kiến có thêm biến `id` (sản phẩm cần xóa) để test EC/BVA cho thao tác Xóa. Tuy nhiên, qua kiểm tra thực tế bằng DevTools (Network tab) và Postman, em xác nhận:
> - Nút "Thêm vào giỏ hàng" (FR-06) **không gọi** `POST /api/cart`
> - Nút "Xóa sản phẩm" (FR-07) **không gọi** bất kỳ API nào
> - `localStorage` chỉ chứa `token`, không lưu dữ liệu giỏ hàng
> - `GET /api/cart` qua Postman luôn trả về `[]` dù UI hiển thị sản phẩm
>
> → Toàn bộ tính năng giỏ hàng hiện tại chỉ tồn tại trong React state tạm thời, không có bất kỳ tích hợp Backend nào. Do đó, **không thể thiết kế EC/BVA cho biến `id` (xóa sản phẩm) ở tầng API** vì không có endpoint nào để test. Đây được ghi nhận là **BUG-07-01 (Critical)** — xem `bug_report.md`.