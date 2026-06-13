# Giai đoạn 01 - Nghiên cứu (Nghiên cứu trước)

Quy tắc: KHẢO SÁT những gì đã tồn tại. Yêu cầu có bằng chứng thực tế - liên kết, trích dẫn, ảnh chụp màn hình.
Nếu nói "Tôi nghĩ không có sản phẩm nào giống thế này" mà không thực hiện tìm kiếm = Không vượt qua vòng đánh giá (gate fail).

Ngày nghiên cứu: 2026-06-13.

## Gate - Kiểm tra TẤT CẢ trước khi chạy `/flow next`
- [x] Tôi đã thực sự TRUY CẬP 3 công cụ/đối thủ cạnh tranh hiện tại (các liên kết bên dưới, kèm theo một nhận xét trung thực cho mỗi bên)
- [x] Tôi đã tìm thấy 3 lời phàn nàn THỰC TẾ của người dùng trên mạng và trích dẫn lại (kèm link nguồn)
- [x] Tôi đã ghi lại mức GIÁ mà đối thủ cạnh tranh đang thu (giá thực tế) và ai đang trả tiền cho họ
- [x] Tôi đã chỉ ra duy nhất MỘT kênh giúp tiếp cận 10 người dùng đầu tiên (một địa điểm cụ thể, không viết chung chung kiểu "mạng xã hội")
- [x] Tôi đã viết lý do tại sao những người dùng đó chọn giải pháp này thay vì hiện tại (một đoạn văn trung thực)
- [x] Tôi đã phân tích những gì là miễn phí (có sẵn thư viện) và những gì là khó khăn về mặt kỹ thuật cho ý tưởng này
- [x] Không còn trường trống (FILL placeholder) nào trong file này

## Những gì đã tồn tại (3 sản phẩm - hãy mở link xem thực tế, không đoán)

1. ServiceNow HR Service Delivery - https://www.servicenow.com/products/hr-service-delivery.html - Bộ công cụ quy trình làm việc nhân sự (HR workflow suite) cực kỳ mạnh mẽ dành cho doanh nghiệp lớn. Sản phẩm cung cấp cổng giao tiếp AI, tự phục vụ dạng agent (agentic self-service), quản lý vụ việc (case management) và chuyển tiếp hỗ trợ (escalation). Tuy nhiên, đây là phần mềm doanh nghiệp cồng kềnh, có khả năng quá đắt và triển khai chậm đối với một MVP phục vụ khóa học hoặc triển khai cục bộ quy mô vừa.
2. Freshservice cho Dịch vụ CNTT và Nhân viên - https://www.freshworks.com/freshservice/pricing/ - Hệ thống hỗ trợ (service desk) mạnh mẽ với các kênh dịch vụ nhân viên, quy trình công việc, cơ sở tri thức (knowledge base), hỗ trợ qua Teams/Slack và định giá minh bạch. Tuy nhiên, nó bao quát rộng hơn mảng nhân sự (HR) và không cung cấp sẵn một trợ lý RAG chạy cục bộ gọn nhẹ (local-first) hỗ trợ các trích dẫn chính sách bằng tiếng Việt.
3. Atlassian Service Collection / Jira Service Management - https://www.atlassian.com/collections/service/pricing - Hệ thống tiếp nhận yêu cầu mạnh mẽ, có sẵn các mẫu HR, cơ sở tri thức, AI Rovo, agent dịch vụ ảo và các kiểm soát doanh nghiệp. Nó hoạt động tốt nhất cho các đội ngũ đã nằm trong hệ sinh thái Atlassian và tạo cảm giác giống như một nền tảng quản lý dịch vụ (service management) hơn là một trợ lý trả lời câu hỏi nhân sự đơn giản.

## Người dùng nói gì (3 lời phàn nàn thực tế, được trích dẫn, kèm nguồn)

1. > "Chúng tôi sẽ xem xét việc này." - Câu chuyện của một nhân viên về việc bộ phận HR phớt lờ các vấn đề lặp đi lặp lại trong suốt 6 tháng: https://m.economictimes.com/magazines/panache/hr-ignored-his-3-issues-for-6-months-with-we-will-look-into-it-when-asked-the-reason-his-resignation-answer-stunned-hr/articleshow/131028994.cms
2. > "lý do cá nhân" - Câu chuyện về việc xin nghỉ phép/quyền riêng tư của nhân viên khi HR vẫn cố đòi hỏi chi tiết lý do: https://m.economictimes.com/magazines/panache/employee-says-leave-is-for-personal-reasons-hr-still-demands-to-know-the-reason-netizens-say-say-you-may-/articleshow/125540527.cms
3. > "Nhận được cuộc gọi từ HR" - Câu chuyện hiểu lầm khi onboard, một nhân viên mới đến ngày đi làm đầu tiên mới phát hiện ra thực tế mình chưa hề được công ty tuyển dụng chính thức: https://m.economictimes.com/magazines/panache/woman-on-first-day-of-new-job-realises-she-was-never-even-hired-by-the-company-received-a-call-from-hr-/articleshow/131631404.cms

