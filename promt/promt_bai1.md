CHỦ ĐỀ: Bài 0.2 – Lượng giác và giải tích cho robotics

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Đường tròn đơn vị, sin, cos, tan; độ và radian, đổi qua lại
- Các công thức cộng góc sin(a+b), cos(a+b) (dùng rất nhiều trong động học)
- atan2(y, x) khác atan(y/x) ở đâu và vì sao robotics luôn dùng atan2
- Định lý cos (dùng trong IK robot 2 khâu)
- Đạo hàm là độ dốc/tốc độ thay đổi; đạo hàm riêng của hàm nhiều biến
- Quy tắc chuỗi (chain rule) và liên hệ với Jacobian

VÌ SAO CẦN HỌC: vị trí đầu cánh tay 2 khâu là x = l1·cosθ1 + l2·cos(θ1+θ2), y = l1·sinθ1 + l2·sin(θ1+θ2); đạo hàm của nó theo góc chính là Jacobian sau này.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Đường tròn đơn vị: slider góc (độ và radian hiển thị song song), điểm chạy trên đường tròn, chiếu xuống trục để thấy cos và sin; bên cạnh vẽ đồ thị sin/cos được "vẽ dần" theo góc (animation phát/dừng).
b) Công thức cộng góc: hai slider a và b, vẽ hình hình học minh họa cos(a+b) và so sánh kết quả hai vế.
c) atan vs atan2: kéo điểm (x, y) khắp 4 góc phần tư; hiển thị song song kết quả atan(y/x) (sai ở góc phần tư II, III) và atan2(y, x) (đúng), tô màu vùng sai.
d) Đạo hàm: đồ thị hàm số, kéo điểm để thấy tiếp tuyến và độ dốc; animation đường cát tuyến co dần thành tiếp tuyến khi h → 0 (slider h).
e) Đạo hàm riêng: hàm f(x, y) vẽ dạng bản đồ nhiệt (heatmap) hoặc đường đồng mức; kéo điểm, hiển thị ∂f/∂x và ∂f/∂y bằng hai mũi tên, và vector gradient.
f) Cánh tay phẳng 2 khâu: hai slider θ1, θ2 (và độ dài khâu), hiển thị tọa độ đầu (x, y); bảng 4 số ∂x/∂θ1, ∂x/∂θ2, ∂y/∂θ1, ∂y/∂θ2 cập nhật theo thời gian thực, kèm mũi tên vận tốc đầu cánh tay khi tăng nhẹ từng góc. Ghi chú: "Bảng 2x2 này chính là Jacobian".

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) đổi 135° sang radian; (2) tính atan2 cho điểm (-1, 1) và so với atan; (3) đạo hàm riêng của x(θ1, θ2) cho cánh tay 2 khâu.

CODE PYTHON MẪU: np.deg2rad, np.rad2deg, np.sin, np.cos, np.arctan2, đạo hàm số bằng sai phân (f(x+h)-f(x))/h, matplotlib vẽ sin/cos và vẽ cánh tay 2 khâu.

LỖI THƯỜNG GẶP: quên đổi độ sang radian, dùng arctan thay arctan2, nhầm tham số của arctan2 (y trước, x sau), sai số khi arccos gần ±1, đạo hàm số chọn h quá lớn hoặc quá nhỏ.

BÀI TẬP: (1) tính đạo hàm riêng của hàm 2 biến rồi vẽ bằng matplotlib; (2) viết hàm tính vị trí đầu cánh tay 2 khâu; (3) kiểm tra đạo hàm giải tích bằng sai phân số.
TRẮC NGHIỆM (5 câu): đổi độ sang radian, kết quả atan2 ở các góc phần tư, công thức cộng góc, ý nghĩa đạo hàm, ý nghĩa đạo hàm riêng.