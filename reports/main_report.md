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

### 2. Equivalence Partitioning

**Biến `Authorization` (Token):**

| Lớp | Mô tả | Giá trị đại diện | Loại | Căn cứ từ spec |
|---|---|---|---|---|
| Token hợp lệ | JWT token của người dùng đang đăng nhập | Bearer eyJhbG... | Valid | "Yêu cầu Header: `Authorization: Bearer <token>`" |
| Thiếu Token | Không gửi Header Authorization | (Rỗng) | Invalid | "Yêu cầu Header" |
| Sai format Token | Chuỗi ngẫu nhiên, không theo chuẩn JWT | Bearer abc123xyz | Invalid | (Suy luận) Hệ thống cần JWT đúng định dạng để giải mã |
| Token hết hạn | JWT token đã quá hạn sử dụng | Bearer (token cũ) | Invalid | (Suy luận) Bảo mật cơ bản của JWT — **Not executable**, cần thời gian chờ token hết hạn thật |

### 3. Boundary Value Analysis

*Không áp dụng* — biến `Authorization` là chuỗi token, không thuộc domain có thứ tự/số học nên không có khái niệm "biên" theo đúng nghĩa BVA.

### 4. Test Case Table

| Test ID | API | Input | Expected Result | Actual | Pass/Fail |
|---|---|---|---|---|---|
| TC-FR07-01 | `GET /api/cart` | Token hợp lệ | Trả về danh sách sản phẩm trong giỏ (200 OK) | Status 200 OK | Passed |
| TC-FR07-02 | `GET /api/cart` | Không có Header Authorization | 401 Unauthorized | Status 401, "error": "Unauthorized" | Passed |
| TC-FR07-03 | `GET /api/cart` | Token sai format (`Bearer abc123xyz`) | 401 Unauthorized | Status 403, "error": "Forbidden" | Failed |
| TC-FR07-04 | `GET /api/cart` | Token hết hạn | 401 Unauthorized | Not executable | N/A |
### 5. Functional/Event-based Testing (Ngoài phạm vi Domain Testing)

| Test ID | Đối tượng | Hành động | Expected Result | Actual | Pass/Fail |
|---|---|---|---|---|---|
| TC_07_FUNC_01 | Nút "Thêm vào giỏ hàng" | Thêm 1 sản phẩm | UI cập nhật giỏ hàng VÀ gọi `POST /api/cart` | UI cập nhật nhưng KHÔNG gọi API nào | Failed |
| TC_07_FUNC_02 | Nút "Xóa sản phẩm" | Xóa 1 sản phẩm trong giỏ | UI xóa sản phẩm VÀ gọi API xóa tương ứng | UI xóa nhưng KHÔNG gọi API nào | Failed |
| TC_07_FUNC_03 | Thao tác F5/reload trang giỏ hàng | Reload sau khi đã thêm sản phẩm | Giỏ hàng vẫn giữ nguyên dữ liệu (do đã lưu Backend) | Giỏ hàng bị mất trắng sau F5 | Failed |
| TC_07_FUNC_04 | Thêm 2 lần cùng 1 sản phẩm | Thêm sản phẩm A, sau đó thêm lại sản phẩm A lần nữa | Tăng số lượng dòng cũ, không tạo dòng mới | Tạo thành 2 dòng mới, không cộng dồn | Failed |
| TC_07_FUNC_05 | Nút Xóa sản phẩm | Bấm nút Xóa | Hiện dialog xác nhận trước khi xóa thật | Không có dialog box xác nhận trước khi xoá | Failed |
| TC_07_FUNC_06 | Giỏ hàng trống | Xóa hết sản phẩm hoặc chưa thêm gì | Hiển thị giỏ hàng trống, có nút chuyển sang danh sách sản phẩm |Hiển thị giỏ hàng trống, có nút chuyển sang danh sách sản phẩm | Passed |
| TC_07_FUNC_07 | Nhãn "Tổng cộng" | Kiểm tra giao diện giỏ hàng | Nhãn "Tổng cộng" hiển thị rõ ràng | Nhãn "Tổng cộng" không xuất hiện | Failed |

### 6. AI Gap Analysis

Trong quá trình áp dụng Domain Testing/BVA cho FR-07 với sự hỗ trợ của AI (Gemini, Antigravity), chúng tôi ghi nhận các khoảng trống (gap) sau:

1. **AI không thể tự phát hiện bug tích hợp giữa Frontend và Backend:** Bug nghiêm trọng nhất của FR-07 (giỏ hàng hoàn toàn không gọi API, chỉ tồn tại ở React state) không thể phát hiện được chỉ qua phân tích spec. Việc phát hiện bug này đòi hỏi em phải dùng DevTools (Network tab) quan sát hành vi thực tế, một thao tác nằm ngoài khả năng của AI khi không có quyền truy cập trình duyệt đang chạy.

2. **AI ban đầu thiết kế EC/BVA cho 1 tính năng không tồn tại (biến `id` xóa sản phẩm):** Do `api_specification.md` không tài liệu hóa endpoint xóa giỏ hàng, AI đã đúng khi gắn nhãn ban đầu là "spec gap" và tự suy luận 1 bộ EC hợp lý (ID hợp lệ, không tồn tại, sai kiểu...). Tuy nhiên, sau khi thực nghiệm, toàn bộ bộ EC này trở nên vô nghĩa vì không có endpoint thật để test — đây không phải lỗi của AI (nó không thể biết trước điều này nếu không có input từ em), nhưng cho thấy giới hạn cố hữu: **EC/BVA thiết kế trên giấy luôn cần được xác thực lại bằng thực nghiệm trước khi coi là hoàn chỉnh**, đặc biệt với các phần spec không tài liệu hóa rõ ràng.

