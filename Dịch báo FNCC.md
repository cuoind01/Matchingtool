# PHƯƠNG PHÁP TĂNG TỐC CĂN CHỈNH HÌNH ẢNH DỰA TRÊN TƯƠNG QUAN BẰNG CPU CHO HỆ THỐNG KIỂM TRA QUANG HỌC TỰ ĐỘNG THỜI GIAN THỰC

Chin-Sheng Chen, Chien-Liang Huang, Chun-Wei Yeh, Wen-Chung Chang
a Viện Công nghệ Tự động, Đại học Công nghệ Đài Bắc, Đài Loan
b Khoa Kỹ thuật Điện, Đại học Công nghệ Đài Bắc, Đài Loan

## TÓM TẮT
Bài báo đề xuất phương pháp tăng tốc căn chỉnh hình ảnh dựa trên tương quan sử dụng CPU cho các ứng dụng kiểm tra quang học tự động (AOI) đòi hỏi thời gian thực. Để nâng cao hiệu quả tính toán, chúng tôi kết hợp **chiến lược tìm kiếm kim tự tháp hình ảnh** với tính toán song song. Chiến lược này giúp nhanh chóng xác định vật thể trong ảnh đơn sắc lẫn ảnh màu có biến đổi xoay, tịnh tiến và co giãn. Độ chính xác ở mức **điểm ảnh con** được áp dụng để đạt kết quả tối ưu. Kết quả thí nghiệm cho thấy:

- Độ chính xác góc xoay: < 0.218°

- Tốc độ xử lý tăng 277–20.841 lần

- Sai số trung bình:

  - Góc xoay: 0.2°

  - Tịnh tiến: 2.07 pixel

  - Co giãn: 0.55%

Kết quả chứng minh phương pháp phù hợp cho các ứng dụng AOI đòi hỏi độ chính xác cao và thời gian xử lý thực.

**Từ khóa**: Căn chỉnh hình ảnh, chiến lược tìm kiếm kim tự tháp, SSE2 (Streaming SIMD Extensions 2), đo lường độ tương đồng.

## 1. GIỚI THIỆU
Nhu cầu về **kiểm tra quang học tự động (AOI)** trong sản xuất ngày càng tăng do yêu cầu kiểm soát chất lượng. **So khớp mẫu** là kỹ thuật quan trọng trong thị giác máy, giúp căn chỉnh đối tượng trong ảnh với mẫu chuẩn. Trong AOI, bài toán thường liên quan đến các biến đổi 2D như **tịnh tiến** và **xoay**.

Phương pháp phổ biến nhất là **Tương quan Chuẩn hóa Chéo (NCC)**, với hệ số tương quan ∈ [−1,1]. Giá trị 1 thể hiện sự trùng khớp hoàn hảo. Tuy nhiên, việc tính toán NCC theo phương pháp duyệt toàn bộ (full-search) tốn nhiều tài nguyên, đặc biệt khi đối tượng có xoay hoặc co giãn.

Các nghiên cứu liên quan:
- Chiến lược loại trừ: Sử dụng ngưỡng để bỏ qua các vùng không khớp [2–5].

- Tính toán song song: Tận dụng lệnh SIMD/SSE để tăng tốc phép nhân-tích lũy (MAC) [9,10].

- Phương pháp dựa trên gradient: Giảm chi phí tính toán bằng cách chỉ xét gradient ảnh [14–16].

**Đóng góp của nghiên cứu này:**

Kết hợp **kim tự tháp hình ảnh, SSE2** và ước lượng **điểm ảnh con**.

Xử lý đồng thời cả ảnh đơn sắc và ảnh màu với biến đổi hình học phức tạp.

Đạt độ chính xác cao và tốc độ xử lý thực (>20.000 lần so với phương pháp truyền thống).

## 2. CƠ SỞ LÝ THUYẾT
### 2.1. Tương quan Chuẩn hóa Chéo (NCC)
**Với ảnh đơn sắc:**


​



### 2.2. Mở rộng cho biến đổi hình học
Khi xét cả góc xoay (θ) và tỷ lệ co giãn (S), công thức NCC được điều chỉnh:


## 3. KIẾN TRÚC HỆ THỐNG
Hệ thống gồm 2 giai đoạn:

1. Tạo mô hình ngoại tuyến:

Xây dựng hình ảnh đa phân giải 

Tính trước giá trị trung bình 
​


2. Quá trình so khớp trực tuyến:

Áp dụng chiến lược kim tự tháp: Từ mức phân giải thô (L) đến mức tinh (0).

Sử dụng SSE2 để tối ưu phép tính:

cpp
Copy
__m128i sum = _mm_add_epi16(T, I); // Cộng 8 giá trị 16-bit cùng lúc
Ước lượng điểm ảnh con bằng mô hình bề mặt bậc 2.

## 4. KẾT QUẢ THỰC NGHIỆM
### 4.1. Độ chính xác góc xoay
Dữ liệu: 72 ảnh xoay từ 0°–359° (bước 5°).

Kết quả:

Phương pháp	Sai số trung bình (°)	Độ lệch chuẩn (°)
Đề xuất	0.169	0.156
NCC_R	0.387	0.408
### 4.2. Xử lý ảnh thực tế
Tốc độ: 57.15 ms/ảnh (so với 15,848 ms của NCC_R).

Đa đối tượng: Thời gian xử lý ổn định (~74 ms) dù số lượng mục tiêu tăng.

## 5. KẾT LUẬN
Phương pháp đề xuất khắc phục các hạn chế của NCC truyền thống bằng:

Giảm độ phức tạp tính toán nhờ kim tự tháp hình ảnh.

Tăng tốc độ xử lý bằng SSE2.

Duy trì độ chính xác cao qua ước lượng điểm ảnh con.

Ứng dụng tiềm năng: Kiểm tra linh kiện điện tử, định vị robot, và hệ thống giám sát công nghiệp.

Lời cảm ơn: Nghiên cứu được tài trợ bởi Bộ Khoa học và Công nghệ Đài Loan (MOST 104-2221-E-027-029-MY2).

## THÔNG TIN TÁC GIẢ
**Chin-Sheng Chen**: Giáo sư chuyên về thị giác máy và điều khiển chuyển động.

**Chien-Liang Huang**: Nghiên cứu sinh về nhận dạng mẫu.

**Chun-Wei Yeh**: Tiến sĩ về xử lý hình ảnh màu.

**Wen-Chung Chang**: Chuyên gia về robot và tự động hóa.

Bản dịch đảm bảo:

Giữ nguyên thuật ngữ chuyên ngành (ví dụ: SIMD, NCC, pixel).

Bảo toàn công thức toán học và cấu trúc bảng biểu.

Diễn đạt tự nhiên trong tiếng Việt nhưng vẫn mang tính học thuật.

Thống nhất cách dịch các khái niệm xuyên suốt bài (ví dụ: "normalized cross correlation" → "tương quan chuẩn hóa chéo").