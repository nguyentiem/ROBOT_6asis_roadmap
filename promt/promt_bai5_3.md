CHỦ ĐỀ: Bài 5.3 – Quỹ đạo qua nhiều điểm (waypoints), spline và làm trơn góc (blending)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Bài toán: robot phải đi qua chuỗi điểm A → B → C → D (waypoints) thay vì chỉ hai điểm
- Cách ngây thơ: nối từng đoạn dừng-dừng (stop-and-go): dừng tại mỗi điểm, mất thời gian, rung, hao mòn
- Yêu cầu: vận tốc liên tục (C¹), gia tốc liên tục (C²) tại các điểm nối; đi qua điểm (pass through) hay chỉ lại gần (via/near)
- Nội suy qua điểm bằng đa thức từng đoạn: chọn vận tốc tại điểm trung gian (ước lượng bằng trung bình độ dốc hai đoạn kề, hoặc 0 tại điểm dừng) rồi dùng cubic từng đoạn
- Spline bậc 3 tự nhiên (cubic spline): 4n hệ số, điều kiện liên tục vị trí/vận tốc/gia tốc tại các nút, điều kiện biên (natural: gia tốc = 0 ở đầu mút, hoặc clamped: vận tốc cho trước); giải hệ tam đường chéo; tính chất: mượt nhất nhưng có thể "vọt" (overshoot) giữa các điểm
- Gán thời gian cho từng đoạn: chia đều, hoặc theo khoảng cách (chord-length), hoặc theo giới hạn vận tốc
- Làm trơn góc (blending/zone/fine-coarse): đường đi cắt góc trong bán kính r quanh waypoint, tránh dừng; đoạn thẳng – cung chuyển tiếp – đoạn thẳng; nội suy parabol tại điểm nối (linear segments with parabolic blends – LSPB)
- Đánh đổi: bán kính blending lớn thì mượt, nhanh nhưng sai lệch so với điểm gốc; lớn quá thì cắt qua vật cản; các hãng gọi là "approximate positioning", "zone data" (ABB: z10, z50), "CNT" (Fanuc), "blend radius" (UR)
- Điểm dừng chính xác (fine point) và điểm đi qua (via point): dùng khi cần gắp/thả chính xác
- Kết hợp với quỹ đạo khớp và Descartes (5.1, 5.2): spline trong không gian khớp (đơn giản, tránh singularity) hoặc trong không gian Descartes (đường tool path)
- Vị trí và hướng: spline vị trí, nội suy hướng riêng (squad hoặc slerp từng đoạn, giới thiệu)
- Tối ưu thời gian (giới thiệu): thời gian tối thiểu qua waypoints với giới hạn khớp (TOPP-RA, Ruckig – ở mức tên gọi để tham khảo)
- Kiểm tra trước khi chạy: va chạm, giới hạn khớp, vận tốc/gia tốc đỉnh toàn quỹ đạo

VÌ SAO CẦN HỌC: pick-and-place thực tế có điểm tiếp cận, điểm gắp, điểm nâng, điểm thả; nếu dừng ở mỗi điểm thì chu kỳ chậm gấp 2-3 lần; blending là cách chạy mượt, nhanh và là tham số hay bị chỉnh sai nhất trên robot thật.

