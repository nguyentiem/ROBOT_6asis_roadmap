CHỦ ĐỀ: Bài 3.4 – Chọn nghiệm IK, giới hạn khớp và tránh nhảy cấu hình

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- IK trả về một danh sách nghiệm (tối đa 8), robot chỉ chạy một: phải có tiêu chí chọn
- Bước lọc 1: loại nghiệm vi phạm giới hạn khớp [q_min, q_max]; chú ý các khớp quay được nhiều vòng (±360°, ±720°) và cách chuẩn hóa góc: thêm/bớt 2π để đưa nghiệm vào giới hạn
- Bước lọc 2: loại nghiệm gây va chạm (tự va chạm, va chạm vật cản) – giới thiệu, chi tiết ở bài 6.4
- Bước chọn: chọn nghiệm gần cấu hình hiện tại nhất theo khoảng cách có trọng số: d = Σ w_i·|q_i − q_hiện_i| (khớp lớn/nặng có trọng số cao vì tốn thời gian và năng lượng di chuyển)
- Tiêu chí khác: tránh điểm kỳ dị (độ đo manipulability lớn nhất, chi tiết ở bài 4.2), tiết kiệm thời gian, giữ nguyên "cấu hình" (vai/khuỷu/cổ tay) khi chạy đường thẳng
- Hiện tượng nhảy cấu hình: khi IK từng điểm trên quỹ đạo, nghiệm gần nhất có thể nhảy giữa các nhánh; hậu quả: robot quay mạnh một khớp đột ngột; cách xử lý: giữ nhánh (branch tracking), chọn theo nghiệm trước đó, kiểm tra Δq lớn
- Khớp cuối (J6) và các khớp cổ tay quay nhiều vòng: giải quyết bằng cách ± 2π cho gần nhất
- Kiểm tra tính khả thi của cả quỹ đạo, không chỉ từng điểm: có đường đi liên tục trong cùng một nhánh nghiệm không (cuspidal robot, giới thiệu)
- Cách các hãng robot cho người dùng chọn cấu hình (ví dụ cờ cấu hình, config flags)

VÌ SAO CẦN HỌC: nếu chọn nghiệm sai, robot có thể quay một vòng lớn vô ích, đâm vào vật, hoặc dừng báo lỗi giữa đường; đây là phần "tinh chỉnh" phân biệt code demo với code chạy được trên robot thật.

