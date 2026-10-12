CHỦ ĐỀ: Bài 3.3 – IK số: Newton-Raphson, giả nghịch đảo Jacobian, damped least squares

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Khi nào cần IK số: robot không có nghiệm giải tích, robot dư thừa (7 DOF), có ràng buộc phụ, hoặc muốn một bộ giải dùng được cho mọi robot
- Ý tưởng: bắt đầu từ q0, đo sai số e = x_đích − FK(q), cập nhật q dần cho tới khi e đủ nhỏ
- Newton-Raphson cho hàm một biến (trực giác: đi theo tiếp tuyến) rồi mở rộng nhiều biến
- Công thức lặp: q ← q + J⁺(q)·e (J⁺ là giả nghịch đảo Moore-Penrose); thêm hệ số bước α: q ← q + α·J⁺·e
- Biểu diễn sai số: sai số vị trí (hiệu hai vector) và sai số hướng (dùng trục-góc của R_đích·R_hiệnᵀ, liên hệ bài 1.3)
- Jacobian ở mức đủ dùng ở bài này: J = ∂x/∂q (tính số bằng sai phân hoặc lấy từ thư viện; bài 4.1 sẽ dạy cách dựng)
- Điều kiện dừng: ‖e‖ < ε hoặc số vòng lặp tối đa; ghi lại lịch sử sai số
- Hội tụ cục bộ: phụ thuộc giá trị khởi tạo q0, có thể rơi vào cực tiểu cục bộ hoặc không hội tụ; chiến lược nhiều điểm khởi tạo (random restarts)
- Vấn đề gần singularity: J⁺ cho bước nhảy rất lớn → damped least squares (Levenberg-Marquardt): Δq = Jᵀ(JJᵀ + λ²I)⁻¹·e
- Giới hạn khớp: kẹp q vào [q_min, q_max] sau mỗi bước hoặc dùng bộ giải có ràng buộc
- So sánh giải tích và số: tốc độ, số nghiệm tìm được, độ chính xác, tính tổng quát

VÌ SAO CẦN HỌC: MoveIt, các thư viện robot và hầu hết robot dư thừa đều dùng IK số; hiểu cơ chế giúp bạn biết vì sao nó đôi khi "kẹt" hoặc ra nghiệm lạ.

