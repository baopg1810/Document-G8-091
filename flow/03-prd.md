# Giai đoạn 03 - Tài liệu Yêu cầu Sản phẩm (PRD)

Tối đa 1-2 trang. Tiêu chí kiểm tra: một người lạ có thể xây dựng phiên bản v1 từ tài liệu này mà không cần hỏi bạn điều gì không?

## Gate - Kiểm tra TẤT CẢ trước khi chạy `/flow next`
- [x] Mọi phần bên dưới đều được điền dựa trên quyết định về phạm vi (giai đoạn 02) của tôi, không tự ý mở rộng thêm
- [x] Chỉ số thành công phải là một CON SỐ cụ thể, không viết chung chung cảm tính (viết "tiết kiệm thời gian" là KHÔNG ĐẠT; "phản hồi đầu tiên < 2 giờ" là ĐẠT)
- [x] Mỗi tính năng đều nêu rõ hành động của người dùng và kết quả có thể quan sát được
- [x] Phần Vấn đề & Giải pháp (Pain & Gain) là một BẢNG ÁNH XẠ: mỗi vấn đề (pain) đều trích dẫn bằng chứng cụ thể (một câu trích dẫn ở giai đoạn 01 hoặc một quan sát thực tế), và chỉ rõ tính năng v1 giải quyết nó; mỗi tính năng v1 phải giải quyết được ít nhất một vấn đề
- [x] Một người lạ có thể xây dựng phiên bản v1 dựa trên tài liệu này mà không cần hỏi thêm điều gì
- [x] Không còn trường trống (FILL placeholder) nào trong file này

## Bối cảnh

Hệ thống HR Helpdesk AI được áp dụng vào doanh nghiệp nơi các câu trả lời về chính sách nhân sự hiện đang nằm rải rác trong nhiều tài liệu khác nhau, lịch sử chat và kiến thức cá nhân của từng nhân viên HR. Nhân viên muốn có câu trả lời nhanh chóng về chế độ nghỉ phép, quy chế lương thưởng, bảo hiểm và quy trình nội bộ, trong khi đội ngũ HR mất nhiều thời gian để trả lời các câu hỏi lặp lại và giải thích các thay đổi chính sách mới. Sản phẩm phiên bản v1 là một ứng dụng web tích hợp khung chat cho nhân viên, hệ thống RAG có trích dẫn nguồn, cơ chế tra cứu số liệu cá nhân an toàn, chuyển tiếp hỗ trợ cho HR và một bảng điều khiển quản trị nhỏ để theo dõi các xu hướng và ticket.

## Người dùng mục tiêu

- Nhân viên (Employee): Nhân viên nội bộ/cán bộ/giảng viên cần câu trả lời về chính sách, nghỉ phép, bảo hiểm và trạng thái nhân sự cá nhân của mình.
- Quản trị viên nhân sự (HR admin): Nhân sự phòng HR, người sở hữu các tài liệu chính sách, theo dõi các câu hỏi lặp lại và giải quyết các ticket chuyển tiếp hỗ trợ.
- Quản trị viên phòng ban (Department admin): Nhân sự hành chính được ủy quyền, có thể xem các câu trả lời chính sách trong phạm vi phòng ban của họ nhưng không được phép xem dữ liệu cá nhân nhạy cảm ngoài vai trò.

## Vấn đề & Giải pháp (Bảng ánh xạ - xương sống của PRD giúp theo dõi luồng tính năng)

