Giáo trình chi tiết: lập trình cánh tay robot 6 trục

Mỗi bài có phần cần nắm được (kiểm tra bản thân) và bài tập (làm bằng Python + numpy, sau đó đối chiếu với Robotics Toolbox).

Module 0: Nền tảng toán và lập trình (1-2 tuần)

Bài 0.1: Vector và ma trận

Cần nắm: cộng/trừ/nhân ma trận, ma trận chuyển vị, ma trận nghịch đảo, định thức, tích vô hướng, tích có hướng, hệ phương trình tuyến tính.
Bài tập: viết hàm nhân ma trận bằng numpy, giải hệ Ax = b.

Bài 0.2: Lượng giác và giải tích

Cần nắm: sin, cos, atan2 (khác atan ở chỗ nào), đạo hàm, đạo hàm riêng, quy tắc chuỗi.
Bài tập: tính đạo hàm riêng của hàm 2 biến, vẽ bằng matplotlib.

Bài 0.3: Python cho robotics

Cần nắm: numpy (array, slicing, @, np.linalg), matplotlib 2D/3D, hàm, class.
Bài tập: vẽ cánh tay phẳng 2 khâu bằng 2 góc cho trước.
Module 1: Biến đổi không gian (2 tuần)

Bài 1.1: Hệ tọa độ và ma trận xoay 2D/3D

Cần nắm: ma trận xoay Rx, Ry, Rz; tính chất R^T = R^-1 và det = +1; xoay quanh trục cố định và trục động cho kết quả khác nhau (thứ tự nhân ma trận).
Bài tập: xoay một điểm quanh các trục, kiểm tra tính chất ma trận xoay.

Bài 1.2: Ma trận biến đổi đồng nhất 4x4

Cần nắm: cấu trúc [R p; 0 1], ghép nhiều phép biến đổi bằng nhân ma trận, chuyển điểm từ hệ này sang hệ khác, nghịch đảo của T.
Bài tập: viết hàm transform(R, p), inv_transform(T), biến đổi điểm giữa 3 hệ tọa độ.

Bài 1.3: Các cách biểu diễn hướng

Cần nắm: góc Euler (ZYX, ZYZ), roll-pitch-yaw, gimbal lock, trục-góc, quaternion; chuyển đổi qua lại.
Bài tập: chuyển RPY sang ma trận xoay và ngược lại; tìm trường hợp gimbal lock.
Module 2: Động học thuận (2 tuần)

Bài 2.1: Khớp, khâu, bậc tự do

Cần nắm: khớp quay và khớp tịnh tiến, bậc tự do (DOF), không gian khớp và không gian làm việc (workspace), cấu hình robot 6 trục điển hình (cổ tay cầu).
Bài tập: xác định DOF và workspace của robot 2 khâu phẳng.

Bài 2.2: Quy ước Denavit-Hartenberg

Cần nắm: gắn hệ tọa độ lên từng khâu, 4 tham số (a, α, d, θ), phân biệt DH chuẩn và DH cải tiến (Craig), công thức ma trận biến đổi từng khớp.
Bài tập: lập bảng DH cho robot 2 khâu phẳng, rồi robot 3 trục, rồi UR5 hoặc Puma 560.

Bài 2.3: Tính và kiểm chứng động học thuận

Cần nắm: T_0^6 = T_0^1 · T_1^2 · ... · T_5^6, đọc vị trí và hướng từ ma trận kết quả.
Bài tập: tự viết fkine(q) cho robot 6 trục, so sánh kết quả với rtb; vẽ robot 3D từ bảng DH.
Module 3: Động học nghịch (3 tuần)

Bài 3.1: Bản chất bài toán IK

Cần nắm: tại sao có nhiều nghiệm (elbow up/down, wrist flip), có thể vô nghiệm (ngoài workspace), phân biệt nghiệm giải tích và nghiệm số.
Bài tập: giải IK 2 khâu phẳng bằng hình học, tìm cả 2 nghiệm.

