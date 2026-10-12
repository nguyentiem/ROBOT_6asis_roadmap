BỔ SUNG CHO BÀI NÀY:
- Phiên bản mục tiêu: ROS 2 [Jazzy trên Ubuntu 24.04] (đổi thành Humble/Ubuntu 22.04 nếu bạn dùng bản đó). Ghi rõ phiên bản ở đầu bài; mọi lệnh phải đúng với phiên bản này.
- Mọi khối lệnh terminal có nút Copy, ghi rõ chạy ở terminal nào (Terminal 1, 2, 3), và có ô "Kết quả mong đợi".
- Nếu không chắc một tên package, tham số hoặc đường dẫn, KHÔNG bịa: ghi chú "kiểm tra bằng ros2 pkg list / ros2 <lệnh> -h hoặc tài liệu chính thức".
- Có mục "Khi gặp lỗi": bảng lỗi thường gặp → nguyên nhân → cách sửa.
- Có checklist cài đặt người học tick từng bước (không dùng localStorage, chỉ lưu trong bộ nhớ trang).
CHỦ ĐỀ: Bài 8.2 – Robot trong ROS 2: robot_state_publisher, TF2, ros2_control, RViz2

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Từ URDF đến robot trong ROS: URDF (bài 6.1) nạp vào tham số robot_description; robot_state_publisher đọc URDF và /joint_states rồi phát cây TF
- /joint_states (sensor_msgs/JointState: name, position, velocity, effort) là "góc khớp hiện tại"; joint_state_publisher và joint_state_publisher_gui để điều khiển giả bằng slider
- TF2: cây hệ tọa độ (frame) theo thời gian; /tf (động) và /tf_static (tĩnh); mỗi link là một frame; liên hệ bài 1.2 (T = ma trận 4x4) và bài 2.3 (fkine): tf2 là fkine tự động; tra cứu transform giữa hai frame bất kỳ (lookup_transform), tf2_echo, view_frames, tf2_ros.Buffer/TransformListener (Python), TransformBroadcaster/StaticTransformBroadcaster
- Quy ước REP-103/105: mét, radian, trục x tới trước, y trái, z lên; frame base_link, odom, map, tool0, ee_link; world
- RViz2: hiển thị RobotModel, TF, Marker, Fixed Frame; thêm display; lưu cấu hình .rviz; Interactive Marker (giới thiệu)
- Gói mô tả robot: ur_description, franka_description, moveit_resources_panda_description (kiểm tra tên bằng ros2 pkg list); cấu trúc package mô tả (urdf/, meshes/, launch/, rviz/, config/)
- Xacro: xacro file.xacro > file.urdf hoặc dùng trong launch; tham số hóa; liên hệ bài 6.1
- ros2_control: tầng trừu tượng giữa phần mềm và phần cứng; các thành phần: controller_manager, hardware interface (command/state interface: position, velocity, effort), controllers (joint_state_broadcaster, joint_trajectory_controller, position_controllers/forward_command_controller); file cấu hình YAML cho controllers; tag <ros2_control> trong URDF/xacro; mock hardware (mock_components/GenericSystem) để chạy không cần robot; hardware thật thay thế bằng plugin driver
- Ý nghĩa: cùng một controller dùng được cho mô phỏng (Gazebo gz_ros2_control) và robot thật; đổi phần cứng chỉ đổi plugin
- Action FollowJointTrajectory của joint_trajectory_controller: nhận quỹ đạo (các điểm q, q̇, thời gian — liên hệ bài 5.1/5.3) rồi thực hiện; ros2 control list_controllers/list_hardware_interfaces; spawner
- Hiển thị và đọc đầu công cụ: ros2 run tf2_ros tf2_echo base_link tool0 → T_base_tool (đối chiếu fkine ở bài 2.3)
- Mô phỏng trong Gazebo (giới thiệu): world, spawn robot, plugin; so sánh với PyBullet (bài 6.2)
- Quy trình đề xuất: (1) hiển thị URDF với joint_state_publisher_gui trong RViz; (2) thêm ros2_control + mock hardware; (3) gửi quỹ đạo tới joint_trajectory_controller; (4) (tùy chọn) Gazebo
- Mẹo debug: ros2 topic echo /joint_states, view_frames, check_urdf, kiểm tra QoS /tf, kiểm tra Fixed Frame trong RViz, thời gian mô phỏng use_sim_time

