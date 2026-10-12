CHỦ ĐỀ: Bài 6.1 – Mô hình robot URDF (Unified Robot Description Format)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- URDF là gì: file XML mô tả robot (khâu, khớp, hình dạng, khối lượng) để mô phỏng, hiển thị và lập kế hoạch; dùng trong ROS, PyBullet, Gazebo, MuJoCo (qua chuyển đổi)
- Hai thành phần chính: <link> (khâu) và <joint> (khớp); robot là một cây (tree) với một link gốc (base_link)
- Link gồm 3 phần: <visual> (hình hiển thị), <collision> (hình dùng tính va chạm, thường đơn giản hơn), <inertial> (khối lượng, khối tâm, ma trận quán tính)
- Joint: tên, loại (revolute, continuous, prismatic, fixed, floating, planar), <parent>, <child>, <origin xyz rpy> (vị trí/hướng của khớp so với link cha), <axis xyz>, <limit lower upper effort velocity>
- Hình học: box, cylinder, sphere, mesh (STL/DAE/OBJ); đơn vị mét, radian, kilogam
- Liên hệ với bài 2.2: URDF dùng origin + axis thay vì bảng DH; cách chuyển giữa hai cách mô tả (mỗi joint = một biến đổi T cố định từ origin, rồi quay quanh axis)
- Cách đọc cây: từ base_link đi qua từng joint tới tool0 / end-effector link; hệ tọa độ của link = hệ tọa độ của joint con
- Giới hạn khớp: lower/upper (rad), velocity (rad/s), effort (N·m); khớp continuous không giới hạn góc
- Xacro (giới thiệu): macro, biến, tham số hóa để URDF gọn hơn
- Lỗi hay gặp khi tự viết URDF: quên inertial, sai đơn vị, khối lượng bằng 0, collision lệch visual, đường dẫn mesh sai
- Công cụ kiểm tra: check_urdf, urdf_to_graphiz, hiển thị bằng PyBullet / RViz / urdf-viz

VÍ DỤ ROBOT: robot 2 khâu phẳng tự viết (đầy đủ URDF ngắn); URDF của UR5 hoặc Franka Panda (pybullet_data có sẵn "franka_panda/panda.urdf"; kuka_iiwa/model.urdf) ở mức đọc và đếm khớp. Hãy ghi chú tên file/đường dẫn, không bịa nội dung dài của file thật.

VÌ SAO CẦN HỌC: mọi simulator và MoveIt đều cần mô hình robot; đọc được URDF là biết robot có bao nhiêu khớp, giới hạn bao nhiêu, và tại sao mô phỏng chạy lạ (sai khối lượng, sai trục).

