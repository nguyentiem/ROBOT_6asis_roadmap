CHỦ ĐỀ: Bài 1.2 – Ma trận biến đổi đồng nhất 4x4 (Homogeneous Transformation)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Vì sao cần ghép xoay và tịnh tiến vào một ma trận duy nhất
- Tọa độ đồng nhất: điểm (x, y, z) viết thành (x, y, z, 1)
- Cấu trúc T = [R p; 0 1]: R là ma trận xoay 3x3, p là vector tịnh tiến
- Ghép nhiều biến đổi bằng phép nhân ma trận (đọc từ trái sang phải theo chuỗi hệ tọa độ)
- Ký hiệu T_A^B (hoặc ^A T_B): biến đổi mô tả hệ B trong hệ A
- Chuyển điểm từ hệ tọa độ này sang hệ khác: ^A p = ^A T_B · ^B p
- Nghịch đảo T^-1 = [R^T  -R^T p; 0 1] (không cần tính nghịch đảo ma trận tổng quát)
- Phép tịnh tiến thuần và xoay thuần là hai trường hợp riêng

VÌ SAO CẦN HỌC: mỗi khâu của robot là một hệ tọa độ; vị trí đầu công cụ so với đế robot là tích của các ma trận 4x4 này; hiểu bài này là hiểu động học thuận.

LƯU Ý KỸ THUẬT: dùng three.js bản r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry), hoặc giả 3D bằng SVG nếu cần ổn định.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Tại sao cần 4x4: so sánh "xoay rồi tịnh tiến" viết bằng hai bước (p' = R·p + d) với cách dùng một ma trận 4x4; animation điểm di chuyển và hiển thị phép nhân ma trận 4x4 với vector (x, y, z, 1), tô màu khối R (xanh dương nhạt), khối p (cam đào), hàng cuối (xám).
b) Trình dựng T: các slider xoay (roll, pitch, yaw) và tịnh tiến (x, y, z); ma trận 4x4 hiển thị trực tiếp với các khối R và p tô màu; một hệ tọa độ con (frame) 3D chuyển động tương ứng so với hệ gốc; nút bật/tắt hiển thị trục.
c) Ghép biến đổi: ba hệ tọa độ {0}, {1}, {2} nối tiếp nhau (hệ 1 so với hệ 0, hệ 2 so với hệ 1); chỉnh từng T bằng slider; animation từng bước "hệ 0 → hệ 1 → hệ 2" với đường nối; hiển thị phép nhân T_0^2 = T_0^1 · T_1^2 và kết quả; có nút phát, dừng, làm lại.
d) Đổi hệ tọa độ của một điểm: một điểm P cố định trong không gian; hiển thị tọa độ của P trong hệ {0}, {1}, {2}; khi kéo hệ {1} hoặc {2}, tọa độ của P trong từng hệ thay đổi nhưng P không đổi; phép nhân ma trận bên cạnh cập nhật.
e) Nghịch đảo: hiển thị T và T^-1 cạnh nhau với công thức [R^T  -R^T p; 0 1]; animation "đi" và "về" (T rồi T^-1 đưa hệ về vị trí ban đầu); ô kiểm tra T·T^-1 = I.
f) Phòng thí nghiệm "cánh tay 2 khâu bằng ma trận 4x4": mô hình 3D hoặc 2D gồm đế, khâu 1, khâu 2; mỗi khâu có ma trận T riêng (xoay quanh Z theo θ, tịnh tiến dọc X theo độ dài khâu); slider θ1, θ2; hiển thị T_0^1, T_1^2, T_0^2 và tọa độ đầu cánh tay; so sánh với công thức lượng giác ở bài 0.2.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) viết T cho "xoay Rz(90°) rồi tịnh tiến (1, 2, 0)" và biến đổi điểm (1, 0, 0); (2) ghép hai biến đổi và tìm vị trí đầu cánh tay 2 khâu khi θ1 = 30°, θ2 = 45°, l1 = l2 = 1; (3) tính nghịch đảo của một T cụ thể và kiểm tra T·T^-1 = I.

CODE PYTHON MẪU (numpy): hàm transform(R, p) trả về ma trận 4x4; hàm inv_transform(T) dùng công thức R^T; hàm biến đổi điểm (thêm số 1 rồi nhân); ghép 3 hệ tọa độ; vẽ các hệ tọa độ bằng matplotlib 3D; kiểm tra bằng np.allclose với np.linalg.inv.

LỖI THƯỜNG GẶP: quên thêm số 1 cho tọa độ đồng nhất, nhầm thứ tự nhân khi ghép biến đổi, nhầm T_A^B với T_B^A, dùng np.linalg.inv thay vì công thức (chậm và tích lũy sai số), nhầm "xoay rồi tịnh tiến" với "tịnh tiến rồi xoay" (hai kết quả khác nhau), quên rằng vector hướng (không phải điểm) có thành phần cuối bằng 0.

BÀI TẬP: (1) viết hàm transform và inv_transform rồi kiểm tra; (2) biến đổi điểm qua chuỗi 3 hệ tọa độ; (3) cho vị trí một vật trong hệ camera và T từ camera đến robot, tính vị trí vật trong hệ robot.
TRẮC NGHIỆM (5 câu): cấu trúc của ma trận 4x4, kích thước kết quả khi nhân với vector điểm, ý nghĩa T_A^B, công thức nghịch đảo, thứ tự nhân khi ghép biến đổi.