LƯU Ý KỸ THUẬT: dùng three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry) cho phần robot 6 trục; phần so sánh nghiệm vẽ biểu đồ 2D bằng Canvas. Bộ giải IK trong JS có thể dùng lại từ bài 3.2 hoặc dùng robot phẳng 2-3 khâu nếu cần nhẹ; nếu không có bộ giải 6 trục, dùng dữ liệu nghiệm tính trước cho vài đích.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Danh sách nghiệm → chọn: đặt đích bằng slider; bảng 8 nghiệm với các cột: loại nghiệm, 6 góc, hợp lệ giới hạn (✓/✗), khoảng cách tới cấu hình hiện tại, điểm số tổng; nghiệm tốt nhất được tô mint; bấm dòng để xem robot ở nghiệm đó (bóng mờ) cạnh robot hiện tại.
b) Giới hạn khớp: sáu thanh giới hạn (min–max) cho mỗi khớp, kéo được hai đầu; các nghiệm vi phạm tô đỏ nhạt và chỉ rõ khớp nào vi phạm; animation "+2π / −2π" cứu một nghiệm tưởng vi phạm thành hợp lệ; hiển thị vòng tròn góc với vùng cho phép tô màu.
c) Khoảng cách có trọng số: các slider trọng số w1–w6 (mặc định khớp đế/vai lớn, cổ tay nhỏ); thay đổi trọng số thì nghiệm tốt nhất có thể đổi; so sánh tiêu chí "tổng |Δq|", "Δq lớn nhất", "có trọng số" trong bảng ba cột.
d) Nhảy cấu hình trên quỹ đạo: robot chạy đường thẳng qua 40-60 điểm; hai chế độ: (1) chọn nghiệm gần nhất từng điểm độc lập, (2) giữ nhánh; đồ thị 6 đường q(t) của hai chế độ cạnh nhau, đánh dấu đỏ chỗ nhảy bất thường; robot minh họa chuyển động và tô đỏ khớp đang quay mạnh; có thể chọn quỹ đạo gần singularity để thấy nhảy rõ.
e) Chuẩn hóa góc và quay nhiều vòng: khớp J6 với giới hạn ±2π; góc hiện tại 170°, đích tương đương −190° (hoặc nghiệm 170° so với 530°); animation hai cách quay: đường dài vô ích và đường ngắn; hiển thị tổng góc quay của từng cách; thấy rằng cùng một hướng nhưng khác Δq.
f) Tránh singularity trong chọn nghiệm: thêm cột "manipulability" (hoặc giả lập bằng khoảng cách tới q5 = 0), cho trọng số cân bằng giữa gần và tránh singularity; ví dụ hai nghiệm có độ gần giống nhau nhưng một nghiệm gần singularity thì bị trừ điểm.
g) Cờ cấu hình: ba nút gạt Shoulder (trái/phải), Elbow (lên/xuống), Wrist (lật/không); chọn tổ hợp thì chỉ còn đúng 1 nghiệm; robot hiển thị; so sánh với cách các hãng đặt tên (mô tả ngắn); nếu tổ hợp không khả dụng (vi phạm giới hạn hoặc vô nghiệm) thì báo lý do.
h) Chọn nghiệm cho cả quỹ đạo (đồ thị nhánh): trục hoành là điểm trên quỹ đạo, mỗi nhánh nghiệm là một đường màu; người học chọn đường đi liên tục, tô đường hợp lệ và chỉ chỗ bị chặn bởi giới hạn khớp; nút "Tìm đường tốt nhất" bằng quy hoạch động đơn giản (tổng Δq nhỏ nhất) và hiển thị kết quả (có thể giới thiệu ở mức hình ảnh).

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) cho cấu hình hiện tại q_hiện và danh sách 4 nghiệm cụ thể (có số liệu), tính khoảng cách có trọng số và chọn nghiệm; (2) đưa nghiệm θ6 = 4.2 rad vào giới hạn [−π, π] (trừ 2π), rồi kiểm tra giới hạn ±2π; (3) một quỹ đạo 5 điểm: nghiệm gần nhất từng điểm có nhảy nhánh không; so với giữ nhánh.

CODE PYTHON MẪU (numpy, chạy được): hàm wrap_to_pi, hàm adjust_to_limits(q, q_min, q_max) thử ± 2π·k cho từng khớp; hàm filter_by_limits(solutions, q_min, q_max); hàm joint_distance(q, q_ref, w); hàm select_solution(solutions, q_current, weights, q_min, q_max) trả về nghiệm tốt nhất hoặc None kèm lý do; hàm track_branch cho quỹ đạo (chọn nghiệm điểm sau gần nghiệm điểm trước) và phát hiện nhảy khi max |Δq| > ngưỡng; chạy thử với ik_6dof từ bài 3.2; vẽ q(t) bằng matplotlib; đối chiếu với tùy chọn config của roboticstoolbox (ví dụ ikine_a với config="lun", "rdn"...).

LỖI THƯỜNG GẶP: chỉ kiểm tra vị trí mà quên kiểm tra giới hạn khớp, quên chuẩn hóa góc (±2π), dùng khoảng cách Euclid không trọng số cho khớp có đơn vị/ý nghĩa khác nhau, chọn nghiệm gần nhất độc lập từng điểm gây nhảy nhánh, quên rằng góc −179° và 179° chỉ cách nhau 2° (nhưng hiệu số là 358°), không xử lý trường hợp danh sách nghiệm rỗng, dùng cấu hình hiện tại sai (cũ) khi chọn, bỏ qua tránh va chạm.

BÀI TẬP: (1) viết hàm select_solution và thử trên 10 đích ngẫu nhiên; (2) tạo quỹ đạo đường thẳng, so sánh chọn gần nhất độc lập với giữ nhánh bằng đồ thị q(t); (3) thêm kiểm tra giới hạn khớp ±2π và chuẩn hóa góc; (4) thử quỹ đạo đi qua gần singularity và quan sát.
TRẮC NGHIỆM (5 câu): vì sao cần chọn nghiệm, cách đưa góc vào giới hạn, ý nghĩa trọng số trong khoảng cách khớp, nhảy nhánh là gì và cách tránh, khoảng cách giữa −179° và 179°.