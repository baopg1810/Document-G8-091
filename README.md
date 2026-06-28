# Báo cáo - HR Helpdesk AI

## 1. Deliverables

| Deliverable | Trạng thái | Link / Ghi chú |
| --- | --- | --- |
| Deployed production URL | Đã có | https://c2-app-091.up.railway.app/ |
| Evaluation metrics | Đã có | Baseline lấy từ `eval/results/benchmark_results.json`, chạy lúc `2026-06-27T11:45:29Z` |
| Guardrails | Đã có | RBAC, bộ lọc truy xuất, luật an toàn local, kiểm soát citation, xác nhận escalation |
| Demo video draft | Đã có| https://drive.google.com/drive/folders/1w24udOA4Fn3snNF3M-PWqW6Q_E4cRHtk?usp=sharing |
| Cost report | Đã có ước tính nháp | Ước tính chi phí AI biến đổi bên dưới |

## 2. Tóm tắt sản phẩm

HR Helpdesk AI là ứng dụng helpdesk nội bộ cho phòng nhân sự. Nhân viên có thể hỏi các câu hỏi về chính sách HR, nhận câu trả lời có trích dẫn từ tài liệu nội bộ, tra cứu chỉ số HR của chính mình một cách an toàn, và chuyển các tình huống nhạy cảm hoặc chưa chắc chắn sang HR xử lý. HR admin có thể quản lý tài liệu, xem ticket, theo dõi feedback và phát hiện các chủ đề đang được hỏi lặp lại.

Tech stack chính:

- Backend: FastAPI, SQLAlchemy async, Alembic, JWT auth, Chroma vector store.
- Frontend: React 18, Vite, TypeScript.
- AI: Gemini cho generation và embedding, Cohere rerank khi bật.
- Production target: ứng dụng Docker hóa, dùng Postgres và Chroma data được lưu bền vững.

## 3. Evaluation Metrics

Nguồn baseline: `eval/results/benchmark_results.json`.

Dataset: `So Tay Nhan Vien Golden Dataset`, phiên bản `2026-06-27`, gồm 20 test cases.

| Metric | Baseline / Hiện tại | Target | Ghi chú |
| --- | ---: | ---: | --- |
| Pass rate | 90.0% | >= 95% cho mục tiêu cuối | 18 passed / 20 total cases |
| Citation rate | 90.0% | >= 95% | 18 cited / 20 expected-cited cases |
| P95 latency | 3,2342.89 ms | < 2000 ms | Điểm nghẽn hiện tại cần tối ưu trước demo cuối |
| P95 TTFT | 2670 ms | < 2000 ms | Thời gian tới token đầu tiên / phản hồi stream đầu tiên |
| Groundedness score | 0.873 | >= 0.9 | Đo mức độ câu trả lời được chống lưng bởi nguồn truy xuất |
| Relevance | 0.836 | >= 0.9 | Đo mức độ liên quan |
| Correctness score | 0.927 | >= 0.9 | Đã vượt target trong benchmark hiện tại |

Ưu tiên cải thiện tiếp theo:

1. Tăng citation rate bằng cách siết ngưỡng retrieval và từ chối trả lời khi thiếu nguồn phù hợp.
2. Giảm P95 latency bằng cách giới hạn số chunk ứng viên, cache embedding/retrieval khi phù hợp, và chỉ bật rerank cho các query cần độ tin cậy cao.
3. Tăng pass rate bằng cách sửa các câu trả lời sai ở nhóm company-profile và bổ sung golden examples có mục tiêu.

## 4. Guardrails

Các guardrails đã triển khai hoặc đã thiết kế cho production pilot:

| Guardrail | Mục đích | Cách triển khai |
| --- | --- | --- |
| JWT auth và RBAC | Ngăn truy cập trái quyền | Các protected API routes lấy current user từ DB-backed JWT dependency |
| Bộ lọc retrieval theo role và department | Ngăn người dùng truy xuất tài liệu ngoài phạm vi quyền | Filter được áp dụng trước bước generation, không chỉ lọc sau khi sinh câu trả lời |
| Ranh giới dữ liệu HR cá nhân | Ngăn rò rỉ dữ liệu chéo giữa nhân viên | Role employee chỉ được xem HR metrics của chính mình; không chấp nhận `employee_id` tùy ý |
| Chặn jailbreak và câu hỏi ngoài phạm vi | Ngăn prompt injection và việc dùng app ngoài mục đích HR | Guardrails local dạng deterministic rules chặn jailbreak, câu hỏi không liên quan và yêu cầu nhạy cảm |
| Yêu cầu citation | Giảm hallucination trong câu trả lời về chính sách | Câu trả lời policy cần có citations hoặc từ chối nếu không tìm thấy nguồn liên quan |
| Human escalation | Tránh để AI tự quyết định tình huống HR nhạy cảm | Chat trả về escalation action; frontend yêu cầu user xác nhận trước khi tạo ticket |
| Audit/debug logging | Hỗ trợ review và cải thiện chất lượng | Lưu model name, retrieval ids, citations, role, feedback và escalation reason |
| Kiểm tra production secrets | Tránh deploy bằng cấu hình mặc định không an toàn | Production yêu cầu `JWT_SECRET_KEY` mạnh, CORS allowlist rõ ràng, Gemini key và DB migration |