3. **AI không tự liên kết bug giữa các feature (cross-feature blind spot):** Vì mỗi feature được xử lý trong 1 mỗi lần làm việc riêng biệt theo đúng cấu trúc đề bài, AI không tự nhận ra rằng lỗi thiếu validate `quantity` ở FR-06 (BUG-06-02) sẽ gây hậu quả trực tiếp lên phần hiển thị tổng tiền ở FR-07 (BUG-07-06 — NaN/số âm). Việc phát hiện mối liên hệ này hoàn toàn đến từ việc em chủ động thử luồng end-to-end (từ trang sản phẩm sang trang giỏ hàng), không phải từ AI tự suy luận.

4. **AI không tự đối chiếu business spec với hành vi UI thực tế:** Business spec FR-07 mô tả rõ "Số lượng (có nút +/- để chỉnh)", nhưng vì AI chỉ làm việc dựa trên văn bản spec (không tự mở trình duyệt kiểm tra), nó không thể tự phát hiện rằng tính năng này không được triển khai trên UI thật (BUG-07-01) cho đến khi  báo lại.

Nguyên nhân chung của các gap trên: AI hỗ trợ tốt ở giai đoạn *thiết kế lý thuyết* (dựa trên văn bản spec), nhưng hoàn toàn phụ thuộc vào bản thân con người review để *xác thực bằng thực nghiệm* — đặc biệt với các loại lỗi chỉ lộ ra khi quan sát hành vi runtime thực tế (network requests, tương tác giữa các trang, ...).

### AI Critique
> Trong quá trình ban đầu AI rất nhanh chóng trong việc xác định input variables. Nhưng với nội dung đã đề cập ở `6. AI Gap Analysis` trên em nhận ra AI rất nhanh xác định biến và chia miền, tạo ra bảng test case nhưng những gì AI có thể làm đó là dựa vào spec đã cho chứ không thể biết thực tế mà phần mềm chưa có làm theo spec. Cần phải 1 bước em phải đọc lại những gì AI gen ra và đối chiếu lại với thực tế sau đó check lại. Đảm bảo rằng tài liệu phải chính xác, phù hợp với thực tế chứ không tin tưởng 100% AI vì AI có thể ảo giác hoặc suy nghĩ quá mức.

## FR-14: Quản lý Danh mục (Category CRUD)

### 1. Input Variables

| Biến | Nguồn | Ràng buộc theo spec | Ghi chú (nếu phải tự suy luận) |
|---|---|---|---|
| `Authorization` (Token) | Header (tất cả API danh mục) | Yêu cầu phải có Token hợp lệ và `role = 'admin'` (theo FR-12) | (Suy luận) Nếu là user thường (role='user') sẽ bị từ chối truy cập (403). |
| `name` | Body field (`POST /api/categories`, `PUT /api/categories/:id`) | Tên danh mục là bắt buộc, không được để trống | (Suy luận) Phải là chuỗi (string). Thường PUT cũng nhận tham số này để đổi tên DM. |
| `id` | Path param (`PUT /api/categories/:id`, `DELETE /api/categories/:id`) | Không có ràng buộc tường minh | (Suy luận) ID phải tồn tại trong CSDL để có thể cập nhật hoặc xóa. |

### 2. Equivalence Partitioning

**Biến `Authorization` (Token):**

| Lớp | Mô tả | Giá trị đại diện | Loại | Căn cứ từ spec |
|---|---|---|---|---|
| Token hợp lệ (Admin) | JWT token của người dùng có `role = 'admin'` | Bearer admin_token | Valid | "Yêu cầu Token hợp lệ", "role = 'admin' trong Token" |
| Token hợp lệ (User) | JWT token của người dùng thường (`role = 'user'`) | Bearer user_token | Invalid | "Chỉ dành cho tài khoản có role = 'admin'" |
| Thiếu Token | Không gửi Header Authorization | (Rỗng) | Invalid | "Yêu cầu Token JWT hợp lệ" |
| Sai format Token | Chuỗi không theo chuẩn JWT | Bearer abcxyz | Invalid | (Suy luận) Hệ thống cần Token hợp lệ |

**Biến `name`:**

| Lớp | Mô tả | Giá trị đại diện | Loại | Căn cứ từ spec |
|---|---|---|---|---|
| Tên hợp lệ | Chuỗi ký tự alphabet  | "Đồ điện tử" | Valid | "Tên danh mục là bắt buộc" |
| Chuỗi rỗng | Chuỗi không chứa ký tự nào | "" | Invalid | "không được để trống" |
| Chỉ toàn khoảng trắng | Chuỗi chỉ chứa dấu cách | "   " | Invalid | (Suy luận) Khoảng trắng cũng tương đương với để trống |
| Sai kiểu dữ liệu | Gửi kiểu số, boolean hoặc object | 123 | Invalid | (Suy luận) Phải là kiểu chuỗi |
| Null / Thiếu field | Không gửi trường name hoặc gửi null | null | Invalid | "Tên danh mục là bắt buộc" |

**Biến `id`:**

| Lớp | Mô tả | Giá trị đại diện | Loại | Căn cứ từ spec |
|---|---|---|---|---|
| ID hợp lệ | ID của một danh mục đang tồn tại trong hệ thống | 1 | Valid | Cập nhật hoặc Xóa danh mục tương ứng |
| ID không tồn tại | Số nguyên dương không có trong DB | 9999 | Invalid | (Suy luận) Lỗi 404 Not Found do không tìm thấy danh mục |
| Sai kiểu dữ liệu | Gửi ID dưới dạng chuỗi/boolean | "abc" | Invalid | (Suy luận) ID thường là số nguyên |
