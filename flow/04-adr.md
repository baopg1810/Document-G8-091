# Giai đoạn 04 - ADR (Quyết định Kiến trúc)

Ngắn gọn. Phần giá trị nhất là những gì bạn KHÔNG làm và lý do tại sao.

## Gate - Kiểm tra TẤT CẢ trước khi chạy `/flow next`
- [x] Mỗi quyết định đều có một dòng giải thích "lý do" và một dòng nêu "phương án bị loại bỏ"
- [x] Danh sách các việc KHÔNG làm đã được ghi nhận
- [x] Các quyết định bao gồm: lưu trữ dữ liệu, phương thức xác thực, mục tiêu triển khai
- [x] Không còn trường trống (FILL placeholder) nào trong file này

## Quyết định

| # | Quyết định | Lý do | Phương án thay thế bị loại bỏ |
|---|---|---|---|
| 1 | Backend sử dụng FastAPI + LangGraph | Kho lưu trữ (repo) đã sử dụng FastAPI/LangGraph và luồng agent cần định tuyến giữa truy xuất dữ liệu (retrieval), rào cản bảo vệ (guardrails), gọi hàm (function calling) và chuyển tiếp yêu cầu lên cấp cao hơn (escalation) | Viết lại bằng Node.js, vì điều này làm chậm tiến độ phát triển MVP và lãng phí mã nguồn hiện có |
| 2 | SQLite cho bản ghi ứng dụng trong v1, Chroma cho dữ liệu vector | Kho lưu trữ cục bộ (local) chi phí thấp cho MVP để lưu người dùng, vé hỗ trợ (tickets), phản hồi, nhật ký truy vấn và các phân đoạn tài liệu (document chunks) | Sử dụng toàn bộ Postgres + pgvector ngay bây giờ, vì chi phí thiết lập/triển khai cao hơn đối với phân đoạn phát triển đầu tiên (first slice) |
| 3 | Xác thực JWT với các quyền vai trò (role claims): employee, department_admin, hr_admin | Đủ tốt để kiểm tra cơ chế phân quyền dựa trên vai trò (RBAC) và các bộ lọc truy xuất dữ liệu mà không cần triển khai hệ thống danh tính doanh nghiệp phức tạp | SSO/SCIM/xác thực tùy chỉnh (custom auth), vì đây là công việc tích hợp bảo mật quan trọng nằm ngoài phạm vi phiên bản v1 |
| 4 | Gemini API với ID mô hình ổn định trong cấu hình | Người dùng yêu cầu sử dụng Gemini, và tài liệu chính thức đề xuất Gemini 3.1 Flash-Lite cùng Gemini Embedding 2 để tối ưu hóa chi phí tạo văn bản và nhúng (embeddings) cho RAG | Viết cứng (hard-code) ID mô hình bản xem trước (preview), vì tài liệu Google chỉ ra rằng các mô hình xem trước/bị loại bỏ (deprecated) có thể bị đóng cửa |
| 5 | Quy trình RAG yêu cầu siêu dữ liệu trích dẫn (citation metadata) trước khi tạo câu trả lời | Sự tin cậy đối với câu trả lời về pháp lý/chính sách phụ thuộc vào tiêu đề nguồn, mục lục và đoạn trích dẫn hiển thị rõ ràng | Câu trả lời LLM tự do từ văn bản tải lên, vì nó không thể chứng minh câu trả lời được căn cứ (grounded) trên nguồn tài liệu nào |
| 6 | Gọi hàm (function calling) chỉ đọc từ bộ điều hợp HRIS giả lập (mock HRIS adapter) trong v1 | Chứng minh khả năng tra cứu thông tin cá nhân một cách an toàn mà không ảnh hưởng đến dữ liệu nhân sự thực tế | Kết nối trực tiếp đến hệ thống HRIS thực tế (production HRIS), vì thông tin đăng nhập, kiểm toán (audit) và ánh xạ dữ liệu chưa sẵn sàng |
| 7 | Tóm tắt xu hướng theo lô có ngưỡng giới hạn (Thresholded batch trending summary) | Giảm tính năng xử lý thời gian thực/phức tạp tốn kém xuống mức MVP loại B nhưng vẫn giữ nguyên giá trị cốt lõi cho người dùng | Gom cụm luồng ngữ nghĩa thời gian thực (realtime semantic stream clustering), vì nó làm tăng rủi ro về xử lý đồng thời và dữ liệu nhiễu |
| 8 | Đóng gói ứng dụng chạy cục bộ bằng Docker trước (Dockerized local app first) | File Dockerfile/docker-compose hiện có tạo ra một quy trình chạy demo có thể lặp lại và ổn định | Triển khai trực tiếp lên môi trường đám mây (cloud production) ngay bây giờ, vì hợp đồng thiết kế (planning contract) và phân mảnh API cần được hoàn thiện trước |

## Các việc KHÔNG làm trong v1 (và lý do tại sao bỏ qua vẫn an toàn)

- Không ghi dữ liệu vào hệ thống HRIS thực tế: An toàn vì phiên bản MVP chỉ đọc các chỉ số cá nhân giả lập và chuyển tiếp các trường hợp phức tạp cho nhân sự xử lý.
- Không tích hợp SSO doanh nghiệp: An toàn vì phân quyền dựa trên vai trò qua mã JWT vẫn đủ để chứng minh cơ chế phân quyền (RBAC) trong bản demo.
- Không xây dựng mô hình SaaS đa người thuê (multi-tenant): An toàn vì những người dùng đầu tiên là người thử nghiệm nội bộ hoặc học viên khóa học, không phải các khách hàng doanh nghiệp riêng biệt.
- Không xử lý luồng xu hướng thời gian thực: An toàn vì việc áp đặt ngưỡng giới hạn trên nhật ký truy vấn (query-log) đã đủ để thể hiện hành vi sản phẩm.
- Không tự động thực hiện các thủ tục nhân sự: An toàn vì bộ phận nhân sự (con người) vẫn là cơ quan có thẩm quyền quyết định cuối cùng đối với các hành động nhạy cảm.
- Không tích hợp bot Slack/Teams: An toàn vì ứng dụng web đã đủ để chứng minh các luồng câu trả lời cốt lõi và quy trình chuyển tiếp hỗ trợ trước.
