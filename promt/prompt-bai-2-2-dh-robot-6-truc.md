# Prompt tạo bài giảng HTML tương tác — Bài 2.2: Quy ước Denavit–Hartenberg cho cánh tay robot 6 trục

Bạn là một giảng viên robotics kiêm lập trình viên front-end giỏi. Hãy tạo **MỘT FILE HTML DUY NHẤT**, trong đó HTML, CSS và JavaScript cùng nằm trong một file. File phải chạy khi mở trực tiếp bằng trình duyệt bằng đường dẫn `file://`, không cần build, server hay tài nguyên ảnh cục bộ.

## 1. Bối cảnh và người học

- **Chủ đề:** Bài 2.2 — Quy ước Denavit–Hartenberg (DH) cho cánh tay robot 6 trục.
- **Đối tượng:** Người mới bắt đầu, biết Python cơ bản, chưa học robotics.
- **Ngôn ngữ:** Tiếng Việt; lần đầu dùng thuật ngữ nên ghi tiếng Anh trong ngoặc, ví dụ: động học thuận (Forward Kinematics), ma trận biến đổi thuần nhất (Homogeneous Transformation Matrix).
- **Mục tiêu sư phạm:** Đi từ trực giác hình học, qua bốn thông số DH chuẩn, đến ma trận 4×4 và phép nhân ma trận để tìm tư thế đầu công cụ so với đế.
- Giải thích rõ rằng mô hình 6R trong bài là robot minh họa. Không trình bày bảng DH giả lập như thông số của một robot thương mại cụ thể.

## 2. Bố cục bài giảng

Tạo một trang cuộn dọc, gồm đúng thứ tự các mục sau. Mỗi mục là một thẻ (card) nền trắng, bo góc 12–16 px, có số thứ tự dễ nhìn:

1. **Mục tiêu bài học:** 3–5 gạch đầu dòng đo lường được.
2. **Vì sao cần học?:** Ví dụ robot 6 trục gắp vật, cần biết vị trí và hướng của đầu gắp khi đã biết góc các khớp.
3. **Khái niệm cốt lõi:** Cách gắn hệ tọa độ, ý nghĩa hình học của θ, d, a, α, thứ tự biến đổi của DH chuẩn, ma trận DH 4×4 và phép nhân từ đế tới công cụ.
4. **Minh họa tương tác 3D:** Mô hình robot, sáu thanh trượt góc khớp, hệ trục trên từng khâu và animation bốn bước DH.
5. **Ví dụ tính toán có lời giải từng bước:** Dùng ví dụ robot 2R phẳng đơn giản để người mới tự kiểm tra phép nhân ma trận; phân biệt rõ ví dụ này với bảng robot 6R mô phỏng.
6. **Code Python mẫu:** Một ví dụ NumPy tự lập ma trận DH chuẩn và một ví dụ `roboticstoolbox` dùng `RevoluteDH`, mỗi khối có nút **Copy**.
7. **Lỗi thường gặp:** Hộp cảnh báo về độ/radian, thứ tự nhân ma trận, trục xoay, và sự khác nhau giữa DH chuẩn với DH sửa đổi.
8. **Tóm tắt bài:** Hộp ghi nhớ ngắn gọn.
9. **Bài tập tự luyện và trắc nghiệm:** Bài tính tay có đáp số ẩn/mở; đúng 5 câu trắc nghiệm, chấm điểm ngay trên trang và giải thích từng đáp án.

Có thanh điều hướng cố định bên trái; bấm vào mục để cuộn mượt, tự đánh dấu mục đang đọc. Trên điện thoại, mục lục thu thành nút ☰. Có thanh tiến độ đọc mỏng ở mép trên. Thẻ nội dung xuất hiện nhẹ bằng fade-in và trượt lên khi cuộn tới.

Dùng bốn kiểu hộp chú thích có biểu tượng và màu riêng: **💡 Ý chính**, **⚠️ Cẩn thận**, **📝 Ví dụ**, **🧪 Thử nghiệm**.

## 3. Tính đúng đắn về Denavit–Hartenberg

Sử dụng **DH chuẩn (standard DH)** nhất quán trong nội dung, công thức, bảng số, mã Python và mô phỏng:

- Với khớp quay i, biến khớp là θᵢ, xoay quanh trục zᵢ₋₁.
- Thứ tự biến đổi: `Rz(θᵢ) · Tz(dᵢ) · Tx(aᵢ) · Rx(αᵢ)`.
- `T₀₆ = A₁A₂A₃A₄A₅A₆`; phép nhân theo thứ tự từ đế đến đầu công cụ.
- Khối 3×3 trên trái của T₀₆ biểu diễn hướng; ba giá trị đầu ở cột cuối biểu diễn vị trí x, y, z.
- Góc có thể hiển thị bằng độ, nhưng phải đổi sang radian trước khi gọi sin/cos.
- Mọi tọa độ hiển thị, ma trận 4×4, hệ trục và hình robot phải dùng cùng một bộ thông số DH và cùng một hàm tính động học thuận. Tránh vẽ robot theo một hình học khác với ma trận đang hiển thị.
- Giải thích rằng cách đặt hệ tọa độ có thể không duy nhất trong một số trường hợp, nhưng phải nhất quán.

Bảng DH mẫu cho robot 6R giả lập, đơn vị chiều dài là mét:

| i | dᵢ (m) | aᵢ (m) | αᵢ (°) | θᵢ ban đầu |
|---|---:|---:|---:|---:|
| 1 | 0.18 | 0 | 90 | 0° |
| 2 | 0 | 0.34 | 0 | 0° |
| 3 | 0 | 0.26 | 0 | 0° |
| 4 | 0.18 | 0 | 90 | 0° |
| 5 | 0 | 0 | −90 | 0° |
| 6 | 0.12 | 0 | 0 | 0° |

Có thể chọn bảng DH mẫu khác nếu nó hợp lý về mặt hình học; khi đó phải cập nhật đồng bộ toàn bộ nội dung, mã và mô phỏng.

## 4. Hình ảnh và mô phỏng 3D — ưu tiên cao nhất

**Không dùng ảnh bên ngoài.** Tự vẽ toàn bộ hình bằng SVG hoặc Canvas. Phần robot tương tác phải tạo cảm nhận không gian 3D rõ ràng và đúng hình học, không chỉ là nét vẽ 2D giả lập.

### 4.1 Mô hình robot

- Dùng phép chiếu phối cảnh 3D từ tọa độ thế giới x, y, z lên màn hình; trục Z là phương đứng.
- Có lưới sàn trên mặt phẳng XY và gốc tọa độ rõ ràng.
- Vẽ các khâu bằng thân có độ dày, ánh sáng/gradient nhẹ và khớp bằng nút tròn để phân biệt chiều sâu.
- Theo từng hàng DH, thể hiện riêng đoạn dịch dọc Z (`d`) và đoạn dịch dọc X (`a`) khi chúng khác 0. Không nối tắt thành một đoạn chéo gây hiểu sai.
- Dùng màu nhất quán cho hệ tọa độ: **X đỏ, Y xanh lá, Z xanh dương**.
- Có công tắc bật/tắt hệ trục gắn trên từng khâu. Giữ nhãn vừa đủ để không chồng lên nhau; có chú giải màu gần hình.
- Vẽ đường nét đứt chiếu đầu công cụ xuống sàn để người học thấy độ cao và quan hệ x, y, z.
- Cho người học kéo trên hình để xoay góc nhìn 3D, cuộn để zoom. Có nút nhìn 3D, mặt trước, từ trên. Trên thiết bị cảm ứng phải kéo được.
- Sáu slider điều chỉnh θ₁…θ₆ trong phạm vi hợp lý. Khi slider đổi, hình robot, tọa độ đầu công cụ, bảng DH và ma trận 4×4 cập nhật ngay, mượt, không nhấp nháy.
- Nêu rõ mô hình vẽ chỉ nhằm học DH, không phải mô hình CAD của robot thật.

### 4.2 Animation biến đổi một hàng DH

- Cho chọn hàng/khớp i.
- Animation thể hiện bốn bước liên tiếp: xoay quanh Z → dịch dọc Z → dịch dọc X → xoay quanh X.
- Vẽ hệ trục ban đầu mờ và hệ trục hiện tại đậm; có nhãn bước, giá trị thông số và lời giải thích ngắn.
- Có các nút **▶ Phát**, **⏸ Dừng**, **⏮ Làm lại**, và thanh điều chỉnh tốc độ.
- Dùng `requestAnimationFrame` hoặc chuyển động CSS mượt; tốc độ vừa phải, tránh nhấp nháy.
- Animation dùng cùng phép chiếu 3D và các phép biến đổi DH với mô hình chính.

### 4.3 Phòng thí nghiệm nhỏ

