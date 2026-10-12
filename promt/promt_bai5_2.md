CHỦ ĐỀ: Bài 5.2 – Quy hoạch quỹ đạo trong không gian Descartes (đường thẳng, cung tròn, nội suy hướng)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Vì sao cần quỹ đạo Descartes: hàn, dán keo, sơn, cắt, lắp ráp đòi hỏi đầu công cụ đi đúng đường (thẳng, tròn), không phải đường cong tùy ý như ở MoveJ
- Khác biệt lệnh: PTP/MoveJ (nội suy khớp, đường đi đầu công cụ không đoán trước, nhanh, an toàn về singularity) và LIN/MoveL (nội suy Descartes, đường thẳng) và CIRC/MoveC (cung tròn)
- Quy trình: tạo chuỗi điểm trung gian x(t) ∈ SE(3) → giải IK cho từng điểm (chọn nghiệm theo bài 3.4, giữ nhánh) → được q(t); hoặc điều khiển vận tốc (bài 4.3)
- Nội suy vị trí đường thẳng: p(s) = p_A + s·(p_B − p_A), với s(t) từ hồ sơ hình thang hoặc bậc 5 (bài 5.1)
- Nội suy hướng bằng slerp (bài 1.3): R(s) = R_A·Exp(s·Log(R_Aᵀ·R_B)); vì sao không nội suy từng góc Euler
- Giới hạn tốc độ: vận tốc dài (mm/s) và vận tốc góc (°/s); thời gian T lấy max của hai yêu cầu (vị trí và hướng)
- Cung tròn qua 3 điểm: tìm tâm, bán kính, mặt phẳng; tham số hóa p(φ) = c + r·(cosφ·u + sinφ·v); hướng có thể giữ nguyên hoặc nội suy
- Tần số lấy mẫu của bộ điều khiển (ví dụ 125 Hz, 500 Hz): số điểm trung gian = T·f
- Các vấn đề: quỹ đạo đi qua singularity (IK ra nghiệm vô hạn), vượt giới hạn khớp, nhảy nhánh nghiệm, ngoài workspace giữa đường dù A và B đều trong workspace
- Kiểm tra khả thi toàn đường trước khi chạy (bài 3.4); MoveL có thể báo lỗi trong khi MoveJ giữa cùng hai điểm chạy được
- Hệ tọa độ: quỹ đạo định nghĩa trong hệ đế hoặc hệ vật (work frame), kèm TCP

VÌ SAO CẦN HỌC: nó là sự khác nhau giữa "robot chạm đúng điểm" và "robot đi đúng đường"; trong công nghiệp hầu hết ứng dụng gia công đều dùng MoveL/MoveC.

