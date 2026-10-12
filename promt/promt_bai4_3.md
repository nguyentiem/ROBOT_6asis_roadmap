CHỦ ĐỀ: Bài 4.3 – Điều khiển vận tốc (Resolved-Rate Control) và damped least squares

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Ý tưởng điều khiển vận tốc: muốn đầu công cụ có vận tốc ẋ mong muốn → tính q̇ = J⁻¹·ẋ (hoặc J⁺·ẋ) → tích phân q để chạy
- Vòng lặp điều khiển: tại mỗi bước dt: tính J(q), tính q̇, cập nhật q ← q + q̇·dt; vì sao sai số tích lũy (drift) và cách khắc phục bằng phản hồi vị trí: ẋ_lệnh = ẋ_mong + K·(x_mong − x_hiện)
- Giả nghịch đảo J⁺ = Jᵀ(JJᵀ)⁻¹ (robot dư thừa) và trường hợp J vuông
- Vấn đề gần singularity: J⁻¹ có thành phần rất lớn → q̇ vượt giới hạn động cơ
- Damped least squares (DLS): q̇ = Jᵀ(JJᵀ + λ²I)⁻¹·ẋ; hệ số giảm chấn λ: λ lớn thì ổn định nhưng sai số bám (không đi đúng vận tốc mong muốn), λ nhỏ thì chính xác nhưng dễ bùng nổ; DLS thích nghi: λ tăng khi w nhỏ
- Giới hạn vận tốc khớp: co tỉ lệ q̇ (scaling) để giữ hướng chuyển động đúng khi một khớp vượt giới hạn
- Quỹ đạo Descartes: đường thẳng, đường tròn: tạo x(t), ẋ(t) từ công thức
- Với 6 DOF: sai số hướng bằng trục-góc (liên hệ bài 3.3), điều khiển cả vị trí và hướng
- Ứng dụng: điều khiển bằng tay cầm (teleoperation), bám vật chuyển động, hàn theo đường

VÌ SAO CẦN HỌC: đây là cách robot đi theo đường thẳng, đường tròn hoặc theo tay người điều khiển một cách mượt mà; đồng thời là nền tảng của servo thị giác và điều khiển thời gian thực.

LƯU Ý KỸ THUẬT: robot phẳng 2-3 khâu vẽ 2D bằng Canvas (tự viết FK, Jacobian, nghịch đảo ma trận nhỏ, giả nghịch đảo và DLS bằng công thức tường minh); robot 6 trục dùng giả 3D hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry) chỉ ở minh họa cuối nếu đủ chỗ. Dùng requestAnimationFrame với bước thời gian cố định (dt) để mô phỏng.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Vòng lặp điều khiển vận tốc: robot phẳng 2 khâu; nhập hoặc kéo vận tốc mong muốn (mũi tên) cho đầu công cụ; animation chạy liên tục: mỗi bước hiện 3 mũi tên (ẋ mong muốn, q̇ tính ra bằng J⁻¹ như hai thanh, ẋ thực tế); đồng thời sơ đồ khối (ẋ → J⁻¹ → q̇ → tích phân → q → FK) với khối đang xử lý sáng lên.
b) Đi theo đường thẳng: chọn điểm A, B (kéo chuột); nút Phát; đầu công cụ đi từ A đến B theo vận tốc đều; vẽ vết (đường mong muốn màu xám, đường thực tế màu primary); hai chế độ "không phản hồi" và "có phản hồi (K)" để thấy drift tích lũy; slider dt và K; đồ thị sai số bám theo thời gian.
c) Đi theo đường tròn: tâm, bán kính, tốc độ góc bằng slider; vẽ vết; so sánh có và không có phản hồi vị trí; đồ thị q1(t), q2(t), q̇1(t), q̇2(t).
d) Gần singularity: đường thẳng đi qua gần điểm kỳ dị; so sánh J⁻¹ (q̇ tăng vọt, cánh tay giật mạnh) và DLS (q̇ mượt nhưng bám lệch nhẹ); slider λ; đồ thị q̇ và sai số bám của hai phương pháp chồng lên nhau; đường giới hạn vận tốc khớp (đỏ).
e) Chọn λ: slider λ từ 0 đến 0.5; đồ thị đánh đổi (trade-off) giữa sai số bám và vận tốc khớp lớn nhất; chọn λ thích nghi theo w (λ² = λ0²·(1 − w/w0)² khi w < w0) và so sánh với λ cố định.
f) Giới hạn vận tốc khớp: đặt giới hạn cho từng khớp (slider); khi vượt giới hạn, hai cách xử lý: (1) kẹp từng khớp riêng (đường đi bị lệch hướng), (2) co tỉ lệ toàn bộ q̇ (giữ đúng hướng, chậm hơn); vẽ cả hai vết để thấy khác biệt.
g) Robot 3 khâu dư thừa: điều khiển vận tốc với giả nghịch đảo; cho thấy nghiệm có chuẩn nhỏ nhất của q̇; thêm "chuyển động không gian null" (null-space): slider cho chuyển động mà đầu công cụ đứng yên nhưng khuỷu di chuyển; (nếu đủ chỗ) dùng null-space để tránh một chướng ngại hoặc tránh giới hạn khớp.
h) Điều khiển bằng tay cầu ảo (teleoperation): vùng kéo chuột/chạm xác định vận tốc mong muốn của đầu công cụ (như joystick); robot chạy theo; có DLS bật/tắt để thấy robot "khựng" gần singularity thế nào.
i) Điều khiển cả hướng (6 DOF) ở mức khái niệm: robot 3 khâu phẳng có vị trí và góc φ; đích gồm (x, y, φ); sai số hướng góc; hoặc giả 3D UR5 với đường thẳng (nếu đủ chỗ).

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) một bước điều khiển vận tốc cho robot 2 khâu l1 = l2 = 1 ở θ = (30°, 60°) với ẋ = (0.1, 0) m/s, dt = 0.01 s: tính J, J⁻¹, q̇, q mới; (2) so sánh J⁻¹ và DLS (λ = 0.1) cho ẋ = (0.1, 0) tại θ2 = 3° (số cụ thể); (3) tính phản hồi: sai số 5 mm với K = 5 thì ẋ_lệnh thay đổi bao nhiêu.

