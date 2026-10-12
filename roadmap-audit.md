# Đối chiếu bài học với `roadmap.md`

Kiểm tra ngày 2026-10-12. Chuẩn so sánh: bài 2.2 trình bày khái niệm bằng hình và công thức, có phép tính theo bước, code NumPy và đối chiếu Robotics Toolbox, mô phỏng gắn với đúng dữ liệu đang giảng. Việc một khái niệm chỉ xuất hiện trong danh sách không được tính là đã dạy đủ.

## Kết quả chung

- Các bài 0.1–2.3 trong roadmap nay đều có file HTML; bài 0.3 vừa được bổ sung.
- Các bài 3.1–9.2 đều có file, công thức, ví dụ, mô phỏng, bài tập và 5 câu trắc nghiệm. Sau rà soát, phần giải thích và ví dụ số đã được bổ sung. Chương 3 đã viết lại phần lõi, code và trắc nghiệm.
- **Chưa thể coi toàn bộ bài 3–9 đã đạt mức chi tiết của prompt riêng.** Các khoảng trống chủ yếu nằm ở code thực hành đầy đủ, phép đối chiếu thư viện/robot thật và một số mô phỏng chuyên biệt. Bảng dưới liệt kê để không nhầm “có đề mục” với “đã hoàn thành bài tập”.

| Bài | Nội dung lõi trong roadmap | Phần còn thiếu để đạt bài thực hành đầy đủ |
|---|---|---|
| 0.3 | Có bài NumPy, Matplotlib, class Robot2R và hình tương tác | Cần chạy code Matplotlib trong môi trường có gói này |
| 2.2 | Có DH chuẩn, mô phỏng 6R, code NumPy và RTB | Bài tập tự lập bảng DH cho robot thật vẫn cần người học đối chiếu datasheet |
| 3.1 | Đã giải IK 2R hai nhánh, vô nghiệm, kiểm FK | Chưa có mô phỏng tám nhánh robot 6R theo prompt riêng |
| 3.2 | Đã có bộ giải 6 trục cổ tay ZYZ lý tưởng, ví dụ RRR không gian, FK kiểm vòng | Chưa phải IK UR5/Puma; bài ghi rõ mô hình lý tưởng để tránh áp sai |
| 3.3 | Đã có lặp DLS, điều kiện dừng, lịch sử sai số | Chưa có đối chiếu chạy trực tiếp `robot.ikine_LM()` và IK 6D đầy đủ |
| 3.4 | Đã có lọc góc tương đương, giới hạn, chọn chi phí thấp | Chưa có kiểm va chạm thực và tối ưu nhánh cho cả quỹ đạo |
| 4.1 | Có đạo hàm J 2R, ví dụ vận tốc, code Jacobian hình học 6×2 từ các frame và kiểm sai phân | Chưa có ví dụ Jacobian 6×6 cho robot công nghiệp cụ thể |
| 4.2 | Có det, SVD, w, κ và cảnh báo trên hình | Chưa có bản đồ nhiệt w trên toàn workspace |
| 4.3 | Có giải thích resolved-rate, so DLS số và code bám đường thẳng 2R với sai số cuối khoảng 0,39 mm | Chưa có vòng bám đường tròn hoàn chỉnh và xử lý giới hạn khớp |
| 4.4 | Có nguyên lý công ảo và tính τ=JᵀF | Chưa có đối chiếu tải/moment UR5 bằng RTB |
| 5.1 | Có cubic/quintic, hình thang vận tốc, đồng bộ 6 khớp và ví dụ cực trị trong code chạy được | Chưa có đồ thị so sánh đủ ba luật theo giới hạn từng trục |
| 5.2 | Có MoveJ/MoveL, code IK dọc đường thẳng 2R, slerp và dựng cung qua ba điểm | Chưa có bộ lập đường MoveC 6D kết hợp IK và kiểm va chạm |
| 5.3 | Có spline tự nhiên, blending cắt góc và ví dụ số trong code | Chưa có LSPB và bộ kiểm giới hạn vận tốc/gia tốc trên toàn đoạn |
| 6.1 | Có link/joint, visual/collision/inertial, URDF nhỏ | Chưa có bài đọc và sửa URDF UR5/Panda thực tế |
| 6.2 | Có vòng bước mô phỏng và code PyBullet nền | Code chưa thực hiện bài tập TCP tới đích bằng IK và kiểm pose |
| 6.3 | Có máy trạng thái và mô phỏng các bước gắp–đặt | Code Python hiện là pseudocode điều phối, chưa phải chương trình PyBullet gắp–đặt chạy được |
| 6.4 | Có va chạm đoạn–tròn và cây RRT minh họa | Code Python chưa cài RRT/PRM hoàn chỉnh và kiểm cạnh toàn đường |
| 7.1 | Có M,C,g 2R và ví dụ số | Chưa đối chiếu robot 6 trục bằng RTB |
| 7.2 | Có PID, anti-windup và mô phỏng đáp ứng trên trang | Code Python chưa tạo/đo đầy đủ đáp ứng bước |
| 7.3 | Có bù trọng lực, computed torque, trở kháng và ví dụ lực | Code chưa mô phỏng đầy đủ robot chạm bề mặt theo lực mong muốn |
| 8.1 | Có ROS 2 Jazzy, terminal, node publisher và sơ đồ | Chưa có package gồm cả subscriber, service/action và launch file chạy được |
| 8.2 | Có luồng JointState→TF và cách tra transform | Chưa có gói cấu hình UR5 để mở thực tế trong RViz2 |
| 8.3 | Có kiến trúc MoveIt, scene và bài tập mô phỏng | Chưa có project Gazebo/MoveIt thực thi kế hoạch thật |
| 8.4 | Có pinhole, tọa độ, hiệu chuẩn nội/ngoại và ví dụ pixel | Chưa có pipeline OpenCV/ROS nhận màu/ArUco rồi gắp vật |
| 9.1 | Có phân lớp giao tiếp, checksum, timeout và wrapper dry-run | Điều khiển robot mini thật phụ thuộc SDK/manual của thiết bị người học; không được bịa API |
| 9.2 | Có homing, TCP 4 điểm, sai số zero và an toàn | Quy trình áp dụng thật phải dùng tài liệu hãng và đánh giá rủi ro tại nơi làm việc |

## Tiêu chí tiếp tục nâng cấp

Với từng bài còn thiếu: (1) bổ sung một bài thực hành chạy được bằng đúng công cụ của roadmap, (2) đối chiếu kết quả với công thức và hình, (3) ghi điều kiện dùng mô hình và đơn vị, (4) kiểm thử trên trình duyệt `file://` và trong môi trường Python/ROS/PyBullet phù hợp. Với robot thật, chỉ dùng API, giới hạn và quy trình từ tài liệu chính thức của thiết bị cụ thể.
