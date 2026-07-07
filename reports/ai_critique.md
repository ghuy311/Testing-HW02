## AI Critique (Tổng kết chung)

Qua 4 lần áp dụng AI (Gemini) cho Domain Testing/BVA trên EShop, em nhận thấy AI hỗ trợ rất tốt ở phần *thiết kế lý thuyết*: xác định biến đầu vào, chia phân hoạch tương đương, và đề xuất giá trị biên đều nhanh và khá đầy đủ, giúp tiết kiệm nhiều thời gian so với làm tay từ đầu.

Tuy nhiên, em phát hiện 2 loại sai sót lặp lại xuyên suốt cả 4 feature. Thứ nhất, AI có xu hướng **tự bịa hoặc trộn lẫn ràng buộc không có trong spec** với ràng buộc thật — ví dụ giả định tồn kho ở FR-06, hay mặc định mã HTTP 201/404 như thể đó là yêu cầu bắt buộc ở FR-14, trong khi thực chất chỉ là quy ước REST AI tự thêm vào. Nếu không đọc lại kỹ, rất dễ đánh giá nhầm hệ thống là lỗi trong khi nó vẫn đúng nghiệp vụ. Thứ hai, AI **không thể tự phát hiện các lỗi chỉ lộ ra khi chạy thật** — như giỏ hàng không gọi API (FR-06, FR-07, cả bản Mobile), lỗ hổng phân quyền RBAC (FR-14), hay UI thiếu tính năng so với spec (thiếu nút +/-, thiếu Update). Tất cả đều chỉ tìm ra được sau khi em tự dùng Postman/DevTools kiểm tra thực tế.

Một điểm đáng chú ý khác: AI xử lý mỗi feature độc lập, không tự liên kết bug giữa các feature với nhau (như lỗi `quantity` ở FR-06 làm tổng tiền FR-07 bị NaN), cũng không tự nhớ lại bug đã gặp ở feature trước để cảnh giác cho feature sau (bug giỏ hàng lặp lại y hệt ở bản Mobile).

Bài học lớn nhất em rút ra: AI là công cụ hỗ trợ thiết kế tốt, nhưng không thể thay thế việc tự tay thực thi và đối chiếu thực tế. Người làm luôn phải là người kiểm tra cuối cùng, không tin tưởng tuyệt đối vào bất kỳ output nào của AI.