Bài 3.2: IK giải tích cho robot cổ tay cầu

Cần nắm: tách bài toán thành vị trí (3 khớp đầu) và hướng (3 khớp cổ tay), tìm tâm cổ tay, giải θ1, θ2, θ3 rồi θ4, θ5, θ6.
Bài tập: giải IK cho robot 3 trục (RRR) ở không gian 3D, sau đó mở rộng lên 6 trục.

Bài 3.3: IK số (Newton-Raphson, Jacobian pseudo-inverse)

Cần nắm: lặp q ← q + J⁺·e, chọn bước lặp, điều kiện dừng, hội tụ cục bộ nên phụ thuộc giá trị khởi tạo.
Bài tập: cài IK số bằng vòng lặp; so sánh với robot.ikine_LM().

Bài 3.4: Chọn nghiệm và giới hạn khớp

Cần nắm: chọn nghiệm gần cấu hình hiện tại nhất, kiểm tra giới hạn góc khớp, tránh nhảy cấu hình đột ngột.
Bài tập: viết hàm chọn nghiệm tối ưu trong danh sách nghiệm IK.
Module 4: Jacobian và điểm kỳ dị (2 tuần)

Bài 4.1: Jacobian vận tốc

Cần nắm: v = J(q)·q̇, cách dựng Jacobian từ bảng DH (cột cho khớp quay và tịnh tiến), Jacobian hình học so với Jacobian giải tích.
Bài tập: tính J cho robot phẳng 2 khâu bằng tay, rồi bằng code; kiểm tra bằng sai phân số.

Bài 4.2: Singularity

Cần nắm: điểm kỳ dị là khi det(J) = 0 (hoặc J mất hạng), các loại: biên workspace, thẳng tay, cổ tay thẳng hàng; hậu quả là vận tốc khớp tiến tới vô hạn.
Bài tập: vẽ độ đo khả năng thao tác (manipulability) w = √det(JJᵀ) trên workspace.

Bài 4.3: Điều khiển vận tốc và damped least squares

Cần nắm: q̇ = J⁺·v, phương pháp bình phương tối thiểu có giảm chấn để vượt gần singularity.
Bài tập: cho đầu công cụ đi theo đường thẳng và đường tròn bằng điều khiển vận tốc.

Bài 4.4: Quan hệ lực và moment

Cần nắm: τ = Jᵀ·F (nguyên lý công ảo).
Bài tập: tính moment khớp cần thiết để đầu công cụ giữ một lực cho trước.
Module 5: Quy hoạch quỹ đạo (2 tuần)

Bài 5.1: Quỹ đạo trong không gian khớp

Cần nắm: nội suy đa thức bậc 3 (cho vị trí và vận tốc đầu cuối) và bậc 5 (thêm gia tốc), hình thang vận tốc (trapezoidal), ràng buộc vận tốc và gia tốc tối đa.
Bài tập: tạo quỹ đạo từ điểm A đến B cho 6 khớp, vẽ đồ thị q(t), q̇(t), q̈(t).

Bài 5.2: Quỹ đạo trong không gian Descartes

Cần nắm: nội suy đường thẳng (vị trí) và slerp (hướng), nội suy cung tròn, khác biệt giữa lệnh kiểu PTP/MoveJ và LIN/MoveL.
Bài tập: robot đi theo đường thẳng, giải IK tại từng điểm, kiểm tra không vượt giới hạn khớp.

Bài 5.3: Nhiều điểm trung gian và làm trơn góc (blending)

Cần nắm: spline bậc 3, đi qua nhiều waypoint, làm tròn góc để không dừng tại mỗi điểm.
Bài tập: lập quỹ đạo qua 5 waypoint, so sánh có và không có blending.
Module 6: Mô phỏng vật lý (3 tuần)

Bài 6.1: Mô hình robot URDF

