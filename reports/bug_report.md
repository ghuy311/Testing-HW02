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