| # | Vai trò | Vấn đề (cụ thể) | Bằng chứng (Trích dẫn/Nguồn ở GĐ01 hoặc quan sát thực tế) | Cách giải quyết tạm thời hiện nay | Tính năng V1 giải quyết vấn đề | Kết quả có thể quan sát được |
|---|---|---|---|---|---|---|
| P1 | Nhân viên | Chờ đợi HR trả lời các câu hỏi chính sách đơn giản | Trích dẫn: "Chúng tôi sẽ xem xét việc này." Nguồn: Câu chuyện HR phớt lờ phản hồi trên Economic Times | Gửi email/chat cho HR và chờ đợi | Hỏi đáp chính sách kèm trích dẫn nguồn | Nhân viên nhận được câu trả lời kèm nguồn tham chiếu trong vòng chưa đầy 10 giây |
| P2 | Quản trị viên HR | Trả lời lặp đi lặp lại cùng một câu hỏi về nghỉ phép/lương/bảo hiểm | Quan sát thực tế từ tài liệu dự án: HR liên tục trả lời các câu hỏi cơ bản | Copy/paste câu trả lời cũ, trả lời trong group chat | Hỏi đáp chính sách, tóm tắt xu hướng, thông báo ghim | Chủ đề lặp lại nhiều lần sẽ được chuyển thành thông báo ghim sau khi vượt qua ngưỡng quy định |
| P3 | Nhân viên | Cần tra cứu số liệu nhân sự cá nhân mà không muốn tiết lộ thêm chi tiết riêng tư | Trích dẫn: "lý do cá nhân" Nguồn: Câu chuyện về nghỉ phép/quyền riêng tư trên Economic Times | Hỏi trực tiếp HR hoặc phải chia sẻ bối cảnh riêng tư | Tra cứu số liệu nhân sự cá nhân với phân quyền RBAC | Nhân viên chỉ xem được trạng thái nghỉ phép/bảo hiểm/khen thưởng của chính mình |
| P4 | Quản trị viên HR | Các trường hợp phức tạp hoặc nhạy cảm cần con người xử lý chứ không thể để AI tự quyết | Quan sát thực tế từ tài liệu dự án: các vấn đề có thể vượt quá thẩm quyền của AI | Chuyển tiếp thủ công qua email/chat | Vé chuyển tiếp hỗ trợ (Escalation tickets) | Ticket hỗ trợ được tạo tự động kèm trạng thái và tóm tắt cuộc trò chuyện |
| P5 | Quản trị viên HR | Tài liệu chính sách khó tìm kiếm và cập nhật liên tục | Bằng chứng từ đối thủ: Các công cụ doanh nghiệp bán giải pháp quản lý tri thức/vụ việc | Tìm kiếm thủ công các file PDF và thông báo cũ | Tải lên tài liệu quản trị và đánh chỉ mục lại | Tài liệu chính sách mới xuất hiện trong kết quả tìm kiếm kèm trích dẫn nguồn |
| P6 | Quản trị viên HR | Không biết liệu các câu trả lời của AI có đáng tin cậy đối với người dùng hay không | Yêu cầu kiểm soát chất lượng MVP từ giai đoạn 02 | Hỏi han người dùng một cách không chính thức | Đánh giá câu trả lời (Answer feedback) | Quản trị viên có thể xem lượt đánh giá hữu ích/không hữu ích cho từng câu trả lời |

### Các vấn đề KHÔNG giải quyết trong v1 (Chủ ý - gắn liền với danh sách cắt giảm phạm vi)

- Các tác vụ ghi dữ liệu vào hệ thống HRIS thực tế - Tạm hoãn vì việc phê duyệt nghỉ phép, thay đổi lương thưởng hoặc thay đổi trạng thái bảo hiểm yêu cầu quy trình phê duyệt thực tế.
- Hệ thống SSO doanh nghiệp và SCIM - Tạm hoãn vì đăng nhập phân quyền bằng JWT đã đủ đáp ứng cho bản MVP cục bộ.
- Tích hợp bot Slack/Teams - Tạm hoãn vì giao diện web chat giúp chứng minh quy trình công việc trước khi mở rộng kênh.

## Phát biểu bài toán

Đội ngũ HR nội bộ mất quá nhiều thời gian để trả lời các câu hỏi hành chính lặp đi lặp lại trong khi nhân viên phải chờ đợi câu trả lời cho các vấn đề chính sách đơn giản và trạng thái cá nhân. Hệ thống HR Helpdesk AI cần phản hồi trong vài giây, trích dẫn nguồn chính thức, bảo vệ dữ liệu theo vai trò và chuyển tiếp an toàn khi AI không được quyền giải quyết vụ việc.

## Các tính năng (Dưới góc nhìn người dùng: Hành động -> Kết quả quan sát được)