## Chiến lược tiếp cận thị trường (GTM) & Thực tế kinh doanh

Xây dựng sản phẩm hiện tại là phần ít tốn kém nhất. Kênh phân phối và mức độ sẵn sàng chi trả mới là nơi các ý tưởng bị khai tử - hãy nghiên cứu chúng TRƯỚC KHI lập kế hoạch, chứ không phải sau khi đã bàn giao sản phẩm.

### Ai đang trả tiền hiện nay, và trả bao nhiêu (các điểm tham chiếu về giá)

- Freshservice - Bảng giá chính thức công bố: gói Starter $19/agent/tháng, Growth $49/agent/tháng, Pro $99/agent/tháng (thanh toán hàng năm), và gói Enterprise theo báo giá tùy chỉnh. Người mua là các đội ngũ CNTT/dịch vụ nhân viên chi trả dựa trên số lượng nhân sự hỗ trợ: https://www.freshworks.com/freshservice/pricing/
- Atlassian Service Collection - Bảng giá chính thức công bố: Miễn phí cho 3 agents, gói Standard $20/agent/tháng, Premium $51.42/agent/tháng, và gói Enterprise liên hệ phòng kinh doanh. Người mua là các nhóm quản lý dịch vụ; số lượng khách hàng/người gửi yêu cầu là không giới hạn và không cần mua bản quyền: https://www.atlassian.com/collections/service/pricing
- ServiceNow HRSD - Trang sản phẩm chính thức bán các AI agent và hệ thống tự động hóa vụ việc/quy trình nhân sự, định giá dạng tùy chỉnh (sales-led). Người mua là các nhóm chuyển đổi số HR/CNTT của doanh nghiệp lớn, thường có sẵn ngân sách mua sắm và triển khai dự án: https://www.servicenow.com/products/hr-service-delivery.html

### Kênh tiếp cận 10 người dùng đầu tiên (duy nhất, chỉ rõ tên)

Các thành viên trong khóa học AI20K/Cohort 2 và nơi làm việc của họ: Nhờ bạn học/thành viên trong nhóm, những người có sẵn tài liệu chính sách hành chính/nhân sự bằng tiếng Việt, chạy mô phỏng hoặc kiểm thử các câu hỏi về chính sách nội bộ thường gặp. Kênh này có thể tiếp cận ngay lập tức thông qua cộng đồng khóa học và cung cấp dữ liệu hạt giống thực tế mà không cần qua quy trình mua sắm phức tạp của doanh nghiệp.

### Tại sao họ chuyển sang giải pháp này (so với hiện tại)

Những người dùng đầu tiên sẽ chọn giải pháp này thay vì các nhóm chat, email hoặc câu trả lời thủ công từ phòng HR vì nó đưa ra phản hồi ngay lập tức đi kèm trích dẫn nguồn rõ ràng, đồng thời có quy trình bàn giao (handoff) rõ ràng khi AI không được phép tự quyết định. Lợi thế cạnh tranh cục bộ không phải là việc tạo ra "một ServiceNow lớn hơn", mà là một trợ lý chính sách nhân sự phản hồi nhanh, ưu tiên tiếng Việt, có thể chạy demo dễ dàng bằng các file PDF chính sách riêng của công ty và một bộ điều hợp chỉ số nhân sự nhỏ gọn.

## Những phần kỹ thuật là miễn phí (có sẵn) so với những phần khó khăn

- Miễn phí / Dễ dàng (được giải quyết bởi các thư viện/nền tảng có sẵn): Tài liệu API bằng FastAPI, các thư viện JWT, định tuyến bằng LangGraph, gọi API Gemini, tạo vector nhúng (embedding), tìm kiếm vector bằng Chroma hoặc pgvector, các endpoint CRUD cho ticket hỗ trợ, bảng điều khiển (dashboard) cơ bản, tóm tắt xu hướng dựa trên ngưỡng.
- Khó khăn (phải tự phát triển, nhiều rủi ro thực tế): Cắt đoạn tài liệu (chunking) chất lượng cao/trích dẫn chính xác cho văn bản pháp lý nhân sự, lọc dữ liệu truy xuất dựa trên vai trò (role-based retrieval filtering), rào cản ngăn chặn ảo giác (hallucination guardrails), gọi hàm hệ thống HRIS an toàn, kiểm soát quyền riêng tư đối với dữ liệu cá nhân, sự trôi lệch của mô hình/phiên bản (model/version drift), và việc chuyển đổi "nhiều câu hỏi tương tự" thành các chủ đề xu hướng hữu ích mà không bị lẫn tạp âm nhiễu từ việc phân cụm.
