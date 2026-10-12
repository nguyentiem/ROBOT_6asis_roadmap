CHỦ ĐỀ: Bài 1.1 – Hệ tọa độ và ma trận xoay 2D/3D

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Hệ tọa độ, hệ tọa độ tay phải (right-handed), trục X-Y-Z
- Ma trận xoay 2D và cách suy ra từ hình học (cột của ma trận = ảnh của các vector đơn vị)
- Ma trận xoay quanh trục 3D: Rx(θ), Ry(θ), Rz(θ)
- Tính chất: R^T = R^-1, det(R) = +1, các cột vuông góc và có độ dài 1 (ma trận trực chuẩn, SO(3))
- Hai cách đọc ma trận: xoay một điểm trong cùng hệ, hoặc đổi tọa độ của điểm sang hệ khác
- Ghép nhiều phép xoay: thứ tự nhân quan trọng (R1·R2 ≠ R2·R1)
- Xoay quanh trục cố định (nhân bên trái) khác xoay quanh trục động (nhân bên phải)

VÌ SAO CẦN HỌC: mỗi khớp quay của robot là một phép xoay quanh một trục; hướng của đầu công cụ là kết quả ghép 6 phép xoay/biến đổi nối tiếp nhau.

LƯU Ý KỸ THUẬT: dùng three.js bản r128 từ cdnjs để vẽ 3D (không dùng OrbitControls có sẵn, hãy tự viết điều khiển xoay góc nhìn bằng kéo chuột/chạm; không dùng THREE.CapsuleGeometry). Nếu không chắc, vẽ giả 3D bằng SVG/Canvas với phép chiếu tự tính.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Xoay 2D: hệ trục Oxy và một hình (ví dụ mũi tên hoặc chữ "F" để thấy rõ hướng); slider θ; hiển thị ma trận 2x2 thay đổi theo thời gian thực; tô màu hai cột của ma trận trùng với màu hai vector đơn vị mới (cột 1 = ảnh của e1, cột 2 = ảnh của e2).
b) Hệ tọa độ 3D: vẽ trục X đỏ, Y xanh lá, Z xanh dương; kéo chuột xoay góc nhìn; nút bật/tắt lưới nền; kiểm tra quy tắc bàn tay phải bằng hình bàn tay đơn giản hoặc hình mũi tên cong quanh trục Z.
c) Rx, Ry, Rz: ba slider độc lập; một vật 3D nhỏ (hình hộp có gắn trục, hoặc mô hình máy bay đơn giản) xoay theo; ma trận 3x3 cập nhật tức thì; tab chọn trục xoay; mũi tên cong cho biết chiều quay dương.
d) Kiểm tra tính chất ma trận xoay: hiển thị R^T·R (gần ma trận đơn vị), det(R), độ dài từng cột; thử nút "Làm hỏng ma trận" (sửa một phần tử) để thấy các tính chất bị vi phạm và vật bị méo.
e) Xoay quanh trục cố định và trục động: hai khung 3D cạnh nhau, cùng thực hiện chuỗi "xoay Rx 90° rồi Rz 90°" nhưng một bên theo trục cố định, một bên theo trục động; animation từng bước (nút Bước tiếp, Phát, Làm lại) để thấy hướng cuối khác nhau; hiện biểu thức nhân ma trận tương ứng (R = Rz·Rx và R = Rx·Rz).
f) Thứ tự xoay: chọn hai phép xoay A và B, bấm "A rồi B" và "B rồi A", thấy hai kết quả khác nhau và hiện hai ma trận so sánh; nút kiểm tra "AB = BA?" có đáp án đúng/sai.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) viết Rz(90°) và xoay điểm (1, 0, 0); (2) tính Rx(90°)·Rz(90°) và Rz(90°)·Rx(90°) cho điểm (1, 0, 0), so sánh; (3) kiểm tra R^T·R = I cho Rz(30°).

CODE PYTHON MẪU (numpy): hàm Rx(theta), Ry(theta), Rz(theta); hàm is_rotation(R) kiểm tra tính trực chuẩn và det = 1; xoay một điểm bằng R @ p; ghép xoay và so sánh thứ tự; vẽ hệ trục 3D bằng matplotlib.

LỖI THƯỜNG GẶP: nhầm radian và độ, nhầm chiều quay (dấu của sin), nhầm thứ tự nhân khi ghép, nhầm "xoay điểm" với "đổi hệ tọa độ" (khi đó dùng R^T), sai số số học làm det lệch 1 (cần np.allclose), nhầm trục cố định với trục động.

BÀI TẬP: (1) xoay điểm quanh từng trục và vẽ 3D; (2) viết hàm kiểm tra ma trận xoay hợp lệ; (3) chứng minh bằng code rằng thứ tự xoay Rx rồi Ry khác Ry rồi Rx.
TRẮC NGHIỆM (5 câu): tính chất của ma trận xoay, nghịch đảo của ma trận xoay, kết quả xoay một điểm đơn giản, ý nghĩa thứ tự nhân, khác biệt trục cố định và trục động.