- Với vai trò là nhân viên, tôi đặt một câu hỏi về chính sách nhân sự và tôi thấy câu trả lời có chứa các trích dẫn gồm tiêu đề tài liệu, chương/mục và đoạn trích dẫn cụ thể.
- Với vai trò là nhân viên, tôi hỏi về số dư ngày phép, tình trạng bảo hiểm hoặc trạng thái khen thưởng của mình và tôi chỉ thấy các chỉ số nhân sự cá nhân của mình được trả về thông qua một cuộc gọi hàm an toàn.
- Với vai trò là nhân viên, tôi hỏi điều gì đó ngoài phạm vi chính sách hoặc quá nhạy cảm đối với AI, và tôi thấy một ticket được tạo cho bộ phận HR với ID cụ thể.
- Với vai trò là quản trị viên HR, tôi tải lên hoặc đăng ký một tài liệu chính sách, và tài liệu đó sẵn sàng cho việc tìm kiếm có trích dẫn nguồn sau khi được đánh chỉ mục.
- Với vai trò là quản trị viên HR, tôi xem các chủ đề xu hướng và tôi thấy các tóm tắt ghim được tạo ra khi lượng câu hỏi vượt qua một ngưỡng nhất định.
- Với vai trò là quản trị viên HR, tôi xem các ticket chuyển tiếp hỗ trợ và tôi có thể cập nhật trạng thái ticket từ Mới (open) sang Đang xử lý (in_progress) đến Đã giải quyết (resolved).
- Với vai trò là bất kỳ người dùng nào, tôi đánh dấu một câu trả lời là hữu ích hoặc không hữu ích, và bộ phận HR có thể xem thống kê số lượt phản hồi.

## Các yêu cầu phi chức năng

- Các câu trả lời phải bao gồm ít nhất một nguồn trích dẫn đối với các câu hỏi chính sách, hoặc từ chối rõ ràng nếu không tìm thấy nguồn liên quan.
- Bộ lọc vai trò (role filters) phải được áp dụng trước khi tạo câu trả lời (generation), chứ không phải chỉ sau khi văn bản câu trả lời đã được tạo ra.
- Các endpoint tra cứu chỉ số nhân sự cá nhân phải yêu cầu xác thực bằng mã token và tuyệt đối không bao giờ chấp nhận ID nhân viên (`employee_id`) tùy ý từ vai trò nhân viên.
- Mục tiêu thời gian phản hồi chat P95 cho dữ liệu demo: dưới 10 giây.
- Lưu trữ tên mô hình, ID tài liệu truy xuất, trích dẫn, vai trò và lý do chuyển tiếp phục vụ kiểm toán và dò lỗi (audit/debug).
- Sử dụng định dạng dữ liệu UTF-8 sẵn sàng cho tiếng Việt, trong khi mã nguồn và cấu hình vẫn giữ đơn giản và dễ kiểm thử.

## Công nghệ sử dụng

- Backend: Python FastAPI, Pydantic, LangGraph.
- AI: Gemini API, dòng mô hình ổn định Gemini 3.1 Flash-Lite khi có sẵn, Gemini Embedding 2 cho các vector nhúng; viết cố định chuỗi tên mô hình chính xác trong cấu hình và tránh sử dụng ID mô hình xem trước (preview) đã bị loại bỏ.
- Dữ liệu: SQLite cho các bản ghi ứng dụng MVP, kho vector Chroma cục bộ cho việc truy xuất thông tin của MVP; định hướng nâng cấp lên Postgres + pgvector.
- Xác thực/Bảo mật: JWT, phân vai trò (role claims), bộ lọc truy xuất dữ liệu, rào cản prompt (prompt guardrails), các nhóm từ chối trả lời.
- Frontend: Giao diện web chat đơn giản và bảng điều khiển quản trị HR (HR admin dashboard), được phát triển sau khi có hợp đồng API.
- Mục tiêu triển khai: Đóng gói Docker ứng dụng FastAPI chạy cục bộ trước; sau đó triển khai lên các host dạng Render/Fly.io/Railway.

## Chỉ số thành công (chỉ dùng các con số cụ thể)

- 30 câu hỏi chính sách nhân sự mẫu được trả lời trong bản demo với tỷ lệ >= 90% có chứa ít nhất một nguồn trích dẫn đúng.
- Thời gian phản hồi P95 dưới 10 giây trên tập dữ liệu demo.
- 5 lượt tra cứu chỉ số nhân sự cá nhân trả về kết quả chính xác từ hệ thống HRIS giả lập và không bị rò rỉ dữ liệu chéo giữa các người dùng.
- 3 câu hỏi nằm ngoài phạm vi hoặc nhạy cảm tạo được ticket chuyển tiếp hỗ trợ thành công.
- 1 chủ đề xu hướng được ghim sau khi nhận được ít nhất 5 truy vấn tương tự.