LƯU Ý KỸ THUẬT: phần robot phẳng vẽ 2D bằng Canvas (tự viết IK 2 khâu từ bài 3.1); phần có hướng dùng robot 3D giả 3D hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry), tự viết slerp bằng quaternion (bài 1.3), IK 6 trục giải tích đơn giản hoặc dữ liệu tính trước. Nếu không có IK 6 trục, dùng robot RRR 3 trục với vị trí và hướng của khâu cuối kiểu đơn giản hóa.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) MoveJ so với MoveL: robot phẳng 2 khâu; kéo A và B; hai khung cạnh nhau: MoveJ (vết cong) và MoveL (vết thẳng); chạy đồng thời; thấy cả hai đến cùng B nhưng đường đi khác nhau; bảng so sánh (đường đi, vận tốc khớp lớn nhất, nguy cơ singularity).
b) Dựng đường thẳng từng bước: hiện chuỗi điểm trung gian dọc AB (mật độ chọn bằng slider tần số/số điểm); với mỗi điểm hiện cánh tay (bóng mờ) và nghiệm IK; thanh trượt s(t) để duyệt; đồ thị q1(t), q2(t); Phát/Dừng/Làm lại; hiển thị hồ sơ s(t) chọn được (hình thang/bậc 5).
c) Vận tốc đều trên đường thẳng: so sánh vận tốc đầu công cụ trong MoveL (không đổi giữa hai đầu) với MoveJ (thay đổi); đồ thị |v_tip|(t) của hai chế độ.
d) Đường thẳng qua vùng nguy hiểm: kéo A, B sao cho đường thẳng đi qua vùng gần singularity hoặc ngoài workspace; hiện vùng cấm (vành khăn lỗ trong và biên ngoài); điểm vi phạm tô đỏ trên đường; thông báo "MoveL không khả thi, MoveJ khả thi"; thử đổi B hoặc dùng điểm trung gian để cứu.
e) Cung tròn qua 3 điểm: kéo 3 điểm P1, P2, P3; hiện tâm, bán kính, vòng tròn, cung đi qua P2; animation đầu công cụ chạy theo cung với vận tốc đều; công thức tâm đường tròn qua 3 điểm (giải bằng hình học) hiển thị từng bước; thử 3 điểm thẳng hàng (suy biến) có cảnh báo.
f) Nội suy hướng: vật 3D có hướng ban đầu và cuối (kéo chọn bằng slider RPY); so sánh nội suy từng góc Euler (xoay thừa, đường lạ) với slerp (xoay đều theo trục-góc); hai vật chạy song song; đồ thị góc xoay theo thời gian.
g) Vị trí + hướng cùng lúc: robot 3 khâu phẳng (x, y, φ) hoặc robot 3D đơn giản; đường thẳng từ (A, φA) đến (B, φB) với cả hai nội suy chung s(t); hiển thị T = max(T_pos, T_rot) và khâu nào là "giới hạn thời gian"; slider tốc độ dài và tốc độ góc tối đa.
h) Lấy mẫu và độ chính xác: slider tần số lấy mẫu (10 Hz – 500 Hz); tần số thấp thì cung tròn thành đa giác, đường thẳng có răng cưa trong điều khiển thực; hiển thị sai số hình học lớn nhất so với đường lý tưởng.
i) Hệ tọa độ vật (work frame): đường thẳng định nghĩa trong hệ vật; kéo xoay/dịch hệ vật thì toàn bộ đường đi trong hệ đế đổi theo (ma trận T_base_work ở bài 1.2); cho thấy vì sao dạy lại điểm bằng work frame nhanh hơn.
j) UR5 giả 3D chạy MoveL (nếu đủ chỗ): chọn A, B bằng slider; robot đi đường thẳng trong không gian 3D; vết đầu công cụ thẳng; đồ thị 6 khớp; cảnh báo khi gần singularity (liên hệ bài 4.2).

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) đường thẳng robot 2 khâu l1 = l2 = 1 từ A = (1.5, 0) đến B = (0.5, 1) trong 2 s, 5 điểm trung gian: tính p(s) và IK cho từng điểm (elbow up), nhận xét vận tốc khớp; (2) tâm và bán kính đường tròn qua P1 = (1, 0), P2 = (0, 1), P3 = (−1, 0) và tham số hóa; (3) slerp giữa Rz(0°) và Rz(90°) tại s = 0.5; (4) A và B đều trong workspace nhưng đường thẳng cắt lỗ trong: tính điểm vi phạm.

CODE PYTHON MẪU (numpy, chạy được): hàm line_path(pA, pB, s); hàm slerp(qA, qB, s) bằng quaternion (bài 1.3); hàm circle_from_3_points(P1, P2, P3) trả về tâm, bán kính, hai vector cơ sở; hàm circ_path(...); hàm cartesian_trajectory(TA, TB, T, f) trả về danh sách T(t_k) dùng hồ sơ hình thang; hàm movel(robot, TA, TB, T, f, q_start) giải IK từng điểm theo giữ nhánh (bài 3.4) và kiểm tra giới hạn; đối chiếu bằng roboticstoolbox: rtb.ctraj(T0, T1, t), robot.ikine_LM với q0 là nghiệm trước, SE3.interp; vẽ vết đầu công cụ và q(t) bằng matplotlib; thử MoveL qua gần singularity của UR5 và quan sát q̇ tăng vọt.

LỖI THƯỜNG GẶP: nội suy từng góc Euler, quên rằng MoveL có thể thất bại trong khi MoveJ thành công, IK từng điểm không giữ nhánh (nhảy cấu hình), bỏ qua singularity dọc đường, lấy mẫu quá thưa, chỉ giới hạn tốc độ dài mà quên tốc độ góc (xoay nhanh khi hướng đổi nhiều), 3 điểm cung tròn thẳng hàng, nhầm chiều quay của cung, quên rằng điểm giữa của MoveC nằm trên cung chứ không phải tâm, nhầm hệ tọa độ (đế/vật/công cụ), chỉ kiểm tra hai đầu mút mà không kiểm tra giữa đường.

BÀI TẬP: (1) viết movel cho robot 2 khâu, vẽ vết thẳng và q(t); (2) so sánh MoveJ và MoveL giữa cùng hai điểm; (3) viết circle_from_3_points và movec; (4) cài slerp và so với nội suy Euler; (5) mở rộng cho UR5 với IK từ bài 3.2/3.3, kiểm tra khả thi cả đường trước khi chạy, so với rtb.ctraj.
TRẮC NGHIỆM (5 câu): khác biệt MoveJ và MoveL, vì sao dùng slerp, T = max(T_pos, T_rot) nghĩa là gì, vì sao MoveL có thể thất bại giữa đường, cách xác định tâm cung tròn từ 3 điểm.