VÍ DỤ ROBOT: UR5 qua gói Universal Robots ROS 2 Description (ur_description; có launch view_ur.launch.py, kiểm tra tên/tham số theo phiên bản) và/hoặc robot Panda trong moveit_resources; robot 2 khâu tự viết từ bài 6.1 làm bài thực hành nhỏ. Không bịa nội dung dài của file; ghi chú tên package/launch phải được kiểm tra.

VÌ SAO CẦN HỌC: đây là bước đưa mô hình robot vào hệ sinh thái ROS; mọi bộ lập kế hoạch, thị giác, điều khiển đều dựa vào TF và controller; hiểu nó mới debug được khi robot "không chuyển động" hoặc "hiện sai chỗ".

LƯU Ý KỸ THUẬT: HTML không chạy ROS; mô phỏng hiệu ứng bằng JavaScript: robot (2 khâu 2D hoặc UR5 giả 3D bằng Canvas/three.js r128 từ cdnjs, tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry); cây TF vẽ bằng SVG; FK tự viết bằng JS. Mô phỏng RViz bằng khung nhìn có danh sách display bật/tắt.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Luồng dữ liệu: sơ đồ URDF → robot_description → robot_state_publisher ← /joint_states ← joint_state_publisher_gui → /tf → RViz; animation gói tin chạy theo luồng khi kéo slider; bấm từng khối để xem mô tả và lệnh tương ứng.
b) RViz thu nhỏ: robot giả 3D hoặc 2D, panel Displays với các mục (RobotModel, TF, Grid, Marker); bật/tắt từng mục thì hiển thị đổi; đổi Fixed Frame thì mọi thứ đổi gốc tọa độ; thông báo lỗi "Frame [x] does not exist" mô phỏng khi chọn frame sai.
c) joint_state_publisher_gui: slider cho từng khớp; hiển thị tin /joint_states dạng YAML cập nhật; robot di chuyển theo; liên hệ bài 2.3.
d) Cây TF: sơ đồ cây các frame (world → base_link → shoulder_link → ... → tool0); bấm hai frame bất kỳ thì hiện T giữa chúng (ma trận 4x4 tô màu R và p, tính bằng JS); animation đi dọc cây nhân dần ma trận (liên hệ bài 2.3a); ô nhập tương đương lệnh tf2_echo và kết quả.
e) TF tĩnh và động: so sánh /tf_static (frame cố định, ví dụ camera gắn trên bàn) và /tf (thay đổi theo khớp); thêm một frame mới (camera_link) bằng ô nhập (x, y, z, rpy) và thấy nó xuất hiện trên cây và RViz; ứng dụng chuyển điểm từ camera sang robot (nối bài 1.2, 8.4).
f) Tra cứu transform theo thời gian: thanh thời gian và bộ đệm TF (buffer); lookup_transform tại thời điểm chọn; lỗi "extrapolation" khi yêu cầu thời gian ngoài bộ đệm; giải thích vì sao TF cần timestamp.
g) ros2_control: sơ đồ phân lớp (controller → controller_manager → hardware interface → phần cứng/mô phỏng); animation lệnh vị trí đi xuống và trạng thái đi lên; công tắc chọn "Mock", "Gazebo", "Robot thật" thì lớp dưới đổi, lớp trên giữ nguyên; liệt kê command interface/state interface.
h) Controllers: danh sách controller với trạng thái (unconfigured, inactive, active); nút load/configure/activate giả lập; đọc cấu hình YAML (ví dụ update_rate, joint_trajectory_controller với danh sách joints, command_interfaces, state_interfaces); lỗi thường gặp khi tên khớp trong YAML không khớp URDF.
i) Gửi quỹ đạo: chọn 3-4 điểm quỹ đạo (bảng q, thời gian) rồi gửi action FollowJointTrajectory; robot chạy theo quỹ đạo nội suy (liên hệ bài 5.1/5.3); hiển thị feedback (sai số, thời gian) và kết quả (SUCCESSFUL/ABORTED); thử điểm vi phạm giới hạn khớp để thấy lỗi.
j) Xacro: đoạn xacro có property/macro; nút "Mở rộng thành URDF"; tham số hóa (ví dụ ur_type); liên hệ bài 6.1.
k) Gazebo vs PyBullet (bảng so sánh): tính năng, độ nặng, tích hợp ROS, độ chính xác; khi nào dùng cái nào.
l) Quy trình debug: flowchart "robot không hiện/không chuyển động" với các câu hỏi (robot_description có chưa? joint_states có không? TF có không? Fixed Frame đúng chưa? controller active chưa?); bấm theo nhánh để tới gợi ý sửa; hoặc mini-quiz tìm lỗi.
m) UR5 giả 3D (nếu đủ chỗ): robot 6 khớp, slider như joint_state_publisher_gui, hiển thị frame tool0 và T_base_tool; ô so sánh với kết quả fkine từ bài 2.3.