- Cho chọn một hàng DH và tự sửa `d`, `a`, `α` bằng input số.
- Khi thay giá trị, cập nhật bảng DH, mô hình 3D, tọa độ và ma trận ngay.
- Có nút khôi phục bảng mẫu.
- Bảo đảm input không gây NaN hoặc làm JavaScript lỗi khi người học tạm xóa nội dung ô.

### 4.4 Các hình giải thích khác

- Mỗi khái niệm khó cần có ít nhất một hình: chuỗi hệ tọa độ từ đế đến công cụ, ý nghĩa bốn thông số DH, và quan hệ giữa ma trận với vị trí/hướng.
- Hình tự vẽ bằng SVG hoặc Canvas; có mũi tên, nhãn và màu phân biệt rõ.

## 5. Giao diện

Phong cách sáng, nhẹ, sạch, nhiều khoảng trắng; dễ đọc lâu. Dùng CSS variables trong `:root` cho **toàn bộ bảng màu**:

- Nền trang: `#F7F9FC`
- Thẻ: `#FFFFFF`, viền `#E6EBF2`, bóng rất nhẹ
- Chủ đạo: `#6C9BCF`
- Nhấn: mint `#7FCDBB`, cam đào `#F4B183`, lavender `#B8A9E8`
- Chữ chính: `#2F3A4A`; chữ phụ: `#6B778C`
- Màu trục 3D: X đỏ, Y xanh lá, Z xanh dương, đủ tương phản trên nền sáng

Font **Be Vietnam Pro**; chữ thân bài tối thiểu 16 px; line-height khoảng 1.7. Code nền sáng, tô màu cú pháp nhẹ. Bố cục responsive để đọc và thao tác tốt trên điện thoại. Canvas/SVG không bị tràn ngang.

## 6. Ràng buộc kỹ thuật

- Một file `.html` duy nhất, chia rõ ba khu vực `<style>`, nội dung HTML và `<script>`; chú thích mã bằng tiếng Việt khi cần.
- Không dùng `localStorage` hoặc `sessionStorage`.
- Chỉ dùng thư viện JavaScript/CSS từ **cdnjs.cloudflare.com**: KaTeX `0.18.0` và highlight.js `11.11.1`; ghi phiên bản cố định trong URL. Font Be Vietnam Pro được phép tải từ Google Fonts theo yêu cầu giao diện.
- Nếu CDN không tải được, phần chữ, nội dung, mô phỏng và điều khiển chính vẫn phải hoạt động; riêng công thức/tô màu code có thể dùng hiển thị dự phòng.
- Code Python mẫu có nút Copy hoạt động khi mở bằng `file://`; xử lý trường hợp Clipboard API không khả dụng.
- Không dùng ảnh bên ngoài, thư viện 3D từ CDN khác, server, bundler hoặc file phụ.
- Mọi nút, slider, menu, quiz và animation phải dùng được bằng chuột; những điều khiển phù hợp phải dùng được trên màn hình cảm ứng.

## 7. Tự kiểm tra trước khi giao

1. Mở file trực tiếp trong Chrome/Firefox bằng `file://`.
2. Kiểm tra trang desktop và điện thoại; mục lục và tiến độ đọc hoạt động.
3. Kéo lần lượt cả sáu slider; xác nhận robot, tọa độ và đủ 16 phần tử ma trận 4×4 thay đổi hợp lý.
4. Bật/tắt hệ trục, kéo xoay góc nhìn, zoom và thử ba góc nhìn có sẵn.
5. Phát, dừng, làm lại và đổi tốc độ animation; chọn khớp khác.
6. Sửa rồi khôi phục `d`, `a`, `α` trong phòng thí nghiệm.
7. Bấm Copy ở cả hai khối Python; chấm và làm lại bài trắc nghiệm.
8. Kiểm tra không có lỗi JavaScript trong console.
9. Đối chiếu ít nhất một cấu hình với mã NumPy để chắc rằng tọa độ và ma trận trên trang đúng.

## 8. Cách trả lời

Trả về **toàn bộ mã HTML trong đúng một khối code duy nhất**, không cắt bớt và không dùng câu “phần còn lại tương tự”. Sau khối code, viết đúng ba dòng ngắn: cách lưu/mở file, cách chỉnh màu, cách chỉnh nội dung hoặc bảng DH. Nếu mã quá dài, ưu tiên theo thứ tự: **(1) mô phỏng 3D và các điều khiển chạy đúng; (2) giải thích dễ hiểu và chính xác; (3) trang trí**.