LƯU Ý KỸ THUẬT: HTML không chạy được PyBullet; hãy hiển thị code Python/URDF để người học copy. Tự viết bộ phân tích URDF đơn giản bằng JavaScript (DOMParser) để người học dán URDF vào ô nhập và thấy cây link-joint, bảng khớp và robot vẽ 2D/giả 3D. Dùng giả 3D bằng Canvas hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry). Dùng hình hộp/trụ đơn giản để vẽ robot từ URDF.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Giải phẫu URDF: file URDF robot 2 khâu hiển thị bên trái (tô màu theo thẻ: link xanh dương, joint cam, visual/collision/inertial các màu nhạt khác nhau); bên phải là robot; rê chuột lên một đoạn XML thì phần tương ứng trên robot sáng lên (và ngược lại).
b) Cây link–joint: sơ đồ cây (base_link → joint1 → link1 → joint2 → link2 → tool) tự vẽ từ URDF; bấm vào một nút thì hiện thuộc tính (loại khớp, trục, giới hạn); animation đi theo cây "từ gốc ra ngọn" cho thấy mỗi joint đặt hệ tọa độ mới (liên hệ bài 1.2: nhân ma trận T cố định rồi quay).
c) Origin và axis: một khớp quay đơn; slider xyz, rpy của <origin> và vector <axis>; hệ tọa độ con và trục quay hiển thị 3D; khi quay θ, link con xoay quanh axis đúng; hiện ma trận T = T_origin · Rot(axis, θ).
d) Visual, collision, inertial: một link; nút bật/tắt từng lớp; visual là hình mesh đẹp (giả lập bằng nhiều khối), collision là hình hộp/trụ bao đơn giản (vẽ khung dây), inertial là khối tâm (chấm) và ellipsoid quán tính; hiển thị vì sao collision nên đơn giản (tính nhanh); thay đổi khối lượng thì ellipsoid đổi.
e) Trình xem URDF thu nhỏ: ô dán URDF (có sẵn 3 ví dụ: robot 2 khâu, robot 3 trục, robot có khớp tịnh tiến); nút "Phân tích" tạo bảng (tên khớp, loại, cha → con, trục, giới hạn) + robot hiển thị + slider cho từng khớp không cố định; báo lỗi dễ hiểu nếu XML sai hoặc thiếu thẻ.
f) Giới hạn khớp: thanh trượt cho từng khớp bị chặn theo <limit>; thử kéo quá giới hạn thì khớp dừng và đổi màu; so sánh revolute với continuous; hiển thị vùng cho phép trên vòng tròn góc.
g) DH ↔ URDF: robot 2 khâu: bảng DH bên trái, URDF bên phải; sửa một thông số DH thì URDF origin tương ứng đổi (hoặc ngược lại); thấy cả hai cho ra cùng robot; ghi chú hạn chế (URDF không ép trục z theo DH).
h) Lỗi URDF thường gặp: mini-quiz "tìm lỗi": 4 đoạn URDF có lỗi (quên inertial, sai đơn vị mm thành m, trục sai, khối lượng 0); người học bấm vào dòng sai; robot mô phỏng hiện hậu quả (đổ, vặn lạ, bay lơ lửng) bằng animation đơn giản.
i) Xacro (nếu đủ chỗ): đoạn xacro có <xacro:property> và <xacro:macro>; nút "Mở rộng" hiện URDF đầy đủ sau khi thay biến; so sánh độ dài hai bản.

VÍ DỤ CÓ LỜI GIẢI: (1) viết URDF đầy đủ cho robot 2 khâu phẳng (base_link, link1, link2, hai joint revolute quanh z, limit, inertial hợp lý; giải thích từng thẻ); (2) đọc URDF Franka Panda (mô tả cấu trúc: 7 khớp quay + 2 ngón tay tịnh tiến, link gốc, link cuối) và đếm khớp điều khiển được; (3) tính vị trí đầu công cụ của robot 2 khâu bằng cách nhân các T từ origin của URDF, đối chiếu với FK bằng DH ở bài 2.3.

CODE PYTHON MẪU (chạy được): đọc URDF bằng xml.etree.ElementTree: liệt kê link, joint, giới hạn; nạp robot bằng PyBullet (pybullet.connect(p.GUI); p.loadURDF(...)); dùng pybullet_data.getDataPath() để lấy robot có sẵn; p.getNumJoints, p.getJointInfo (in tên, loại, giới hạn, trục); chỉnh một thông số (ví dụ đổi khối lượng bằng p.changeDynamics, hoặc sửa file URDF rồi nạp lại); đối chiếu với thư viện: urdfpy hoặc yourdfpy, roboticstoolbox: rtb.Robot.URDF(...) (ghi chú: xem phiên bản đang cài); kiểm tra bằng check_urdf (ROS) nếu có.

LỖI THƯỜNG GẶP: nhầm đơn vị mm/m, quên <inertial> hoặc khối lượng 0 (robot đổ/biến mất khi mô phỏng), collision quá phức tạp làm mô phỏng chậm, đường dẫn mesh sai (hiện robot không có hình), nhầm trục quay trong <axis> (xoay sai chiều), origin cha/con nhầm hệ, quên rằng joint origin tính theo hệ link cha, thiếu limit cho revolute (lỗi khi nạp), nhầm joint "fixed" với "revolute", dùng pybullet_data sai tên file.

BÀI TẬP: (1) đọc URDF của UR5 hoặc Franka Panda, đếm khớp, in giới hạn từng khớp; (2) viết URDF robot 2 khâu của riêng bạn, nạp vào PyBullet và quay thử bằng slider (p.addUserDebugParameter); (3) chỉnh một thông số (chiều dài khâu hoặc khối lượng), quan sát sự khác biệt; (4) so vị trí đầu công cụ trong PyBullet (getLinkState) với FK bài 2.3.
TRẮC NGHIỆM (5 câu): ý nghĩa link/joint, vai trò của collision so với visual, ý nghĩa origin và axis, đơn vị trong URDF, hậu quả khi quên inertial.