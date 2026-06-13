# Giai đoạn 00 - Ý tưởng (Idea)

## Gate - Kiểm tra TẤT CẢ trước khi chạy `/flow next`
- [x] Ý tưởng (pitch) dưới đây có độ dài không quá 3 câu
- [x] Tôi có thể chỉ ra ít nhất MỘT người/nhóm người thực tế gặp phải vấn đề này (được nêu bên dưới)
- [x] Không còn trường trống (FILL placeholder) nào trong file này

## Ý tưởng (Pitch) (Độ dài 3 câu: đối tượng, vấn đề, giải pháp bạn xây dựng)

Nhân viên nội bộ, cán bộ/giảng viên và nhân sự hành chính trong doanh nghiệp cần hỏi đáp nhanh về nghỉ phép, lương thưởng, bảo hiểm và quy trình nội bộ.
Phòng nhân sự (HR) bị quá tải vì phải trả lời lặp đi lặp lại các câu hỏi cơ bản, trong khi nhân viên phải chờ đợi phản hồi cho những việc đơn giản và dễ bị tồn đọng câu hỏi khi có chính sách mới.
Tôi sẽ xây dựng hệ thống HR Helpdesk AI: một trợ lý ảo sử dụng RAG với trích dẫn chính xác, có cơ chế phân quyền (RBAC)/rào cản bảo vệ (guardrails), tóm tắt xu hướng câu hỏi (trending summary), tra cứu số liệu cá nhân qua cơ chế gọi hàm (function calling) và tự động tạo ticket chuyển tiếp (escalation ticket) sang nhân sự thực tế khi vượt quá phạm vi xử lý.

## Người/Nhóm thực tế gặp phải vấn đề này

Bộ phận HR và nhân viên nội bộ tại các doanh nghiệp/đơn vị từ 100 - 1000 người có số lượng câu hỏi hành chính nhân sự lặp lại cao, đặc biệt là các nhóm cần tra cứu nghỉ phép, bảo hiểm, lương thưởng và quy trình nội bộ hàng ngày.
