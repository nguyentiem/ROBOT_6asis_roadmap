CHỦ ĐỀ: Bài 3.2 – IK giải tích cho robot 6 trục có cổ tay cầu (tách vị trí và hướng, Pieper)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Điều kiện Pieper: ba trục cổ tay cắt nhau tại một điểm (cổ tay cầu) → có thể tách bài toán thành hai phần
- Ý tưởng: 3 khớp đầu quyết định vị trí tâm cổ tay (wrist center); 3 khớp cuối quyết định hướng
- Bước 1: từ T đích (R, p) tìm tâm cổ tay: p_wc = p − d6·R·[0, 0, 1]ᵀ (trừ đoạn từ cổ tay tới mặt bích, và cả tool nếu có)
- Bước 2: giải θ1, θ2, θ3 từ p_wc (bài toán định vị: vai, khuỷu; hình học phẳng + định lý cos như bài 3.1; θ1 từ atan2(y_wc, x_wc), có hai nghiệm vai trái/phải)
- Bước 3: tính R_0^3 từ θ1, θ2, θ3 (dùng FK 3 khớp đầu)
- Bước 4: R_3^6 = (R_0^3)ᵀ · R_0^6; giải θ4, θ5, θ6 như bài toán Euler (ZYZ hoặc tương tự) từ R_3^6, có hai nghiệm (cổ tay lật/không), xử lý trường hợp suy biến θ5 = 0 (θ4 và θ6 phụ thuộc nhau)
- Tổng hợp: tối đa 2 (vai) × 2 (khuỷu) × 2 (cổ tay) = 8 nghiệm
- Kiểm tra từng nghiệm bằng FK, loại nghiệm vi phạm giới hạn khớp

VÍ DỤ ROBOT: ưu tiên robot đơn giản để dạy được: (i) robot RRR phẳng/không gian 3 trục để học bước 2; (ii) robot cổ tay cầu kiểu Puma/Stanford đơn giản hóa hoặc kiểu "RRR + cổ tay cầu" với bảng DH ghi rõ; (iii) UR5 chỉ giới thiệu ở mức khái niệm vì công thức dài (có offset, cổ tay không phải cầu thuần theo DH). Hãy tự kiểm tra công thức bằng cách chạy FK ngược lại trong JS và ghi chú rõ mô hình robot dùng.

VÌ SAO CẦN HỌC: IK giải tích cho tất cả nghiệm, nhanh (micro-giây), không phụ thuộc điểm khởi tạo; đây là cách robot công nghiệp tính trong bộ điều khiển.

