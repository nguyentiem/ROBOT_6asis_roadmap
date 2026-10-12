CHỦ ĐỀ: Bài 0.1 – Vector và ma trận cho robotics

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Vector là gì (điểm, hướng, độ dài), cộng/trừ vector
- Tích vô hướng (dot) và tích có hướng (cross), ý nghĩa hình học
- Ma trận, phép nhân ma trận, ma trận chuyển vị, ma trận đơn vị
- Định thức và ma trận nghịch đảo
- Giải hệ phương trình tuyến tính Ax = b
- Ma trận như một "phép biến đổi" (nền tảng cho phép xoay robot sau này)

VÌ SAO CẦN HỌC: nối với robot: vị trí đầu công cụ là vector, mỗi khớp là một phép biến đổi bằng ma trận, giải IK là giải hệ phương trình.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Vector 2D: kéo thả đầu mũi tên bằng chuột (và chạm trên điện thoại), hiện tọa độ, độ dài, góc. Animation cộng vector theo quy tắc "nối đuôi" và quy tắc hình bình hành, có nút phát.
b) Tích vô hướng: hai vector, slider góc giữa chúng, vẽ hình chiếu vector này lên vector kia, hiển thị a·b = |a||b|cosθ thay đổi theo thời gian thực; tô màu khi vuông góc (=0).
c) Tích có hướng: vẽ hình bình hành, diện tích = |a×b|, hướng theo quy tắc bàn tay phải (vẽ giả 3D bằng SVG, có slider xoay góc nhìn).
d) Nhân ma trận từng bước: nhập hai ma trận nhỏ (2x2 hoặc 3x3); animation tô sáng hàng của A và cột của B, hiện phép tính từng phần tử rồi điền kết quả vào ô tương ứng. Có nút "Bước tiếp" và "Tự chạy".
e) Ma trận 2x2 biến đổi lưới: ô nhập a, b, c, d; lưới tọa độ và hình vuông đơn vị biến dạng mượt theo ma trận; hiển thị định thức (diện tích mới) và cảnh báo khi det = 0 (hình bị "ép bẹp", không có nghịch đảo). Có các nút preset: xoay 30°, co giãn, phản chiếu, shear.
f) Giải hệ Ax = b: hệ 2 phương trình 2 ẩn vẽ thành hai đường thẳng, giao điểm là nghiệm; kéo hệ số để thấy trường hợp vô nghiệm (song song) và vô số nghiệm (trùng nhau).

VÍ DỤ TÍNH CÓ LỜI GIẢI: nhân hai ma trận 2x2 chi tiết; tìm nghịch đảo ma trận 2x2; giải hệ 2 ẩn bằng nghịch đảo.

CODE PYTHON MẪU (numpy): np.array, +, -, @ (khác với *), np.dot, np.cross, A.T, np.linalg.inv, np.linalg.det, np.linalg.solve.

LỖI THƯỜNG GẶP: nhầm * với @, nhân ma trận không giao hoán (AB ≠ BA), nhầm kích thước ma trận, lấy nghịch đảo ma trận suy biến, vector hàng và vector cột trong numpy.

BÀI TẬP: (1) viết hàm nhân ma trận bằng vòng lặp rồi so với numpy; (2) giải Ax = b bằng np.linalg.solve; (3) kiểm tra A @ inv(A) = I.
TRẮC NGHIỆM (5 câu): về dot/cross, tính chất nhân ma trận, ý nghĩa định thức bằng 0, kích thước kết quả phép nhân, khi nào hệ có nghiệm duy nhất.