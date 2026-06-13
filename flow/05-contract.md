# Giai đoạn 05 - Hợp đồng Giao diện (Interface Contract - Điểm kết nối)

Hợp đồng giao diện là lớp trung gian kết nối phần lõi hệ thống và bên tiêu thụ dịch vụ. Đối với ứng dụng web này, đó chính là HTTP API. Được viết TRƯỚC KHI lập trình. Các tác vụ Backend xây dựng DỰA TRÊN bảng này; các tác vụ UI sử dụng dữ liệu TỪ bảng này.

## Gate - Kiểm tra TẤT CẢ trước khi chạy `/flow next`
- [x] Mỗi tính năng trong PRD đều ánh xạ đến ít nhất một endpoint bên dưới
- [x] Mỗi endpoint đều được mô tả rõ cấu trúc dữ liệu gửi lên (Request) và phản hồi về (Response)
- [x] Cột xác thực (Auth) được điền đầy đủ cho tất cả các endpoint (public / token / admin)
- [x] Không còn trường trống (FILL placeholder) nào trong file này

## Quy tắc OpenAPI / Swagger

FastAPI tự động cung cấp tài liệu `/docs` và `/openapi.json`. Bảng này là nguồn chân lý để lập kế hoạch; tài liệu kỹ thuật được cung cấp khi chạy ứng dụng chính là sản phẩm thực tế của hợp đồng này.

## Các Endpoint

| Phương thức | Đường dẫn (Path) | Xác thực | Cấu trúc Request | Cấu trúc Response |
|---|---|---|---|---|
| GET | `/health` | public | không có | `{status: "ok", env: string}` |
| POST | `/api/v1/auth/login` | public | `{email: string, password: string}` | `{access_token: string, token_type: "bearer", user: User}` |
| GET | `/api/v1/me` | token | không có | `User` |
| POST | `/api/v1/chat` | token | `ChatRequest` | `ChatResponse` |
| POST | `/api/v1/documents` | admin | multipart `{file, title: string, visibility_roles: string[], department_ids?: string[]}` | `DocumentIngestResult` |
| GET | `/api/v1/documents` | admin | query `{status?: indexed|failed|pending}` | `{documents: Document[]}` |
| GET | `/api/v1/me/hr-metrics` | token | không có | `PersonalHrMetrics` |
| POST | `/api/v1/escalations` | token | `EscalationCreate` | `Ticket` |
| GET | `/api/v1/admin/tickets` | admin | query `{status?: open|in_progress|resolved, priority?: low|normal|high}` | `{tickets: Ticket[]}` |
| PATCH | `/api/v1/admin/tickets/{ticket_id}` | admin | `{status?: open|in_progress|resolved, assignee_id?: string, internal_note?: string}` | `Ticket` |
| GET | `/api/v1/trending/pins` | token | không có | `{pins: TrendPin[]}` |
| POST | `/api/v1/admin/trending/run` | admin | `{window_minutes: int = 60, threshold: int = 5}` | `{created_pins: TrendPin[], skipped_topics: string[]}` |
| POST | `/api/v1/feedback` | token | `{message_id: string, rating: "up"|"down", comment?: string}` | `{ok: true}` |

## Các cấu trúc dữ liệu dùng chung (đối tượng được sử dụng bởi nhiều endpoint)

```text
User {
  id: string,
  email: string,
  full_name: string,
  role: "employee"|"department_admin"|"hr_admin",
  department_id: string|null
}

ChatRequest {
  message: string,
  session_id: string|null
}

ChatResponse {
  message_id: string,
  session_id: string,
  answer: string,
  citations: Citation[],
  actions: ChatAction[],
  escalated_ticket_id: string|null,
  refusal_reason: string|null
}

Citation {
  document_id: string,
  document_title: string,
  section: string|null,
  excerpt: string,
  page: int|null,
  score: float
}

ChatAction {
  type: "hr_metric_lookup"|"escalation_created"|"none",
  label: string,
  data: object|null
}

Document {
  id: string,
  title: string,
  status: "pending"|"indexed"|"failed",
  visibility_roles: string[],
  department_ids: string[],
  created_at: string,
  chunk_count: int
}

DocumentIngestResult {
  document: Document,
  indexed_chunk_count: int,
  warnings: string[]
}

PersonalHrMetrics {
  employee_id: string,
  leave_days_remaining: float,
  insurance_status: "active"|"pending"|"inactive",
  reward_review_status: "not_started"|"in_review"|"approved"|"rejected",
  as_of_date: string
}

EscalationCreate {
  session_id: string|null,
  message: string,
  reason: "no_source"|"outside_scope"|"sensitive"|"user_requested"|"low_confidence",
  priority: "low"|"normal"|"high"
}

Ticket {
  id: string,
  requester_id: string,
  status: "open"|"in_progress"|"resolved",
  priority: "low"|"normal"|"high",
  reason: string,
  summary: string,
  assignee_id: string|null,
  created_at: string,
  updated_at: string
}

TrendPin {
  id: string,
  title: string,
  summary: string,
  source_query_count: int,
  citations: Citation[],
  created_at: string,
  expires_at: string|null
}
```

## Bản đồ ánh xạ Tính năng -> Endpoint

- Hỏi đáp chính sách kèm trích dẫn nguồn -> `POST /api/v1/chat`
- Phân quyền dựa trên vai trò (RBAC) và rào cản bảo vệ -> tất cả các endpoint token/admin, đặc biệt là `POST /api/v1/chat` và `POST /api/v1/documents`
- Tra cứu chỉ số nhân sự cá nhân -> `GET /api/v1/me/hr-metrics`, hiển thị thông qua `POST /api/v1/chat`
- Vé chuyển tiếp hỗ trợ (Escalation tickets) -> `POST /api/v1/escalations`, `GET /api/v1/admin/tickets`, `PATCH /api/v1/admin/tickets/{ticket_id}`
- Tóm tắt xu hướng và thông báo ghim -> `GET /api/v1/trending/pins`, `POST /api/v1/admin/trending/run`
- Quản trị viên tải tài liệu/đánh chỉ mục lại -> `POST /api/v1/documents`, `GET /api/v1/documents`
- Đánh giá câu trả lời -> `POST /api/v1/feedback`
