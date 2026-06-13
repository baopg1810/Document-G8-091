# Giai đoạn 02 - Phạm vi (Quyết định tiếp tục/dừng lại)

Phạm vi (Scope) = các tính năng được lựa chọn dựa trên công thức TÁC ĐỘNG x CHI PHÍ, nằm trong quỹ thời gian cho phép.
DỪNG LẠI (KILL) ở giai đoạn này là ít tốn kém và sáng suốt. Việc loại bỏ một ý tưởng yếu tại cổng kiểm soát này là một KẾT QUẢ THÀNH CÔNG.

## Tiêu chí đánh giá Tác động (Rubric đánh giá giá trị kinh doanh - cho điểm TRƯỚC KHI xem xét chi phí)

| Tác động (Impact) | Ý nghĩa |
|---|---|
| H (High) | Thúc đẩy doanh thu hoặc cam kết cốt lõi: thu hút người dùng mới (acquisition), chuyển đổi thanh toán (revenue), hoặc mang lại giá trị cốt lõi nhất mà người dùng tìm kiếm |
| M (Medium) | Giữ chân người dùng / tiết kiệm thời gian thực tế hàng tuần (retention, vận hành) |
| L (Low) | Tính năng bổ sung (nice-to-have); không ai sẵn sàng trả tiền hoặc thay đổi thói quen vì nó |

## Tiêu chí xếp loại mức độ phức tạp lập trình AI

| Phân loại | Ý nghĩa | Ví dụ |
|---|---|---|
| A | Dễ dàng/Ít tốn kém cho AI | CRUD (thêm, đọc, sửa, xóa), biểu mẫu (forms), bảng điều khiển (dashboards), trang nội dung tĩnh, các API wrappers đơn giản |
| B | Trung bình | Xử lý tệp tin, tích hợp bên thứ ba, xác thực qua thư viện có sẵn, một cuộc gọi LLM đơn lẻ, bản nháp AI có sự tham gia của con người (HITL) |
| C | Phức tạp/Đắt đỏ | Xử lý thời gian thực, thanh toán tự phát triển từ đầu, xác thực tùy chỉnh, các luồng AI agent tự chủ phức tạp, xử lý đồng thời mức độ cao |

## Gate - Kiểm tra TẤT CẢ trước khi chạy `/flow next`
- [x] Mỗi tính năng dưới đây đều có mức TÁC ĐỘNG (H/M/L đi kèm lý do kinh doanh) VÀ phân loại độ khó (A/B/C)
- [x] Không có tính năng tác động thấp (L) nào có độ khó trên mức A được giữ lại trong phiên bản v1
- [x] Phần tính năng đề xuất đã thực sự được cân nhắc (mỗi đề xuất đều có quyết định Đưa vào (IN) hoặc Loại bỏ (OUT))
- [x] Đảm bảo điều kiện fit(grades, budget) - mọi tính năng loại C trong phạm vi đều phải được giải trình lý do chính đáng (ghi chú bên cạnh tính năng)
- [x] Nếu bản thân sản phẩm LÀ một tính năng loại C: nó phải được ưu tiên xây dựng ĐẦU TIÊN, và các tính năng loại C phụ trợ khác phải được đưa vào danh sách cắt giảm
- [x] Danh sách cắt giảm đã được ghi lại (những gì tôi KHÔNG xây dựng trong v1)
- [x] Quyết định TIẾP TỤC (GO) / DỪNG LẠI (KILL) được ghi nhận bên dưới
- [x] Không còn trường trống (FILL placeholder) nào trong file này

## Quỹ thời gian

30-40 giờ tập trung cho một bản MVP chạy thử nghiệm (demo MVP), sử dụng mã nguồn FastAPI/LangGraph sẵn có và dữ liệu cục bộ/giả lập thay vì tích hợp toàn diện với hệ thống doanh nghiệp.

## Các tính năng trong v1 (mỗi tính năng đi kèm mức tác động VÀ độ khó)

