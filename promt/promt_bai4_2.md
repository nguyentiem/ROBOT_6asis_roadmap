CHỦ ĐỀ: Bài 4.2 – Điểm kỳ dị (Singularity) và độ đo khả năng thao tác (Manipulability)

CẦN NẮM ĐƯỢC (mục tiêu bài học):
- Singularity là gì: tư thế mà J mất hạng (det J = 0 với J vuông); robot mất khả năng chuyển động theo một hướng nào đó dù động cơ vẫn quay
- Hậu quả: để đi theo đường thẳng qua gần singularity, vận tốc khớp q̇ = J⁻¹·ẋ tiến tới vô hạn; robot dừng khẩn cấp hoặc quay cực nhanh
- Các loại singularity của robot 6 trục: (1) singularity biên workspace (tay duỗi thẳng hoặc gập hoàn toàn), (2) singularity vai (tâm cổ tay nằm trên trục J1), (3) singularity cổ tay (θ5 = 0, trục J4 và J6 thẳng hàng); giải thích bằng hình
- Singularity bên trong và singularity biên: bên trong có thể tránh bằng đường đi, biên thì không
- Công thức: robot 2 khâu det(J) = l1·l2·sinθ2; singularity tại θ2 = 0, π
- Độ đo khả năng thao tác (Yoshikawa): w = √det(J·Jᵀ); elip khả năng thao tác (trục dài/ngắn theo giá trị suy biến của J)
- Số điều kiện (condition number) κ = σ_max/σ_min: nhỏ thì tốt (κ=1 là đẳng hướng), lớn thì gần singularity
- Phân tích SVD của J (giới thiệu): giá trị suy biến σ_i là độ dài các bán trục elip; hướng nào robot "yếu" (σ nhỏ)
- Cách phát hiện và tránh: kiểm tra w hoặc σ_min trước khi chạy, đổi quỹ đạo, đổi cấu hình, dùng DLS (bài 4.3)
- Singularity và lực: gần singularity robot tạo lực rất lớn theo một hướng nhưng không thể chuyển động theo hướng đó (liên hệ bài 4.4)

VÌ SAO CẦN HỌC: đây là nguyên nhân hàng đầu khiến robot dừng với lỗi "singularity" khi chạy đường thẳng; người lập trình phải biết nhận diện và lập đường đi tránh.

LƯU Ý KỸ THUẬT: robot phẳng 2 khâu vẽ 2D (SVG/Canvas); singularity của robot 6 trục dùng giả 3D hoặc three.js r128 từ cdnjs (tự viết điều khiển xoay góc nhìn, không dùng OrbitControls có sẵn, không dùng CapsuleGeometry). Tự viết FK, Jacobian, định thức, SVD cho ma trận 2x2 bằng công thức tường minh; ma trận 6x6 dùng độ đo đơn giản hoặc dữ liệu tính trước nếu không có thư viện SVD (có thể tự viết hàm phép lặp lũy thừa để tìm giá trị suy biến).

