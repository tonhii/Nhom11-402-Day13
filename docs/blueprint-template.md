# Day 13 Observability Lab Report

> **Instruction**: Fill in all sections below. This report is designed to be parsed by an automated grading assistant. Ensure all tags (e.g., `[GROUP_NAME]`) are preserved.

## 1. Team Metadata
- [GROUP_NAME]: Nhom11-402-Day13
- [REPO_URL]: https://github.com/tonhii/Nhom11-402-Day13
- [MEMBERS]:
  - Member A: Hoàng Văn Kiên | Role: Logging & PII
  - Member B: Lê Hoàng Long | Role: Tracing & Enrichment
  - Member C: Hà Hữu An | Role: SLO & Alerts
  - Member D: Đỗ Văn Quyết | Role: Load Test & Incident Injection
  - Member E: Lê Thị Phương | Role: Dashboard & Evidence
  - Member F: Hồ Thị Tố Nhi | Role: Blueprint & Demo Lead

---

## 2. Group Performance (Auto-Verified)
- [VALIDATE_LOGS_FINAL_SCORE]: 100/100
- [TOTAL_TRACES_COUNT]: >10
- [PII_LEAKS_FOUND]: 0

---

## 3. Technical Evidence (Group)

### 3.1 Logging & Tracing
- [EVIDENCE_CORRELATION_ID_SCREENSHOT]: ![Correlation ID](/docs/images/A_logs.png)
- [EVIDENCE_PII_REDACTION_SCREENSHOT]: Log mẫu đã che PII `[REDACTED_...]` - ![Validation](/docs/images/A_validation_score.png)
- [EVIDENCE_TRACE_WATERFALL_SCREENSHOT]: ![Trace Waterfall](/docs/images/B_trace_waterfall.png)
- [TRACE_WATERFALL_EXPLANATION]: Căn cứ theo Trace ID tiêu biểu `e4a39177-411c-4806-8474-36669d124580`. Trace thể hiện chuỗi các quá trình (waterfall) từ lúc API nhận request đến xử lý workflow, trong đó có thể thấy span gọi mô hình LLM là nguyên nhân chính chiếm tỷ trọng độ trễ (latency) lớn nhất.

### 3.2 Dashboard & SLOs
- [DASHBOARD_6_PANELS_SCREENSHOT]: ![Dashboard 6 Panels](/docs/images/E_dashboard_all.png)

- [SLO_TABLE]:
| SLI | Target | Window | Current Value |
|---|---:|---|---:|
| Latency P95 | < 2500ms | 28d | 151ms |
| Error Rate | < 1% | 28d | 0% |
| Cost Budget | < $2.0/day | 1d | $0.0399 |

### 3.3 Alerts & Runbook
- [ALERT_RULES_SCREENSHOT]: ![Alert Rules](/docs/images/C_alert_rules.png)
- [SAMPLE_RUNBOOK_LINK]: [docs/alerts.md](/docs/alerts.md)

---

## 4. Incident Response (Group)
- [SCENARIO_NAME]: tool_fail (Ghi nhận từ báo cáo của nhóm)
- [SYMPTOMS_OBSERVED]: Số lượng request trả về HTTP Status Code 500 tăng vọt, lên đến 20 failed requests. Các request lỗi có latency rất thấp (dưới 50ms).
- [ROOT_CAUSE_PROVED_BY]: Error log chi tiết phát hiện lỗi `error_type: RuntimeError` kèm thông tin `detail: "Vector store timeout"`. Lỗi này minh chứng RAG layer (Vector DB kết nối chậm, đứng) khiến công cụ tìm kiếm của LLM bị timeout và văng lỗi hệ thống.
- [FIX_ACTION]: Xác định và khởi động lại Vector DB, kiểm tra network security group, hoặc điều hướng read request sang các replica khác của database đang rảnh.
- [PREVENTIVE_MEASURE]: Cần thiết lập circuit breaker, thêm connection timeout hợp lý cho RAG DB và áp dụng các cơ chế fallback memory (sử dụng cache/trải nghiệm mặc định) thay vì thẳng thừng ném ra lỗi 500 với client.

---

## 5. Individual Contributions & Evidence

### [MEMBER_A: Hoàng Văn Kiên]
- [TASKS_COMPLETED]: Thực hiện format log JSON tiêu chuẩn với hash IDs và kiểm duyệt ẩn thông tin nhạy cảm định dạng cá nhân (PII Redaction). Chạy script check `validate_logs` đạt số điểm 100/100 tuyệt đối.
- [EVIDENCE_LINK]: (https://github.com/tonhii/Nhom11-402-Day13/commit/ce432426f2a69610399324ed63d01f3a32862a09)

### [MEMBER_B: Lê Hoàng Long]
- [TASKS_COMPLETED]: Cài đặt Langfuse callback, tích hợp Tracing trực tiếp vào quá trình inference của hệ thống. Kiểm tra thu nhận dữ liệu trace dạng Waterfall trên dashboard.
- [EVIDENCE_LINK]: (https://github.com/tonhii/Nhom11-402-Day13/commit/2d7751ed52816dbd344fc67b6ab30c429305fd67)

### Hà Hữu An
- [TASKS_COMPLETED]: Thiết lập SLO trong config/slo.yaml, cấu hình Alert Rules trong config/alert_rules.yaml, và hoàn thiện Runbook trong docs/alerts.md. Phân tích và xử lý sự cố.
- [EVIDENCE_LINK]: (https://github.com/tonhii/Nhom11-402-Day13/commit/93bf87704a5165b966584b8c70119398e9f83b59)

### Đỗ Văn Quyết
- [TASKS_COMPLETED]: Implemented and executed load testing and incident injection for the project:
  - Ran `scripts/load_test.py` (multiple runs, concurrency tests) to measure baseline and stressed traffic.
  - Triggered incidents with `scripts/inject_incident.py` for `rag_slow`, `tool_fail`, and `cost_spike` scenarios.
  - Collected and analyzed metrics (latency P95/P99, error breakdown, cost) and identified root cause for `tool_fail` (vector store timeout).
  - Documented findings and results in `docs/grading-evidence.md` and captured dashboard screenshot(s).
- [EVIDENCE_LINK]: (https://github.com/tonhii/Nhom11-402-Day13/commit/3e65ba2237f27b96171863846f4245011fb74911)

### [MEMBER_E: Lê Thị Phương]
- [TASKS_COMPLETED]: Xây dựng Dashboard 6 panels trên Langfuse bao phủ (Latency, Traffic, Errors, Cost, Tokens, Quality). Chạy giả lập load test để lấy dữ liệu metric thực tế.
- [EVIDENCE_LINK]: (https://github.com/tonhii/Nhom11-402-Day13/commit/3c03dbe92df7133d6f474e86bf227efc97cf4c22)

### Hồ Thị Tố Nhi
- [TASKS_COMPLETED]: Role: Blueprint & Demo Lead. Tổng hợp kết quả đánh giá (grading evidence) toàn diện từ các thành viên trong nhóm, biên soạn báo cáo Blueprint Template tổng quan, rà soát lại tiến trình merge nhánh (git pull/merge) và hoàn thiện các bước chuẩn bị kết thúc bài lab.
- [EVIDENCE_LINK]: (https://github.com/tonhii/Nhom11-402-Day13/commit/cd2598c513ebc5da41adf11c9dc6b13ff632a2b7)

---

## 6. Bonus Items (Optional)
- [BONUS_COST_OPTIMIZATION]: N/A
- [BONUS_AUDIT_LOGS]: N/A
- [BONUS_CUSTOM_METRIC]: N/A