LƯU Ý KỸ THUẬT: robot phẳng 2 khâu vẽ 2D bằng Canvas (tự viết IK 2 khâu), đồ thị tự vẽ; thuật toán spline giải hệ tam đường chéo tự viết bằng JavaScript; UR5 giả 3D hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry) chỉ ở minh họa cuối nếu đủ chỗ.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Dừng-dừng so với liên tục: đặt 4-5 waypoint bằng cách bấm lên canvas (kéo để đổi chỗ, bấm phải hoặc nút để xóa); hai khung: (1) dừng ở mỗi điểm (đồ thị vận tốc có các "hố" về 0), (2) đi liên tục; hiển thị tổng thời gian và so sánh (ví dụ "nhanh hơn 35%"); Phát/Dừng/Làm lại.
b) Spline bậc 3 từng bước: các waypoint (trong không gian khớp 1 khớp hoặc vị trí 1D theo thời gian); hiển thị từng đoạn đa thức với màu khác nhau, đồ thị q(t), q̇(t), q̈(t) thấy liên tục tại các nút; slider chọn điều kiện biên (natural/clamped); tô đỏ chỗ không liên tục khi dùng ghép cubic ngây thơ (vận tốc nhảy bậc) để so sánh.
c) Overshoot của spline: kéo một waypoint cao lên, spline vọt qua giá trị lớn nhất/nhỏ nhất giữa điểm; hiển thị đường giới hạn khớp (đỏ) bị vượt; so sánh spline tự nhiên với nội suy bảo toàn đơn điệu (PCHIP) và nội suy tuyến tính; giải thích đánh đổi mượt – bám sát.
d) Gán thời gian cho đoạn: so sánh chia đều thời gian và chia theo khoảng cách; waypoint cách nhau không đều; đồ thị vận tốc thấy khác biệt; vết quỹ đạo thay đổi (chia đều thời gian có thể làm đường đi bị lệch hoặc vọt).
e) Blending trong 2D: đường đi qua ba điểm A, B, C với góc ở B; slider bán kính blending r từ 0 (dừng chính xác tại B) đến lớn; hiển thị đoạn thẳng, cung chuyển tiếp hoặc parabol; điểm B bị "cắt góc"; vẽ khoảng cách từ đường đi thực tới B (sai lệch) bằng số và hình tròn zone quanh B; đồ thị vận tốc cho thấy không còn hố khi blending.
f) LSPB (linear segment with parabolic blends): trục 1D qua nhiều điểm; mỗi điểm có đoạn parabol chuyển tiếp; hiển thị gia tốc là các xung chữ nhật; slider thời gian blending t_b; đồ thị q, q̇, q̈; thấy vận tốc liên tục nhưng gia tốc nhảy bậc.
g) Pick-and-place thực tế: robot phẳng 2 khâu với 5 điểm (xuất phát, tiếp cận, gắp, nâng, thả) và vật; so sánh "mọi điểm là fine" (chậm) với "các điểm trung gian dùng blending, điểm gắp/thả là fine"; vết quỹ đạo, tổng thời gian, sai lệch tại điểm gắp; cảnh báo khi blending lớn khiến đầu công cụ cắt vào vật cản hoặc bàn (vẽ vật cản).
h) Tên gọi của các hãng: bảng tra cứu (ABB zone z10/z50/fine, Fanuc CNT0-100/FINE, KUKA C_DIS/C_PTP, UR blend radius, Dobot, Yaskawa PL); chọn hãng thì giải thích ý nghĩa tham số tương ứng bằng hình mini; không cần số liệu chính xác, nếu không chắc thì ghi chú "xem tài liệu hãng".
i) Đồ thị tổng hợp 6 khớp: UR5 hoặc robot 3 khâu; nhiều waypoint trong không gian khớp; vẽ q(t), q̇(t), q̈(t) toàn bộ; đường giới hạn vận tốc và gia tốc; điểm vi phạm tô đỏ; nút "Giãn thời gian": kéo dài thời gian đến khi hợp lệ (dùng hệ số tỉ lệ t' = k·t).
j) So sánh nhiều phương pháp: bảng chạy cùng bộ waypoint với 4 phương pháp (dừng-dừng, cubic ghép vận tốc trung bình, spline tự nhiên, LSPB/blending) và hiển thị: tổng thời gian, vận tốc đỉnh, gia tốc đỉnh, sai lệch so với waypoint, tính liên tục; đồ thị chồng.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) spline bậc 3 qua 3 điểm (t, q) = (0, 0), (1, 1), (2, 0) với điều kiện natural: giải hệ cho các hệ số (hãy tính chính xác và kiểm tra liên tục); (2) cubic ghép với vận tốc trung bình qua ba điểm 0°, 60°, 30° với T mỗi đoạn 1 s: tìm vận tốc tại điểm giữa và hệ số; (3) blending hai đoạn thẳng vuông góc ở B với bán kính r = 0.1 m: tính điểm bắt đầu/kết thúc blending và sai lệch lớn nhất so với B (cung tròn: r·(√2−1) với bán kính cung phù hợp; ghi rõ cách định nghĩa r và tính chính xác); (4) so sánh thời gian dừng-dừng và liên tục cho ví dụ số.

CODE PYTHON MẪU (numpy, chạy được): hàm cubic_via(q_points, t_points, v_mode) với vận tốc trung gian ước lượng; hàm natural_cubic_spline(t, q) giải hệ tam đường chéo (np.linalg.solve hoặc thuật toán Thomas) và hàm đánh giá; đối chiếu bằng scipy.interpolate.CubicSpline(t, q, bc_type='natural') và PchipInterpolator; hàm lspb(q_points, t_points, t_blend); hàm blend_corner(A, B, C, r) cho đường thẳng–cung tròn–đường thẳng trong không gian Descartes; hàm check_limits(t, q, vmax, amax) và time_scale(...); đối chiếu bằng roboticstoolbox: rtb.mstraj(viapoints, dt, tacc, qdmax) (LSPB nhiều điểm), rtb.mtraj; nhắc tới thư viện tối ưu thời gian (toppra, ruckig) ở mức giới thiệu và ví dụ gọi ngắn nếu có; vẽ q(t), q̇(t), q̈(t) bằng matplotlib.

LỖI THƯỜNG GẶP: nối cubic từng đoạn dừng-dừng rồi tưởng đã mượt, vận tốc nhảy bậc ở điểm nối khi ghép cubic ngây thơ, spline vọt vượt giới hạn khớp hoặc vọt vào vật cản, chia thời gian đều cho các đoạn có độ dài rất khác nhau, đặt bán kính blending lớn hơn nửa đoạn ngắn nhất (đường chồng chéo), blending ở điểm gắp/thả làm mất độ chính xác, nhầm "điểm đi qua" với "điểm lại gần", chỉ kiểm tra giới hạn tại waypoint chứ không kiểm tra trên toàn quỹ đạo, spline trong không gian khớp rồi tưởng đường đầu công cụ là thẳng, quên giữ nhánh nghiệm IK giữa các đoạn.

BÀI TẬP: (1) tạo quỹ đạo qua 5 waypoint cho 1 khớp bằng cubic spline tự nhiên, vẽ q, q̇, q̈; (2) so sánh có và không blending (dừng-dừng và LSPB), đo tổng thời gian; (3) cài