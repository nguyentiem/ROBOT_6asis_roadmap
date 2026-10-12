CHỦ ĐỀ: Bài 1.3 – Các cách biểu diễn hướng (Euler, RPY, trục-góc, quaternion)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Hướng của vật cứng trong 3D có 3 bậc tự do, nhưng ma trận xoay dùng 9 số; vì sao cần các cách biểu diễn gọn hơn
- Góc Euler: các quy ước ZYX, ZYZ; ý nghĩa xoay lần lượt quanh trục động
- Roll-Pitch-Yaw (RPY): quy ước phổ biến trong robot và máy bay, cách đọc từng góc
- Gimbal lock: khi pitch = ±90° mất một bậc tự do, hậu quả với robot
- Trục-góc (axis-angle) và công thức Rodrigues
- Quaternion: cấu trúc (w, x, y, z), quaternion đơn vị, nhân quaternion, ưu điểm (không gimbal lock, nội suy mượt, gọn)
- Chuyển đổi qua lại giữa ma trận xoay, Euler/RPY, trục-góc, quaternion
- Nội suy hướng bằng slerp (giới thiệu)
- Hãng robot dùng quy ước khác nhau (KUKA ABC, Fanuc WPR, ABB quaternion, UR vector xoay)

VÌ SAO CẦN HỌC: khi lập trình robot bạn nhập hướng đầu công cụ bằng góc hoặc quaternion, không ai nhập 9 số ma trận; chọn sai quy ước là robot quay sai hướng.

LƯU Ý KỸ THUẬT: dùng three.js bản r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry), hoặc giả 3D bằng SVG. Hàm chuyển đổi và quaternion viết bằng JavaScript thuần để tính trực tiếp.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Euler/RPY trực quan: một vật 3D (máy bay đơn giản hoặc đầu công cụ robot có mũi chỉ hướng) với ba slider Roll, Pitch, Yaw; animation từng bước: xoay quanh trục thứ nhất, rồi thứ hai, rồi thứ ba (nút Bước tiếp/Phát), mỗi trục có màu riêng và mũi tên cong; hiển thị ma trận xoay tương ứng bên cạnh.
b) Chọn quy ước: dropdown chọn ZYX, ZYZ, XYZ; với cùng bộ ba góc, vật cho thấy hướng khác nhau; bảng so sánh hai quy ước đặt cạnh nhau.
c) Gimbal lock: mô hình ba vòng gimbal (ba vòng tròn lồng nhau vẽ bằng SVG hoặc 3D) khớp với ba góc Euler; kéo pitch tới ±90° thì hai vòng trùng trục, hiện cảnh báo "mất một bậc tự do"; thử thay đổi roll và yaw để thấy chúng cho cùng một chuyển động; so sánh với quaternion không bị lỗi này.
d) Trục-góc: chọn trục (vector đơn vị bằng slider hoặc ô nhập) và góc; vẽ trục là đường thẳng xuyên qua vật, vật xoay quanh trục đó (animation phát); hiển thị ma trận xoay Rodrigues tương ứng.
e) Quaternion: slider trục và góc → hiển thị (w, x, y, z) = (cos(θ/2), sin(θ/2)·n); hiển thị độ dài quaternion luôn = 1; nút nhân hai quaternion và cho thấy tương đương nhân hai ma trận xoay.
f) Bộ chuyển đổi tổng hợp: nhập hướng ở một dạng bất kỳ (ma trận, RPY, trục-góc, quaternion), tự tính ra tất cả các dạng còn lại và hiển thị đồng thời; vật 3D xoay theo; có nút "Ngẫu nhiên".
g) Slerp: hai hướng bắt đầu và kết thúc (kéo chọn), so sánh nội suy tuyến tính từng góc Euler (đường đi lạ, xoay thừa) với slerp (đường xoay ngắn nhất, đều); animation phát song song hai vật cạnh nhau.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) RPY = (0°, 0°, 90°) sang ma trận xoay; (2) quaternion của phép xoay 90° quanh trục Z; (3) tìm hai bộ Euler khác nhau cho cùng hướng tại gimbal lock (pitch = 90°).

CODE PYTHON MẪU: dùng numpy tự viết hàm rpy2r, r2rpy (xử lý trường hợp gimbal lock), axis_angle2r (Rodrigues), quat_from_axis_angle, quat_multiply, quat2r; sau đó đối chiếu bằng thư viện scipy.spatial.transform.Rotation hoặc spatialmath-python (SO3.RPY, SO3.Angvec, UnitQuaternion); ví dụ slerp.

LỖI THƯỜNG GẶP: nhầm quy ước thứ tự (XYZ so với ZYX, cố định so với động), nhầm độ và radian, quaternion (w, x, y, z) so với (x, y, z, w) giữa các thư viện (scipy dùng x, y, z, w), quaternion q và -q là cùng một hướng, quaternion không chuẩn hóa sau nhiều phép nhân, dùng Euler để nội suy hoặc để tính trung bình, bỏ qua gimbal lock khi chọn hướng đầu công cụ.

BÀI TẬP: (1) chuyển RPY sang ma trận xoay và ngược lại, kiểm tra vòng tròn; (2) tìm trường hợp gimbal lock bằng code và quan sát r2rpy; (3) so sánh nội suy Euler với slerp cho hai hướng bất kỳ.
TRẮC NGHIỆM (5 câu): số bậc tự do của hướng, khi nào xảy ra gimbal lock, ý nghĩa thành phần w của quaternion, quaternion q và -q, ưu điểm của quaternion so với Euler.