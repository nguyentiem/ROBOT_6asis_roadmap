CHỦ ĐỀ: Bài 4.4 – Quan hệ lực và moment: τ = Jᵀ·F (tĩnh học robot)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Vector lực/moment đầu công cụ (wrench): F = [f; n] gồm lực 3 thành phần và moment 3 thành phần
- Nguyên lý công ảo: công khớp bằng công ở đầu công cụ: τᵀ·δq = Fᵀ·δx với δx = J·δq → τ = Jᵀ(q)·F
- Ý nghĩa: biết lực cần tạo ở đầu công cụ thì tính được moment từng động cơ; ngược lại biết moment khớp thì tính lực đầu công cụ: F = (Jᵀ)⁻¹·τ (khi J không suy biến)
- Tính đối ngẫu vận tốc – lực: elip lực (force ellipse) vuông góc/nghịch đảo với elip vận tốc; hướng robot chuyển động nhanh (σ lớn) thì tạo lực yếu, hướng chuyển động chậm (σ nhỏ) thì tạo lực mạnh
- Singularity và lực: gần singularity robot chịu được lực rất lớn theo hướng mất chuyển động (khâu chịu lực bằng kết cấu, motor ít tải) nhưng không thể tạo lực theo hướng khác; liên hệ bài 4.2
- Bù trọng lực tĩnh: moment cần để giữ robot đứng yên τ_g = Jcᵀ·m·g (khối tâm), giới thiệu chi tiết ở bài 7.1
- Hệ tọa độ: F biểu diễn trong hệ đế hoặc hệ công cụ, J và F phải cùng hệ
- Ứng dụng: ước lượng lực chạm từ moment động cơ (không cần cảm biến lực), kiểm tra tải của động cơ cho một tư thế, đánh bóng/mài với lực không đổi, hợp tác an toàn (cobot)
- Chọn tư thế để giảm moment lên động cơ (tư thế tốt cho tải nặng)

VÌ SAO CẦN HỌC: chọn động cơ/hộp số cho robot, kiểm tra robot có nâng nổi vật nặng ở tư thế đó không, điều khiển lực khi mài hoặc lắp ráp, và hiểu vì sao cobot ước lượng được va chạm.

