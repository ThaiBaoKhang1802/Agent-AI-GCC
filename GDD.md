# GAME DESIGN DOCUMENT (GDD) — VÂN MỘNG TU TIÊN (MVP MAP 1)

## 1. Tổng quan & Công nghệ
- **Thể loại:** 2D Top-Down Action RPG / Tu Tiên (Pixel Art).
- **Mục tiêu bản Test (MVP):** Hoàn thiện 1 bản đồ (Map 1) với đầy đủ vòng lặp: Di chuyển $\rightarrow$ Đánh quái/Hái linh thảo $\rightarrow$ Đả tọa hấp thụ linh khí $\rightarrow$ Đột phá cảnh giới.
- **Công nghệ:** HTML5 Canvas + Vanilla JavaScript + CSS.
- **Quy tắc kỹ thuật bắt buộc:** 
  - Dùng thẻ `<script>` thông thường load tuần tự trong `index.html` (dùng chung namespace `window.Game = {}`), **không dùng** ES6 `import/export` để người chơi mở trực tiếp file `index.html` trên trình duyệt là chạy ngay không bị lỗi CORS.
  - Đồ họa Pixel Art (lưới 16x16 hoặc 32x32, `ctx.imageSmoothingEnabled = false`) vẽ bằng mã màu/hình khối trực tiếp trong code (`src/sprites.js`) để chạy được ngay không cần file ảnh `.png` ngoài.

## 2. Cấu trúc thư mục & Phân chia file (Mỗi file dưới 250 dòng)
- `index.html`: Khung trang web chứa thẻ `<canvas id="gameCanvas" width="800" height="600">`.
- `style.css`: Giao diện nền tối, căn giữa canvas, font chữ kiểu pixel/cổ trang *(ChatGPT phụ trách)*.
- `src/config.js`: Hằng số game, chỉ số 3 cảnh giới, mã loại ô bản đồ *(Claude khởi tạo)*.
- `src/sprites.js`: Hàm vẽ pixel art bằng code cho Nhân vật, Quái, Linh thảo, Phi kiếm, Ô đất *(ChatGPT nâng cấp chi tiết sau bản thô của Claude)*.
- `src/map.js`: Dữ liệu Map 1 (kích thước 30x25 ô, mỗi ô 32x32px) và xử lý va chạm tường/đá *(Claude phụ trách)*.
- `src/player.js`: Di chuyển, đả tọa, tính toán Tu Vi, đột phá, phóng phi kiếm *(Claude phụ trách)*.
- `src/enemy.js`: Quái vật (tuần tra, đuổi theo, tấn công, rơi Yêu Đan) *(ChatGPT phụ trách)*.
- `src/ui.js`: Thanh máu (HP), Linh lực (MP), thanh Tu Vi, tên cảnh giới, chữ sát thương/đột phá bay lên *(ChatGPT phụ trách)*.
- `src/main.js`: Vòng lặp chính (`requestAnimationFrame`), camera theo nhân vật, bắt phím/chuột *(Claude phụ trách)*.

## 3. Thiết kế Map 1: Thanh Vân Sơn Cốc (30x25 tiles)
Bản đồ chia làm 3 khu vực rõ rệt để test cơ chế:
1. **Khu Trung Tâm — Bồ Đoàn Tụ Linh (An toàn):** Có 1 bồ đoàn phát sáng. Đứng lên đây và giữ phím đả tọa sẽ tăng tốc độ hấp thụ Tu Vi gấp 2 lần và hồi HP/MP nhanh.
2. **Khu Phía Đông — Rừng Linh Thảo:** Mọc ngẫu nhiên *Tụ Khí Thảo* (tối đa 5 cây trên map, tự mọc lại sau 10 giây). Chạm vào để thu thập, tự động cộng +15 Tu Vi và hồi 20 MP.
3. **Khu Phía Nam & Tây — Bãi Yêu Thú:** Khu vực đất sẫm màu, sinh ra tối đa 4 con *Hỏa Lang* (Sói Lửa). Tiêu diệt rơi *Yêu Đan* (+30 Tu Vi).

## 4. Điều khiển & Cơ chế cốt lõi
- **Di chuyển:** `W A S D` hoặc 4 phím mũi tên.
- **Tấn công (Ngự Kiếm Thuật):** `Chuột trái` hoặc phím `J` — phóng 1 thanh phi kiếm bay về hướng trỏ chuột (hoặc hướng đang nhìn). Tốn 5 MP, hồi chiêu 0.4s.
- **Đả Tọa Tu Luyện:** Nhấn/giữ phím `C` — nhân vật ngồi khoanh chân, hiện vòng hào quang xung quanh, không thể di chuyển, tăng dần Tu Vi (+5/giây thường, +10/giây trên Bồ Đoàn) và hồi HP/MP.
- **Đột Phá Cảnh Giới:** Khi thanh Tu Vi đạt 100%, nhấn phím `B` để đột phá lên cảnh giới tiếp theo (có hiệu ứng lóe sáng và tăng mạnh chỉ số).

## 5. Hệ thống Cảnh Giới (Bản Test)
1. **Phàm Nhân (Cấp 1):** 100 HP | 50 MP | Sát thương kiếm: 15 | Cần 100 Tu Vi để đột phá.
2. **Luyện Khí Kỳ (Cấp 2):** 200 HP | 120 MP | Sát thương kiếm: 35 | Cần 250 Tu Vi để đột phá (Hào quang xanh lục).
3. **Trúc Cơ Kỳ (Cấp 3):** 450 HP | 300 MP | Sát thương kiếm: 80 | Cảnh giới tối đa bản test (Hào quang vàng kim).