- Hỏi đáp chính sách kèm trích dẫn nguồn - Tác động H (Nhiệm vụ cốt lõi: nhân viên nhận được câu trả lời đáng tin cậy ngay lập tức) - Độ khó B - Sử dụng Gemini, Gemini Embedding 2 và một kho lưu trữ vector nhỏ; giới hạn việc nạp dữ liệu ở các tệp chính sách nhân sự và lưu trữ siêu dữ liệu trích dẫn nguồn.
- Phân quyền dựa trên vai trò (RBAC) và rào cản bảo vệ (guardrails) - Tác động H (Bảo mật là bắt buộc để tạo niềm tin trong quản lý nhân sự) - Độ khó B - Sử dụng vai trò JWT kết hợp với các bộ lọc truy xuất dữ liệu và các quy tắc từ chối trả lời; không tích hợp SSO doanh nghiệp trong phiên bản v1.
- Tra cứu chỉ số nhân sự cá nhân - Tác động H (Nhiệm vụ giá trị cao của nhân viên vượt ra ngoài các câu hỏi FAQ thông thường) - Độ khó B - Triển khai cơ chế gọi hàm (function calling) an toàn kết nối với một bộ điều hợp HRIS giả lập để tra cứu số dư ngày phép, tình trạng bảo hiểm và trạng thái đánh giá khen thưởng.
- Chuyển tiếp hỗ trợ (Escalation tickets) - Tác động H (Ngăn chặn các quyết định AI không an toàn và tạo hàng đợi xử lý cho phòng HR) - Độ khó A - Tạo các bản ghi ticket chứa lý do chuyển tiếp, tóm tắt cuộc trò chuyện, mức độ ưu tiên và trạng thái.
- Tóm tắt xu hướng và thông báo ghim - Tác động M (Giảm tần suất các câu hỏi lặp lại dồn dập khi chính sách thay đổi) - Độ khó B - Tái thiết kế kiến trúc gom cụm thời gian thực xuống mức ghi nhật ký truy vấn có ngưỡng và tóm tắt theo lô (batch summarization); không xây dựng luồng xử lý thời gian thực liên tục trong phiên bản v1.
- Bảng điều khiển cho quản trị viên HR - Tác động M (Giúp bộ phận HR theo dõi các xu hướng, quản lý ticket và các câu trả lời được ghim) - Độ khó A - Các bảng biểu/thẻ hiển thị thông tin đơn giản lấy dữ liệu từ các endpoint sẵn có.

## Các tính năng đề xuất (Ưu tiên tác động trước - đề xuất, chưa quyết định)

- Tải lên tài liệu quản trị và đánh chỉ mục lại (re-index) - Tác động H (Giúp người dùng thấy được giá trị khi sử dụng chính sách riêng của họ) - Độ khó B - ĐƯA VÀO v1 (IN), giới hạn ở việc tải lên tệp PDF/văn bản và thực hiện đánh chỉ mục thủ công.
- Đánh giá phản hồi câu trả lời (thích/không thích) - Tác động M (Tăng độ tin cậy và cung cấp tín hiệu phản hồi cho bộ phận HR) - Độ khó A - ĐƯA VÀO v1 (IN) vì chi phí phát triển thấp và hỗ trợ kiểm soát chất lượng trong bản demo.
- Tích hợp bot Slack/Teams - Tác động M (Tiếp cận nhân viên trực tiếp trên các kênh làm việc) - Độ khó B - LOẠI BỎ khỏi v1 (OUT) vì giao diện chat trên web đã đủ để chứng minh giá trị cốt lõi, đồng thời giúp cơ chế xác thực và quản lý dữ liệu đơn giản hơn.

## Danh sách cắt giảm (KHÔNG có trong v1 - tạm hoãn, không xóa bỏ)

- Tích hợp đầy đủ với hệ thống HRIS thực tế - Tạm hoãn vì hệ thống thực tế yêu cầu thông tin đăng nhập bảo mật, ánh xạ dữ liệu, đánh giá kiểm toán và phê duyệt bảo mật.
- Xác thực SSO/SCIM doanh nghiệp và cấu trúc tổ chức phức tạp - Tạm hoãn vì phương thức đăng nhập phân vai trò bằng JWT đã đủ đáp ứng yêu cầu của MVP.
- Tự động thực thi các thủ tục nhân sự - Tạm hoãn vì việc phê duyệt nghỉ phép, thay đổi bảo hiểm và các tác vụ liên quan đến bảng lương có rủi ro cao.
- Gom cụm luồng dữ liệu thời gian thực cho xu hướng - Tạm hoãn, thay thế bằng giải pháp tóm tắt theo lô có áp ngưỡng.
- Hệ thống thanh toán SaaS đa người thuê (multi-tenant) và cô lập tài khoản khách hàng - Tạm hoãn vì những người dùng đầu tiên là các tài khoản demo nội bộ, không phải khách hàng doanh nghiệp trả phí bên ngoài.
- Trò chuyện bằng giọng nói và ứng dụng di động - Tạm hoãn vì giao diện web chat đã đủ để chứng minh giá trị.

## Quyết định

TIẾP TỤC (GO) - Phiên bản v1 sẽ chứng minh giá trị kinh doanh cốt lõi thông qua các câu trả lời chính sách nhân sự có trích dẫn nguồn, cơ chế tra cứu số liệu cá nhân an toàn và khả năng chuyển tiếp yêu cầu cho con người, đồng thời tạm gác các tích hợp phức tạp cấp doanh nghiệp ra ngoài phạm vi dự án.