VÍ DỤ CÓ LỜI GIẢI: (1) chuỗi lệnh hiển thị UR5 trong RViz bằng launch của ur_description với joint_state_publisher_gui (ghi chú kiểm tra tên launch và tham số theo phiên bản; liệt kê kết quả mong đợi); (2) đọc T_base_tool bằng ros2 run tf2_ros tf2_echo và so với fkine ở bài 2.3 (giải thích đổi quaternion ↔ ma trận); (3) viết node Python dùng tf2_ros để lấy transform base_link→tool0 và in ra; (4) viết node phát frame tĩnh camera_link; (5) gửi quỹ đạo 3 điểm cho joint_trajectory_controller bằng ros2 action send_goal hoặc node Python.

CODE MẪU (thật, ghi rõ phiên bản): launch file Python hiển thị robot (robot_state_publisher với robot_description lấy từ xacro bằng Command và FindExecutable, joint_state_publisher_gui, rviz2 Node); lệnh kiểm tra: ros2 topic echo /joint_states, ros2 run tf2_tools view_frames, ros2 run tf2_ros tf2_echo A B, check_urdf; node Python lookup_transform với tf2_ros.Buffer và TransformListener (xử lý ngoại lệ); StaticTransformBroadcaster; file YAML ros2_controllers (controller_manager, joint_state_broadcaster, joint_trajectory_controller) và đoạn <ros2_control> mẫu với mock_components/GenericSystem (nhắc kiểm tra theo phiên bản và tên khớp); lệnh ros2 control list_controllers, list_hardware_interfaces; node Python gửi FollowJointTrajectory (rclpy.action.ActionClient); phần Gazebo ở mức giới thiệu (nhắc kiểm tra gz_ros2_control và phiên bản Gazebo tương thích). Không bịa tên package/launch; nếu không chắc ghi "kiểm tra bằng ros2 pkg list / ros2 launch -h".

LỖI THƯỜNG GẶP: Fixed Frame sai hoặc không tồn tại, thiếu joint_states (robot hiện sai tư thế hoặc nằm chồng), quên robot_description hoặc xacro lỗi, tên khớp trong YAML/controller không khớp URDF, thiếu dependency hoặc controller không active (cần spawner), nhầm tf và tf_static (QoS khác nhau), lookup_transform quá sớm (chưa có dữ liệu) hoặc sai timestamp, nhầm hướng transform (A→B so với B→A), use_sim_time không đồng bộ, mesh không tải được (đường dẫn package://), trộn quaternion (x, y, z, w) với (w, x, y, z), đơn vị mm/m, quên source workspace, dùng Gazebo cũ/mới lẫn lộn (Gazebo Classic so với Gazebo mới), mock hardware nhưng tưởng là robot thật, dùng robot thật mà không kiểm tra giới hạn tốc độ.

BÀI TẬP: (1) hiển thị UR5 trong RViz và kéo slider; (2) đọc T_base_tool bằng tf2_echo và so với fkine của bạn (bài 2.3); (3) viết node Python in vị trí đầu công cụ theo thời gian; (4) thêm frame camera_link tĩnh và hiển thị trong RViz; (5) cấu hình mock hardware + joint_trajectory_controller và gửi quỹ đạo; (6) (tùy chọn) chạy robot trong Gazebo.
TRẮC NGHIỆM (5 câu): vai trò robot_state_publisher, TF2 là gì và nối với fkine ra sao, khác biệt /tf và /tf_static, ros2_control giúp đổi mô phỏng–robot thật thế nào, action FollowJointTrajectory dùng để làm gì.