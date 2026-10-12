CHỦ ĐỀ: Bài 2.3 – Tính và kiểm chứng động học thuận (Forward Kinematics) cho robot 6 trục

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Công thức tổng: T_0^6 = A_1·A_2·A_3·A_4·A_5·A_6 (nhân từ đế tới đầu công cụ)
- Đọc kết quả từ ma trận 4x4: cột 4 là vị trí (x, y, z), ma trận con 3x3 là hướng; trích ra RPY/quaternion (liên hệ bài 1.3)
- Viết hàm fkine(q) cho robot 6 trục từ bảng DH, đầu vào là vector 6 góc, đầu ra là ma trận 4x4
- Thêm hệ tọa độ công cụ (tool, TCP) và hệ tọa độ đế (base): T = T_base · T_0^6 · T_tool
- Tính vị trí các khớp trung gian (để vẽ robot): ma trận T_0^1, T_0^2, ..., T_0^6
- Kiểm chứng kết quả: so sánh với roboticstoolbox, thử các tư thế đặc biệt (q = 0, tư thế thẳng đứng, tư thế đã biết kết quả), kiểm tra tính chất ma trận xoay
- Cách debug khi kết quả sai: kiểm tra từng A_i, vẽ các hệ tọa độ trung gian, so với hình robot thật
- Không gian khớp → không gian làm việc: một bộ q cho một T duy nhất (khác IK có nhiều nghiệm)
- Offset khớp (joint offset) và giới hạn khớp trong mô hình

VÍ DỤ ROBOT: UR5 (DH chuẩn: d1 = 0.0892, a2 = -0.425, a3 = -0.392, d4 = 0.1093, d5 = 0.0947, d6 = 0.0823; α = [π/2, 0, 0, π/2, -π/2, 0]) làm ví dụ chính; Puma 560 làm ví dụ thứ hai. Hãy kiểm tra các số này khi viết và ghi chú nếu có chỗ không chắc chắn.

VÌ SAO CẦN HỌC: đây là cầu nối từ lý thuyết sang robot thật: biết góc 6 động cơ thì biết chính xác đầu công cụ đang ở đâu và hướng nào; mọi bài sau (IK, Jacobian, quỹ đạo) đều dựa trên fkine.

LƯU Ý KỸ THUẬT: dùng three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry), robot vẽ từ các hình hộp/trụ đơn giản đặt theo ma trận tính bằng JavaScript (tự viết hàm nhân ma trận 4x4 và hàm fkine bằng JS). Nếu không ổn, giả 3D bằng Canvas.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Nhân ma trận dây chuyền: sáu khối ma trận A_1 đến A_6 xếp thành hàng; animation nhân dần từng ma trận (A_1, rồi A_1A_2, ...) và hệ tọa độ tương ứng trên robot sáng lên theo từng bước; nút Bước tiếp, Phát, Làm lại, tốc độ.
b) Robot 6 trục 3D (UR5) có sáu slider J1-J6 (đúng giới hạn khớp), hiển thị theo thời gian thực: ma trận T_0^6 với các khối tô màu (R xanh dương nhạt, p cam đào), vị trí x, y, z, RPY; hệ tọa độ trên từng khâu có thể bật/tắt; đường vết của đầu công cụ (nút bật/tắt, nút xóa).
c) Tư thế đặc biệt: các nút preset (Home, Duỗi thẳng lên, Duỗi ngang, Tư thế "chữ L", Tư thế gần singularity); bấm nút thì robot chuyển động mượt sang tư thế đó, bảng kết quả cập nhật, kèm câu hỏi dự đoán "đầu công cụ sẽ ở khoảng nào?" rồi bấm hiện đáp án.
d) Bảng DH → robot: bảng DH của UR5 hiển thị bên cạnh robot, khi chọn một dòng thì khâu và hệ tọa độ tương ứng sáng lên; cho phép sửa giá trị (ví dụ thay d4) và thấy robot thay đổi hình dạng.
e) Hệ tọa độ công cụ: thêm TCP bằng ô nhập (x, y, z, xoay) gắn vào mặt bích; hiển thị đầu công cụ mới (ví dụ kẹp dài 150 mm); thấy vị trí TCP thay đổi nhưng mặt bích không đổi; đồng thời cho nhập hệ tọa độ đế để thấy T_base ảnh hưởng.
f) Quét không gian làm việc: nút chạy ngẫu nhiên nhiều bộ q hợp lệ, vẽ đám mây điểm vị trí đầu công cụ (ít nhất vài nghìn điểm, dùng Points hoặc Canvas), xoay được; so sánh với bài 2.1.
g) Công cụ debug: danh sách hệ tọa độ T_0^1...T_0^6, người học chọn một cái để xem vị trí/hướng; nút "Tìm lỗi": mô phỏng một robot có sai số (ví dụ nhầm dấu α hoặc thiếu offset), so sánh với robot đúng, hiện độ lệch vị trí đầu công cụ.
h) Đối chiếu code ↔ hình: khung code Python (fkine bằng numpy) có dòng sáng lên đồng bộ khi animation (a) chạy.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) fkine robot 2 khâu phẳng l1 = l2 = 1, θ = (30°, 45°), nhân từng ma trận; (2) fkine UR5 ở q = 0, giải thích vì sao đầu công cụ ở vị trí đó (nhìn từng khâu); (3) tính một T cụ thể rồi đọc ra vị trí, RPY, so với kết quả của rtb.

CODE PYTHON MẪU (đầy đủ, chạy được): hàm dh_matrix, hàm fkine(q, dh_table) nhân dây chuyền và trả về cả danh sách ma trận trung gian; class UR5 dùng lại class Robot ở bài 0.3; so sánh np.allclose với rtb.models.DH.UR5().fkine(q) (lưu ý: một số mô hình UR5 trong thư viện dùng quy ước khác, hãy ghi chú cách kiểm tra); thêm tool/base; vẽ robot bằng matplotlib 3D từ các T trung gian; hàm kiểm tra is_rotation; test với assert.

LỖI THƯỜNG GẶP: nhân sai thứ tự, thiếu offset θ, nhầm đơn vị (mm/m, độ/rad), nhầm DH chuẩn/cải tiến khi so với thư viện, lấy sai khối p khi đọc vị trí, quên hệ tool/base, so sánh số thực bằng == thay vì np.allclose, tin kết quả khi chưa vẽ hình kiểm tra.

BÀI TẬP: (1) tự viết fkine cho UR5 và so sánh với rtb ở 10 bộ q ngẫu nhiên; (2) vẽ robot 3D từ các T trung gian bằng matplotlib; (3) thêm một công cụ dài 0.15 m và tính vị trí đầu công cụ; (4) vẽ đường đi của đầu công cụ khi J1 quay 0→90°.
TRẮC NGHIỆM (5 câu): thứ tự nhân ma trận, đọc vị trí từ T, ý nghĩa T_base và T_tool, một tư thế đơn giản cho kết quả gì, nguyên nhân thường gặp khi fkine sai.