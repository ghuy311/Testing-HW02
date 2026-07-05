# AI Audit Report — Mẫu 5 mục cho mỗi Artifact

*Phụ lục bắt buộc đính kèm cho mọi bài tập có dùng AI (HW#01–HW#06, Seminar).*
*Tài liệu được biên soạn lại từ Med Kharbach, PhD (2026) — Mẫu Chính sách Sử dụng AI cho Giáo dục Đại học.*
*Giấy phép CC BY-NC-SA 4.0. Phiên bản này được FIT@HCMUS điều chỉnh cho môn CS423 / CSC15003 Kiểm chứng Phần mềm.*

---

## 1. Thông tin Sinh viên

| Mục | Giá trị |
| :--- | :--- |
| **Họ tên sinh viên (in hoa):** |HỒ GIA HUY |
| **MSSV:** | 23127376 |
| **Lớp / Khoá:** |23KTPM2 / 23CLC |
| **Mã bài tập (ví dụ HW#00, HW#02):** |HW02 |
| **Ngày làm bài:** |04/07/2026 |
| **Công cụ AI đã dùng:** |  |
| **Công cụ AI đã dùng:** | `[X] Có`  `[] Không` |

---

## 2. Hướng dẫn (đọc trước khi điền)
* Thêm 1 hàng cho mỗi artifact AI sinh (test case, script, checklist, OpenAPI spec, JMeter plan…).
* Dán nguyên văn prompt — **KHÔNG** paraphrase.
* Dán nguyên văn output AI (hoặc kèm screenshot có chú thích trong báo cáo).
* Gắn nhãn: `VALID` / `INVALID` / `INCOMPLETE`.
* Lý do phải dẫn chiếu slide, mục ISTQB, hoặc RFC kỹ thuật.
* Hiển thị bản sửa với phần thay đổi được tô sáng.
* *Hàng mẫu in nghiêng — thay trước khi nộp.*

---

## 3. Bảng Audit — 1 hàng / artifact

| (1) Prompt + Công cụ | (2) Output AI | (3) Verdict | (4) Lý do (ISTQB) | (5) Bản SV sửa |
| :--- | :--- | :--- | :--- | :--- |
| **Artifact #1**<br>**Công cụ:** Antigravity <br>**Thời gian** : 04/07/2026 10:27AM <br> **Prompt:** "Trước khi làm gì, hãy đọc kỹ file skills/eshop-domain-testing-skill.md trong project này và tuân thủ CHÍNH XÁC toàn bộ quy trình, ràng buộc, và định dạng file output được mô tả trong đó — không được bỏ qua bước nào hay tự ý làm khác.<br><br>Sau khi đọc xong, xác nhận ngắn gọn bạn đã hiểu:<br>1. Vai trò & ràng buộc (không đọc source code, không tự bịa domain, không tự điền Actual/Pass-Fail)<br>2. Các file cần đọc (docs/eshop-spec.md, docs/api_specification.md) và các file được phép ghi (reports/main_report.md, reports/ai_audit_report.md) — tuyệt đối không đụng vào reports/bug_report.md<br><br>Sau khi xác nhận, áp dụng skill để làm Domain Testing + BVA cho FR-XX." |Tôi đã đọc kỹ và xác nhận hiểu rõ các yêu cầu từ file `skills/eshop-domain-testing-skill.md`:<br><br>1. **Vai trò & ràng buộc:** Chỉ phân tích thiết kế test dựa trên tài liệu đặc tả, không đọc source code, không tự suy diễn các ràng buộc không có trong tài liệu và tuyệt đối để trống các cột Actual/Pass-Fail để con người (sinh viên) tự thực thi.<br>2. **Quản lý File:** Chỉ đọc thông tin từ `docs/eshop-spec.md` và `docs/api_specification.md`. Kết quả sẽ chỉ được ghi nối (append) vào `reports/main_report.md` và log vào `reports/ai_audit_report.md`. Tuyệt đối **không** tạo, sửa hay chạm vào file `reports/bug_report.md`.<br><br>**Về việc áp dụng cho FR-XX:** <br>Dựa theo quy trình bắt buộc (Bước 1), tôi đã kiểm tra file `docs/eshop-spec.md` nhưng **không tìm thấy tính năng nào có mã "FR-XX"**.<br><br>Theo đúng quy tắc của skill (không tự bịa mô tả feature), tôi phải dừng lại tại đây. Vui lòng cung cấp cho tôi mã tính năng chính xác (ví dụ: `FR-01`, `FR-09`,...) mà bạn muốn áp dụng kiểm thử, tôi sẽ tiến hành phân tích và tạo test case ngay! |Complete |AI đã thực hiện đúng theo các điều kiện tiên quyết (Preconditions) và ràng buộc được cấu hình trong Prompt | |
| **Artifact #2**<br>**Công cụ:** Antigravity <br>**Thời gian** : 04/07/2026 10:29AM <br> **Prompt:** "Áp dụng skill Domain Testing cho FR-06" | Tôi đã áp dụng thành công kỹ thuật Domain Testing và Boundary Value Analysis (BVA) cho tính năng **FR-06: Xem chi tiết sản phẩm**. <br><br>**Các thay đổi đã thực hiện:**<br>- Bảng Test Case hoàn chỉnh đã được nối (append) vào file main_report.md.<br>- Nhật ký thực hiện (log) đã được nối vào cuối file ai_audit_report.md.<br>- Các cột *Actual* và *Pass/Fail* trong Test Case Table hoàn toàn để trống cho bạn tự điền khi chạy test thực tế.<br><br>---<br><br>### Nguyên liệu cho AI Gap Analysis (Bước 6)<br>*Lưu ý: Dưới đây là các điểm thiếu sót từ tài liệu đặc tả mà tôi phải tự suy luận để xây dựng test case. Hãy dùng thông tin này để tự viết vào phần AI Critique/Gap Analysis của bạn.*<br><br>1. **Các biến/lớp phải tự suy luận:**<br>   - **`id` (Product ID):** Spec không mô tả bất kỳ ràng buộc nào. Tôi phải tự suy luận đây là một số nguyên dương tự tăng, và phải tồn tại trong CSDL.<br>   - **`quantity` (Số lượng):** Spec chỉ ghi là "số nguyên dương, tối thiểu là 1", do đó tôi phải suy luận thêm các test case cho loại dữ liệu sai (string, boolean, null) và số thập phân.<br>2. **Biên không xác định (Spec gaps):**<br>   - Cả `id` và `quantity` đều **không có giới hạn trên (Max)** trong tài liệu đặc tả.<br>   - Tôi đã đề xuất dùng max value của số nguyên 32-bit (`2147483647` và `2147483648`) làm test case robustness để thăm dò hiện tượng tràn bộ nhớ (overflow) của hệ thống thực.<br>3. **Mâu thuẫn nhỏ đáng lưu ý:**<br>   - Trong mô tả FR-06, người dùng chỉ nhập "Số lượng" rồi bấm Thêm vào giỏ. Tuy nhiên, ở `api_specification.md`, body của `POST /api/cart` lại yêu cầu gửi kèm cả `name` và `price`. Về nguyên tắc, server không nên tin tưởng `price` gửi từ client để tránh rủi ro bảo mật (nhưng trong phạm vi test đầu vào tôi chỉ tập trung vào `id` và `quantity`).<br><br>Hãy tiến hành thực thi bằng Postman/cURL và cập nhật file **bug_report.md** nếu bạn phát hiện ra lỗi thực tế nhé! | Incomplete | Agent định nghĩa boundary value ở mục 3 nhưng không tạo đủ test case tương ứng ở mục 4. Và bỏ quên vấn đề bảo mật trong skill có đề cập. | Bổ sung TC-FR06-14 -> TC-FR06-18 |
| **Artifact #3** <br> **Công cụ :** Antigravity <br> **Thời gian** : 04/07/2026 10:12PM <br> **Prompt :** "làm cho tôi FR-07 tuần tự theo các bước của quy trình kiểm thử để tôi tách thành nhiều commit theo trình tự"|<h2>FR-07: Giỏ hàng (Shopping Cart)</h2><h3>1. Input Variables</h3><table><thead><tr><th>Biến</th><th>Nguồn</th><th>Ràng buộc theo spec</th><th>Ghi chú (nếu phải tự suy luận)</th></tr></thead><tbody><tr><td><code>Authorization</code> (Token)</td><td>Header (<code>GET /api/cart</code>)</td><td>Yêu cầu phải có Token hợp lệ</td><td>(Suy luận) JWT token hợp lệ của user đã đăng nhập.</td></tr><tr><td>Các tham số xóa/sửa số lượng</td><td>Tương tác UI (nút <code>+/-</code>, <code>Xóa sản phẩm</code>)</td><td>Không tìm thấy API tương ứng trong <code>api_specification.md</code></td><td><strong>(Spec gap)</strong> Không rõ gọi API nào khi bấm xóa hoặc sửa số lượng. (Suy luận) Cần truyền ID sản phẩm và số lượng mới, tương tự <code>POST /api/cart</code>.</td></tr></tbody></table><h3>2. Equivalence Partitioning</h3><p><strong>Biến <code>Authorization</code> (Token):</strong></p><table><thead><tr><th>Lớp</th><th>Mô tả</th><th>Giá trị đại diện</th><th>Loại</th><th>Căn cứ từ spec</th></tr></thead><tbody><tr><td>Token hợp lệ</td><td>JWT token của người dùng đang đăng nhập</td><td>Bearer eyJhbG...</td><td>Valid</td><td>"Yêu cầu Header: <code>Authorization: Bearer &lt;token&gt;</code>"</td></tr><tr><td>Thiếu Token</td><td>Không gửi Header Authorization</td><td>(Rỗng)</td><td>Invalid</td><td>"Yêu cầu Header"</td></tr><tr><td>Sai format Token</td><td>Chuỗi ngẫu nhiên, không theo chuẩn JWT</td><td>Bearer abc123xyz</td><td>Invalid</td><td>(Suy luận) Hệ thống cần JWT đúng định dạng để giải mã</td></tr><tr><td>Token hết hạn</td><td>JWT token đã quá thời hạn sử dụng</td><td>Bearer (token cũ)</td><td>Invalid</td><td>(Suy luận) Bảo mật cơ bản của JWT</td></tr></tbody></table><p><strong>Biến <code>Tham số cập nhật/xóa</code> (Suy luận: <code>id</code>, <code>quantity</code>):</strong><br><em>(Lưu ý: Do spec thiếu mô tả API cho thao tác giỏ hàng, các lớp dưới đây được suy luận dựa trên logic chung)</em></p><table><thead><tr><th>Lớp</th><th>Mô tả</th><th>Giá trị đại diện</th><th>Loại</th><th>Căn cứ từ spec</th></tr></thead><tbody><tr><td>ID hợp lệ (có trong giỏ)</td><td>Sản phẩm đang nằm trong giỏ hàng</td><td>1</td><td>Valid</td><td>Tương tác "Xóa sản phẩm" hoặc chỉnh "+/-"</td></tr><tr><td>ID không có trong giỏ</td><td>Thao tác trên sản phẩm không thuộc giỏ</td><td>999</td><td>Invalid</td><td>(Suy luận) Không thể xóa/sửa sản phẩm không tồn tại trong giỏ</td></tr><tr><td>Quantity &gt; 0</td><td>Tăng/giảm số lượng về mức dương</td><td>2</td><td>Valid</td><td>Cập nhật số lượng</td></tr><tr><td>Quantity = 0</td><td>Giảm số lượng về 0</td><td>0</td><td>Invalid (hoặc Valid tùy logic)</td><td>(Spec gap) Không rõ giảm về 0 sẽ báo lỗi hay tự động xóa sản phẩm</td></tr><tr><td>Quantity âm</td><td>Cố tình gửi số lượng âm qua API</td><td>-1</td><td>Invalid</td><td>"Số lượng chỉ nhận số nguyên dương" (từ FR-06)</td></tr></tbody></table> |Incomplete | Business spec (FR-07) yêu cầu "Số lượng (có nút +/- để chỉnh)", nhưng qua kiểm tra thực tế trên UI, chức năng này **không tồn tại** — giỏ hàng chỉ có nút Xóa sản phẩm, không có cách nào chỉnh số lượng trực tiếp trong trang giỏ hàng. Đây được ghi nhận là **BUG-07-01 (Missing Feature)**, không phải spec gap — vì spec có mô tả rõ ràng nhưng hệ thống không triển khai | Sửa lại phần Input Variables như trong `main_report.md` hiện tại |
| **Artifact #4** | | | | |
| **Artifact #5** | | | | |
| **Artifact #6** | | | | |
| **Artifact #7** | | | | |
| **Artifact #8** | | | | |
| **Artifact #9** | | | | |
| **Artifact #10** | | | | |

---

## 4. Tổng kết Độ chính xác AI
*Tổng hợp verdict từ Mục 3 và điền vào bảng dưới.*

| Chỉ số | Số lượng | Tỉ lệ |
| :--- | :---: | :---: |
| **Tổng artifact AI sinh đã audit** | | 100\% |
| **VALID** *(đúng, dùng nguyên)* | | % |
| **INVALID** *(sai; loại bỏ)* | | % |
| **INCOMPLETE** *(chấp nhận sau khi sửa)* | | % |

---

## 5. Kết luận — Khi nào nên / không nên dùng AI?
*Viết 80–150 chữ mô tả pattern quan sát được. AI mạnh ở đâu? AI sai ở đâu? Khuyến nghị của bạn cho việc dùng AI trong loại công việc này?*

................................................................................................................................................................................................
................................................................................................................................................................................................
................................................................................................................................................................................................

---

## 6. Mandatory Disclosure (dán nguyên văn)

> "[Test case / script / dataset / báo cáo] này được sinh phiên bản đầu bởi [tên công cụ AI]; tôi đã rà soát và chỉnh sửa [phần X], bổ sung [edge case Y, Z]; [phần W] do tôi tự viết. AI Audit Report chi tiết đính kèm ở Phụ lục A. Tôi cam đoan không dùng AI để sinh bất kỳ artifact nào thuộc danh mục bị cấm."

**Chữ ký:**

| | |
| :--- | :--- |
| **Họ tên sinh viên (in hoa):** |HỒ GIA HUY |
| **MSSV:** |23127376 |
| **Lớp / Khoá:** |23KTPM2 / 23CLC |
| **Môn học:** | CS423 / CSC13003 – Kiểm chứng Phần mềm |
| **Giảng viên:** |LÂM QUANG VŨ |
| **Ngày:** | |
| **Chữ ký:** | |

---

## Tham khảo
* Kharbach, M. (2026). AI Use Policy Templates for Higher Education. CC BY-NC-SA 4.0.
* ISTQB Foundation Level Syllabus (latest version).
* Hardman, P. (2025). A Post-AI Learning Taxonomy.
* Fuster Rabella, M. (2025). OECD Education Working Paper No. 338.
* Perkins, M., Roe, J., & Furze, L. (2025). AI Assessment Scale.
* Anthropic (2025). Building reliable AI test agents — engineering blog.
* DeepEval & Promptfoo documentation — testing frameworks for LLM systems.