MINH HỌA TƯƠNG TÁC BẮT BUỘC:
a) Singularity robot 2 khâu: slider θ2 từ −180° đến 180°; elip vận tốc ở đầu cánh tay dẹt dần thành đoạn thẳng khi θ2 → 0 hoặc ±180°; hiển thị det(J) = l1·l2·sinθ2 và đồ thị det theo θ2; tô cảnh báo màu cam khi |det| nhỏ; mũi tên chỉ hướng mà đầu cánh tay "không thể" chuyển động (hướng dọc cánh tay).
b) Vì sao vận tốc khớp bùng nổ: đầu công cụ đi theo đường thẳng với vận tốc đều qua gần singularity; hai đồ thị: vị trí đầu công cụ (đều) và q̇1, q̇2 (tăng vọt); animation phát/dừng, slider khoảng cách từ đường thẳng đến singularity (đường đi sát hay xa); hiển thị vận tốc khớp lớn nhất và đường giới hạn vận tốc động cơ (vượt thì tô đỏ).
c) Bản đồ khả năng thao tác: workspace của robot 2 khâu tô bản đồ nhiệt theo w (hoặc κ); vùng tốt (mint), vùng kém (peach), singularity (đỏ nhạt); kéo đầu cánh tay trên bản đồ thấy elip tương ứng; nút chuyển giữa w, κ, det(J).
d) Hai loại singularity trên robot 2 khâu: duỗi thẳng (θ2 = 0) và gập hoàn toàn (θ2 = π); so sánh hình dạng, vị trí trên biên workspace; kéo điểm đích dọc biên để thấy hai nghiệm khép lại (liên hệ bài 3.1).
e) Singularity của robot 6 trục: robot kiểu UR giả 3D; ba nút "Singularity vai", "Singularity khuỷu (duỗi)", "Singularity cổ tay"; bấm thì robot chuyển mượt sang tư thế đó, hiện hình giải thích (trục nào thẳng hàng, tâm cổ tay trên trục nào) và tô sáng trục; hiển thị w hoặc σ_min thay đổi khi tiến gần.
f) Singularity cổ tay: slider θ5 từ −30° đến 30°; khi θ5 = 0 các trục J4 và J6 thẳng hàng (hiện hai mũi tên trục trùng nhau); mô tả "mất một bậc tự do, J4 và J6 tạo cùng một chuyển động" bằng hoạt hình xoay riêng J4 rồi J6; hiển thị định thức hoặc hạng của J (giá trị suy biến nhỏ nhất).
g) Elip khả năng thao tác 3D (nếu đủ chỗ): với robot 3 trục không gian, vẽ ellipsoid vận tốc tại đầu công cụ, kéo slider thấy ellipsoid dẹt dần thành đĩa hoặc que khi gần singularity.
h) SVD trực quan: hiển thị các giá trị suy biến σ1, σ2 dưới dạng thanh ngang; hai hướng riêng trên elip (trục dài, trục ngắn); gắn với "hướng robot linh hoạt nhất / yếu nhất".
i) Tìm và tránh: đặt hai điểm đầu và cuối của đường thẳng; công cụ vẽ w dọc đường thẳng; nếu có đoạn w nhỏ thì cảnh báo; thử đổi cấu hình (elbow up/down) hoặc dịch đường đi để tránh; so sánh các phương án.

VÍ DỤ TÍNH CÓ LỜI GIẢI: (1) chứng minh det(J) = l1·l2·sinθ2 cho robot 2 khâu và tìm singularity; (2) tính w, κ cho robot 2 khâu l1 = l2 = 1 ở θ2 = 90° và θ2 = 10° (số cụ thể); (3) với ẋ = (0.1, 0) m/s ở θ = (0°, 5°), tính q̇ = J⁻¹ẋ và nhận xét độ lớn; (4) cho ví dụ tư thế singularity cổ tay của robot 6 trục.

CODE PYTHON MẪU (numpy, chạy được): hàm manipulability(J) = np.sqrt(np.linalg.det(J @ J.T)); hàm condition_number(J) bằng np.linalg.svd hoặc np.linalg.cond; quét θ1, θ2 vẽ bản đồ nhiệt w bằng matplotlib; hàm near_singularity(J, thresh); chạy đường thẳng bằng IK từng điểm và vẽ q̇(t) tăng vọt; đối chiếu bằng thư viện: robot.manipulability(q), robot.jacob0(q), robot.plot_ellipse hoặc robot.vellipse; tư thế singularity cổ tay của UR5 và kiểm tra bằng np.linalg.matrix_rank hoặc σ_min.

LỖI THƯỜNG GẶP: nghĩ singularity chỉ xảy ra ở biên workspace, chỉ kiểm tra det = 0 chính xác (số thực nên cần ngưỡng), dùng det(J) khi J không vuông (nên dùng det(JJᵀ) hoặc SVD), nhầm det(J) lớn là tốt (đơn vị khác nhau giữa v và ω, cần cẩn thận khi trộn), chạy đường thẳng qua tâm cổ tay trên trục J1 (singularity vai), bỏ qua giới hạn vận tốc động cơ khi đi gần singularity, nhầm w với κ, quên rằng singularity phụ thuộc cấu hình robot chứ không chỉ điểm đích.

BÀI TẬP: (1) vẽ w trên workspace của robot 2 khâu và đánh dấu singularity; (2) viết hàm cảnh báo singularity cho robot 6 trục dùng σ_min của J; (3) cho đầu công cụ đi đường thẳng gần singularity và vẽ q̇(t); (4) tìm tư thế singularity vai, cổ tay của UR5 bằng cách quét và kiểm tra.
TRẮC NGHIỆM (5 câu): điều kiện det J = 0 nghĩa là gì, ba loại singularity của robot 6 trục, hậu quả khi đi gần singularity, ý nghĩa elip khả năng thao tác, κ nhỏ và lớn nói lên điều gì.