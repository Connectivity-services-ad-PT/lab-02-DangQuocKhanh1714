# Biên bản đàm phán hợp đồng API

- Cặp đàm phán: #3 Core Business ↔ Access Gate
- Product: B
- Provider: Core Business (B6)
- Consumer: Access Gate (A3/B3)
- Phiên: v1.0
- Ngày: 2026-05-20

---

## Issue #1

- Raised by: Consumer (dự kiến)
- Endpoint: POST /access/check
- Concern: Thời gian response timeout tối đa?
- Proposal: Core Business cam kết response trong 500ms
- Resolution: Chờ đàm phán
- Rationale: (sẽ điền sau)
- Impact: (sẽ điền sau)

---

## Issue #2

- Raised by: Consumer (dự kiến)
- Endpoint: POST /access/check
- Concern: Khi Core lỗi, Gate nên fail-open hay fail-closed?
- Proposal: Fail-closed (từ chối mở cổng)
- Resolution: Chờ đàm phán
- Rationale: (sẽ điền sau)
- Impact: (sẽ điền sau)

---

## Issue #3

- Raised by: Provider
- Endpoint: POST /access/check
- Concern: Cần idempotencyKey để tránh xử lý trùng?
- Proposal: Consumer gửi idempotencyKey cho mỗi lần quẹt
- Resolution: Chờ đàm phán
- Rationale: (sẽ điền sau)
- Impact: (sẽ điền sau)

---

## Issue #4

- Raised by: Consumer (dự kiến)
- Endpoint: GET /policies/access/{policyId}
- Concern: Policy có cache được không?
- Proposal: Hỗ trợ ETag hoặc lastModified, TTL = 60s
- Resolution: Chờ đàm phán
- Rationale: (sẽ điền sau)
- Impact: (sẽ điền sau)

---

## Issue #5

- Raised by: Provider
- Endpoint: POST /access/check
- Concern: reasonCode nên enum hay free text?
- Proposal: Dùng enum cố định do Core định nghĩa
- Resolution: Chờ đàm phán
- Rationale: (sẽ điền sau)
- Impact: (sẽ điền sau)

---

## Issue #6

- Raised by: Consumer (dự kiến)
- Endpoint: GET /decisions/{decisionId}
- Concern: Có cần trả về requestPayload không?
- Proposal: Chỉ trả khi có traceId (debug mode)
- Resolution: Chờ đàm phán
- Rationale: (sẽ điền sau)
- Impact: (sẽ điền sau)

---

# Chốt hợp đồng v1.0

Provider sign-off: (sẽ ký sau)  
Consumer sign-off: (sẽ ký sau)  
Witness (GV/TA): (sẽ ký sau)  
Date: (sẽ điền sau)

---

## Ghi chú warning nếu Spectral còn cảnh báo

| Warning | Lý do chấp nhận tạm thời | Kế hoạch sửa |
|---|---|---|
|  |  |  |