BỔ SUNG CHO BÀI NÀY:
- Phiên bản mục tiêu: ROS 2 [Jazzy trên Ubuntu 24.04] (đổi thành Humble/Ubuntu 22.04 nếu bạn dùng bản đó). Ghi rõ phiên bản ở đầu bài; mọi lệnh phải đúng với phiên bản này.
- Mọi khối lệnh terminal có nút Copy, ghi rõ chạy ở terminal nào (Terminal 1, 2, 3), và có ô "Kết quả mong đợi".
- Nếu không chắc một tên package, tham số hoặc đường dẫn, KHÔNG bịa: ghi chú "kiểm tra bằng ros2 pkg list / ros2 <lệnh> -h hoặc tài liệu chính thức".
- Có mục "Khi gặp lỗi": bảng lỗi thường gặp → nguyên nhân → cách sửa.
- Có checklist cài đặt người học tick từng bước (không dùng localStorage, chỉ lưu trong bộ nhớ trang).
CHỦ ĐỀ: Bài 8.1 – Linux (Ubuntu) và ROS 2 cơ bản: workspace, package, node, topic, service, action, launch

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Linux đủ dùng cho ROS: terminal, cấu trúc thư mục (/home, ~, đường dẫn tuyệt đối/tương đối), lệnh cd, ls, pwd, mkdir, cp, mv, rm, cat, nano, grep, chmod; quyền file; biến môi trường; .bashrc; sudo và apt; chạy nhiều terminal; Ctrl+C
- Cách có Ubuntu: máy thật, dual boot, WSL2 trên Windows (hạn chế GUI/USB), máy ảo, Docker; khuyến nghị nên dùng
- Cài ROS 2 (theo tài liệu chính thức): thêm kho apt, cài ros-<distro>-desktop, source /opt/ros/<distro>/setup.bash, đặt vào .bashrc; kiểm tra bằng ros2 run demo_nodes_cpp talker và listener; colcon và rosdep (cài, init, update)
- Kiến trúc ROS 2: đồ thị tính toán (node nối với nhau); DDS làm middleware, ROS_DOMAIN_ID, không có roscore
- Node: một tiến trình làm một việc; đặt tên; ros2 node list/info
- Topic (publish/subscribe): kênh dữ liệu một chiều, nhiều–nhiều; kiểu tin nhắn (msg), ví dụ std_msgs/String, sensor_msgs/JointState, geometry_msgs/Twist; ros2 topic list/echo/info/hz/pub; QoS ở mức giới thiệu (reliable/best effort, depth)
- Service (request/response): gọi đồng bộ một lần, ví dụ bật tắt gripper; ros2 service list/call; file srv
- Action (mục tiêu dài hạn có phản hồi và hủy): ví dụ "đi tới tư thế" mất vài giây; goal – feedback – result; ros2 action list/send_goal
- Parameter: cấu hình node lúc chạy; ros2 param list/get/set
- Workspace và package: cấu trúc ~/ros2_ws/src, colcon build, source install/setup.bash; tạo package Python (ros2 pkg create --build-type ament_python) và C++ (ament_cmake); package.xml, setup.py/CMakeLists.txt; entry_points
- Viết node Python (rclpy): publisher, subscriber, timer, service server/client; vòng spin; logger
- Launch file (Python): chạy nhiều node cùng lúc, đổi tên, truyền tham số
- Công cụ: rqt_graph (xem đồ thị node/topic), rqt, ros2 bag (ghi/phát lại dữ liệu), ros2 doctor
- Liên hệ robot: topic /joint_states chứa góc khớp, topic lệnh khớp, action FollowJointTrajectory (bài 8.2–8.3)
- Mẹo: một chữ ký lỗi thường gặp (quên source, quên build, sai tên package, hai bản ROS lẫn nhau)

VÍ DỤ ROBOT/ỨNG DỤNG: tránh dùng robot thật ở bài này; dùng turtlesim (ros2 run turtlesim turtlesim_node, turtle_teleop_key) để thấy topic /turtle1/cmd_vel và /turtle1/pose, service /spawn, action /rotate_absolute; sau đó viết node Python publish JointState giả cho robot 2 khâu (nối bài 6.1/8.2).

VÌ SAO CẦN HỌC: ROS 2 là "hệ điều hành" của robot hiện đại; MoveIt, driver robot, camera, SLAM đều giao tiếp qua node/topic/service/action nên không biết cơ bản thì mọi bài sau đều mơ hồ.

