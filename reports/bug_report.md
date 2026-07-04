# Bug Report — FR-06: Xem chi tiết sản phẩm

| Bug ID | Mô tả ngắn gọn | Test Case liên quan  | GitHub Issue |
|---|---|---|---|
| BUG-06-01 | `GET /api/products/:id` trả `200 OK` + `{}` cho mọi ID không hợp lệ (không tồn tại, 0, sai kiểu, vượt INT32_MAX) | TC-FR06-02, 03, 04, 15, 16 | [1](<https://github.com/ghuy311/Testing-HW02/issues/1>) |
| BUG-06-02 | `POST /api/cart` chấp nhận mọi `quantity` không hợp lệ (0, âm, thập phân, sai kiểu, null, tràn số) mà vẫn trả "added to cart" | TC-FR06-06, 07, 08, 09, 10, 13 | [2](<https://github.com/ghuy311/Testing-HW02/issues/2>) |
| BUG-06-03 | UI Bug: "Thêm vào giỏ hàng" chỉ cập nhật số lượng sau lần click thứ 2 | TC_06_FUNC_01 | [3](<https://github.com/ghuy311/Testing-HW02/issues/4>) |