CHỦ ĐỀ: Bài 4.1 – Jacobian vận tốc

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Từ vị trí sang vận tốc: nếu x = f(q) thì ẋ = J(q)·q̇, với J = ∂f/∂q (ma trận đạo hàm riêng, liên hệ bài 0.2)
- Ý nghĩa: Jacobian là "bộ khuếch đại" biến vận tốc khớp thành vận tốc đầu công cụ; mỗi cột là vận tốc đầu công cụ khi chỉ một khớp quay với tốc độ 1 rad/s
- Jacobian của robot phẳng 2 khâu: viết đầy đủ ma trận 2x2 từ đạo hàm riêng
- Kích thước J: 6 x n (3 dòng vận tốc dài v, 3 dòng vận tốc góc ω); với robot 6 trục là ma trận vuông 6x6
- Dựng Jacobian hình học từ bảng DH: với khớp quay i: cột i = [z_{i-1} × (p_n − p_{i-1}); z_{i-1}]; với khớp tịnh tiến: cột i = [z_{i-1}; 0]
- Jacobian hình học (geometric) và Jacobian giải tích (analytic): khác ở cách biểu diễn vận tốc góc (ω so với đạo hàm góc Euler), liên hệ qua ma trận biến đổi
- Biểu diễn Jacobian trong các hệ tọa độ khác nhau (hệ đế, hệ công cụ)
- Kiểm chứng: so Jacobian giải tích/dựng bằng DH với Jacobian tính bằng sai phân số
- Liên hệ bài 3.3: Jacobian dùng trong IK số (q ← q + J⁺e)

VÌ SAO CẦN HỌC: điều khiển robot đi theo đường thẳng với tốc độ đều, IK số, phát hiện singularity, tính lực đều dựa vào Jacobian.

LƯU Ý KỸ THUẬT: phần robot phẳng vẽ 2D bằng SVG/Canvas; phần dựng Jacobian từ bảng DH dùng robot 3 trục không gian bằng giả 3D Canvas hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry). Toàn bộ FK, Jacobian, nhân ma trận viết bằng JavaScript thuần.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Từ đạo hàm đến Jacobian: robot phẳng 2 khâu; tăng nhẹ θ1 một lượng nhỏ → đầu cánh tay dịch một đoạn, vẽ mũi tên (cột 1 của J); tăng nhẹ θ2 → mũi tên khác (cột 2 của J); hai mũi tên tô màu khác nhau và khớp với hai cột ma trận hiển thị bên cạnh; slider θ1, θ2 thay đổi thì ma trận và hai mũi tên đổi theo.
b) Công thức ẋ = J·q̇: hai slider vận tốc khớp ω1, ω2; mũi tên vận tốc đầu công cụ là tổng (theo quy tắc hình bình hành) của ω1·cột1 và ω2·cột2; animation cánh tay chuyển động liên tục với vận tốc đó (nút Phát/Dừng) và vẽ vết; hiển thị phép nhân ma trận số cụ thể.
c) Cột Jacobian bằng hình học: với khớp quay, mũi tên vuông góc với đoạn nối từ khớp đến đầu công cụ, độ dài = khoảng cách × ω; kéo khớp để thấy; nối trực quan với công thức z × (p_n − p_i).
d) Dựng từng cột: robot 3 trục không gian; bấm từng khớp thì hiện trục z_{i-1}, vector (p_n − p_{i-1}), tích có hướng cho ra cột; bảng 6x3 điền dần; nút Bước tiếp, Phát, Làm lại.
e) Kiểm tra bằng sai phân số: cạnh Jacobian dựng bằng công thức, hiển thị Jacobian sai phân (f(q+h·e_i) − f(q))/h với slider h; sai lệch giữa hai ma trận hiển thị bằng màu; thấy h quá lớn hoặc quá nhỏ đều không tốt.
f) Ánh xạ hình tròn → elip: các vận tốc khớp có độ lớn bằng 1 (vòng tròn trong không gian khớp) biến thành elip vận tốc ở đầu công cụ (elip vận tốc); kéo tư thế để thấy elip đổi hình dạng, dẹt dần khi gần singularity (giới thiệu cho bài 4.2).
g) Jacobian trong các hệ tọa độ khác nhau: chuyển giữa biểu diễn trong hệ đế và hệ công cụ; hiển thị phép xoay áp dụng cho các khối v và ω; ví dụ công cụ xoay thì J trong hệ công cụ thay đổi.
h) Jacobian hình học và giải tích: robot có hướng đầu công cụ dạng một góc φ (robot phẳng 3 khâu); so sánh ω = φ̇ cho trường hợp phẳng (trùng nhau) và giải thích khác biệt cho 3D bằng sơ đồ ngắn (không cần tương tác phức tạp).
i) Jacobian của UR5 (nếu đủ chỗ): robot 6 trục giả 3D với sáu slider; ma trận 6x6 hiển thị, tô màu hai khối v và ω; bấm cột thì hiện mũi tên ảnh hưởng của khớp đó.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) Jacobian robot 2 khâu l1 = l2 = 1 tại θ = (30°, 45°): đạo hàm riêng rồi ra số cụ thể; (2) với q̇ = (0.2, −0.1) rad/s tính ẋ; (3) dựng Jacobian 6x3 cho robot RRR không gian tại một tư thế cho trước bằng công thức tích có hướng; (4) kiểm tra bằng sai phân số.

CODE PYTHON MẪU (numpy, chạy được): hàm jacobian_2link(q, l1, l2) giải tích; hàm jacobian_geometric(T_list, joint_types) dựng từ danh sách T_0^i của fkine (bài 2.3) với cột v và ω; hàm jacobian_numeric(fk, q, h) sai phân; kiểm tra bằng np.allclose; vận tốc đầu công cụ: xdot = J @ qdot; đối chiếu bằng thư viện: robot.jacob0(q), robot.jacobe(q) của roboticstoolbox (ghi chú hệ tọa độ đế và hệ công cụ), robot.jacob0_analytic(q, analytic='rpy/xyz'); vẽ elip vận tốc bằng matplotlib.

LỖI THƯỜNG GẶP: nhầm thứ tự khối v (trên) và ω (dưới), quên trừ p_{i-1} khi tính tích có hướng, dùng sai trục z_{i-1} (z của khớp trước, theo DH chuẩn), nhầm Jacobian hình học với giải tích khi dùng góc Euler, nhầm hệ tọa độ biểu diễn (jacob0 và jacobe), nhầm đơn vị (rad/s và độ/s), sai phân số chọn h không phù hợp, quên rằng J phụ thuộc q (phải tính lại ở mỗi tư thế).

BÀI TẬP: (1) tính J robot 2 khâu bằng tay rồi bằng code; (2) dựng jacobian_geometric từ fkine ở bài 2.3 và so với robot.jacob0 ở nhiều q ngẫu nhiên; (3) kiểm tra bằng sai phân số; (4) vẽ elip vận tốc tại ba tư thế khác nhau.
TRẮC NGHIỆM (5 câu): ý nghĩa mỗi cột của J, kích thước J của robot 6 trục, công thức cột cho khớp quay, khác biệt Jacobian hình học và giải tích, vì sao J thay đổi theo q.