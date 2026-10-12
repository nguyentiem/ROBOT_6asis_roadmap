CHỦ ĐỀ: Bài 2.1 – Khớp, khâu, bậc tự do, không gian làm việc

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Khâu (link) và khớp (joint); chuỗi động học hở (serial) của cánh tay robot
- Các loại khớp: khớp quay (revolute, R), khớp tịnh tiến (prismatic, P), và giới thiệu khớp cầu, khớp trụ
- Bậc tự do (DOF): vật cứng tự do trong 3D có 6 DOF; công thức Grübler cho cơ cấu phẳng và không gian (giới thiệu)
- Vì sao robot cần ít nhất 6 DOF để tới mọi vị trí và hướng trong workspace; robot 7 DOF dư thừa (redundant)
- Không gian khớp (joint space) và không gian làm việc (task space / Cartesian space)
- Không gian làm việc (workspace): tầm với (reachable) và tầm khéo léo (dexterous)
- Cấu hình điển hình của robot 6 trục: articulated (RRR + cổ tay), SCARA, Delta, Cartesian (PPP); ký hiệu cấu hình như RRR-RRR
- Cổ tay cầu (spherical wrist): 3 trục cổ tay cắt nhau tại một điểm; vì sao thiết kế này giúp giải IK dễ
- Tên gọi các trục robot công nghiệp: J1 (base), J2 (shoulder), J3 (elbow), J4-J5-J6 (wrist)

VÌ SAO CẦN HỌC: trước khi viết bất kỳ dòng code nào, phải biết robot có bao nhiêu khớp, loại gì, nối thế nào, và nó với tới được đâu.

LƯU Ý KỸ THUẬT: minh họa chính vẽ 2D bằng SVG/Canvas; phần cánh tay 6 trục dùng giả 3D bằng Canvas (tự viết phép chiếu) hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry).

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Phân loại khớp: ba khối hoạt hình: khớp quay (hai thanh xoay quanh chốt, có slider góc), khớp tịnh tiến (thanh trượt, có slider độ dài), khớp cầu (đầu thanh quay tự do, kéo chuột); mỗi khối ghi rõ ký hiệu, số DOF, và ví dụ thực tế trên robot.
b) Đếm bậc tự do: vật cứng trong mặt phẳng (3 DOF: x, y, φ) và trong không gian (6 DOF: 3 tịnh tiến + 3 xoay), kéo/xoay vật và hiển thị từng tham số; sau đó thêm các ràng buộc (khớp) và xem DOF giảm đi; ô tính Grübler tự cập nhật.
c) Không gian khớp và không gian làm việc: cánh tay phẳng 2 khâu; hai slider θ1, θ2 ở bên trái (không gian khớp, vẽ ô vuông θ1-θ2 với một điểm di chuyển); bên phải là cánh tay và điểm đầu công cụ (không gian làm việc); kéo điểm ở một bên thì bên kia tự cập nhật; nút "Vẽ vết quỹ đạo".
d) Workspace: cánh tay 2 khâu, tô vùng với tới được bằng cách quét θ1, θ2 (animation chấm dần ra vùng hình vành khăn); slider độ dài l1, l2 và giới hạn góc khớp, vùng workspace đổi theo thời gian thực; hiện bán kính trong |l1-l2| và ngoài l1+l2; đánh dấu điểm ngoài tầm với bằng màu cam đào.
e) Robot 2 khâu với giới hạn khớp: thêm giới hạn góc (ví dụ θ2 từ -150° đến 150°), vùng workspace bị khuyết; kéo điểm đích vào vùng khuyết sẽ có cảnh báo.
f) Cấu hình robot: bốn thẻ chọn (Articulated, SCARA, Delta, Cartesian), mỗi thẻ có hình đơn giản, ký hiệu khớp (ví dụ RRR, RRP, PPP), hình dạng workspace (cầu rỗng, hình trụ, hình hộp) và ứng dụng điển hình.
g) Robot 6 trục giả 3D: sáu slider J1-J6, mỗi khớp có màu riêng và nhãn; nút bật/tắt hiển thị trục quay của từng khớp (mũi tên cong); nút highlight "3 khớp đầu định vị trí, 3 khớp cổ tay định hướng"; cho thấy ba trục cổ tay giao nhau tại một điểm.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) đếm DOF của robot 2 khâu phẳng bằng Grübler; (2) xác định workspace của robot 2 khâu với l1 = 2, l2 = 1 (bán kính trong 1, ngoài 3); (3) phân tích robot 6 trục dạng RRR-RRR: khớp nào quay quanh trục nào.

CODE PYTHON MẪU: numpy tính workspace bằng cách quét θ1, θ2 rồi matplotlib vẽ scatter; hàm kiểm tra điểm có trong tầm với (|l1-l2| ≤ r ≤ l1+l2); bước đầu khai báo robot bằng roboticstoolbox: rtb.models.DH.Planar2() hoặc rtb.DHRobot với hai khớp quay.

LỖI THƯỜNG GẶP: nhầm DOF của robot với số motor hoặc số khâu, quên khâu đế cố định khi đếm bậc tự do, nhầm không gian khớp với không gian làm việc, nghĩ rằng 6 DOF luôn với tới mọi điểm theo mọi hướng (bị giới hạn khớp và tự va chạm), nhầm tầm với và tầm khéo léo.

BÀI TẬP: (1) xác định DOF và vẽ workspace của robot 2 khâu phẳng với giới hạn khớp; (2) vẽ workspace của robot 3 khâu phẳng; (3) liệt kê loại khớp và trục quay của một robot 6 trục thật (ví dụ UR5) từ ảnh/datasheet.
TRẮC NGHIỆM (5 câu): số DOF của vật cứng trong không gian, loại khớp của một chi tiết cụ thể, hình dạng workspace của robot 2 khâu, vì sao cổ tay cầu giúp giải IK, phân biệt joint space và task space.