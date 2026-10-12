CHỦ ĐỀ: Bài 5.1 – Quy hoạch quỹ đạo trong không gian khớp (Joint-space Trajectory)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Đường đi (path) khác quỹ đạo (trajectory): đường đi chỉ là hình học, quỹ đạo là đường đi kèm thời gian q(t)
- Vì sao cần quỹ đạo: nhảy thẳng từ q_A sang q_B tạo vận tốc/gia tốc vô hạn, gây giật, rung, hỏng hộp số; cần q(t) trơn
- Yêu cầu: điều kiện biên (vị trí, vận tốc, gia tốc đầu và cuối), giới hạn vận tốc/gia tốc/jerk của động cơ
- Đa thức bậc 3 (cubic): q(t) = a0 + a1·t + a2·t² + a3·t³; từ 4 điều kiện q(0), q̇(0), q(T), q̇(T) giải ra 4 hệ số (viết công thức tường minh cho trường hợp dừng-dừng: q̇(0) = q̇(T) = 0)
- Đa thức bậc 5 (quintic): thêm điều kiện gia tốc q̈(0), q̈(T) → 6 hệ số; gia tốc liên tục, ít giật hơn
- Hình thang vận tốc (trapezoidal velocity profile): 3 pha tăng tốc – đều – giảm tốc; công thức thời gian t_a, t_c; điều kiện tam giác khi quãng đường ngắn (không đạt vận tốc tối đa)
- Ràng buộc: cho trước q_max, q̇_max, q̈_max → tính thời gian tối thiểu T; với 6 khớp: khớp chậm nhất quyết định T, các khớp khác kéo dài theo (đồng bộ)
- Đồng bộ khớp: tất cả khớp bắt đầu và kết thúc cùng lúc (PTP/MoveJ), dùng chung biến chuẩn hóa s(t) ∈ [0, 1], q(t) = q_A + s(t)·(q_B − q_A)
- So sánh: bậc 3, bậc 5, hình thang, S-curve (giới thiệu, jerk hữu hạn)
- Hệ quả: đường đi của đầu công cụ trong không gian Descartes là đường cong khó đoán (không phải đường thẳng) → mở sang bài 5.2

VÌ SAO CẦN HỌC: mọi lệnh MoveJ/PTP của robot đều dùng quỹ đạo khớp; hiểu nó giúp bạn đặt tốc độ/gia tốc hợp lý và hiểu vì sao robot "mềm" hay "giật".

