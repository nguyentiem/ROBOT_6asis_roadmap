CHỦ ĐỀ: Bài 7.1 – Động lực học robot: M(q)q̈ + C(q,q̇)q̇ + g(q) = τ

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Động học chỉ nói robot ở đâu; động lực học nói cần lực/moment bao nhiêu để robot chuyển động như vậy và robot sẽ chuyển động thế nào khi có moment
- Hai bài toán: động lực học nghịch (biết q, q̇, q̈ → tìm τ; dùng để điều khiển và chọn động cơ) và động lực học thuận (biết τ → tìm q̈ rồi tích phân ra q(t); dùng để mô phỏng)
- Phương trình chuyển động dạng chuẩn: M(q)q̈ + C(q,q̇)q̇ + g(q) = τ
  • M(q): ma trận quán tính (đối xứng, xác định dương, phụ thuộc cấu hình: tay duỗi thì quán tính lớn hơn)
  • C(q,q̇)q̇: lực ly tâm và Coriolis (tỉ lệ bình phương vận tốc, kéo các khớp ảnh hưởng lẫn nhau)
  • g(q): moment trọng lực
  • τ: moment khớp (có thể thêm ma sát nhớt b·q̇ và ma sát Coulomb)
- Hai cách thiết lập phương trình: Euler-Lagrange (L = K − P, dùng năng lượng, tốt cho robot ít khớp) và Newton-Euler đệ quy (hiệu quả cho robot nhiều khớp, là thuật toán thư viện hay dùng); chỉ cần hiểu ý tưởng, tính tay cho robot 2 khâu
- Robot 2 khâu phẳng (khối lượng m1, m2 tập trung ở đầu mỗi khâu, chiều dài l1, l2, góc đo từ phương ngang, trọng lực hướng xuống):
  M11 = (m1+m2)·l1² + m2·l2² + 2·m2·l1·l2·cos q2
  M12 = M21 = m2·l2² + m2·l1·l2·cos q2
  M22 = m2·l2²
  h = −m2·l1·l2·sin q2; C·q̇ = [h·q̇2·(2q̇1 + q̇2) ; −h·q̇1²]... (hãy tự rút ra và kiểm tra lại dạng đúng, ghi rõ nếu dùng ma trận C = [[h·q̇2, h·(q̇1+q̇2)], [−h·q̇1, 0]])
  g1 = (m1+m2)·g·l1·cos q1 + m2·g·l2·cos(q1+q2); g2 = m2·g·l2·cos(q1+q2)
  (Hãy kiểm tra các công thức này bằng cách so với mô phỏng số hoặc năng lượng; nếu mô hình khối lượng phân bố đều trên khâu thì công thức khác, ghi chú rõ giả định.)
- Tính chất: M đối xứng xác định dương; Ṁ − 2C phản đối xứng (giới thiệu); năng lượng cơ học bảo toàn khi không có ma sát và τ = 0 (dùng để kiểm tra mô phỏng)
- Thông số quán tính trên robot thật: khối lượng, khối tâm, tensor quán tính từ URDF (liên hệ bài 6.1); sai số thông số làm điều khiển lệch
- Mô phỏng động lực học thuận: q̈ = M⁻¹(τ − C·q̇ − g); tích phân Euler/RK4; bước dt nhỏ để ổn định, kiểm tra bảo toàn năng lượng
- Ứng dụng: chọn động cơ (moment đỉnh, moment liên tục), tính moment bù trọng lực, điều khiển computed torque (bài 7.3), mô phỏng

VÌ SAO CẦN HỌC: không có mô hình động lực học thì không biết robot có nâng nổi tải ở tốc độ đó không, vì sao cánh tay rung khi tăng tốc, hay vì sao động cơ khớp 2 bị quá tải khi khớp 1 quay nhanh.