LƯU Ý KỸ THUẬT: HTML không chạy được ROS; hãy làm minh họa bằng JavaScript mô phỏng hiệu ứng: đồ thị node/topic tự vẽ bằng SVG/Canvas, gói tin chạy trên mũi tên; terminal giả lập (ô nhập lệnh với vài lệnh ros2 có sẵn kết quả mẫu: ros2 node list, ros2 topic list, ros2 topic echo...), turtle 2D vẽ bằng Canvas. Nhấn mạnh: kết quả mẫu chỉ để minh họa, người học chạy lệnh thật trên máy.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Bản đồ Linux: cây thư mục (/, home, opt/ros, ...) bấm vào để mở; terminal giả lập cho cd, ls, pwd, mkdir với cây thư mục thật trong bộ nhớ; thử đúng/sai và nhận phản hồi; bảng lệnh cơ bản.
b) Cài ROS 2 từng bước: checklist có tick, mỗi bước có lệnh (Copy), kết quả mong đợi, và hộp "nếu lỗi"; thanh tiến độ; có nhánh cho Ubuntu thật, WSL2, Docker (tóm tắt khác biệt).
c) Đồ thị tính toán: các node là hình tròn, topic là hình chữ nhật; animation tin nhắn chạy từ publisher qua topic tới subscriber; thêm/bớt node, đổi tần số publish, thấy nhiều subscriber nhận cùng tin; bấm node hoặc topic để xem thông tin (như ros2 node info).
d) Topic, service, action so sánh: ba khung cạnh nhau với cùng một ví dụ (robot tới điểm): topic (phát liên tục, không biết kết quả), service (hỏi–đáp một lần, có thể chờ), action (gửi mục tiêu, nhận phản hồi liên tục, có thể hủy); animation dòng tin; bảng chọn khi nào dùng loại nào.
e) QoS: slider mất gói tin và chế độ reliable/best effort; subscriber nhận đủ hay thiếu; depth hàng đợi; giải thích vì sao camera dùng best effort còn lệnh quan trọng dùng reliable (mức giới thiệu).
f) Turtlesim mô phỏng: con rùa vẽ trên Canvas, điều khiển bằng phím mũi tên hoặc nút; hiển thị tin /cmd_vel gửi đi và /pose nhận về dưới dạng JSON/YAML; nút giả lập service spawn/clear; action rotate_absolute với thanh phản hồi.
g) Workspace và package: sơ đồ cây ~/ros2_ws/{src,build,install,log}; animation quy trình "tạo package → viết node → colcon build → source → ros2 run" với trạng thái từng thư mục; ô chọn Python/C++ thì hiện cấu trúc file tương ứng (package.xml, setup.py hoặc CMakeLists.txt); tô sáng chỗ khai báo entry_points.
h) Viết node từng dòng: đoạn code publisher/subscriber Python; bấm dòng thì sơ đồ hiển thị phần tương ứng (tạo node, tạo publisher, timer, callback, spin); animation vòng spin gọi callback theo timer.
i) Launch file: file launch hiển thị; bấm "Chạy" thì nhiều node xuất hiện trong đồ thị; sửa remap/tham số và thấy kết nối đổi.
j) Công cụ: giả lập rqt_graph, ros2 topic hz (hiển thị tần số), ros2 bag record/play (nút ghi, phát lại, thanh thời gian).
k) Tìm lỗi: mini-quiz "robot không nhận lệnh": 5 tình huống (quên source, sai tên topic, khác ROS_DOMAIN_ID, quên build, kiểu tin sai); người học chọn nguyên nhân, nhận giải thích.
l) JointState giả (nối sang bài 8.2): node publish /joint_states cho robot 2 khâu với slider; hiển thị tin nhắn và robot 2D xoay theo, như RViz thu nhỏ.

VÍ DỤ CÓ LỜI GIẢI: (1) chuỗi lệnh cài ROS 2 và kiểm tra bằng talker/listener, mô tả kết quả mong đợi; (2) điều khiển turtlesim bằng ros2 topic pub (đủ cú pháp YAML), gọi service spawn, gửi action; (3) tạo package Python my_pkg, viết publisher/subscriber hoàn chỉnh, build và chạy; (4) viết service server cộng hai số và client; (5) launch file chạy hai node.

CODE MẪU (thật, chạy được, ghi rõ phiên bản): các lệnh cài đặt theo docs.ros.org cho phiên bản mục tiêu (nhắc kiểm tra tài liệu chính thức vì có thể đổi); lệnh ros2 (node, topic, service, action, param, bag, doctor); publisher.py và subscriber.py dùng rclpy (class kế thừa Node, create_publisher, create_timer, create_subscription, rclpy.spin); service server/client; action client đơn giản (hoặc ví dụ dùng lệnh ros2 action send_goal); launch file Python (LaunchDescription, Node); setup.py entry_points; package.xml; colcon build --symlink-install. Phiên bản C++ ngắn (tham khảo). Không bịa package; nếu không chắc ghi "kiểm tra bằng ros2 pkg list".

LỖI THƯỜNG GẶP: quên source setup.bash ở terminal mới, quên colcon build sau khi sửa code (nếu không dùng --symlink-install), sai tên package/executable, thiếu entry_points nên ros2 run không thấy node, lẫn lộn hai bản ROS trong cùng terminal, khác ROS_DOMAIN_ID, quên rclpy.shutdown, dùng time.sleep chặn spin, lỗi định dạng YAML khi topic pub, hai node trùng tên, dùng WSL2 mà không cấu hình GUI, sai quyền file chạy script, quên cài dependencies bằng rosdep.

BÀI TẬP: (1) cài ROS 2 và chạy talker/listener; (2) điều khiển turtlesim bằng topic pub và vẽ hình vuông bằng node Python; (3) viết package có publisher/subscriber; (4) viết service cộng hai số; (5) viết launch file chạy 2 node có tham số; (6) ghi và phát lại dữ liệu bằng ros2 bag; (7) viết node publish /joint_states giả cho robot 2 khâu.
TRẮC NGHIỆM (5 câu): phân biệt node/topic/service/action, khi nào dùng service so với action, vai trò colcon build và source, ý nghĩa QoS reliable/best effort, nguyên nhân thường gặp khi ros2 run không tìm thấy package.