LƯU Ý KỸ THUẬT: đồ thị vẽ bằng Canvas/SVG tự viết (không dùng thư viện biểu đồ ngoài cdnjs nếu không cần; nếu dùng thì Chart.js từ cdnjs, ghi rõ phiên bản); robot minh họa là robot phẳng 2 khâu (2D) và UR5 giả 3D hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry) chỉ ở minh họa cuối nếu đủ chỗ. Toàn bộ nội suy viết bằng JavaScript thuần.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Nhảy thẳng so với quỹ đạo trơn: robot 1 khớp (một thanh quay) từ 0° đến 90°; chế độ 1: đổi góc tức thời (vận tốc là xung vô hạn, thanh "giật"); chế độ 2: quỹ đạo trơn; hai đồ thị q(t), q̇(t); mũi tên màu thể hiện vận tốc; nút Phát/Dừng/Làm lại.
b) Đa thức bậc 3 từng bước: nhập q0, qT, T, q̇0, q̇T (mặc định 0); hiện hệ phương trình 4x4 và hệ số a0–a3 tính ra; ba đồ thị q(t), q̇(t), q̈(t); thanh trượt thời gian có chấm chạy trên đồ thị, thanh thực hiện đồng bộ trên robot 1 khớp; đánh dấu rằng gia tốc không liên tục ở hai đầu (nhảy bậc).
c) Bậc 3 so với bậc 5: hai bộ đồ thị chồng lên (màu primary và peach); thấy gia tốc của bậc 5 bắt đầu từ 0 và liên tục; slider T để thấy thời gian ngắn thì gia tốc đỉnh lớn; bảng so sánh vận tốc đỉnh, gia tốc đỉnh (số cụ thể).
d) Hình thang vận tốc: slider q̇_max, q̈_max, quãng đường D; đồ thị vận tốc hình thang; ba pha tô màu khác nhau (tăng tốc mint, đều lavender, giảm tốc peach); hiện t_a, t_c, T; khi D nhỏ hoặc q̇_max lớn thì hình thang biến thành hình tam giác kèm thông báo; robot chạy theo.
e) Giới hạn và thời gian tối thiểu: nhập q_max, q̇_max, q̈_max cho từng khớp; bấm "Tính T tối thiểu" → thời gian cho từng khớp; hiển thị khớp nào là "nút cổ chai" (tô đậm); nếu chạy nhanh hơn T_min thì đường vận tốc vượt giới hạn (tô đỏ).
f) Đồng bộ nhiều khớp: robot phẳng 3 khâu hoặc UR5 với 6 khớp; chọn q_A, q_B (bằng slider hoặc preset); so sánh hai chế độ: (1) mỗi khớp tự chạy hết tốc độ (không đồng bộ, khớp ngắn tới trước), (2) đồng bộ theo khớp chậm nhất; đồ thị 6 đường q(t); robot chuyển động; hiển thị vết đầu công cụ cho hai chế độ để thấy đồng bộ cho đường đi "gọn" hơn.
g) Biến chuẩn hóa s(t): đồ thị s(t) với ba hồ sơ (bậc 3, bậc 5, hình thang); chọn một hồ sơ rồi áp dụng cho robot nhiều khớp; thấy q_i(t) = q_A,i + s(t)·Δq_i đều có cùng dạng, chỉ khác biên độ.
h) Đường đi Descartes của quỹ đạo khớp: robot phẳng 2 khâu; từ A tới B trong không gian khớp, vẽ vết đầu công cụ (đường cong, không thẳng); kéo điểm B để thấy vết thay đổi; ghi chú dẫn sang bài 5.2.
i) S-curve (giới thiệu, nếu đủ chỗ): thêm giới hạn jerk, đồ thị gia tốc hình thang thay cho hình chữ nhật; so với hình thang vận tốc.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) đa thức bậc 3 cho q0 = 0, qT = 90° trong T = 2 s, dừng-dừng: tìm a0..a3, vận tốc và gia tốc đỉnh (số cụ thể); (2) quintic cho cùng dữ liệu: hệ số, vận tốc/gia tốc đỉnh; (3) hình thang vận tốc với D = 90°, q̇_max = 60°/s, q̈_max = 120°/s²: tính t_a, t_c, T và kiểm tra; (4) hai khớp Δq1 = 90°, Δq2 = 30° với cùng giới hạn: tính T đồng bộ.

CODE PYTHON MẪU (numpy, chạy được): hàm cubic(q0, qT, T, v0, vT, t) trả về q, q̇, q̈; hàm quintic(...); hàm trapezoid(D, vmax, amax) trả về hàm s(t) và T (xử lý trường hợp tam giác); hàm sync_joint_trajectory(qA, qB, vmax, amax) cho nhiều khớp; vẽ q(t), q̇(t), q̈(t) bằng matplotlib; đối chiếu bằng roboticstoolbox: rtb.jtraj(q0, qf, t) (đa thức bậc 5), rtb.trapezoidal(q0, qf, t), rtb.quintic; ví dụ với UR5 từ bài 2.3: nội suy 100 điểm rồi vẽ vết đầu công cụ bằng fkine.

LỖI THƯỜNG GẶP: nhầm đường đi với quỹ đạo, quên điều kiện vận tốc đầu/cuối bằng 0 (dừng-dừng) khi cần, bậc 3 gây gia tốc không liên tục, chọn T quá nhỏ làm vượt giới hạn động cơ, chỉ kiểm tra giới hạn vận tốc mà quên gia tốc, quên trường hợp tam giác của hình thang, không đồng bộ các khớp, nhầm đơn vị (độ/s và rad/s), lấy mẫu thời gian quá thưa làm mất điều kiện biên, tưởng đường đi đầu công cụ là đường thẳng.

BÀI TẬP: (1) cài cubic và quintic, vẽ q, q̇, q̈; (2) cài trapezoid và kiểm tra trường hợp tam giác; (3) tạo quỹ đạo đồng bộ cho 6 khớp UR5 từ q_A đến q_B với giới hạn cho trước, tìm T tối thiểu; (4) vẽ vết đầu công cụ của quỹ đạo và nhận xét; (5) so sánh với rtb.jtraj.
TRẮC NGHIỆM (5 câu): khác biệt đường đi và quỹ đạo, số điều kiện biên của bậc 3 và bậc 5, vì sao bậc 5 mượt hơn, trường hợp tam giác của hình thang, khớp nào quyết định thời gian khi đồng bộ.