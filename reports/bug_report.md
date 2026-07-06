# Bug Report — FR-06: Xem chi tiết sản phẩm

| Bug ID | Mô tả ngắn gọn | Test Case liên quan  | GitHub Issue |
|---|---|---|---|
| BUG-06-01 | `GET /api/products/:id` trả `200 OK` + `{}` cho mọi ID không hợp lệ (không tồn tại, 0, sai kiểu, vượt INT32_MAX) | TC-FR06-02, 03, 04, 15, 16 | [1](<https://github.com/ghuy311/Testing-HW02/issues/1>) |
| BUG-06-02 | `POST /api/cart` chấp nhận mọi `quantity` không hợp lệ (0, âm, thập phân, sai kiểu, null, tràn số) mà vẫn trả "added to cart" | TC-FR06-06, 07, 08, 09, 10, 13 | [2](<https://github.com/ghuy311/Testing-HW02/issues/2>) |
| BUG-06-03 | UI Bug: "Thêm vào giỏ hàng" chỉ cập nhật số lượng sau lần click thứ 2 | TC_06_FUNC_01 | [3](<https://github.com/ghuy311/Testing-HW02/issues/4>) |

## FR-07: Giỏ hàng (Shopping Cart)

| Bug ID | Mô tả ngắn gọn | Test Case liên quan | GitHub Issue |
|---|---|---|---|
| BUG-07-01 | Thiếu chức năng nút +/- để chỉnh số lượng (spec có yêu cầu nhưng không triển khai) | Domain Testing/EP — Input Variables | [5](https://github.com/ghuy311/Testing-HW02/issues/5) |
| BUG-07-02 | Giỏ hàng (thêm + xóa) hoàn toàn không gọi API, chỉ tồn tại ở React state — mất dữ liệu khi F5 | TC_07_FUNC_01, 02, 03 | [6](<https://github.com/ghuy311/Testing-HW02/issues/6>) |
| BUG-07-03 | `GET /api/cart` trả `403` thay vì `401` khi token sai format | TC-FR07-03 | [7](<https://github.com/ghuy311/Testing-HW02/issues/7>) |
| BUG-07-04 | Thêm trùng sản phẩm tạo 2 dòng thay vì cộng dồn số lượng | TC_07_FUNC_04 | [8](<https://github.com/ghuy311/Testing-HW02/issues/8>) |
| BUG-07-05 | Xóa sản phẩm không có dialog xác nhận | TC_07_FUNC_05 | [9](<https://github.com/ghuy311/Testing-HW02/issues/9>) |
| BUG-07-06 | Tổng tiền/Thành tiền bị âm/NaN do `quantity` không hợp lệ từ FR-06 lan sang | Cross-feature (FR-06 → FR-07) | [10](<https://github.com/ghuy311/Testing-HW02/issues/10>) |
| BUG-07-07 | Nhãn "Tổng cộng" không xuất hiện trong giao diện giỏ hàng | TC_07_FUNC_07 | [11](<https://github.com/ghuy311/Testing-HW02/issues/11>) |


## FR-14: Quản lý Danh mục (Category CRUD)

| Bug ID | Mô tả ngắn gọn | Test Case liên quan | GitHub Issue |
|---|---|---|---|
| BUG-14-01 | Tên category quá dài sẽ khiến UI bị vỡ dẫn đến không tìm thấy được nút xoá category | TC_14_FUNC_04 | [12](<https://github.com/ghuy311/Testing-HW02/issues/12>) |
| BUG-14-02 | Có thể tạo danh mục với tên rỗng hoặc chỉ chứa khoảng trắng, dù spec yêu cầu bắt buộc | TC-FR14-06, TC-FR14-07, TC-FR14-08, TC_14_FUNC_01, TC_14_FUNC_02 | [13](<https://github.com/ghuy311/Testing-HW02/issues/13>) |
| BUG-14-03 | Tài khoản role "user" vẫn Thêm/Sửa/Xóa được danh mục — Broken Access Control | TC-FR14-01, TC-FR14-10, TC-FR14-15 | [14](<https://github.com/ghuy311/Testing-HW02/issues/14>) |
| ISSUE-14-04 | Mã lỗi không nhất quán: token thiếu → 401, token sai định dạng → 403 (không phải vi phạm spec tường minh) | TC-FR14-03, TC-FR14-12, TC-FR14-17 | [15](<https://github.com/ghuy311/Testing-HW02/issues/15>) |
| BUG-14-05 | PUT/DELETE trên ID không tồn tại/không hợp lệ vẫn báo phản hồi "thành công" | TC-FR14-14, TC-FR14-19, TC-FR14-20 | [16](<https://github.com/ghuy311/Testing-HW02/issues/16>) |