LƯU Ý KỸ THUẬT: phần Newton-Raphson một biến vẽ 2D bằng Canvas; robot minh họa là robot phẳng 2-3 khâu (2D, tự viết FK và Jacobian bằng JavaScript, giải giả nghịch đảo cho ma trận nhỏ bằng công thức tường minh hoặc tự viết hàm nghịch đảo ma trận 2x2/3x3); robot 6 trục dùng giả 3D hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry) chỉ ở minh họa cuối nếu đủ chỗ.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Newton-Raphson một biến: đồ thị f(x); kéo điểm x0; animation từng bước vẽ tiếp tuyến, giao với trục hoành, ra x1, lặp lại; hiển thị bảng x_k, f(x_k), sai số; thử hàm có nhiều nghiệm để thấy phụ thuộc điểm khởi tạo.
b) IK số từng bước trên robot phẳng 2 khâu: đặt đích bằng cách kéo chấm; cánh tay (màu mờ) hiện tư thế ban đầu; nút Bước tiếp / Phát / Làm lại; mỗi vòng lặp: vẽ mũi tên sai số e, mũi tên Δx do Δq tạo ra, cánh tay cập nhật; bảng bên cạnh: vòng, q1, q2, ‖e‖; biểu đồ sai số giảm dần theo vòng lặp.
c) Hệ số bước và hội tụ: slider α (0.05–1.5); thấy α nhỏ thì chậm, α quá lớn thì dao động hoặc phân kỳ; hiển thị số vòng lặp cần thiết và đồ thị so sánh.
d) Phụ thuộc điểm khởi tạo: robot có hai nghiệm; kéo tư thế ban đầu (hoặc chọn q0 trên mặt phẳng θ1-θ2); bản đồ màu cho thấy vùng q0 nào hội tụ về nghiệm elbow up (mint), elbow down (peach), không hội tụ (xám); animation vài đường đi của q trên mặt phẳng θ1-θ2.
e) Gần singularity: robot 2 khâu duỗi thẳng; so sánh giả nghịch đảo thường (bước nhảy rất lớn, cánh tay giật) với damped least squares (bước ổn định); slider λ; hiển thị det(J), số điều kiện của J; đồ thị độ lớn Δq theo vòng lặp.
f) Robot 3 khâu dư thừa: đích (x, y) không ràng buộc hướng; mỗi lần bấm "Giải" từ q0 khác nhau cho ra tư thế khác nhau; hiển thị nhiều nghiệm mờ chồng lên; giải thích giả nghịch đảo cho nghiệm gần q0 nhất (Δq nhỏ nhất).
g) Giới hạn khớp: thêm giới hạn góc cho từng khớp (slider); bật/tắt kẹp; thấy nghiệm thay đổi hoặc không hội tụ; vùng cấm tô đỏ nhạt trên vòng cung khớp.
h) So sánh phương pháp: bảng chạy cùng một bài toán với 3 bộ giải (giả nghịch đảo, DLS, Jacobian chuyển vị) và hiển thị số vòng lặp, thời gian (tương đối), có hội tụ không, đồ thị sai số chồng lên nhau.
i) Cầu nối tới robot 6 trục: bấm nút để chạy IK số trên UR5 giả 3D (nếu đủ chỗ): đặt đích bằng slider, xem robot "trườn" về đích sau mỗi vòng lặp; có thể thay bằng video-like animation dữ liệu tính trước nếu quá nặng.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) Newton-Raphson giải x² − 2 = 0 từ x0 = 1 (ba vòng lặp); (2) một vòng IK số cho robot 2 khâu l1 = l2 = 1, q0 = (0.3, 0.5) tới (1, 1): tính FK, e, J, J⁺, Δq và q mới bằng tay (kết quả số cụ thể, hãy tính chính xác); (3) so sánh bước nhảy của J⁺ và DLS tại q = (0.5, 0.01).

CODE PYTHON MẪU (numpy, chạy được): hàm jacobian_numeric(fk, q, h); hàm ik_newton(fk, x_target, q0, alpha, tol, max_iter) trả về q và lịch sử sai số; phiên bản DLS với lambda; xử lý sai số hướng bằng trục-góc cho 6 DOF (skeleton đủ chạy); kẹp giới hạn khớp; random restarts chọn nghiệm tốt nhất; vẽ đồ thị hội tụ bằng matplotlib; đối chiếu bằng thư viện: robot.ikine_LM(T), robot.ikine_min(T) (roboticstoolbox) và so sánh số vòng lặp, thời gian, nghiệm; ghi chú các tùy chọn quan trọng (mask, ilimit, slimit, tol, joint_limits).

LỖI THƯỜNG GẶP: dùng sai số hướng bằng hiệu hai góc Euler thay vì trục-góc, nhầm đơn vị vị trí và hướng khi ghép vào một vector sai số (cần trọng số), bước α quá lớn gây phân kỳ, không chuẩn hóa góc, không giới hạn số vòng lặp (lặp vô hạn), tin kết quả khi chưa kiểm tra hội tụ (success flag), khởi tạo q0 trùng điểm singularity (J mất hạng), tính J bằng sai phân với h không phù hợp, lấy nghịch đảo thẳng thay vì pinv hoặc DLS, bỏ qua giới hạn khớp.

BÀI TẬP: (1) cài ik_newton cho robot 2 khâu và vẽ đồ thị sai số; (2) thêm DLS và so sánh gần singularity; (3) thêm random restarts và thống kê tỉ lệ hội tụ trên 200 đích ngẫu nhiên; (4) mở rộng lên 6 DOF (UR5 từ bài 2.3) với sai số hướng bằng trục-góc; (5) so sánh với robot.ikine_LM().
TRẮC NGHIỆM (5 câu): ý nghĩa công thức q ← q + J⁺e, vì sao phụ thuộc q0, DLS giải quyết vấn đề gì, vai trò hệ số bước, khi nào nên dùng IK số thay vì giải tích.