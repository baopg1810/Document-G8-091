# Weekly Report - HR Helpdesk AI

Ngày lập báo cáo: 13/06/2026

## 1. Tổng quan

Trong tuần này, dự án HR Helpdesk AI đã được triển khai từ phần scaffold ban đầu đến một bản demo chạy được end-to-end. Hệ thống hiện có backend FastAPI, luồng chat có xác thực, RAG-lite kèm citations, RBAC/guardrails, tra cứu chỉ số HR cá nhân, escalation ticket, trending pin, feedback, frontend tĩnh và bộ kiểm thử hợp đồng/e2e.

Mục tiêu chính đã hoàn thành: biến ý tưởng "trợ lý HR nội bộ" thành một MVP có thể demo tại `http://127.0.0.1:8000/app`, với API contract rõ ràng và báo cáo kiểm thử tại `eval/results/report.md`.
### 1.1. Tài liệu đã cập nhật

- [`flow/03-prd.md`](flow/03-prd.md): PRD và success metrics.
- [`flow/05-contract.md`](flow/05-contract.md): API contract.
- [`cards/C-001`](cards/C-001-scaffold-contract-baseline.md) đến [`cards/C-010`](cards/C-010-e2e-verify-demo.md): trạng thái và evidence theo từng card.

### 1.2. `flow` và `cards`

- ** `flow/`**: Chứa tài liệu quy trình thiết kế và đặc tả kỹ thuật hệ thống theo trình tự phát triển dự án, bao gồm ý tưởng sơ khởi (`00-idea.md`), nghiên cứu công nghệ (`01-research.md`), định hình phạm vi (`02-scope.md`), tài liệu yêu cầu sản phẩm (`03-prd.md`), quyết định kiến trúc (`04-adr.md`), và thiết kế API contract (`05-contract.md`).
- ** `cards/`**: Chứa các thẻ công việc (Task/Kanban Cards từ `C-001` đến `C-010`) chia nhỏ tiến trình triển khai. Mỗi thẻ định rõ phạm vi công việc (Scope), danh sách file được phép chỉnh sửa (Allowed files), tiêu chí xác minh (Verify) và bằng chứng hoàn thành thực tế (Evidence).

## 2. Các module đã triển khai

| Module | Card | Mô tả | File chính |
|--------|------|-------|-----------|
| API scaffold & contract baseline | C-001 | Khởi tạo FastAPI app, health check, OpenAPI/docs, cấu trúc route baseline. | `src/main.py`, `src/api/routes.py`, `flow/05-contract.md` |
| Authentication & demo users | C-002 | Đăng nhập JWT demo, user context, endpoint `/me`, chat yêu cầu token. | `src/services/auth.py`, `src/services/demo_users.py`, `src/api/routes.py` |
| Document ingestion & RAG-lite | C-003 | HR admin upload tài liệu, chunking nội dung, embedding lexical, truy hồi citations. | `src/services/documents.py`, `src/services/retrieval.py`, `src/services/llm.py` |
| RBAC & guardrails | C-004 | Lọc tài liệu theo role/department, chặn jailbreak/out-of-scope/sensitive prompt, không trả dữ liệu ngoài quyền. | `src/services/guardrails.py`, `src/services/retrieval.py` |
| HR metrics lookup | C-005 | Function-calling style lookup cho số ngày phép, trạng thái bảo hiểm, xét duyệt khen thưởng của chính user. | `src/services/hris.py`, `src/api/routes.py` |
| Escalation tickets | C-005 | Tạo ticket tự động cho tình huống nhạy cảm/no-source/out-of-scope và cho phép HR admin cập nhật ticket. | `src/services/tickets.py`, `src/api/routes.py` |
| Trending pins | C-006 | Ghi log query, gom nhóm topic, tạo pinned notice khi vượt threshold 5 câu hỏi tương tự. | `src/services/trending.py` |
| Feedback | C-006 | Người dùng đánh giá câu trả lời helpful/not helpful theo `message_id`. | `src/services/feedback.py`, `src/api/routes.py` |
| Contract/API tests | C-007 | Kiểm thử shape của API, auth, documents, RBAC, tickets, metrics, trending, feedback. | `tests/test_api/**`, `tests/test_agents/**` |
| UI mock | C-008 | Mockup HTML mô tả trải nghiệm chat nhân viên và dashboard HR admin. | `mockups/hr-helpdesk.html` |
| Static frontend app | C-009 | Frontend tĩnh được serve tại `/app`, gọi API thật để login/chat/upload/run trend/ticket/feedback. | `src/static/hr-helpdesk.html`, `src/main.py` |
| End-to-end verification | C-010 | Script/test chạy demo live, đo metric PRD, ghi báo cáo kết quả. | `scripts/e2e_demo.py`, `tests/e2e/test_demo_flow.py`, `eval/results/report.md` |

## 3. Chi tiết backend

### 3.1 FastAPI application

- App chính nằm ở `src/main.py`.
- API prefix: `/api/v1`.
- Static frontend được mount qua `/static` và route `/app`.
- Health endpoint: `GET /health`.
- OpenAPI docs: `GET /docs`, `GET /openapi.json`.

### 3.2 Authentication

- Endpoint đăng nhập: `POST /api/v1/auth/login`.
- Demo users:
  - `employee@example.com` / `employee123`
  - `admin@example.com` / `admin123`
- Token JWT được dùng để xác định role và user hiện tại.
- Endpoint `/api/v1/me` trả thông tin user đang đăng nhập.

### 3.3 Chat orchestration

Endpoint chính: `POST /api/v1/chat`.

Luồng xử lý:

1. Kiểm tra token và lấy current user.
2. Nếu câu hỏi là tra cứu chỉ số cá nhân, gọi HRIS mock.
3. Nếu guardrail phát hiện sensitive/out-of-scope/jailbreak, từ chối hoặc tạo escalation ticket.
4. Nếu là câu hỏi chính sách, truy hồi tài liệu phù hợp quyền truy cập.
5. Nếu có citation, trả lời kèm nguồn.
6. Nếu không có nguồn phù hợp nhưng là câu hỏi HR, tạo ticket `no_source`.
7. Ghi query vào trending log.

## 4. Chi tiết AI/RAG

### 4.1 Document ingestion

- Endpoint: `POST /api/v1/documents`.
- Chỉ HR admin được upload tài liệu.
- Tài liệu được chia chunk và lưu trong memory store.
- Mỗi chunk lưu:
  - document id
  - title
  - section
  - excerpt
  - visibility roles
  - department ids
  - embedding lexical

### 4.2 Retrieval

- Retrieval nằm ở `src/services/retrieval.py`.
- Dùng gemini embedding cho MVP.
- Có `MIN_SCORE` và `TOP_K` để kiểm soát citations.
- RBAC được áp dụng trước khi trả citation, tránh để model thấy tài liệu ngoài quyền.

### 4.3 Citation response

- Citation gồm document title, section, excerpt, score.
- Khi có nguồn phù hợp, assistant trả lời theo contract `ChatResponse`.
- Khi không có nguồn, hệ thống không bịa câu trả lời mà chuyển sang refusal/escalation.

## 5. Security & RBAC

Đã triển khai các lớp bảo vệ chính:

- JWT auth cho endpoint cần đăng nhập.
- HR admin role mới được upload/list documents, run trending, list/update tickets.
- Employee chỉ xem được tài liệu visibility cho `employee`.
- Department admin chỉ xem được tài liệu đúng department.
- Guardrails chặn:
  - jailbreak prompt
  - câu hỏi ngoài phạm vi HR
  - yêu cầu dữ liệu nhạy cảm của người khác
- Sensitive request tạo ticket thay vì trả lời trực tiếp.

