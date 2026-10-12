CHỦ ĐỀ: Bài 0.3 – Python cho robotics

CẦN NẮM ĐƯỢC:
- NumPy: array, shape, slicing, toán tử @, np.linalg.solve, inv, det, norm.
- Matplotlib: đồ thị 2D, trục cùng tỉ lệ, hình robot phẳng và đồ thị theo thời gian; giới thiệu Axes3D.
- Viết hàm có đầu vào/đầu ra rõ, kiểm tra đơn vị radian và mét.
- Class Robot đơn giản lưu chiều dài khâu, góc khớp và phương thức fkine.

VÌ SAO CẦN HỌC: các bài sau dùng NumPy để tính DH, FK, IK, Jacobian và dùng Matplotlib để nhìn hình/kiểm chứng số.

MINH HỌA TƯƠNG TÁC: hai slider góc khớp, hai slider chiều dài, SVG/Canvas vẽ cánh tay 2R và hiển thị tọa độ đầu công cụ; nút phát/dừng/làm lại, tốc độ, kiểm tra bằng công thức.

VÍ DỤ: l1=1 m, l2=0.8 m, q1=30°, q2=45°; tính khuỷu và TCP từng bước, đổi độ sang radian trước khi gọi sin/cos.

CODE PYTHON: NumPy tính FK, class Robot2R, Matplotlib vẽ robot với axis equal; thêm ví dụ solve Ax=b và kiểm tra bằng np.allclose.

BÀI TẬP: vẽ cánh tay phẳng 2 khâu bằng hai góc cho trước; quét q2 và vẽ đường đi TCP; giải một hệ tuyến tính bằng np.linalg.solve.

TRẮC NGHIỆM: 5 câu về @ so với *, shape/slicing, radian, axis equal và cách kiểm tra Ax=b.