LƯU Ý KỸ THUẬT: robot phẳng 2 khâu vẽ 2D bằng SVG/Canvas (tự viết FK, Jacobian, nhân ma trận nhỏ, hàm nghịch đảo 2x2); robot 3 trục không gian hoặc UR5 dùng giả 3D hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry) chỉ ở minh họa cuối nếu đủ chỗ.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Nguyên lý công ảo bằng hình: robot phẳng 2 khâu; đẩy nhẹ đầu công cụ một đoạn δx bằng lực F; hiện công của lực ở đầu công cụ và công của moment khớp (τ1·δθ1 + τ2·δθ2); hai giá trị bằng nhau; animation chuyển động ảo nhỏ, hiển thị đẳng thức τᵀδq = Fᵀδx.
b) Tính moment từ lực: kéo mũi tên lực F ở đầu công cụ (độ lớn và hướng); hiển thị moment cần của từng khớp (thanh màu dương/âm ở mỗi khớp, ký hiệu mũi tên cong quay chiều nào); phép tính τ = Jᵀ·F với số cụ thể cập nhật tức thì; kéo tư thế để thấy moment đổi theo.
c) Cánh tay dài cần moment lớn: đặt vật nặng ở đầu công cụ (slider khối lượng); duỗi tay ra xa thì moment J1/J2 tăng; đồ thị moment theo góc; đường giới hạn moment động cơ (đỏ) và vùng "robot không nâng nổi"; gợi ý tư thế gập tay gần thân thì nhẹ hơn.
d) Elip lực và elip vận tốc: hai elip vẽ chồng ở đầu công cụ (vận tốc màu mint, lực màu lavender); trục dài của cái này là trục ngắn của cái kia; kéo tư thế để thấy; ghi chú hướng "nhanh – yếu" và "chậm – khỏe".
e) Gần singularity: robot duỗi thẳng; đẩy vào đầu công cụ theo dọc cánh tay: moment khớp gần như bằng 0 (lực truyền qua kết cấu); đẩy vuông góc thì moment lớn; hiển thị τ cho các hướng lực khác nhau trong "la bàn" hướng lực (vẽ độ lớn moment theo hướng đẩy dạng đồ thị cực).
f) Tính lực từ moment: đặt moment động cơ τ (slider hai khớp); tính lực đầu công cụ F = (Jᵀ)⁻¹τ; mũi tên lực ở đầu công cụ; cảnh báo khi det J ≈ 0 (không tính được hoặc rất lớn); bài toán ngược để ước lượng lực chạm từ dòng điện động cơ.
g) Bù trọng lực tĩnh: robot 2 khâu có khối lượng m1, m2 ở trung tâm mỗi khâu (slider); tính moment giữ đứng yên ở từng tư thế bằng công thức τ_g; đồ thị τ_g theo θ2; kéo tư thế để thấy thay đổi; liên hệ bài 7.1 (động lực học).
h) Cặp nhiệm vụ "mài với lực không đổi": đầu công cụ ép xuống bề mặt với lực F_n mong muốn; hiển thị moment cần cho từng khớp khi trượt dọc bề mặt; thấy rằng cùng một lực nhưng moment thay đổi theo vị trí.
i) Hệ tọa độ của F (nếu đủ chỗ): chuyển F giữa hệ đế và hệ công cụ bằng phép xoay; hiển thị cả J ở hệ đế và hệ công cụ; chọn sai hệ → moment sai (thấy rõ trên hình).
j) Ví dụ 3D (nếu đủ chỗ): robot 6 trục giả 3D; nhập F (6 thành phần) bằng slider, bảng 6 moment khớp; thanh giới hạn moment từng khớp; khớp nào quá tải tô đỏ.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) robot 2 khâu l1 = l2 = 1 ở θ = (30°, 60°) giữ lực F = (0, −10) N (10 N hướng xuống): tính J, Jᵀ và τ; (2) cùng tư thế, tính lực đầu công cụ khi τ = (5, 2) N·m; (3) robot duỗi thẳng θ = (0°, 0°) chịu lực dọc theo cánh tay và vuông góc: so sánh moment; (4) tính moment bù trọng lực đơn giản cho một khâu dài l = 1 m, m = 2 kg ở góc 30° và 90°.

CODE PYTHON MẪU (numpy, chạy được): hàm joint_torque(J, F) = J.T @ F; hàm tip_force(J, tau) dùng np.linalg.solve hoặc lstsq và kiểm tra det; hàm gravity_torque_2link(q, m1, m2, l1, l2, g); vẽ đồ thị moment theo tư thế bằng matplotlib; vẽ elip lực/vận tốc bằng SVD của J; đối chiếu với roboticstoolbox: robot.jacob0(q).T @ F, robot.gravload(q), robot.pay(W, q) (tính moment khớp do tải); ví dụ UR5 với tải 2 kg ở vài tư thế và kiểm tra moment so với giới hạn khớp (ghi chú: số khối lượng/quán tính cụ thể cần lấy từ mô hình hoặc datasheet).

LỖI THƯỜNG GẶP: nhầm J với Jᵀ, trộn hệ tọa độ giữa J và F, quên rằng F có moment (wrench 6 thành phần), nhầm đơn vị (N và N·m, mm và m), bỏ qua trọng lực của chính robot khi tính tải, tin rằng gần singularity robot "yếu" ở mọi hướng, nhầm dấu moment, quên tính bù trọng lực khi ước lượng lực chạm từ dòng điện, nghịch đảo Jᵀ khi suy biến.

BÀI TẬP: (1) tính τ = JᵀF cho robot 2 khâu bằng tay và bằng code, đối chiếu rtb; (2) vẽ moment khớp theo góc θ2 khi giữ lực không đổi; (3) vẽ elip lực và vận tốc ở 3 tư thế; (4) tính moment bù trọng lực của robot 2 khâu; (5) với UR5 và tải 2 kg, kiểm tra tư thế nào vượt moment cho phép.
TRẮC NGHIỆM (5 câu): công thức τ = JᵀF, ý nghĩa nguyên lý công ảo, quan hệ elip lực và elip vận tốc, điều gì xảy ra với moment khi robot duỗi thẳng và chịu lực dọc cánh tay, vì sao F và J phải cùng hệ tọa độ.