## 6. HR metrics & escalation

### 6.1 HR metrics

Module `src/services/hris.py` cung cấp mock HRIS data:

- Số ngày phép còn lại.
- Trạng thái bảo hiểm.
- Trạng thái xét duyệt khen thưởng.

Kết quả e2e xác nhận 5/5 lượt lookup trả đúng dữ liệu của `emp-001`, không cho truyền arbitrary `employee_id`.

### 6.2 Escalation

Module `src/services/tickets.py` hỗ trợ:

- Tạo ticket từ chat.
- Tạo ticket thủ công qua `/api/v1/escalations`.
- HR admin list ticket.
- HR admin update status, assignee.


## 7. Trending & feedback

### 7.1 Trending

Module `src/services/trending.py`:

- Ghi lại query sau mỗi lượt chat.
- Xác định topic key như `nghi-phep`, `bao-hiem`, `luong`, `khen-thuong`.
- HR admin chạy `/api/v1/admin/trending/run`.
- Khi số lượng câu hỏi tương tự đạt threshold, hệ thống tạo trend pin.


### 7.2 Feedback

Module `src/services/feedback.py`:

- Nhận feedback theo `message_id`.
- Kiểm tra message tồn tại trước khi ghi nhận.
- Hỗ trợ rating `up` hoặc `down`.

## 8. Frontend

Frontend tĩnh nằm tại `src/static/hr-helpdesk.html` và được serve tại `/app`.

Các chức năng frontend đã có:

- Login nhanh bằng employee/admin demo account.
- Chat với API thật.
- Hiển thị citations.
- Hiển thị action/escalation khi có.
- Upload tài liệu HR.
- Run trending.
- Xem ticket admin.
- Gửi feedback.

Ngoài ra có mockup UI ở `mockups/hr-helpdesk.html` dùng cho card C-008.

## 9. Testing & verification

### 9.1 Test coverage theo module

- Auth: `tests/test_api/test_auth.py`
- Chat contract: `tests/test_api/test_chat_slice.py`
- Documents: `tests/test_api/test_documents.py`
- RAG: `tests/test_agents/test_rag.py`
- Guardrails/RBAC: `tests/test_agents/test_guardrails.py`, `tests/test_api/test_rbac.py`
- HR metrics: `tests/test_api/test_hr_metrics.py`, `tests/test_agents/test_function_calling.py`
- Tickets: `tests/test_api/test_tickets.py`
- Trending: `tests/test_api/test_trending.py`
- Feedback: `tests/test_api/test_feedback.py`
- Static app: `tests/test_api/test_static_app.py`
- Contract tests: `tests/test_api/test_contract.py`
- E2E demo: `tests/e2e/test_demo_flow.py`

### 9.2 Kết quả kiểm thử gần nhất

```powershell
.\.venv\Scripts\python.exe -m pytest tests -q
```

Kết quả:

```text
49 passed in 1.18s
```

### 9.3 E2E success metrics

Nguồn: `eval/results/report.md`

| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Seeded policy questions | 30 | 30 | PASS |
| Cited policy answers | >= 90% | 30/30 (100.0%) | PASS |
| P95 chat latency | < 10s | 0.003s | PASS |
| Safe HR metric lookups | 5 | 5 | PASS |
| Escalation tickets | 3 | 3 | PASS |
| Trend pin after similar queries | 1 | Nghi phep | PASS |


## 10. Trạng thái hiện tại

Hệ thống đã đạt trạng thái MVP demo-ready:

- Backend chạy được local.
- Frontend mở được tại `/app`.
- API contract có kiểm thử.
- RAG-lite có citations.
- RBAC/guardrails có kiểm thử.
- HR metric lookup hoạt động.
- Escalation ticket hoạt động.
- Trending pin hoạt động.
- Feedback hoạt động.
- E2E verification đạt toàn bộ metric PRD.