LƯU Ý KỸ THUẬT: dùng three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry); hàm FK và IK viết bằng JavaScript thuần (tự viết nhân ma trận 4x4, atan2, acos có kẹp [-1, 1]). Nếu 3D quá nặng, giả 3D bằng Canvas.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Tổng quan thuật toán: sơ đồ 4 bước dạng flowchart (đích T → tâm cổ tay → θ1θ2θ3 → R_3^6 → θ4θ5θ6); bấm từng bước thì hình robot hiển thị phần tương ứng sáng lên; nút Phát chạy toàn bộ.
b) Cổ tay cầu: hình ba trục cổ tay cắt nhau tại tâm cổ tay (chấm lớn màu lavender); xoay J4, J5, J6 thì tâm cổ tay đứng yên còn mặt bích xoay; so sánh với cổ tay không cầu (tâm cổ tay lệch theo khi xoay) để thấy vì sao không tách được.
c) Tìm tâm cổ tay: kéo đích (vị trí và hướng) bằng slider/ô nhập; mũi tên lùi từ đầu công cụ theo trục z của công cụ một đoạn d6 để ra tâm cổ tay; hiển thị phép tính p_wc = p − d6·R·z.
d) Giải 3 khớp đầu: nhìn từ trên xuống để thấy θ1 (hai nghiệm vai trái/phải); nhìn nghiêng để thấy tam giác vai-khuỷu-tâm cổ tay và θ2, θ3 (hai nghiệm khuỷu lên/xuống); animation dựng từng bước, hiện công thức và số liệu.
e) Giải cổ tay: hiển thị R_0^3, R_0^6, R_3^6 = R_0^3ᵀ·R_0^6 (tô màu); các góc θ4, θ5, θ6 tính ra, hai nghiệm cổ tay (θ5 dương/âm); nút chuyển giữa hai nghiệm với animation; cảnh báo vàng khi θ5 ≈ 0 (suy biến).
f) Bộ giải hoàn chỉnh: kéo đích (3 slider vị trí + 3 slider RPY); nút "Giải IK" liệt kê tất cả nghiệm (tối đa 8) trong bảng, tô xám nghiệm vi phạm giới hạn khớp; bấm một dòng thì robot hiển thị nghiệm đó; mỗi dòng có cột "sai số FK".
g) Duyệt 8 nghiệm: nút Trước/Sau và Phát tự động, robot chuyển mượt giữa các nghiệm nhưng đầu công cụ luôn ở cùng đích (có bóng mờ đích); hiển thị ghi chú loại nghiệm.
h) Vùng ngoài tầm: kéo đích ra xa thì thuật toán dừng ở bước 2 với thông báo cụ thể (ngoài tầm với của tâm cổ tay), phân biệt với trường hợp tâm cổ tay trong tầm nhưng hướng đạt được vẫn xử lý được (cổ tay cầu luôn đạt mọi hướng nếu không giới hạn khớp).

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) robot RRR không gian 3 trục: cho p_wc = (0.5, 0.3, 0.4), tính θ1, θ2, θ3 (cả nghiệm khuỷu lên/xuống), kiểm tra bằng FK; (2) tính θ4, θ5, θ6 từ một R_3^6 cụ thể (ví dụ ma trận xoay đơn giản), xử lý trường hợp θ5 = 0; (3) một bài hoàn chỉnh: cho T đích của robot cổ tay cầu, đi từ bước 1 đến 4, liệt kê vài nghiệm và kiểm tra FK.

CODE PYTHON MẪU (numpy, chạy được): hàm wrist_center(T, d6); hàm ik_position(p_wc, ...) trả về danh sách (θ1, θ2, θ3); hàm ik_orientation(R36) trả về hai nghiệm (θ4, θ5, θ6) và xử lý θ5 ≈ 0; hàm ik_6dof(T) ghép lại và lọc nghiệm bằng giới hạn khớp và kiểm tra FK với np.allclose; ví dụ chạy với một bộ q ngẫu nhiên: FK → T → IK, kiểm tra q gốc nằm trong danh sách nghiệm; đối chiếu bằng thư viện: robot.ikine_a() của roboticstoolbox cho robot có cổ tay cầu (ví dụ Puma 560: rtb.models.DH.Puma560().ikine_a(T, config="lun")), hoặc ur_ikfast / ikpy nếu cần.

LỖI THƯỜNG GẶP: quên trừ d6 (và tool) khi tìm tâm cổ tay, nhầm trục z của công cụ, quên bù offset θ trong bảng DH, nhầm dấu khi giải θ2, θ3 (phụ thuộc DH và quy ước khớp), không xử lý θ5 = 0 gây chia cho 0 hoặc nhảy góc, quên chuẩn hóa góc về (−π, π], không kiểm tra giới hạn khớp, chỉ kiểm tra sai số vị trí mà bỏ qua sai số hướng, áp dụng cho cổ tay không phải cầu.

BÀI TẬP: (1) giải IK cho robot RRR 3 trục không gian và kiểm tra bằng FK; (2) viết hàm ik_orientation cho cổ tay ZYZ, thử cả trường hợp suy biến; (3) ghép thành ik_6dof cho robot cổ tay cầu và kiểm tra vòng tròn FK→IK→FK trên 100 điểm ngẫu nhiên; (4) so sánh với ikine_a của rtb.
TRẮC NGHIỆM (5 câu): vì sao cổ tay cầu cho phép tách bài toán, công thức tâm cổ tay, tối đa bao nhiêu nghiệm và từ đâu mà ra, điều gì xảy ra khi θ5 = 0, thứ tự các bước giải.