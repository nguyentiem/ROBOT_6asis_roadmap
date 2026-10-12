CHỦ ĐỀ: Bài 6.3 – Gắp và đặt vật (Pick and Place) trong mô phỏng

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Bài toán: gắp một vật tại vị trí A và đặt tại vị trí B; chia nhỏ thành các bước có thể lập trình
- Máy trạng thái hữu hạn (finite state machine): HOME → APPROACH (tiếp cận phía trên vật) → DESCEND (hạ xuống) → GRASP (đóng kẹp) → LIFT (nâng lên) → TRANSPORT (di chuyển ngang) → PLACE_DESCEND (hạ xuống) → RELEASE (mở kẹp) → RETREAT (rút lên) → HOME; điều kiện chuyển trạng thái (đến đích trong sai số, gripper đóng đủ, hết thời gian)
- Điểm trung gian (waypoints): pre-grasp (cách vật một khoảng an toàn theo trục z công cụ), grasp, post-grasp; vì sao không đi thẳng tới vật (va chạm bàn, đẩy vật đi)
- Hướng kẹp: hướng công cụ chỉ xuống, xoay quanh trục z theo hướng vật (liên hệ bài 1.3); chọn quaternion bằng p.getQuaternionFromEuler
- Phép biến đổi: vị trí vật trong hệ thế giới → T_world_object; T_world_grasp = T_world_object · T_object_grasp (tay kẹp so với vật); pre-grasp = T_world_grasp lùi dọc trục z công cụ (bài 1.2)
- Gripper: Panda có 2 ngón (khớp 9, 10), điều khiển vị trí 0 – 0.04 m; lực kẹp; kiểm tra "kẹp được vật" bằng độ mở ngón, getContactPoints, hoặc ràng buộc cứng
- Giữ vật: ma sát (lateralFriction, changeDynamics), lực kẹp đủ, khối lượng vật; hoặc dùng ràng buộc p.createConstraint (JOINT_FIXED) như "keo dính" để đơn giản hóa (nêu hạn chế: không thật)
- Vật và bàn: tạo khối hộp bằng createMultiBody hoặc nạp "cube_small.urdf", "table/table.urdf", "tray/tray.urdf" từ pybullet_data; khối lượng, ma sát, màu
- Thực thi chuyển động: nội suy trong không gian Descartes (bài 5.2) rồi IK từng điểm hoặc đặt target PyBullet IK và chờ hội tụ; kiểm tra đến đích bằng sai số vị trí < ngưỡng
- Kiểm tra thành công: vật nằm trong khu vực đích (so tọa độ), vật còn nằm trên robot hay rơi, đếm số lần thành công qua nhiều lần chạy
- Xử lý lỗi: không kẹp được (gripper đóng hết), vật rơi giữa chừng, IK thất bại, hết thời gian; chiến lược thử lại (retry)
- Mở rộng: nhiều vật, ngẫu nhiên vị trí, xếp chồng, dùng thị giác (bài 8.4) để biết vị trí vật

VÍ DỤ ROBOT: Franka Panda trong pybullet_data cùng "table/table.urdf" và "cube_small.urdf"; chỉ số end-effector (link 11) và ngón tay (khớp 9, 10) cần kiểm tra bằng getJointInfo (ghi chú).

VÌ SAO CẦN HỌC: pick and place là bài toán ứng dụng phổ biến nhất của cánh tay robot (đóng gói, lắp ráp, xếp pallet) và là bước ghép tất cả kiến thức: FK, IK, quỹ đạo, điều khiển gripper, xử lý lỗi.

