Bạn là giảng viên robotics kiêm lập trình viên front-end giỏi. Hãy tạo MỘT FILE HTML DUY NHẤT (HTML + CSS + JS chung một file, mở trực tiếp bằng trình duyệt là chạy) làm bài giảng tương tác cho người mới bắt đầu, biết Python cơ bản, chưa học robotics. Ngôn ngữ: tiếng Việt, giữ thuật ngữ tiếng Anh trong ngoặc, ví dụ: động học thuận (Forward Kinematics).

BỐ CỤC
- Mục lục cố định bên trái (điện thoại thu thành nút ☰), bấm là cuộn mượt, tự đánh dấu mục đang đọc. Thanh tiến độ đọc mỏng trên cùng.
- Mỗi mục là một thẻ (card) bo góc 12-16px, có số thứ tự. Khi cuộn, thẻ hiện ra bằng fade-in + trượt lên nhẹ.
- Thứ tự các mục: (1) Mục tiêu bài học, (2) Vì sao cần học (gắn với robot), (3) Khái niệm cốt lõi: từ trực giác đến công thức, (4) Minh họa tương tác, (5) Ví dụ có lời giải từng bước, (6) Code Python mẫu có nút Copy, (7) Lỗi thường gặp, (8) Tóm tắt, (9) Bài tập tự luyện + 5 câu trắc nghiệm chấm điểm tại chỗ kèm giải thích.
- Hộp chú thích có màu và icon riêng: 💡 Ý chính, ⚠️ Cẩn thận, 📝 Ví dụ, 🧪 Thử nghiệm.
- Công thức dùng KaTeX; tô cú pháp code dùng highlight.js. Chỉ dùng CDN cdnjs.cloudflare.com, ghi rõ phiên bản.

HÌNH ẢNH VÀ ANIMATION
- Không dùng ảnh ngoài. Tự vẽ toàn bộ bằng SVG hoặc Canvas, có mũi tên, nhãn, màu phân biệt rõ.
- Mỗi minh họa tương tác có: slider hoặc ô nhập, nút ▶ Phát / ⏸ Dừng / ⏮ Làm lại, thanh tốc độ (nếu có animation), và bảng số liệu cập nhật thời gian thực.
- Animation mượt (requestAnimationFrame hoặc CSS transition), chậm rãi, không nhấp nháy.

MÀU SẮC (pastel, sáng, nhẹ)
:root { --bg:#F7F9FC; --card:#FFFFFF; --border:#E6EBF2; --primary:#6C9BCF; --mint:#7FCDBB; --peach:#F4B183; --lavender:#B8A9E8; --text:#2F3A4A; --muted:#6B778C; }
- Font Google Fonts "Be Vietnam Pro", chữ thân bài ≥ 16px, giãn dòng 1.7, nhiều khoảng trắng, đổ bóng rất nhẹ.
- Quy ước màu trục tọa độ: X đỏ nhạt, Y xanh lá, Z xanh dương.

KỸ THUẬT
- Responsive. Không dùng localStorage/sessionStorage. Code có chú thích tiếng Việt, tách rõ <style>, HTML, <script>.
- Tự kiểm tra: mọi slider, nút, animation phải chạy, không có lỗi console.
- Trước khi dùng KaTeX/highlight.js, kiểm tra URL CSS và JS trên cdnjs trả HTTP 200; không dùng phiên bản chỉ đoán theo số phát hành. KaTeX `0.16.22` trả 404; `0.18.0` đã được xác minh ngày 2026-10-12.
- Mở file HTML bằng `file://` trong trình duyệt thật để kiểm tra công thức: KaTeX đã tải, tất cả phần tử công thức đều được render, không có `.katex-error`, và không có mã LaTeX thô trên trang. Kiểm tra trực quan công thức dài/ma trận và không có lỗi JavaScript trong console.
- Nếu KaTeX không tải được, nội dung dự phòng phải dễ đọc. Khi lưu LaTeX trong thuộc tính HTML, escape đúng các ký tự đặc biệt như `&`.

CÁCH TRẢ LỜI
- Trả TOÀN BỘ code trong MỘT khối code duy nhất, không cắt bớt, không viết "phần còn lại tương tự".
- Nếu dài, ưu tiên: (1) minh họa tương tác chạy đúng, (2) giải thích rõ, (3) trang trí.
- Cuối cùng ghi 3 dòng hướng dẫn mở file và chỉnh màu/nội dung.

NỘI DUNG BÀI HỌC CỤ THỂ:
