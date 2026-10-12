CHỦ ĐỀ: Bài 7.2 – Điều khiển PID cho khớp robot

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Điều khiển phản hồi (feedback): đo vị trí thực, so với đích, tính sai số e = q_d − q, tạo moment u để giảm sai số; sơ đồ khối vòng kín (bộ điều khiển → cơ cấu chấp hành/robot → cảm biến → phản hồi)
- Vì sao điều khiển hở (open-loop) không đủ: ma sát, tải thay đổi, nhiễu, sai số mô hình
- PID: u = Kp·e + Ki·∫e dt + Kd·ė
  • P: tỉ lệ với sai số; Kp lớn thì nhanh nhưng dễ dao động, Kp nhỏ thì chậm và còn sai số tĩnh
  • I: tích phân sai số; triệt sai số tĩnh (ví dụ do trọng lực/ma sát) nhưng làm tăng vọt lố và dao động
  • D: đạo hàm sai số; tăng giảm chấn, giảm vọt lố, nhưng khuếch đại nhiễu đo
- Các chỉ tiêu đáp ứng bước: thời gian lên (rise time), độ vọt lố (overshoot %), thời gian xác lập (settling time), sai số tĩnh (steady-state error)
- Mô hình một khớp: J·q̈ + b·q̇ + τ_g(q) = u (khối quán tính, ma sát nhớt, trọng lực); với trọng lực thì P đơn thuần để lại sai số tĩnh e_ss = τ_g/Kp; thêm I hoặc bù trọng lực để triệt
- Hiện tượng bão hòa tích phân (integral windup): moment bị chặn bởi giới hạn động cơ nhưng tích phân vẫn cộng dồn → vọt lố lớn; cách xử lý: kẹp tích phân (clamping), tích phân có điều kiện, back-calculation
- Lọc thành phần D: đạo hàm của phép đo thay vì đạo hàm sai số (tránh "derivative kick" khi đích đổi đột ngột), bộ lọc thông thấp
- Rời rạc hóa: chạy ở tần số cố định (ví dụ 1 kHz), công thức e_k, tổng tích lũy Σe·dt, (e_k − e_{k−1})/dt
- Chỉnh thông số: quy trình thủ công (tăng Kp đến khi dao động nhẹ, thêm Kd, rồi thêm Ki nhỏ); phương pháp Ziegler-Nichols (Ku, Tu) ở mức giới thiệu; không có công thức vạn năng
- PID độc lập từng khớp trên robot nhiều khớp: bỏ qua ảnh hưởng qua lại (quán tính thay đổi theo q, Coriolis) coi như nhiễu; hộp số tỉ số lớn giúp giảm ảnh hưởng này; hạn chế khi robot nhanh hoặc nhẹ nhàng cần chính xác → bài 7.3
- Cascade: vòng dòng điện trong cùng, vòng vận tốc, vòng vị trí ngoài cùng (thực tế động cơ servo), giới thiệu
- Giới hạn thực tế: bão hòa moment, độ phân giải encoder, trễ, ma sát tĩnh (stick-slip)
- Liên hệ bài 6.2: POSITION_CONTROL của PyBullet là PD (positionGain, velocityGain); tự cài PID bằng TORQUE_CONTROL

VÌ SAO CẦN HỌC: hầu hết động cơ khớp trong robot công nghiệp chạy PID (hoặc biến thể) ở tầng thấp nhất; hiểu PID giúp bạn chỉnh servo, đọc đồ thị đáp ứng và biết vì sao robot rung, chậm hoặc lệch.

LƯU Ý KỸ THUẬT: một khớp (con lắc/thanh quay có trọng lực) vẽ 2D bằng Canvas, tích phân RK4 bằng JavaScript với dt cố định; đồ thị đáp ứng tự vẽ bằng Canvas (có lưới, nhãn, đơn vị). Robot 2 khâu 2D ở minh họa cuối. Không dùng thư viện biểu đồ ngoài trừ Chart.js từ cdnjs nếu cần (ghi rõ phiên bản).

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Điều khiển hở so với kín: thanh quay 1 khớp với ma sát và trọng lực; chế độ 1: đặt moment tính trước (hở) thấy không tới đúng đích khi thay đổi tải; chế độ 2: có phản hồi; slider tải và ma sát; mỗi chế độ hiển thị sai số cuối; sơ đồ khối hai chế độ.
b) Sơ đồ khối PID động: sơ đồ (đích → trừ → P, I, D → cộng → bão hòa → robot → cảm biến → phản hồi); khi chạy, các khối sáng lên và hiển thị giá trị số hiện tại (e, ∫e, ė, u); ba đường chồng màu P (primary), I (mint), D (peach) đóng góp vào u theo thời gian.
c) Vai trò từng thành phần P-I-D: ba slider Kp, Ki, Kd; đáp ứng bước (đích nhảy từ 0 đến 90°); đồ thị q(t) và đích; hiển thị chỉ tiêu tự tính: rise time, overshoot, settling time, e_ss; nút preset ("P nhỏ", "P lớn dao động", "PD tốt", "PID có vọt lố"); chú thích bằng lời cho từng trường hợp.
d) Chỉ P: slider Kp với trọng lực bật; thanh võng xuống một sai số tĩnh; công thức e_ss = τ_g/Kp hiển thị và đối chiếu với đo trên đồ thị; Kp lớn thì e_ss nhỏ nhưng dao động nhiều hơn.
e) Thêm D: so sánh P và PD với cùng Kp; Kd tăng thì dao động tắt nhanh hơn; vùng "quá giảm chấn" (chậm); thêm nhiễu đo (slider) thì D khuếch đại nhiễu làm moment run mạnh; thử lọc D (slider hằng số lọc).
f) Thêm I: P + I triệt sai số tĩnh do trọng lực; Ki tăng thì hội tụ nhanh nhưng vọt lố; đồ thị tích phân ∫e dt tăng rồi đứng yên; thấy vì sao Ki quá lớn gây dao động chậm.
g) Windup: moment giới hạn ±u_max (slider); đích lớn hoặc có lực cản lớn; so sánh không có và có anti-windup (kẹp tích phân/back-calculation); đồ thị q(t), u(t) (bị cắt phẳng khi bão hòa), ∫e (phình ra khi windup); nút bật/tắt anti-windup; thấy vọt lố lớn khi tắt.
h) Derivative kick: đích đổi đột ngột; so sánh D trên sai số (xung lớn tức thời) và D trên phép đo (êm); đồ thị u(t).
i) Tự chỉnh thông số (trò chơi): đề bài ngẫu nhiên (thanh nặng, ma sát lớn...), người học chỉnh Kp, Ki, Kd để đạt mục tiêu (vọt lố < 10%, thời gian xác lập < 1 s, sai số tĩnh = 0); bảng chấm điểm; nút "Gợi ý" hiển thị quy trình chỉnh; nút Ziegler-Nichols: tăng Kp đến dao động duy trì, đọc Ku và Tu rồi tính Kp, Ki, Kd (bảng quy tắc).
j) Tần số điều khiển và trễ: slider tần số cập nhật (20 Hz – 1000 Hz) và độ trễ (0–50 ms); tần số thấp hoặc trễ lớn thì mất ổn định với cùng Kp; đồ thị.
k) PID độc lập trên robot 2 khâu: PID cho từng khớp, bám chuỗi đích; so sánh chậm và nhanh: khi chạy chậm bám tốt, khi nhanh có sai số bám do Coriolis và quán tính thay đổi; đồ thị sai số bám theo tốc độ quỹ đạo; ghi chú dẫn sang bài 7.3.
l) Cascade (nếu đủ chỗ): sơ đồ ba vòng (dòng, tốc độ, vị trí), đáp ứng của vòng vận tốc nằm trong vòng vị trí; hoạt hình đơn giản.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) thanh quay có J = 0.1 kg·m², b = 0.2, trọng lực moment τ_g = 3 N·m; chỉ P với Kp = 30: tính sai số tĩnh (0.1 rad); (2) PD cho hệ J·q̈ + b·q̇ = u: tính tần số tự nhiên ωn = √(Kp/J), hệ số tắt dần ζ = (b+Kd)/(2·√(Kp·J)), chọn Kd để ζ = 1 (giới hạn tắt dần tới hạn), số cụ thể; (3) một bước PID rời rạc (dt = 0.01) với e_k, tích lũy, đạo hàm: tính u; (4) back-calculation đơn giản với u_max.

CODE PYTHON MẪU (numpy, chạy được): class PID (kp, ki, kd, dt, u_min, u_max, anti-windup, derivative on measurement, lọc D); mô phỏng một khớp bằng RK4 hoặc scipy.integrate.solve_ivp; hàm step_response_metrics(t, y, y_target) trả về rise time, overshoot, settling time, e_ss; vẽ q(t), u(t) bằng matplotlib; so sánh P, PD, PID; ví dụ windup; Ziegler-Nichols; PyBullet: PID tự viết bằng TORQUE_CONTROL cho 1 khớp (tắt motor mặc định bằng VELOCITY_CONTROL force=0), so sánh với POSITION_CONTROL (đặt positionGain, velocityGain) và vẽ đáp ứng; đối chiếu bằng thư viện: python-control (control.step_response) hoặc simple-pid (ghi chú: kiểm tra phiên bản); PID độc lập cho 2-6 khớp trong PyBullet bám quỹ đạo.

LỖI THƯỜNG GẶP: Kp quá lớn gây dao động/mất ổn định, Ki quá lớn, bỏ qua windup, quên giới hạn moment, tính đạo hàm trên sai số khi đích nhảy (kick), không lọc D khi có nhiễu, dt không cố định (khiến tích phân/đạo hàm sai), nhầm dấu sai số (e = q_d − q hay ngược lại), chỉnh PID khi robot đang ở một tư thế rồi dùng cho mọi tư thế (quán tính thay đổi), đơn vị độ/rad, dùng PID cho khớp có ma sát tĩnh lớn mà không tính hiện tượng giật (stick-slip), bỏ qua giới hạn vận tốc/dòng.

BÀI TẬP: (1) cài class PID và mô phỏng thanh quay 1 khớp có trọng lực; (2) vẽ đáp ứng bước với P, PD, PID và tính chỉ tiêu; (3) tái hiện windup và thêm anti-windup; (4) cài PID bằng TORQUE_CONTROL trong PyBullet và so với POSITION_CONTROL; (5) PID độc lập 2 khớp bám quỹ đạo quintic ở ba tốc độ, vẽ sai số bám.
TRẮC NGHIỆM (5 câu): vai trò P, I, D, sai số tĩnh do trọng lực và cách xử lý, windup là gì, vì sao D khuếch đại nhiễu, hạn chế của PID độc lập từng khớp.