LƯU Ý KỸ THUẬT: HTML không chạy được PyBullet; hãy hiển thị code Python đầy đủ để người học copy, và dùng JavaScript mô phỏng *hiệu ứng* bằng robot phẳng 2-3 khâu có kẹp (2D) tự viết: IK 2-3 khâu bằng công thức, vật là hình chữ nhật chịu trọng lực đơn giản (rơi nếu không kẹp), máy trạng thái chạy bằng requestAnimationFrame. Robot Panda/UR5 3D giả 3D bằng Canvas hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry), chỉ ở minh họa cuối nếu đủ chỗ.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Máy trạng thái trực quan: sơ đồ các trạng thái (hộp bo tròn nối mũi tên); robot 2D chạy pick-and-place; trạng thái hiện tại sáng lên; điều kiện chuyển (đến đích, kẹp xong, hết giờ) hiển thị trên mũi tên; nút Phát/Dừng/Bước tiếp/Làm lại; tốc độ.
b) Điểm trung gian: robot, bàn, vật; hiển thị các waypoint (pre-grasp, grasp, lift, pre-place, place, retreat) bằng chấm và đường nối; kéo vật hoặc vị trí đích thì waypoint cập nhật; so sánh đi thẳng tới vật (đẩy vật đi hoặc đâm vào bàn, đánh dấu đỏ) với đi qua pre-grasp.
c) Hạ xuống từng bước: slider khoảng cách an toàn (độ cao pre-grasp) và tốc độ hạ; thấy đầu kẹp chạm vật khi quá thấp hoặc quá nhanh; hiển thị sai số vị trí lúc kẹp.
d) Kẹp và giữ vật: gripper hai ngón mô phỏng; slider độ mở, lực kẹp, ma sát, khối lượng vật; vật trượt rơi khi lực/ma sát không đủ; cảnh báo màu; đồ thị lực kẹp so với trọng lượng; so sánh với cách "ràng buộc cứng" (vật dính vào tay không rơi) để hiểu giả định.
e) Biến đổi tọa độ: sơ đồ T_world_object, T_object_grasp, T_world_grasp; kéo xoay vật thì hướng kẹp tự xoay theo (kẹp luôn song song cạnh vật); hiển thị phép nhân ma trận 4x4 (liên hệ bài 1.2) với giá trị cập nhật; điểm pre-grasp lùi theo trục z công cụ.
f) Kiểm tra thành công: khu vực đích vẽ hình chữ nhật; sai số vị trí đặt hiển thị; nút "Chạy 20 lần" với vị trí vật ngẫu nhiên và nhiễu nhỏ; thống kê tỉ lệ thành công và biểu đồ cột; nguyên nhân thất bại (không kẹp được, rơi, ngoài tầm với) phân loại bằng màu.
g) Xử lý lỗi và thử lại: mô phỏng kẹp trượt; máy trạng thái phát hiện (độ mở ngón = 0 hoặc vật không còn dính tay) và quay về APPROACH thử lại; đếm số lần thử; sơ đồ thêm nhánh lỗi.
h) Nhiều vật và xếp chồng: 3-4 vật ngẫu nhiên; thứ tự gắp (gần nhất trước hoặc cao nhất trước); xếp vào khay hoặc chồng lên nhau (độ cao đặt tăng dần); animation cả chu trình.
i) Thời gian chu kỳ: biểu đồ thời gian mỗi pha (tiếp cận, hạ, kẹp, nâng, di chuyển, thả); slider tốc độ và độ cao an toàn; so sánh dừng ở mọi điểm với dùng blending (liên hệ bài 5.3) để thấy giảm thời gian.
j) Panda 3D (nếu đủ chỗ): robot 7 khớp giả 3D và vật khối lập phương; chạy một chu trình pick-and-place bằng máy trạng thái và IK DLS tự viết; có thể thay bằng animation dữ liệu tính trước.

VÍ DỤ CÓ LỜI GIẢI: (1) liệt kê tọa độ các waypoint cho vật ở (0.5, 0.1, 0.02), đích (0.5, −0.2, 0.02), độ cao an toàn 0.15 m, hướng công cụ chỉ xuống (viết quaternion); (2) tính T_world_grasp từ T_world_object (vật xoay 30° quanh z) và T_object_grasp (kẹp ngay trên đỉnh, chỉ xuống); (3) lực kẹp tối thiểu để giữ vật 0.1 kg với hệ số ma sát 0.8, hai ngón (F ≥ m·g/(2·μ)); (4) máy trạng thái viết bằng class State/enum trong Python (từng trạng thái và chuyển trạng thái).

CODE PYTHON MẪU (đầy đủ, chạy được): thiết lập cảnh (plane, table, cube, Panda); hàm get_ee_pose(robot); hàm move_to_pose(robot, pos, orn, steps, tol) dùng calculateInverseKinematics rồi bước mô phỏng cho tới khi sai số < tol; hàm set_gripper(robot, width, force); hàm is_grasped(robot, cube) kiểm tra bằng getContactPoints hoặc độ mở ngón; class PickPlace với enum State và hàm step(); hàm pick_and_place(robot, cube, target_pos) chạy toàn bộ chu trình (có kiểm tra thành công và retry); chạy lặp 20 lần với vị trí vật ngẫu nhiên và thống kê tỉ lệ thành công; (tùy chọn) createConstraint JOINT_FIXED để "dính" vật; đối chiếu với tham chiếu: thư mục examples của PyBullet (ví dụ pybullet_robots, panda) và gói panda-gym (nếu muốn học thêm). Ghi chú những chỗ cần kiểm tra theo phiên bản (chỉ số khớp ngón, end-effector, tên URDF).

LỖI THƯỜNG GẶP: đi thẳng tới vật mà không qua pre-grasp (đâm hoặc đẩy vật), hướng kẹp sai (không xoay theo vật), đóng kẹp quá sớm (khi chưa tới vị trí), nâng quá nhanh làm vật trượt, lực kẹp quá nhỏ hoặc ma sát chưa đặt (vật rơi), quên chờ bước mô phỏng để robot tới đích, so sánh sai số bằng == thay vì ngưỡng, dùng ràng buộc cứng nhưng tưởng là kẹp thật, quên reset vật khi chạy lại, quaternion sai thứ tự, không xử lý trường hợp IK thất bại, bỏ qua giới hạn khớp.

BÀI TẬP: (1) cài chu trình pick-and-place cho một khối lập phương từ A sang B; (2) thêm xoay hướng kẹp theo góc của vật; (3) thêm kiểm tra thành công và retry; (4) chạy 20 lần với vị trí ngẫu nhiên, thống kê tỉ lệ; (5) mở rộng: xếp 3 vật chồng lên nhau hoặc vào khay.
TRẮC NGHIỆM (5 câu): vì sao cần pre-grasp, thứ tự các trạng thái trong chu trình, lực kẹp tối thiểu, cách kiểm tra đã kẹp được vật, nguyên nhân thường gặp khi vật rơi.