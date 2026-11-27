# QUY TẮC PHỐI HỢP MULTI-AGENT

## 1. Phân công vai trò
- **Gemini (Game Designer & Reviewer):** Quản lý GDD.md, thiết kế luật chơi, chỉ số, màn chơi và kiểm tra xung đột logic giữa các file.
- **Claude (Lead Programmer):** Dựng khung dự án, core game loop, vật lý, state management.
- **ChatGPT (Feature & QA):** Code tính năng phụ, UI/HUD, hiệu ứng, âm thanh và bắt bug.

## 2. Luật làm việc & Bàn giao
- Không tự ý sửa cấu trúc thư mục hoặc viết lại code thuộc vai trò AI khác nếu không được yêu cầu.
- Làm xong task nào phải cập nhật cuối file PROGRESS.md theo mẫu:
  **[Tên AI]:** Đã làm gì | File đã tạo/sửa | Việc tiếp theo cho AI kế tiếp.
