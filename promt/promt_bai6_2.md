CHỦ ĐỀ: Bài 6.2 – PyBullet cơ bản: nạp robot, điều khiển khớp, bước mô phỏng

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- PyBullet là gì: thư viện mô phỏng vật lý (engine Bullet) cho Python, nhẹ, miễn phí, cài bằng pip install pybullet
- Vòng đời một chương trình: connect (GUI/DIRECT) → setAdditionalSearchPath → setGravity → loadURDF → vòng lặp stepSimulation → disconnect
- Bước mô phỏng: setTimeStep (mặc định 1/240 s), stepSimulation tiến một bước; chế độ thời gian thực (setRealTimeSimulation) so với tự bước; time.sleep để khớp thời gian thực
- Nạp vật thể: p.loadURDF (robot, bàn, plane.urdf), tạo hình cơ bản bằng createCollisionShape + createVisualShape + createMultiBody (hộp, cầu); đặt vị trí và hướng (quaternion – liên hệ bài 1.3)
- Thông tin khớp: getNumJoints, getJointInfo (chỉ số, tên, loại, giới hạn, trục), getJointState (vị trí, vận tốc, lực), phân biệt khớp điều khiển được và khớp fixed
- Ba chế độ điều khiển khớp qua setJointMotorControl2/setJointMotorControlArray: POSITION_CONTROL (vị trí + PD tích hợp: targetPosition, force, positionGain, velocityGain, maxVelocity), VELOCITY_CONTROL (vận tốc mục tiêu; đặt force = 0 để tắt motor mặc định), TORQUE_CONTROL (đặt moment trực tiếp; phải tắt motor mặc định bằng VELOCITY_CONTROL với force = 0 trước)
- Động học: calculateInverseKinematics(bodyUniqueId, endEffectorLinkIndex, targetPosition, targetOrientation, lowerLimits, upperLimits, jointRanges, restPoses, ...) trả về góc khớp; getLinkState để đọc vị trí/hướng link (cẩn thận: computeForwardKinematics=True để lấy thông tin chính xác)
- Hạn chế của calculateInverseKinematics: IK số (damped least squares), một nghiệm, có thể không hội tụ chính xác, nên kiểm tra sai số bằng getLinkState (liên hệ bài 3.3)
- Camera: getCameraImage (RGB, depth, segmentation), computeViewMatrix, computeProjectionMatrixFOV; resetDebugVisualizerCamera đổi góc nhìn GUI
- Công cụ debug: addUserDebugLine/Text, addUserDebugParameter (slider trong GUI), getContactPoints
- Ghi video/ảnh: startStateLogging(STATE_LOGGING_VIDEO_MP4) hoặc lưu ảnh từ camera
- Ổn định số: bước thời gian nhỏ, lực quá lớn gây rung, cần setJointMotorControl2 mỗi bước hoặc chỉ cần đặt một lần (position control giữ mục tiêu)
- Cấu trúc code khuyến nghị: class Simulation + class Robot (liên hệ bài 0.3): hàm get_q, set_q_target, step, move_to_pose

VÍ DỤ ROBOT: Franka Panda ("franka_panda/panda.urdf", 7 khớp tay + 2 ngón; end-effector link index 11, ghi chú cần kiểm tra bằng getJointInfo) hoặc KUKA iiwa ("kuka_iiwa/model.urdf", 7 khớp, end-effector index 6) có sẵn trong pybullet_data; robot 6 trục: UR5 cần file URDF riêng (tải từ ur_description); nêu rõ chỗ nào cần kiểm tra lại chỉ số link trên máy người học vì có thể khác.

VÌ SAO CẦN HỌC: đây là sân chơi để thử mọi thứ đã học (FK, IK, Jacobian, quỹ đạo) mà không sợ hỏng robot thật; code viết ở đây dễ chuyển sang ROS và robot thật.

