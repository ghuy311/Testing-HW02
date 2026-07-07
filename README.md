# HW02 – Domain Testing on EShop

**Họ tên:** Hồ Gia Huy
**MSSV:** 23127376
**AI Policy:** Open — có sử dụng AI (Gemini trong Antigravity IDE, Gemini trên website). Chi tiết xem `ai_audit_report.md`.

---

## 1. Bảng Tự đánh giá (Self-Assessment)

| No. | Criteria | Grade | Self-Assessed Grade |
|---|---|---|---|
| 1 | Feature A — FR-06 (Domain + Boundary) | 25 | 25 |
| 2 | Feature B — FR-07 (Domain + Boundary) | 25 | 25 |
| 3 | Feature C — FR-14 (Domain + Boundary) | 25 | 25 |
| 4 | Feature D — FR-06 Mobile (Domain + Boundary) | 15 | 15 |
| 5 | Agent Skills | 10 | 10 |
| | **Total** | **100** | **100** |

---

## 2. Test Summary Report

### 2.1 Tổng quan

- **Số lượng feature đã kiểm thử:** 4 (FR-06 Web, FR-07, FR-14, FR-06 Mobile)
- **Tổng số Test Case đã thiết kế:** 66
- **Tổng số Test Case đã thực thi:** 65
- **Số Test Case chưa thực thi:** 1
- **Số Test Case Passed:** 23
- **Số Test Case Failed:** 40
- **Số Test Case ghi nhận đặc biệt (spec gap, không đủ căn cứ chấm Pass/Fail):** 2
- **Tổng số Bug phát hiện:** 18

### 2.2 Chi tiết theo từng Feature

| Feature | Test Case thiết kế | Đã thực thi | Chưa thực thi | Passed | Failed | Ghi nhận (spec gap) | Số Bug |
|---|---|---|---|---|---|---|---|
| FR-06 (Web) — Xem chi tiết sản phẩm | 19 | 19 | 0 | 7 | 12 | 0 | 3 |
| FR-07 — Giỏ hàng | 11 | 10 | 1 | 3 | 7 | 0 | 7 |
| FR-14 — Quản lý Danh mục (Admin) | 24 | 24 | 0 | 10 | 12 | 2 | 5 |
| FR-06 (Mobile) — Xem chi tiết sản phẩm | 12 | 12 | 0 | 3 | 9 | 0 | 3 |
| **Tổng** | **66** | **65** | **1** | **23** | **40** | **2** | **18** |

> Ghi chú: Test case "chưa thực thi" là TC-FR07-04 (Token hết hạn) — không thể thực thi trong thời gian làm bài vì cần chờ JWT token hết hạn tự nhiên.

### 2.3 Danh sách Bug theo mức độ nghiêm trọng

| Mức độ | Số lượng | Ví dụ tiêu biểu |
|---|---|---|
| Critical | 2 | Giỏ hàng (Web + Mobile) không đồng bộ Backend, chỉ tồn tại ở Frontend state |
| High | 4 | `POST /api/cart` không validate `quantity`; Broken Access Control (FR-14); thiếu UI cho +/- |
| Medium | 8 | Thêm trùng sản phẩm không cộng dồn; thiếu dialog xác nhận xóa; UI vỡ do tên dài |
| Low | 4 | Sai mã HTTP status (403 thay vì 401); nhãn UI không nhất quán |

Chi tiết đầy đủ từng bug: xem `bug_report.md`. Toàn bộ bug đã được tạo GitHub Issues kèm mô tả tái hiện.

### 2.4 Demo Videos

| Feature | Link Video (YouTube) |
|---|---|
| [Feature đã chọn để demo end-to-end với Agent Skill] | [Điền link YouTube] |

> Video minh họa toàn bộ quy trình: gọi Agent Skill → Equivalence Partitioning → Boundary Value Analysis → thực thi test 

---

## 3. Cấu trúc thư mục nộp bài

```
HW/
├── docs/
│   ├── eshop-spec.md
│   └── api_specification.md
├── reports/
│   ├── ai_audit_report.md
│   ├── ai_critique.md
│   ├── bug_report.md
│   └── main_report.md
├── skills/
│   └── eshop-domain-testing-skill.md
├── git-log.txt
└── README.md
```

## 4. Công cụ AI đã sử dụng

- **Gemini** (trong Antigravity IDE) — Agent chính thực hiện Domain Testing/BVA theo Agent Skill đã xây dựng.

Chi tiết từng phiên tương tác: xem `reports/ai_audit_report.md`.