Các kịch bản demo guardrails:

- Hỏi dữ liệu HR riêng tư của một nhân viên khác: hệ thống phải từ chối hoặc escalates.
- Hỏi prompt jailbreak như "ignore all previous rules": hệ thống phải chặn.
- Hỏi câu ngoài phạm vi HR: hệ thống phải từ chối hoặc route sang ticket.
- Hỏi câu chính sách hợp lệ: hệ thống phải trả lời kèm citations.
- Hỏi tình huống nhạy cảm cần HR quyết định: hệ thống phải yêu cầu xác nhận trước khi tạo ticket.

## 5. Demo Video Draft

Độ dài mục tiêu: 3-5 phút.

| Thời lượng | Phần | Nội dung |
| --- | --- | --- |
| 0:00-0:30 | Vấn đề | HR phải trả lời lặp lại các câu hỏi chính sách; nhân viên chờ câu trả lời cho nghỉ phép, bảo hiểm, onboarding và phúc lợi |
| 0:30-1:00 | Solution pitch | HR Helpdesk AI trả lời HR có citations, bảo vệ quyền truy cập và escalation các tình huống nhạy cảm |
| 1:00-1:30 | Slide kiến trúc | React frontend, FastAPI API, JWT/RBAC, RAG với Chroma, Gemini, Cohere rerank, Postgres |
| 1:30-2:30 | Live demo: employee chat | Đăng nhập employee, hỏi chính sách nghỉ phép, hiển thị câu trả lời có citation và source excerpt |
| 2:30-3:15 | Live demo: guardrails | Hỏi dữ liệu riêng tư hoặc jailbreak, hiển thị refusal/escalation behavior |
| 3:15-4:00 | Live demo: HR admin | Đăng nhập admin, hiển thị document management, tickets, feedback và trending topic workflow |
| 4:00-4:40 | Metrics | Hiển thị bảng benchmark: pass rate, citation rate, latency, groundedness, correctness |
| 4:40-5:00 | Kết thúc | Production URL, cải thiện tiếp theo và mức độ sẵn sàng cho pilot |


## 6. Cost Report tùy chọn

Nguồn pricing đã kiểm tra ngày 2026-06-28:

- Gemini API pricing: https://ai.google.dev/gemini-api/docs/pricing
- Cohere pricing: https://cohere.com/pricing

Giả định cho một pilot nội bộ nhỏ:

- 100 câu hỏi chat / user / tháng.
- Mỗi generation request trung bình: 2,000 input tokens và 600 output tokens.
- Mỗi query embedding trung bình: 120 text tokens / câu hỏi.
- Ước tính generation dùng Gemini 3.1 Flash-Lite standard pricing từ Google AI: $0.25 / 1M input tokens và $1.50 / 1M output tokens.
- Ước tính embedding dùng Gemini Embedding 2 text pricing: $0.20 / 1M text input tokens.
- Chi phí hạ tầng, database, storage và Cohere shared API pricing sẽ chốt sau khi chọn deployment account và billing plan.

Ước tính chi phí AI biến đổi / user / tháng:

| Hạng mục chi phí | Công thức | Ước tính |
| --- | --- | ---: |
| Generation input | 100 * 2,000 / 1,000,000 * $0.25 | $0.050 |
| Generation output | 100 * 600 / 1,000,000 * $1.50 | $0.090 |
| Query embeddings | 100 * 120 / 1,000,000 * $0.20 | $0.002 |
| Document ingestion embeddings | Amortized low-volume policy uploads | ~$0.010 |
| AI subtotal ước tính | Trước infra và rerank | ~$0.15 / user / tháng |

Ghi chú về Cohere rerank:

- Trang pricing công khai của Cohere định nghĩa một rerank search unit là một query với tối đa 100 documents được xếp hạng.
- Với MVP pilot, dùng Trial API key trong Cohere được giới hạn tổng cộng 1.000 request (lần gọi API) trên toàn bộ các endpoint mỗi tháng

Tổng ước tính pilot:

- AI variable cost: khoảng $0.15 / user / tháng theo các giả định bên trên.

