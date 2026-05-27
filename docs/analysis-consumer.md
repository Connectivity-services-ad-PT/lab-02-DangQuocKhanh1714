# Phân tích yêu cầu — vai Consumer

- Cặp đàm phán: #3 Core Business + Access Gate
- Product: A / B
- Provider service: Core Business (B6)
- Consumer service: Access Gate (B3)
- Người viết: B6
- Ngày: 20-05-2026

---

## 1. Resource Consumer cần nhận/gửi

| Resource | Mô tả | Thuộc tính bắt buộc | Thuộc tính tùy chọn |
|---|---|---|---|
| AccessCheck | Kết quả kiểm tra quyền ra/vào của thẻ tại cổng | decision, reasonCode, policyId, evaluatedAt | expiresAt, traceId, additionalData |
| Policy | Thông tin chi tiết về policy kiểm soát truy cập | policyId, name, effect (ALLOW/DENY), priority | description, timeRange, condition |
| DecisionHistory | Lưu lại quyết định đã kiểm tra | decisionId, policyId, subjectId, decision, timestamp | gateId, requestPayload |

---

## 2. API Consumer cần gọi

| Method | Path | Mục đích | Consumer gọi khi nào? |
|---|---|---|---|
| POST | `/access/check` | Kiểm tra realtime policy trước khi mở cổng | Mỗi lần có thẻ quẹt tại gate |
| GET | `/policies/access/{policyId}` | Lấy chi tiết policy (cấu hình rule) | Khi gate cần cache hoặc debug |
| GET | `/decisions/{decisionId}` | Tra cứu lại quyết định đã xử lý | Khi audit hoặc xử lý bất đồng bộ |
| GET | `/health` | Kiểm tra sức khỏe service | Định kỳ hoặc startup |

---

## 3. Error case Consumer cần xử lý

Tối thiểu 5 case.

| Status | Tình huống | Response body dự kiến |
|---:|---|---|
| 400 | Payload thiếu cardId hoặc gateId | `Problem` với detail |
| 401 | Thiếu Bearer token | `Problem` |
| 403 | Token hợp lệ nhưng service không được phép gọi API | `Problem` |
| 404 | PolicyId không tồn tại | `Problem` |
| 409 | Request trùng lặp (retry trong thời gian ngắn) | `Problem` |
| 422 | cardId đúng format nhưng thẻ đã bị khóa/hết hạn | `Problem` |

---

## 4. Giả định bổ sung

- Giả định 1: Access Gate có token riêng để gọi Core Business.
- Giả định 2: Thời gian timeout tối đa cho `/access/check` là 500ms.
- Giả định 3: Core Business không lưu ảnh hoặc face embedding, chỉ lưu decision log.
- Giả định 4: Nếu policy không match, mặc định decision = DENY với reasonCode = `NO_MATCHING_POLICY`.

---

## 5. Câu hỏi cho Provider

1. Access Gate cần `expiresAt` cho decision không (có thể cache decision trong bao lâu)?
2. Khi Core Business lỗi, Gate muốn fail-open (mở cổng) hay fail-closed (đóng cổng)?
3. Mỗi lần quẹt có cần gửi `idempotencyKey` để tránh xử lý trùng không?

---

## 6. Rủi ro tích hợp

| Rủi ro | Tác động | Đề xuất xử lý |
|---|---|---|
| Provider đổi kiểu dữ liệu | Consumer parse lỗi | Chốt type/format/pattern |
| Provider thiếu mã lỗi | Consumer khó xử lý lỗi | Chuẩn hóa Problem Details |