CODE PYTHON MẪU (numpy, chạy được): hàm resolved_rate(fk, jac, x_start, x_goal, T, dt, K, damping) vòng lặp mô phỏng; hàm dls(J, xdot, lam); hàm scale_joint_velocity(qdot, qdot_max) co tỉ lệ; tạo quỹ đạo đường thẳng và đường tròn; null-space: q̇ = J⁺ẋ + (I − J⁺J)·q̇0; vẽ q(t), q̇(t) và vết đầu công cụ bằng matplotlib; đối chiếu với roboticstoolbox: rtb.jtraj, rtb.ctraj và robot.jacob0, robot.jacob0_pinv, hoặc robot.cartesian_servo / rtb.p_servo (ghi chú: xem phiên bản thư viện đang cài); chạy với UR5 và tư thế gần singularity cổ tay.

LỖI THƯỜNG GẶP: bước thời gian dt quá lớn làm mô phỏng phân kỳ, không có phản hồi vị trí nên drift, dùng J⁻¹ ngay cả khi J không vuông hoặc gần suy biến, λ quá lớn làm robot không bám quỹ đạo, kẹp từng khớp làm lệch hướng thay vì co tỉ lệ, quên chuẩn hóa góc, sai số hướng bằng hiệu góc Euler, không giới hạn gia tốc (giật khi bắt đầu/kết thúc), tính J ở q cũ thay vì q hiện tại, nhầm hệ tọa độ của ẋ (hệ đế hay hệ công cụ).

BÀI TẬP: (1) cài resolved_rate cho robot 2 khâu đi theo đường thẳng và đường tròn, vẽ vết; (2) thêm phản hồi vị trí và so sánh drift; (3) thêm DLS, chạy đường thẳng qua gần singularity và vẽ q̇(t); (4) cài co tỉ lệ vận tốc theo giới hạn khớp; (5) mở rộng lên UR5 (6 DOF) và đi đường thẳng với sai số hướng bằng trục-góc.
TRẮC NGHIỆM (5 câu): công thức q̇ từ ẋ, vì sao cần phản hồi vị trí, DLS thay đổi gì khi λ tăng, co tỉ lệ khác kẹp từng khớp ra sao, khi nào dùng giả nghịch đảo.