LƯU Ý KỸ THUẬT: minh họa chính là robot phẳng 2 khâu vẽ 2D bằng Canvas, tự viết bằng JavaScript: M(q), C, g, động lực học nghịch và thuận (tích phân RK4 với requestAnimationFrame), nghịch đảo ma trận 2x2 bằng công thức. Đồ thị tự vẽ bằng Canvas. Robot 6 trục chỉ nhắc ở phần code Python.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Cánh tay thả rơi: robot 2 khâu, τ = 0, trọng lực bật/tắt; thả từ tư thế bất kỳ (kéo chuột để đặt rồi thả); animation chuyển động hỗn loạn như con lắc kép; đồ thị q1(t), q2(t); đồ thị năng lượng (động năng, thế năng, tổng) để thấy tổng gần như không đổi khi không có ma sát; slider ma sát nhớt b thì năng lượng giảm dần.
b) Giải phẫu phương trình: phương trình M q̈ + C q̇ + g = τ hiển thị lớn, bấm vào từng số hạng thì phần đó sáng lên và hiện mô tả bằng hình; ví dụ bấm g(q) thì vẽ mũi tên trọng lực tại khối tâm từng khâu và moment tương ứng; bấm M thì vẽ "độ khó quay"; bấm C thì vẽ hiệu ứng khớp này quay làm khớp kia bị kéo.
c) Ma trận quán tính thay đổi theo tư thế: slider q2 (và q1); hiển thị M 2x2 với số cụ thể; vẽ elip quán tính (đường M⁻¹ hoặc hiệu ứng "gia tốc đạt được khi đặt moment đơn vị theo mọi hướng"); thấy tay duỗi (q2 = 0) quán tính M11 lớn nhất, tay gập (q2 = π) nhỏ nhất; đồ thị M11, M12, M22 theo q2.
d) Moment trọng lực: slider q1, q2, m1, m2; hiển thị g1, g2 bằng thanh moment; đồ thị g1, g2 theo q2; nút "tư thế tốn moment nhất"; liên hệ bài 4.4.
e) Động lực học nghịch (tính moment cần thiết): chọn quỹ đạo q(t) (hình thang/bậc 5 từ bài 5.1) bằng preset; nút Phát; robot chuyển động và hiển thị τ1(t), τ2(t) chia thành ba thành phần chồng màu: quán tính (M·q̈), Coriolis (C·q̇), trọng lực (g); thấy khi nào thành phần nào lớn; đường giới hạn moment động cơ (đỏ), tô cảnh báo khi vượt.
f) Coriolis và ly tâm: robot 2 khâu, đặt q̇1 lớn, q̈ = 0, hiển thị moment lên khớp 2 (do ly tâm/Coriolis); slider q̇1; vẽ mũi tên lực ly tâm; thấy moment tỉ lệ q̇² (đồ thị parabol); giải thích vì sao nhiều khớp quay nhanh thì ảnh hưởng lẫn nhau.
g) Động lực học thuận từng bước: nhập τ1, τ2 bằng slider và nút "Cho robot chạy"; mỗi bước dt hiển thị các số: tính M, C·q̇, g, rồi q̈ = M⁻¹(τ − Cq̇ − g), cập nhật q̇, q (tích phân Euler); sơ đồ khối sáng lần lượt; Bước tiếp/Phát/Làm lại.
h) Bước thời gian và ổn định: so sánh Euler với RK4; slider dt (1 ms – 50 ms); với Euler và dt lớn năng lượng tăng dần (mô phỏng "nổ"); với RK4 gần như bảo toàn; đồ thị sai số năng lượng theo thời gian.
i) Tải và chọn động cơ: thêm tải m_tải ở đầu công cụ (slider); đồ thị moment đỉnh và moment hiệu dụng (RMS) của từng khớp trên quỹ đạo; nhập moment liên tục/moment đỉnh của động cơ, kết luận "đủ/không đủ"; gợi ý giảm tốc độ hoặc dùng hộp số (tỉ số truyền N: moment động cơ = τ/N, quán tính phản xạ N²·J_motor, giới thiệu).
j) So sánh mô hình: ba robot cùng quỹ đạo nhưng khác khối lượng (nhẹ/trung bình/nặng); đồ thị moment chồng nhau; ghi chú ảnh hưởng thông số.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) tính M, C·q̇, g cho robot 2 khâu l1 = l2 = 1 m, m1 = m2 = 1 kg, q = (30°, 60°), q̇ = (1, 0.5) rad/s, g = 9.81 (số cụ thể, tính chính xác); (2) tính τ cần để q̈ = (2, 0) rad/s² tại cùng trạng thái; (3) tính q̈ khi τ = 0 (rơi tự do) tại q = (0°, 0°) và q = (90°, 0°); (4) thử bảo toàn năng lượng sau 1 s mô phỏng RK4 với dt = 0.001.

CODE PYTHON MẪU (numpy, chạy được): hàm dynamics_matrices(q, qd, params) trả về M, C, g cho robot 2 khâu; hàm inverse_dynamics(q, qd, qdd) = M@qdd + C@qd + g; hàm forward_dynamics(q, qd, tau) = np.linalg.solve(M, tau − C@qd − g); hàm simulate(q0, qd0, tau_func, T, dt) với RK4; kiểm tra bảo toàn năng lượng; vẽ q(t), τ(t) bằng matplotlib; đối chiếu bằng roboticstoolbox: robot.rne(q, qd, qdd) (động lực học nghịch), robot.inertia(q), robot.coriolis(q, qd), robot.gravload(q), robot.accel(q, qd, tau), robot.fdyn(...) cho robot 2 khâu rtb.models.DH.Planar2() (ghi chú: kiểm tra quy ước khối lượng/khối tâm của mô hình có sẵn và gán lại thông số cho khớp với hàm tự viết); ví dụ UR5/Puma 560: robot.gravload(q) và robot.rne(...) ở vài tư thế; PyBullet: p.calculateInverseDynamics(robot, q, qd, qdd) và đối chiếu (ghi chú vài lưu ý về khớp fixed và danh sách khớp).

LỖI THƯỜNG GẶP: nhầm quy ước góc (từ phương ngang hay phương thẳng đứng) làm sai dấu g, nhầm M q̈ với M⁻¹ q̈, lấy nghịch đảo thay vì np.linalg.solve, quên thành phần Coriolis, đơn vị (kg, m, N·m, rad), dt quá lớn khiến mô phỏng nổ, dùng Euler thay RK4 mà không kiểm tra năng lượng, quên ma sát khiến mô phỏng không giống thật, thông số quán tính sai (đặc biệt tensor quán tính trong URDF), nhầm moment khớp với moment động cơ (hộp số), bỏ qua quán tính rotor ở tốc độ cao.

BÀI TẬP: (1) cài M, C, g cho robot 2 khâu và kiểm tra tính đối xứng, xác định dương của M; (2) mô phỏng robot thả rơi, vẽ năng lượng và kiểm tra bảo toàn; (3) tính moment cần thiết theo quỹ đạo quintic và vẽ ba thành phần; (4) so với rtb (rne, gravload); (5) với UR5 và tải 2 kg, tìm tư thế tốn moment bù trọng lực nhất ở khớp 2.
TRẮC NGHIỆM (5 câu): ý nghĩa từng số hạng của phương trình, khác biệt động lực học thuận và nghịch, vì sao M phụ thuộc q, khi nào Coriolis lớn, vì sao mô phỏng Euler với dt lớn bị nổ.