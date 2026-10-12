CHỦ ĐỀ: Bài 3.1 – Bản chất bài toán động học nghịch (Inverse Kinematics)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- IK là gì: biết vị trí/hướng mong muốn của đầu công cụ → tìm góc các khớp; so sánh với FK (FK luôn có đúng 1 đáp án, IK thì không)
- Ba tình huống: vô nghiệm (ngoài workspace), nghiệm duy nhất, nhiều nghiệm (thậm chí vô số nghiệm với robot dư thừa)
- Vì sao có nhiều nghiệm: elbow up / elbow down, shoulder left / right, wrist flip; robot 6 trục tối đa 8 nghiệm
- Nghiệm giải tích (closed-form) và nghiệm số (iterative): ưu nhược điểm (nhanh, đủ nghiệm, nhưng chỉ có với một số cấu trúc / chậm hơn, tổng quát, phụ thuộc giá trị khởi tạo)
- Phương pháp hình học cho robot 2 khâu phẳng: định lý cos, atan2
- Công thức: cosθ2 = (x² + y² − l1² − l2²) / (2·l1·l2); θ1 = atan2(y, x) − atan2(l2·sinθ2, l1 + l2·cosθ2)
- Điều kiện có nghiệm: |cosθ2| ≤ 1, tức |l1 − l2| ≤ r ≤ l1 + l2
- Kiểm tra nghiệm bằng cách đưa vào FK (luôn làm bước này)

VÌ SAO CẦN HỌC: người dùng ra lệnh "đưa đầu kẹp tới điểm này", robot phải tự tính góc 6 động cơ; đây là bài toán trung tâm của lập trình robot.

LƯU Ý KỸ THUẬT: minh họa chính vẽ 2D bằng SVG/Canvas (robot phẳng 2 khâu); phần nhiều nghiệm của robot 6 trục dùng giả 3D bằng Canvas hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry). Hàm IK viết bằng JavaScript thuần.

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) FK so với IK: hai khung cạnh nhau; khung FK kéo slider góc → đầu cánh tay di chuyển; khung IK kéo điểm đích → cánh tay tự uốn theo; mũi tên chiều thông tin (góc → vị trí; vị trí → góc) để thấy hai chiều ngược nhau.
b) Hai nghiệm của robot 2 khâu: kéo điểm đích bằng chuột; vẽ đồng thời cánh tay "elbow up" (màu mint) và "elbow down" (màu peach) cùng chạm đích; bảng hiển thị θ1, θ2 của từng nghiệm; nút chuyển nghiệm với animation mượt.
c) Hình học từng bước: animation dựng nghiệm bằng định lý cos: vẽ tam giác (gốc, khuỷu, đích) với các cạnh l1, l2, r; tô các góc; hiển thị công thức tương ứng từng bước (Bước tiếp, Phát, Làm lại); slider l1, l2 để thay đổi.
d) Vùng không có nghiệm: tô workspace (hình vành khăn); kéo điểm đích ra ngoài/vào lỗ trong thì cánh tay duỗi hết cỡ hướng về đích, hiện cảnh báo "Không có nghiệm: |cosθ2| = ..."; hiển thị giá trị cosθ2 theo thời gian thực với thanh đo từ −1 đến 1.
e) Biên workspace và nghiệm suy biến: khi đích nằm sát biên, hai nghiệm nhập thành một (cánh tay duỗi thẳng hoặc gập hoàn toàn); animation kéo đích từ trong ra ngoài thấy hai nghiệm khép lại.
f) Robot 3 khâu phẳng (dư thừa): cho đích (x, y) và hướng φ của khâu cuối hoặc không ràng buộc hướng; khi bỏ ràng buộc hướng thì có vô số nghiệm; slider "tư thế khâu cuối" để quét các nghiệm và vẽ nhiều tư thế mờ chồng lên nhau.
g) Tám nghiệm của robot 6 trục: giả 3D một robot kiểu UR; nút "Hiện 8 nghiệm" chạy lần lượt từng nghiệm (ghi nhãn: vai trái/phải, khuỷu lên/xuống, cổ tay lật/không), tất cả cùng đưa đầu công cụ tới một đích; bảng 8 dòng 6 góc; có thể dùng nghiệm chuẩn bị sẵn từ hàm IK viết bằng JS hoặc dữ liệu tính trước.
h) Kiểm tra nghiệm: sau mỗi lần giải, hiển thị FK(q_nghiệm) so với đích và sai số; tô xanh khi sai số < 1e-6.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) IK robot 2 khâu l1 = l2 = 1 tới điểm (1, 1): tìm cả 2 nghiệm, kiểm tra bằng FK; (2) điểm (2.5, 0) với l1 = l2 = 1 có nghiệm không? giải thích; (3) tìm θ1, θ2 cho điểm (1.2, 0.8) với l1 = 1, l2 = 0.8, so sánh với kết quả trên hình.

CODE PYTHON MẪU (numpy): hàm ik_2link(x, y, l1, l2, elbow='up') trả về (θ1, θ2) hoặc None khi vô nghiệm, dùng np.arctan2 và np.clip hợp lý; hàm fk_2link để kiểm tra vòng tròn; vẽ hai nghiệm bằng matplotlib; so sánh với rtb: robot.ikine_LM(T, mask=[1,1,0,0,0,0]) cho robot phẳng.

LỖI THƯỜNG GẶP: dùng arctan thay atan2, không kiểm tra |cosθ2| ≤ 1 (arccos báo nan), chỉ lấy một nghiệm rồi tưởng là duy nhất, nhầm dấu θ2 giữa elbow up/down, quên kiểm tra nghiệm bằng FK, sai số arccos gần ±1, nhầm radian và độ.

BÀI TẬP: (1) giải IK 2 khâu bằng hình học, tìm cả 2 nghiệm, kiểm tra bằng FK; (2) vẽ vùng workspace và đánh dấu điểm có 1, 2, 0 nghiệm; (3) tạo animation đầu cánh tay đi theo đường thẳng bằng IK từng điểm.
TRẮC NGHIỆM (5 câu): điều kiện để IK có nghiệm, số nghiệm của robot 2 khâu trong từng trường hợp, khác biệt FK và IK, ưu điểm của nghiệm giải tích, vì sao luôn kiểm tra nghiệm bằng FK.