LƯU Ý KỸ THUẬT: HTML không chạy được PyBullet. Hiển thị code Python đầy đủ để người học copy và dùng JavaScript mô phỏng *hiệu ứng* của từng lệnh bằng robot phẳng 2-3 khâu (tự viết mô phỏng đơn giản: PD control trên mỗi khớp, tích phân Euler với dt cố định, trọng lực tùy chọn). Robot 3D (Panda/UR5) giả 3D bằng Canvas hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry), chỉ ở minh họa cuối nếu đủ chỗ.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Vòng đời chương trình: sơ đồ khối (connect → load → step loop → disconnect) tương tác; bấm từng khối thì hiện dòng code tương ứng và mô tả; animation "chạy" qua các khối; nút Phát chạy tự động; vòng lặp step hiển thị một biểu tượng đếm số bước và thời gian mô phỏng.
b) Bước thời gian: robot 1 khớp (con lắc hoặc thanh quay) rơi dưới trọng lực; slider dt (1/60, 1/120, 1/240, 1/1000); thấy dt lớn thì kém chính xác hoặc "nổ" (mất ổn định) còn dt nhỏ thì chậm; đồ thị góc theo thời gian; hiển thị số bước mỗi giây thực tế; giải thích time.sleep(dt) để chạy đúng tốc độ thực.
c) Ba chế độ điều khiển khớp: ba khung cạnh nhau cùng một thanh quay có trọng lực: POSITION_CONTROL (đặt góc đích, thanh tiến tới và giữ), VELOCITY_CONTROL (đặt vận tốc, thanh quay đều), TORQUE_CONTROL (đặt moment; thấy thanh rơi nếu moment không đủ chống trọng lực); slider tham số tương ứng và đồ thị q(t), τ(t); ghi chú "TORQUE_CONTROL cần tắt motor mặc định".
d) PD trong position control: slider positionGain (Kp) và velocityGain (Kd), force tối đa; đáp ứng bước: quá độ nhỏ/quá lớn, dao động, chậm; so sánh các bộ thông số; ghi rõ ý nghĩa vật lý; đồ thị q(t) và sai số.
e) Robot 2-3 khâu mô phỏng: gồm PD cho từng khớp và trọng lực; slider góc đích từng khớp; cánh tay chuyển động mượt tới đích theo động lực học; so sánh có và không có trọng lực (cánh tay "võng" khi thiếu lực); hiển thị q_target, q thực, sai số; nút bật/tắt trọng lực.
f) IK trong PyBullet bằng hình: kéo chấm đích 2D; robot phẳng nhiều khâu hoặc dư thừa tự đưa đầu công cụ tới đích (IK số DLS tự viết bằng JS); khi đích ngoài tầm với thì robot "hết cỡ" và báo sai số; thêm khớp có giới hạn để thấy kết quả đổi; ghi chú rằng calculateInverseKinematics cũng làm điều tương tự.
g) Đọc thông tin khớp: bảng giả lập getJointInfo cho robot Panda (7 khớp quay + 2 ngón, vài khớp fixed); lọc "khớp điều khiển được"; bảng getJointState cập nhật theo robot mô phỏng; nhắc cách tìm đúng end-effector index.
h) Camera ảo: một mặt phẳng và khối hộp; slider vị trí camera, FOV, khoảng cách; ảnh RGB mô phỏng (vẽ lại cảnh từ góc nhìn camera bằng phép chiếu) và ảnh độ sâu (thang xám); ghi chú viewMatrix/projectionMatrix; nối sang bài 8.4 (thị giác).
i) Debug: bấm nút giả lập addUserDebugLine (vẽ tia từ đầu công cụ), addUserDebugParameter (slider), getContactPoints (chấm tại điểm tiếp xúc giữa hai vật); giải thích mục đích từng lệnh.
j) Panda 3D (nếu đủ chỗ): robot 7 khớp giả 3D, 7 slider, đích 3D; nút "IK" chạy thuật toán DLS đơn giản và robot trườn tới đích; hiển thị quaternion hướng đích.

VÍ DỤ CÓ LỜI GIẢI: (1) chương trình PyBullet tối thiểu (khoảng 15 dòng): nạp plane và Panda, đặt gravity, chạy 1000 bước, in vị trí đầu công cụ (giải thích từng dòng); (2) đưa khớp 1 của Panda tới 0.5 rad bằng POSITION_CONTROL và đo sai số sau 240 bước; (3) tính IK cho điểm (0.5, 0, 0.5) bằng calculateInverseKinematics, đặt vào các khớp rồi đo sai số thực bằng getLinkState; (4) cho một thanh quay 1 khớp rơi tự do bằng TORQUE_CONTROL (tắt motor trước).

CODE PYTHON MẪU (đầy đủ, chạy được): (1) khung chương trình connect/loadURDF/step/disconnect có chế độ GUI và DIRECT; (2) hàm get_controllable_joints(robot) trả về danh sách chỉ số khớp không phải fixed; (3) POSITION_CONTROL cho nhiều khớp bằng setJointMotorControlArray; (4) TORQUE_CONTROL đúng cách (tắt motor mặc định bằng VELOCITY_CONTROL force=0); (5) ik_move(robot, target_pos, target_orn, steps) dùng calculateInverseKinematics với lowerLimits/upperLimits/jointRanges/restPoses, đặt target rồi bước mô phỏng và đo sai số; (6) class Robot bọc toàn bộ (get_q, set_q, move_to_pose, ee_pose); (7) chụp ảnh RGB-D từ camera