Cần nắm: link, joint, visual và collision geometry, giới hạn khớp, khối lượng và quán tính, cấu trúc file URDF.
Bài tập: đọc URDF của UR5 hoặc Franka Panda, đếm khớp, chỉnh một thông số.

Bài 6.2: PyBullet cơ bản

Cần nắm: nạp robot, đọc và điều khiển khớp (position/velocity/torque control), bước mô phỏng, camera, tạo vật thể.
Bài tập: robot đưa đầu công cụ tới một điểm bằng calculateInverseKinematics.

Bài 6.3: Gắp và đặt vật (pick and place)

Cần nắm: máy trạng thái (tiếp cận → hạ → kẹp → nâng → di chuyển → thả), điều khiển gripper, phát hiện va chạm/tiếp xúc.
Bài tập: gắp một khối lập phương từ A đặt sang B.

Bài 6.4: Tránh va chạm cơ bản

Cần nắm: kiểm tra va chạm giữa các hình, khoảng cách tối thiểu, ý tưởng của RRT và PRM.
Bài tập: robot đi vòng qua một chướng ngại vật.
Module 7: Động lực học và điều khiển (3-4 tuần)

Bài 7.1: Động lực học

Cần nắm: phương trình M(q)q̈ + C(q,q̇)q̇ + g(q) = τ; ý nghĩa của ma trận quán tính, Coriolis, trọng lực; động lực học thuận và nghịch.
Bài tập: tính moment bù trọng lực cho robot 2 khâu, rồi cho robot 6 trục bằng rtb.

Bài 7.2: Điều khiển PID

Cần nắm: vai trò P, I, D, chỉnh thông số, hiện tượng bão hòa tích phân (windup), PID độc lập từng khớp.
Bài tập: cài PID cho 1 khớp trong mô phỏng, vẽ đáp ứng bước.

Bài 7.3: Điều khiển nâng cao

Cần nắm: điều khiển moment tính trước (computed torque), bù trọng lực, điều khiển trở kháng/tuân thủ (impedance/compliance), điều khiển lực.
Bài tập: cho robot chạm nhẹ lên bề mặt với lực mong muốn.
Module 8: ROS 2 và MoveIt 2 (4-6 tuần)

Bài 8.1: Linux và ROS 2 cơ bản

Cần nắm: terminal Ubuntu, workspace, package, node, topic, service, action, launch file.
Bài tập: viết 2 node publisher/subscriber bằng Python.

Bài 8.2: Robot trong ROS 2

Cần nắm: robot_state_publisher, TF2 (cây hệ tọa độ), ros2_control, RViz2.
Bài tập: hiển thị UR5 trong RViz, đọc transform đầu công cụ.

Bài 8.3: MoveIt 2

Cần nắm: Setup Assistant, planning group, bộ giải IK (KDL, TRAC-IK), bộ lập kế hoạch (OMPL), planning scene.
Bài tập: lập kế hoạch và thực thi chuyển động có tránh vật cản trong Gazebo.

Bài 8.4: Thị giác máy tính tích hợp

Cần nắm: hiệu chuẩn camera, hiệu chuẩn tay-mắt (hand-eye calibration), chuyển tọa độ ảnh sang tọa độ robot.
Bài tập: nhận diện vật bằng màu/ArUco rồi cho robot gắp.
Module 9: Robot thật và an toàn (song song / sau cùng)

Bài 9.1: Giao tiếp phần cứng

Cần nắm: giao thức (Modbus, TCP/IP, EtherCAT, CAN), API của hãng, điều khiển tốc độ chậm khi thử.
Bài tập: điều khiển robot mini qua API Python.

Bài 9.2: Hiệu chuẩn và an toàn

Cần nắm: hiệu chuẩn điểm gốc (homing), TCP (Tool Center Point), giới hạn vùng làm việc, nút dừng khẩn cấp, quy tắc làm việc gần robot.
Bài tập: xác định TCP của một công cụ gắn lên robot.