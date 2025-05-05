# Dò Tìm Khuôn Mẫu Sử Dụng Tương Quan Chéo Chuẩn Hóa Nhanh

Kai Briechle và Uwe D. Hanebeck  
Viện Kỹ thuật Điều khiển Tự động, Đại học Kỹ thuật Munich,  
80290 Munich, Đức

## TÓM TẮT

Trong bài báo này, chúng tôi trình bày một thuật toán tính toán nhanh hệ số tương quan chéo chuẩn hóa (NCC) và ứng dụng của nó trong vấn đề dò tìm khuôn mẫu. Với một khuôn mẫu t cần xác định vị trí trong một ảnh f, ý tưởng cơ bản của thuật toán là biểu diễn khuôn mẫu, mà hệ số tương quan chéo chuẩn hóa được tính toán, dưới dạng tổng của các hàm cơ sở hình chữ nhật. Sau đó, phép tính tương quan được thực hiện cho từng hàm cơ sở thay vì toàn bộ khuôn mẫu. Kết quả của phép tính tương quan giữa khuôn mẫu t và ảnh f được tính bằng tổng có trọng số của các hàm tương quan của các hàm cơ sở.

Tùy thuộc vào mức độ xấp xỉ, thuật toán có thể vượt trội hơn nhiều so với các cài đặt dựa trên phép biến đổi Fourier của thuật toán tương quan chéo chuẩn hóa và đặc biệt phù hợp với các bài toán cần tìm nhiều khuôn mẫu khác nhau trong cùng một ảnh f.

**Từ khóa:** Tương quan chéo chuẩn hóa, xử lý ảnh, dò tìm khuôn mẫu, hàm cơ sở

## 1. GIỚI THIỆU

Một vấn đề cơ bản thường gặp trong xử lý ảnh là xác định vị trí của một mẫu đã cho trong một ảnh, hoặc một phần của ảnh, còn gọi là vùng quan tâm. Vấn đề này liên quan chặt chẽ đến việc xác định một tín hiệu số nhận được trong xử lý tín hiệu sử dụng, ví dụ như bộ lọc phù hợp.

Hai trường hợp cơ bản có thể được phân biệt:
- Vị trí của mẫu không xác định
- Có một ước lượng cho vị trí của mẫu

Thông thường, cả hai trường hợp đều phải được xử lý để giải quyết vấn đề xác định vị trí của một mẫu đã cho trong một ảnh. Trong trường hợp sau, thông tin về vị trí của mẫu có thể được sử dụng để giảm đáng kể khối lượng tính toán. Nó cũng được biết đến như theo dõi đặc trưng trong một chuỗi ảnh.

Đối với cả theo dõi đặc trưng và ước lượng ban đầu về vị trí của mẫu đã cho, nhiều thuật toán khác nhau, nổi tiếng đã được phát triển. Một phương pháp cơ bản có thể được sử dụng trong cả hai trường hợp nêu trên là dò tìm khuôn mẫu. Điều này có nghĩa là vị trí của mẫu đã cho được xác định bằng cách so sánh từng điểm ảnh của ảnh với một khuôn mẫu đã cho, chứa mẫu mong muốn. Để thực hiện điều này, khuôn mẫu được dịch chuyển u bước rời rạc theo hướng x và v bước theo hướng y của ảnh, sau đó phép so sánh được tính toán trên vùng của khuôn mẫu cho mỗi vị trí (u, v). Để tính toán phép so sánh này, tương quan chéo chuẩn hóa là một lựa chọn hợp lý trong nhiều trường hợp. Tuy nhiên, nó đòi hỏi nhiều tính toán và do đó một thuật toán tương quan nhanh đòi hỏi ít tính toán hơn phiên bản cơ bản là rất đáng quan tâm.

Trong phần 2, vấn đề được đề cập trong bài báo này được định nghĩa và một tóm tắt ngắn gọn về thuật toán tương quan chéo chuẩn hóa được trình bày. Phần 3 giới thiệu một thuật toán mới, nhanh tính toán tương quan chéo chuẩn hóa một cách hiệu quả cho một xấp xỉ của khuôn mẫu. Hiệu suất của thuật toán mới được so sánh với cách cài đặt tiêu chuẩn đơn giản của tương quan chéo chuẩn hóa và với phép biến đổi Fourier nổi tiếng. Phần 4 mô tả ngắn gọn cách thuật toán có thể được áp dụng đệ quy. Trong phần 5, một ví dụ được trình bày, trong đó thuật toán được đề xuất được áp dụng cho dò tìm khuôn mẫu. Cuối cùng là triển vọng cho các hoạt động nghiên cứu trong tương lai.

## 2. THUẬT TOÁN NCC

Vấn đề được đề cập trong bài báo này là xác định vị trí của một mẫu đã cho trong một ảnh hai chiều f. Gọi f(x, y) là giá trị cường độ của ảnh f có kích thước $M_x × M_y$ tại điểm (x, y); x ∈ {0, ..., $M_x - 1$}, y ∈ {0, ..., $M_y - 1$}. Mẫu được biểu diễn bởi một khuôn mẫu t đã cho có kích thước $N_x × N_y$. Một cách phổ biến để tính toán vị trí $(u_{pos}, v_{pos})$ của mẫu trong ảnh f là đánh giá giá trị tương quan chéo chuẩn hóa tại mỗi điểm (u, v) cho f và khuôn mẫu t, đã được dịch chuyển u bước theo hướng x và v bước theo hướng y. Phương trình (1) đưa ra một định nghĩa cơ bản cho hệ số tương quan chéo chuẩn hóa.

$$\gamma(u,v) = \frac{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})(t(x-u,y-v) - \bar{t})}{\sqrt{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})^2 \sum_{x,y} (t(x-u,y-v) - \bar{t})^2}}$$

Trong (1), $\bar{f}_{u,v}$ biểu thị giá trị trung bình của f(x, y) trong vùng của khuôn mẫu t được dịch chuyển đến (u, v) được tính bằng

$$\bar{f}_{u,v} = \frac{1}{N_x N_y} \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y)$$

Với ký hiệu tương tự, $\bar{t}$ là giá trị trung bình của khuôn mẫu t. Mẫu số trong (1) là phương sai của hàm ảnh có giá trị trung bình bằng không f(x, y) - $\bar{f}_{u,v}$ và hàm khuôn mẫu dịch chuyển có giá trị trung bình bằng không t(x-u, y-v) - $\bar{t}$. Do chuẩn hóa này, γ(u, v) độc lập với sự thay đổi về độ sáng hoặc độ tương phản của ảnh, liên quan đến giá trị trung bình và độ lệch chuẩn.

Vị trí mong muốn $(u_{pos}, v_{pos})$ của mẫu, được biểu diễn bởi t, tương đương với vị trí $(u_{max}, v_{max})$ của giá trị lớn nhất $\gamma_{max}$ của γ(u, v). Do chuẩn hóa, việc sử dụng (1) để tính toán vị trí của mẫu mạnh mẽ hơn các phép đo tương đồng khác, như hiệp phương sai đơn giản hoặc tổng các sai biệt tuyệt đối (SAD). Tuy nhiên nhược điểm chính là việc tính toán (1) đòi hỏi nhiều tính toán. Đối với mẫu số, chuẩn hóa hệ số tương quan chéo, tại mỗi điểm (u, v); u ∈ {0, ..., $M_x - N_x$}, v ∈ {0, ..., $M_y - N_y$} của ảnh, tại đó γ(u, v) được xác định, năng lượng của ảnh có giá trị trung bình bằng không

$$e_f(u,v) = \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} (f(x,y) - \bar{f}_{u,v})^2$$

và giá trị trung bình của ảnh trong vùng của hàm khuôn mẫu $\bar{f}_{u,v}$ (2) phải được tính toán lại. Nếu phép tính này được thực hiện theo cách đơn giản nhất, theo (1), số lượng phép tính tỷ lệ với $N_x N_y (M_x - N_x)(M_y - N_y)$, mặc dù năng lượng của hàm khuôn mẫu có giá trị trung bình bằng không

$$e_t(u,v) = \sum_{x=1}^{N_x} \sum_{y=1}^{N_y} (t(x,y) - \bar{t})^2$$

và giá trị trung bình của hàm khuôn mẫu $\bar{t}$ chỉ cần được tính toán trước một lần. Khối lượng tính toán này không thể chấp nhận được đối với hầu hết các ứng dụng thực tế. Tử số trong (1) có thể được tính toán trong miền tần số sử dụng phép biến đổi Fourier nổi tiếng, tuy nhiên số lượng phép tính vẫn tương đối cao, và một thuật toán tính toán tương quan chéo chuẩn hóa với ít phép tính hơn rất được quan tâm.

Để khắc phục những vấn đề phức tạp này, một phương pháp hiệu quả để tính toán mẫu số của hệ số tương quan chéo chuẩn hóa được đề xuất bởi Lewis. Ý tưởng chính là tính toán trước các bảng tổng chứa tích phân qua hàm ảnh f(x, y) và hàm ảnh bình phương $f^2(x, y)$ (tổng chạy) một lần cho mỗi ảnh f, và sử dụng các bảng này để tính toán hiệu quả biểu thức $(f(x, y) - \bar{f}_{u,v})^2$ tại mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa được đánh giá. Sử dụng các bảng tổng này, số lượng phép tính kết quả cho mẫu số không còn phụ thuộc vào kích thước của khuôn mẫu $N_x, N_y$ mà chỉ phụ thuộc vào kích thước của hàm ảnh $M_x, M_y$. Một mô tả ngắn gọn về phép tính này được đưa ra trong phần 3.1.

Trong phần 3.2, ý tưởng chính, cho phép tính toán rất hiệu quả tử số của (1) được giải thích chi tiết. Do đó, hệ số tương quan chéo chuẩn hóa (1) có thể được tính toán cho một hàm khuôn mẫu xấp xỉ $\tilde{t}(x, y)$ với số lượng phép tính ít hơn một bậc so với phương pháp FFT tiêu chuẩn, mở ra nhiều ứng dụng mới.

## 3. THUẬT TOÁN NCC NHANH

### 3.1. Tính toán mẫu số

Để đơn giản hóa việc tính toán mẫu số của hệ số tương quan chéo chuẩn hóa, ý tưởng chính là sử dụng hai bảng tổng s(u, v) và $s_2(u, v)$ qua hàm ảnh f(x, y) và năng lượng ảnh $f^2(x, y)$. Bảng tổng của hàm ảnh được định nghĩa đệ quy bởi

$$s(u,v) = f(u,v) + s(u-1,v) + s(u,v-1) - s(u-1,v-1)$$

Một định nghĩa đệ quy tương tự cho bảng tổng qua năng lượng ảnh được đưa ra bởi

$$s_2(u,v) = f^2(u,v) + s_2(u-1,v) + s_2(u,v-1) - s_2(u-1,v-1)$$

với s(u, v) = $s_2(u, v) = 0$ khi u, v < 0. Thuật toán sau đây để tính toán đơn giản hóa mẫu số có thể được áp dụng cho toàn bộ hàm ảnh hoặc bất kỳ ảnh con nào của hàm ảnh f(x, y). Sau đó, các bảng tổng chỉ cần được tính toán cho vùng ảnh con này. Với các bảng này, (2) có thể được tính toán một cách rất hiệu quả, độc lập với kích thước $N_x, N_y$ của khuôn mẫu

$$\sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y) = s(u+N_x-1,v+N_y-1) - s(u-1,v+N_y-1) - s(u+N_x-1,v-1) + s(u-1,v-1)$$

Có thể thấy từ (7), chỉ cần ba phép cộng/trừ để đánh giá tổng kép qua f(x, y) bằng cách đánh giá bảng tổng s(u, v).

Mẫu số của (1) sau đó được đánh giá sử dụng các bảng tổng chạy (5), (6) và

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - 2\bar{f}_{u,v} \sum_x \sum_y f(x,y) + \sum_x \sum_y \bar{f}_{u,v}^2$$

Trong (8) và tất cả các phương trình trong phần còn lại của đoạn này, tổng kép $\sum_x \sum_y$ được đánh giá trên vùng của khuôn mẫu, có nghĩa là u < x < u + $N_x - 1$ và v < y < v + $N_y - 1$. Với

<!-- $$\sum_x \sum_y \bar{f}_{u,v}^2 = N_x N_y \lef# Dò Tìm Khuôn Mẫu Sử Dụng Tương Quan Chéo Chuẩn Hóa Nhanh

Kai Briechle và Uwe D. Hanebeck  
Viện Kỹ thuật Điều khiển Tự động, Đại học Kỹ thuật Munich,  
80290 Munich, Đức

## TÓM TẮT

Trong bài báo này, chúng tôi trình bày một thuật toán tính toán nhanh hệ số tương quan chéo chuẩn hóa (NCC) và ứng dụng của nó trong vấn đề dò tìm khuôn mẫu. Với một khuôn mẫu t cần xác định vị trí trong một ảnh f, ý tưởng cơ bản của thuật toán là biểu diễn khuôn mẫu, mà hệ số tương quan chéo chuẩn hóa được tính toán, dưới dạng tổng của các hàm cơ sở hình chữ nhật. Sau đó, phép tính tương quan được thực hiện cho từng hàm cơ sở thay vì toàn bộ khuôn mẫu. Kết quả của phép tính tương quan giữa khuôn mẫu t và ảnh f được tính bằng tổng có trọng số của các hàm tương quan của các hàm cơ sở.

Tùy thuộc vào mức độ xấp xỉ, thuật toán có thể vượt trội hơn nhiều so với các cài đặt dựa trên phép biến đổi Fourier của thuật toán tương quan chéo chuẩn hóa và đặc biệt phù hợp với các bài toán cần tìm nhiều khuôn mẫu khác nhau trong cùng một ảnh f.

**Từ khóa:** Tương quan chéo chuẩn hóa, xử lý ảnh, dò tìm khuôn mẫu, hàm cơ sở

## 1. GIỚI THIỆU

Một vấn đề cơ bản thường gặp trong xử lý ảnh là xác định vị trí của một mẫu đã cho trong một ảnh, hoặc một phần của ảnh, còn gọi là vùng quan tâm. Vấn đề này liên quan chặt chẽ đến việc xác định một tín hiệu số nhận được trong xử lý tín hiệu sử dụng, ví dụ như bộ lọc phù hợp.

Hai trường hợp cơ bản có thể được phân biệt:
- Vị trí của mẫu không xác định
- Có một ước lượng cho vị trí của mẫu

Thông thường, cả hai trường hợp đều phải được xử lý để giải quyết vấn đề xác định vị trí của một mẫu đã cho trong một ảnh. Trong trường hợp sau, thông tin về vị trí của mẫu có thể được sử dụng để giảm đáng kể khối lượng tính toán. Nó cũng được biết đến như theo dõi đặc trưng trong một chuỗi ảnh.

Đối với cả theo dõi đặc trưng và ước lượng ban đầu về vị trí của mẫu đã cho, nhiều thuật toán khác nhau, nổi tiếng đã được phát triển. Một phương pháp cơ bản có thể được sử dụng trong cả hai trường hợp nêu trên là dò tìm khuôn mẫu. Điều này có nghĩa là vị trí của mẫu đã cho được xác định bằng cách so sánh từng điểm ảnh của ảnh với một khuôn mẫu đã cho, chứa mẫu mong muốn. Để thực hiện điều này, khuôn mẫu được dịch chuyển u bước rời rạc theo hướng x và v bước theo hướng y của ảnh, sau đó phép so sánh được tính toán trên vùng của khuôn mẫu cho mỗi vị trí (u, v). Để tính toán phép so sánh này, tương quan chéo chuẩn hóa là một lựa chọn hợp lý trong nhiều trường hợp. Tuy nhiên, nó đòi hỏi nhiều tính toán và do đó một thuật toán tương quan nhanh đòi hỏi ít tính toán hơn phiên bản cơ bản là rất đáng quan tâm.

Trong phần 2, vấn đề được đề cập trong bài báo này được định nghĩa và một tóm tắt ngắn gọn về thuật toán tương quan chéo chuẩn hóa được trình bày. Phần 3 giới thiệu một thuật toán mới, nhanh tính toán tương quan chéo chuẩn hóa một cách hiệu quả cho một xấp xỉ của khuôn mẫu. Hiệu suất của thuật toán mới được so sánh với cách cài đặt tiêu chuẩn đơn giản của tương quan chéo chuẩn hóa và với phép biến đổi Fourier nổi tiếng. Phần 4 mô tả ngắn gọn cách thuật toán có thể được áp dụng đệ quy. Trong phần 5, một ví dụ được trình bày, trong đó thuật toán được đề xuất được áp dụng cho dò tìm khuôn mẫu. Cuối cùng là triển vọng cho các hoạt động nghiên cứu trong tương lai.

## 2. THUẬT TOÁN NCC

Vấn đề được đề cập trong bài báo này là xác định vị trí của một mẫu đã cho trong một ảnh hai chiều f. Gọi f(x, y) là giá trị cường độ của ảnh f có kích thước $M_x × M_y$ tại điểm (x, y); x ∈ {0, ..., $M_x - 1$}, y ∈ {0, ..., $M_y - 1$}. Mẫu được biểu diễn bởi một khuôn mẫu t đã cho có kích thước $N_x × N_y$. Một cách phổ biến để tính toán vị trí $(u_{pos}, v_{pos})$ của mẫu trong ảnh f là đánh giá giá trị tương quan chéo chuẩn hóa tại mỗi điểm (u, v) cho f và khuôn mẫu t, đã được dịch chuyển u bước theo hướng x và v bước theo hướng y. Phương trình (1) đưa ra một định nghĩa cơ bản cho hệ số tương quan chéo chuẩn hóa.

$$\gamma(u,v) = \frac{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})(t(x-u,y-v) - \bar{t})}{\sqrt{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})^2 \sum_{x,y} (t(x-u,y-v) - \bar{t})^2}}$$

Trong (1), $\bar{f}_{u,v}$ biểu thị giá trị trung bình của f(x, y) trong vùng của khuôn mẫu t được dịch chuyển đến (u, v) được tính bằng

$$\bar{f}_{u,v} = \frac{1}{N_x N_y} \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y)$$

Với ký hiệu tương tự, $\bar{t}$ là giá trị trung bình của khuôn mẫu t. Mẫu số trong (1) là phương sai của hàm ảnh có giá trị trung bình bằng không f(x, y) - $\bar{f}_{u,v}$ và hàm khuôn mẫu dịch chuyển có giá trị trung bình bằng không t(x-u, y-v) - $\bar{t}$. Do chuẩn hóa này, γ(u, v) độc lập với sự thay đổi về độ sáng hoặc độ tương phản của ảnh, liên quan đến giá trị trung bình và độ lệch chuẩn.

Vị trí mong muốn $(u_{pos}, v_{pos})$ của mẫu, được biểu diễn bởi t, tương đương với vị trí $(u_{max}, v_{max})$ của giá trị lớn nhất $\gamma_{max}$ của γ(u, v). Do chuẩn hóa, việc sử dụng (1) để tính toán vị trí của mẫu mạnh mẽ hơn các phép đo tương đồng khác, như hiệp phương sai đơn giản hoặc tổng các sai biệt tuyệt đối (SAD). Tuy nhiên nhược điểm chính là việc tính toán (1) đòi hỏi nhiều tính toán. Đối với mẫu số, chuẩn hóa hệ số tương quan chéo, tại mỗi điểm (u, v); u ∈ {0, ..., $M_x - N_x$}, v ∈ {0, ..., $M_y - N_y$} của ảnh, tại đó γ(u, v) được xác định, năng lượng của ảnh có giá trị trung bình bằng không

$$e_f(u,v) = \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} (f(x,y) - \bar{f}_{u,v})^2$$

và giá trị trung bình của ảnh trong vùng của hàm khuôn mẫu $\bar{f}_{u,v}$ (2) phải được tính toán lại. Nếu phép tính này được thực hiện theo cách đơn giản nhất, theo (1), số lượng phép tính tỷ lệ với $N_x N_y (M_x - N_x)(M_y - N_y)$, mặc dù năng lượng của hàm khuôn mẫu có giá trị trung bình bằng không

$$e_t(u,v) = \sum_{x=1}^{N_x} \sum_{y=1}^{N_y} (t(x,y) - \bar{t})^2$$

và giá trị trung bình của hàm khuôn mẫu $\bar{t}$ chỉ cần được tính toán trước một lần. Khối lượng tính toán này không thể chấp nhận được đối với hầu hết các ứng dụng thực tế. Tử số trong (1) có thể được tính toán trong miền tần số sử dụng phép biến đổi Fourier nổi tiếng, tuy nhiên số lượng phép tính vẫn tương đối cao, và một thuật toán tính toán tương quan chéo chuẩn hóa với ít phép tính hơn rất được quan tâm.

Để khắc phục những vấn đề phức tạp này, một phương pháp hiệu quả để tính toán mẫu số của hệ số tương quan chéo chuẩn hóa được đề xuất bởi Lewis. Ý tưởng chính là tính toán trước các bảng tổng chứa tích phân qua hàm ảnh f(x, y) và hàm ảnh bình phương $f^2(x, y)$ (tổng chạy) một lần cho mỗi ảnh f, và sử dụng các bảng này để tính toán hiệu quả biểu thức $(f(x, y) - \bar{f}_{u,v})^2$ tại mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa được đánh giá. Sử dụng các bảng tổng này, số lượng phép tính kết quả cho mẫu số không còn phụ thuộc vào kích thước của khuôn mẫu $N_x, N_y$ mà chỉ phụ thuộc vào kích thước của hàm ảnh $M_x, M_y$. Một mô tả ngắn gọn về phép tính này được đưa ra trong phần 3.1.

Trong phần 3.2, ý tưởng chính, cho phép tính toán rất hiệu quả tử số của (1) được giải thích chi tiết. Do đó, hệ số tương quan chéo chuẩn hóa (1) có thể được tính toán cho một hàm khuôn mẫu xấp xỉ $\tilde{t}(x, y)$ với số lượng phép tính ít hơn một bậc so với phương pháp FFT tiêu chuẩn, mở ra nhiều ứng dụng mới.

## 3. THUẬT TOÁN NCC NHANH

### 3.1. Tính toán mẫu số

Để đơn giản hóa việc tính toán mẫu số của hệ số tương quan chéo chuẩn hóa, ý tưởng chính là sử dụng hai bảng tổng s(u, v) và $s_2(u, v)$ qua hàm ảnh f(x, y) và năng lượng ảnh $f^2(x, y)$. Bảng tổng của hàm ảnh được định nghĩa đệ quy bởi

$$s(u,v) = f(u,v) + s(u-1,v) + s(u,v-1) - s(u-1,v-1)$$

Một định nghĩa đệ quy tương tự cho bảng tổng qua năng lượng ảnh được đưa ra bởi

$$s_2(u,v) = f^2(u,v) + s_2(u-1,v) + s_2(u,v-1) - s_2(u-1,v-1)$$

với s(u, v) = $s_2(u, v) = 0$ khi u, v < 0. Thuật toán sau đây để tính toán đơn giản hóa mẫu số có thể được áp dụng cho toàn bộ hàm ảnh hoặc bất kỳ ảnh con nào của hàm ảnh f(x, y). Sau đó, các bảng tổng chỉ cần được tính toán cho vùng ảnh con này. Với các bảng này, (2) có thể được tính toán một cách rất hiệu quả, độc lập với kích thước $N_x, N_y$ của khuôn mẫu

$$\sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y) = s(u+N_x-1,v+N_y-1) - s(u-1,v+N_y-1) - s(u+N_x-1,v-1) + s(u-1,v-1)$$

Có thể thấy từ (7), chỉ cần ba phép cộng/trừ để đánh giá tổng kép qua f(x, y) bằng cách đánh giá bảng tổng s(u, v).

Mẫu số của (1) sau đó được đánh giá sử dụng các bảng tổng chạy (5), (6) và

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - 2\bar{f}_{u,v} \sum_x \sum_y f(x,y) + \sum_x \sum_y \bar{f}_{u,v}^2$$

Trong (8) và tất cả các phương trình trong phần còn lại của đoạn này, tổng kép $\sum_x \sum_y$ được đánh giá trên vùng của khuôn mẫu, có nghĩa là u < x < u + $N_x - 1$ và v < y < v + $N_y - 1$. Với

$$\sum_x \sum_y \bar{f}_{u,v}^2 = N_x N_y \left(\frac{1}{N_x N_y} \sum_x \sum_y f(x,y)\right)^2$$

(8) có thể được đơn giản hóa thành

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - \frac{1}{N_x N_y}\left(\sum_x \sum_y f(x,y)\right)^2$$

Các biểu thức tổng trong (10) qua $f^2(x, y)$ và f(x, y) có thể được tính toán hiệu quả sử dụng các bảng tổng chạy (5), (6). Hơn nữa, một phép tính căn bậc hai phải được tính toán cho mỗi điểm (u, v) để xác định mẫu số của (1).

### 3.2. Tính toán tử số

Áp dụng thuật toán được trình bày trong phần trước cho phép tính toán hiệu quả mẫu số, nhưng số lượng phép tính cần thiết để tính toán tử số của hệ số NCC (1) vẫn tương đối cao, ngay cả khi nó được thực hiện trong miền tần số với thuật toán FFT. Do đó, cần đơn giản hóa thêm phép tính này. Tử số có thể được viết lại như

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v) - \bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$$

trong đó $t'(x-u,y-v)$ là hàm khuôn mẫu có giá trị trung bình bằng không được định nghĩa bởi

$$t'(x-u,y-v) = t(x-u,y-v) - \bar{t}$$

Vì $t'(x,y)$ có giá trị trung bình bằng không và do đó cũng có tổng bằng không, nên số hạng $\bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$ cũng bằng không, và tử số của (1) có thể được viết là

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v)$$

Ý tưởng cơ bản để đơn giản hóa việc tính toán tử số là mở rộng hàm khuôn mẫu có giá trị trung bình bằng không $t'(x,y)$ thành tổng có trọng số của K hàm cơ sở hình chữ nhật $t_i$, tạo ra một xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu

$$\tilde{t}(x,y) = \sum_{i=1}^K k_i t_i(x,y)$$

<!-- Tử số của hệ số tương quan chéo chuẩn hóa sau đó có thể được đánh giá hiệu quả với xấp xỉ này. Hàm cơ sở $t_i(x,y)$ là hằng số bằng 1 bên trong một vùng hình chữ nhật $x_l^i ≤ x ≤ x_u^i \wedge y_l^i ≤ y ≤ y_u^i$ và bằng không nếu khác. Do đó $x_l^i$ và $x_u^i$ là các chỉ số của giới hạn dưới và giới hạn trên của vùng hình chữ nhật bằng 1 hằng số theo hướng x. $y_l^i$ và $y_u^i$ là các chỉ số tương ứng theo hướng y. Hình 1 cho thấy một ví dụ về một hàm cơ sở hình chữ nhật $t_i(x,y)$ trong đó $N_x = N_y = 20$. Nó bằng một hằng số cho $6 ≤ x ≤ 14 \wedge 8 ≤ y ≤ 12$. Chất lượng của xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu có giá trị trung bình bằng không ban đầu $t'(x,y)$ phụ thuộc vào cả việc lựa chọn các hàm cơ sở, có nghĩa là, trên giới hạn dưới và trên của chúng $x_l^i, x_u^i$ và $y_l^i, y_u^i$ và số lượng hàm cơ sở được sử dụng.# Dò Tìm Khuôn Mẫu Sử Dụng Tương Quan Chéo Chuẩn Hóa Nhanh

Kai Briechle và Uwe D. Hanebeck  
Viện Kỹ thuật Điều khiển Tự động, Đại học Kỹ thuật Munich,  
80290 Munich, Đức

## TÓM TẮT

Trong bài báo này, chúng tôi trình bày một thuật toán tính toán nhanh hệ số tương quan chéo chuẩn hóa (NCC) và ứng dụng của nó trong vấn đề dò tìm khuôn mẫu. Với một khuôn mẫu t cần xác định vị trí trong một ảnh f, ý tưởng cơ bản của thuật toán là biểu diễn khuôn mẫu, mà hệ số tương quan chéo chuẩn hóa được tính toán, dưới dạng tổng của các hàm cơ sở hình chữ nhật. Sau đó, phép tính tương quan được thực hiện cho từng hàm cơ sở thay vì toàn bộ khuôn mẫu. Kết quả của phép tính tương quan giữa khuôn mẫu t và ảnh f được tính bằng tổng có trọng số của các hàm tương quan của các hàm cơ sở.

Tùy thuộc vào mức độ xấp xỉ, thuật toán có thể vượt trội hơn nhiều so với các cài đặt dựa trên phép biến đổi Fourier của thuật toán tương quan chéo chuẩn hóa và đặc biệt phù hợp với các bài toán cần tìm nhiều khuôn mẫu khác nhau trong cùng một ảnh f.

**Từ khóa:** Tương quan chéo chuẩn hóa, xử lý ảnh, dò tìm khuôn mẫu, hàm cơ sở

## 1. GIỚI THIỆU

Một vấn đề cơ bản thường gặp trong xử lý ảnh là xác định vị trí của một mẫu đã cho trong một ảnh, hoặc một phần của ảnh, còn gọi là vùng quan tâm. Vấn đề này liên quan chặt chẽ đến việc xác định một tín hiệu số nhận được trong xử lý tín hiệu sử dụng, ví dụ như bộ lọc phù hợp.

Hai trường hợp cơ bản có thể được phân biệt:
- Vị trí của mẫu không xác định
- Có một ước lượng cho vị trí của mẫu

Thông thường, cả hai trường hợp đều phải được xử lý để giải quyết vấn đề xác định vị trí của một mẫu đã cho trong một ảnh. Trong trường hợp sau, thông tin về vị trí của mẫu có thể được sử dụng để giảm đáng kể khối lượng tính toán. Nó cũng được biết đến như theo dõi đặc trưng trong một chuỗi ảnh.

Đối với cả theo dõi đặc trưng và ước lượng ban đầu về vị trí của mẫu đã cho, nhiều thuật toán khác nhau, nổi tiếng đã được phát triển. Một phương pháp cơ bản có thể được sử dụng trong cả hai trường hợp nêu trên là dò tìm khuôn mẫu. Điều này có nghĩa là vị trí của mẫu đã cho được xác định bằng cách so sánh từng điểm ảnh của ảnh với một khuôn mẫu đã cho, chứa mẫu mong muốn. Để thực hiện điều này, khuôn mẫu được dịch chuyển u bước rời rạc theo hướng x và v bước theo hướng y của ảnh, sau đó phép so sánh được tính toán trên vùng của khuôn mẫu cho mỗi vị trí (u, v). Để tính toán phép so sánh này, tương quan chéo chuẩn hóa là một lựa chọn hợp lý trong nhiều trường hợp. Tuy nhiên, nó đòi hỏi nhiều tính toán và do đó một thuật toán tương quan nhanh đòi hỏi ít tính toán hơn phiên bản cơ bản là rất đáng quan tâm.

Trong phần 2, vấn đề được đề cập trong bài báo này được định nghĩa và một tóm tắt ngắn gọn về thuật toán tương quan chéo chuẩn hóa được trình bày. Phần 3 giới thiệu một thuật toán mới, nhanh tính toán tương quan chéo chuẩn hóa một cách hiệu quả cho một xấp xỉ của khuôn mẫu. Hiệu suất của thuật toán mới được so sánh với cách cài đặt tiêu chuẩn đơn giản của tương quan chéo chuẩn hóa và với phép biến đổi Fourier nổi tiếng. Phần 4 mô tả ngắn gọn cách thuật toán có thể được áp dụng đệ quy. Trong phần 5, một ví dụ được trình bày, trong đó thuật toán được đề xuất được áp dụng cho dò tìm khuôn mẫu. Cuối cùng là triển vọng cho các hoạt động nghiên cứu trong tương lai.

## 2. THUẬT TOÁN NCC

Vấn đề được đề cập trong bài báo này là xác định vị trí của một mẫu đã cho trong một ảnh hai chiều f. Gọi f(x, y) là giá trị cường độ của ảnh f có kích thước $M_x × M_y$ tại điểm (x, y); x ∈ {0, ..., $M_x - 1$}, y ∈ {0, ..., $M_y - 1$}. Mẫu được biểu diễn bởi một khuôn mẫu t đã cho có kích thước $N_x × N_y$. Một cách phổ biến để tính toán vị trí $(u_{pos}, v_{pos})$ của mẫu trong ảnh f là đánh giá giá trị tương quan chéo chuẩn hóa tại mỗi điểm (u, v) cho f và khuôn mẫu t, đã được dịch chuyển u bước theo hướng x và v bước theo hướng y. Phương trình (1) đưa ra một định nghĩa cơ bản cho hệ số tương quan chéo chuẩn hóa.

$$\gamma(u,v) = \frac{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})(t(x-u,y-v) - \bar{t})}{\sqrt{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})^2 \sum_{x,y} (t(x-u,y-v) - \bar{t})^2}}$$

Trong (1), $\bar{f}_{u,v}$ biểu thị giá trị trung bình của f(x, y) trong vùng của khuôn mẫu t được dịch chuyển đến (u, v) được tính bằng

$$\bar{f}_{u,v} = \frac{1}{N_x N_y} \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y)$$

Với ký hiệu tương tự, $\bar{t}$ là giá trị trung bình của khuôn mẫu t. Mẫu số trong (1) là phương sai của hàm ảnh có giá trị trung bình bằng không f(x, y) - $\bar{f}_{u,v}$ và hàm khuôn mẫu dịch chuyển có giá trị trung bình bằng không t(x-u, y-v) - $\bar{t}$. Do chuẩn hóa này, γ(u, v) độc lập với sự thay đổi về độ sáng hoặc độ tương phản của ảnh, liên quan đến giá trị trung bình và độ lệch chuẩn.

Vị trí mong muốn $(u_{pos}, v_{pos})$ của mẫu, được biểu diễn bởi t, tương đương với vị trí $(u_{max}, v_{max})$ của giá trị lớn nhất $\gamma_{max}$ của γ(u, v). Do chuẩn hóa, việc sử dụng (1) để tính toán vị trí của mẫu mạnh mẽ hơn các phép đo tương đồng khác, như hiệp phương sai đơn giản hoặc tổng các sai biệt tuyệt đối (SAD). Tuy nhiên nhược điểm chính là việc tính toán (1) đòi hỏi nhiều tính toán. Đối với mẫu số, chuẩn hóa hệ số tương quan chéo, tại mỗi điểm (u, v); u ∈ {0, ..., $M_x - N_x$}, v ∈ {0, ..., $M_y - N_y$} của ảnh, tại đó γ(u, v) được xác định, năng lượng của ảnh có giá trị trung bình bằng không

$$e_f(u,v) = \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} (f(x,y) - \bar{f}_{u,v})^2$$

và giá trị trung bình của ảnh trong vùng của hàm khuôn mẫu $\bar{f}_{u,v}$ (2) phải được tính toán lại. Nếu phép tính này được thực hiện theo cách đơn giản nhất, theo (1), số lượng phép tính tỷ lệ với $N_x N_y (M_x - N_x)(M_y - N_y)$, mặc dù năng lượng của hàm khuôn mẫu có giá trị trung bình bằng không

$$e_t(u,v) = \sum_{x=1}^{N_x} \sum_{y=1}^{N_y} (t(x,y) - \bar{t})^2$$

và giá trị trung bình của hàm khuôn mẫu $\bar{t}$ chỉ cần được tính toán trước một lần. Khối lượng tính toán này không thể chấp nhận được đối với hầu hết các ứng dụng thực tế. Tử số trong (1) có thể được tính toán trong miền tần số sử dụng phép biến đổi Fourier nổi tiếng, tuy nhiên số lượng phép tính vẫn tương đối cao, và một thuật toán tính toán tương quan chéo chuẩn hóa với ít phép tính hơn rất được quan tâm.

Để khắc phục những vấn đề phức tạp này, một phương pháp hiệu quả để tính toán mẫu số của hệ số tương quan chéo chuẩn hóa được đề xuất bởi Lewis. Ý tưởng chính là tính toán trước các bảng tổng chứa tích phân qua hàm ảnh f(x, y) và hàm ảnh bình phương $f^2(x, y)$ (tổng chạy) một lần cho mỗi ảnh f, và sử dụng các bảng này để tính toán hiệu quả biểu thức $(f(x, y) - \bar{f}_{u,v})^2$ tại mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa được đánh giá. Sử dụng các bảng tổng này, số lượng phép tính kết quả cho mẫu số không còn phụ thuộc vào kích thước của khuôn mẫu $N_x, N_y$ mà chỉ phụ thuộc vào kích thước của hàm ảnh $M_x, M_y$. Một mô tả ngắn gọn về phép tính này được đưa ra trong phần 3.1.

Trong phần 3.2, ý tưởng chính, cho phép tính toán rất hiệu quả tử số của (1) được giải thích chi tiết. Do đó, hệ số tương quan chéo chuẩn hóa (1) có thể được tính toán cho một hàm khuôn mẫu xấp xỉ $\tilde{t}(x, y)$ với số lượng phép tính ít hơn một bậc so với phương pháp FFT tiêu chuẩn, mở ra nhiều ứng dụng mới.

## 3. THUẬT TOÁN NCC NHANH

### 3.1. Tính toán mẫu số

Để đơn giản hóa việc tính toán mẫu số của hệ số tương quan chéo chuẩn hóa, ý tưởng chính là sử dụng hai bảng tổng s(u, v) và $s_2(u, v)$ qua hàm ảnh f(x, y) và năng lượng ảnh $f^2(x, y)$. Bảng tổng của hàm ảnh được định nghĩa đệ quy bởi

$$s(u,v) = f(u,v) + s(u-1,v) + s(u,v-1) - s(u-1,v-1)$$

Một định nghĩa đệ quy tương tự cho bảng tổng qua năng lượng ảnh được đưa ra bởi

$$s_2(u,v) = f^2(u,v) + s_2(u-1,v) + s_2(u,v-1) - s_2(u-1,v-1)$$

với s(u, v) = $s_2(u, v) = 0$ khi u, v < 0. Thuật toán sau đây để tính toán đơn giản hóa mẫu số có thể được áp dụng cho toàn bộ hàm ảnh hoặc bất kỳ ảnh con nào của hàm ảnh f(x, y). Sau đó, các bảng tổng chỉ cần được tính toán cho vùng ảnh con này. Với các bảng này, (2) có thể được tính toán một cách rất hiệu quả, độc lập với kích thước $N_x, N_y$ của khuôn mẫu

$$\sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y) = s(u+N_x-1,v+N_y-1) - s(u-1,v+N_y-1) - s(u+N_x-1,v-1) + s(u-1,v-1)$$

Có thể thấy từ (7), chỉ cần ba phép cộng/trừ để đánh giá tổng kép qua f(x, y) bằng cách đánh giá bảng tổng s(u, v).

Mẫu số của (1) sau đó được đánh giá sử dụng các bảng tổng chạy (5), (6) và

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - 2\bar{f}_{u,v} \sum_x \sum_y f(x,y) + \sum_x \sum_y \bar{f}_{u,v}^2$$

Trong (8) và tất cả các phương trình trong phần còn lại của đoạn này, tổng kép $\sum_x \sum_y$ được đánh giá trên vùng của khuôn mẫu, có nghĩa là u < x < u + $N_x - 1$ và v < y < v + $N_y - 1$. Với

$$\sum_x \sum_y \bar{f}_{u,v}^2 = N_x N_y \left(\frac{1}{N_x N_y} \sum_x \sum_y f(x,y)\right)^2$$

(8) có thể được đơn giản hóa thành

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - \frac{1}{N_x N_y}\left(\sum_x \sum_y f(x,y)\right)^2$$

Các biểu thức tổng trong (10) qua $f^2(x, y)$ và f(x, y) có thể được tính toán hiệu quả sử dụng các bảng tổng chạy (5), (6). Hơn nữa, một phép tính căn bậc hai phải được tính toán cho mỗi điểm (u, v) để xác định mẫu số của (1).

### 3.2. Tính toán tử số

Áp dụng thuật toán được trình bày trong phần trước cho phép tính toán hiệu quả mẫu số, nhưng số lượng phép tính cần thiết để tính toán tử số của hệ số NCC (1) vẫn tương đối cao, ngay cả khi nó được thực hiện trong miền tần số với thuật toán FFT. Do đó, cần đơn giản hóa thêm phép tính này. Tử số có thể được viết lại như

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v) - \bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$$

trong đó $t'(x-u,y-v)$ là hàm khuôn mẫu có giá trị trung bình bằng không được định nghĩa bởi

$$t'(x-u,y-v) = t(x-u,y-v) - \bar{t}$$

Vì $t'(x,y)$ có giá trị trung bình bằng không và do đó cũng có tổng bằng không, nên số hạng $\bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$ cũng bằng không, và tử số của (1) có thể được viết là

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v)$$

Ý tưởng cơ bản để đơn giản hóa việc tính toán tử số là mở rộng hàm khuôn mẫu có giá trị trung bình bằng không $t'(x,y)$ thành tổng có trọng số của K hàm cơ sở hình chữ nhật $t_i$, tạo ra một xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu

$$\tilde{t}(x,y) = \sum_{i=1}^K k_i t_i(x,y)$$

Tử số của hệ số tương quan chéo chuẩn hóa sau đó có thể được đánh giá hiệu quả với xấp xỉ này. Hàm cơ sở $t_i(x,y)$ là hằng số bằng 1 bên trong một vùng hình chữ nhật $x_l^i ≤ x ≤ x_u^i \wedge y_l^i ≤ y ≤ y_u^i$ và bằng không nếu khác. Do đó $x_l^i$ và $x_u^i$ là các chỉ số của giới hạn dưới và giới hạn trên của vùng hình chữ nhật bằng 1 hằng số theo hướng x. $y_l^i$ và $y_u^i$ là các chỉ số tương ứng theo hướng y. Hình 1 cho thấy một ví dụ về một hàm cơ sở hình chữ nhật $t_i(x,y)$ trong đó $N_x = N_y = 20$. Nó bằng một hằng số cho $6 ≤ x ≤ 14 \wedge 8 ≤ y ≤ 12$. Chất lượng của xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu có giá trị trung bình bằng không ban đầu $t'(x,y)$ phụ thuộc vào cả việc lựa chọn các hàm cơ sở, có nghĩa là, trên giới hạn dưới và trên của chúng $x_l^i, x_u^i$ và $y_l^i, y_u^i$ và số lượng hàm cơ sở được sử dụng.

Sử dụng khai triển tổng của hàm khuôn mẫu $\tilde{t}(x,y)$ cho phép tính toán một xấp xỉ cho tử số của hệ số tương quan chéo

$$\tilde{N}(u,v) = \sum_{i=1}^K k_i \sum_{x=x_l^i+u}^{x_u^i+u} \sum_{y=y_l^i+v}^{y_u^i+v} f(x,y)$$

trong đó K là số lượng hàm cơ sở và $k_i$ là hệ số cho hàm cơ sở i. Nhớ rằng $x_l^i$ và $y_l^i$ biểu thị các chỉ số của các giới hạn dưới của hàm cơ sở i và $x_u^i$ và $y_u^i$ sau đó là các chỉ số của các giới hạn trên. (15) thu được từ (13) sử dụng (14), bởi vì các hàm cơ sở $t_i(x,y)$ hoặc là hằng số bằng một hoặc bằng không. Với bảng tổng chạy qua hàm ảnh (5), đã được tính toán để đơn giản hóa việc xác định mẫu số, mà không thể được tính toán trong miền tần số, tổng kép bên trong trong (15) có thể được tính toán với chỉ 3 phép toán cộng/trừ.

$$\sum_{x=x_l^i}^{x_u^i} \sum_{y=y_l^i}^{y_u^i} f(x,y) = s(x_u^i, y_u^i) - s(x_u^i, y_l^i-1) - s(x_l^i-1, y_u^i) + s(x_l^i-1, y_l^i-1)$$

### 3.3. Phân tích độ phức tạp cho việc tính toán tử số

Số lượng phép tính cần thiết để tính toán tử số của hệ số NCC cho một ảnh có kích thước $M_x * M_y$ và một khuôn mẫu có kích thước $N_x * N_y$ đã được xấp xỉ với K hàm cơ sở hình chữ nhật được đưa ra trong bảng 1. Có thể thấy rằng số lượng phép nhân phụ thuộc tuyến tính vào số lượng hàm cơ sở K cho một hàm ảnh f(x, y) đã cho. Đối với mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa γ(u, v) được đánh giá, K phép nhân được yêu cầu để tính toán tử số, một cho mỗi hàm cơ sở.

Ngược lại, một đánh giá trực tiếp của γ(u, v) yêu cầu $N_x * N_y$ phép nhân tại mỗi điểm (u, v) tại đó γ được đánh giá. Độ phức tạp của FFT phụ thuộc vào kích thước của khuôn mẫu và hàm ảnh. Khi M lớn hơn nhiều so với N, nó thậm chí có thể vượt quá số lượng phép tính yêu cầu bởi phương pháp trực tiếp. Độc lập với M và N, nó cũng có thể vượt xa số lượng phép tính yêu cầu bởi thuật toán được đề xuất tùy thuộc vào số lượng hàm cơ sở K.

Số lượng phép tính cần thiết để thiết lập các bảng tổng và đánh giá mẫu số của hệ số tương quan chéo chuẩn hóa không được đề cập trong bảng 1 và bảng 2. Để tính toán mẫu số, chuẩn hóa hệ số tương quan chéo, phương pháp được đề xuất bởi Lewis hiệu quả hơn nhiều so với một tính toán trực tiếp, đơn giản. Do đó, các phép tính để thiết lập các bảng tổng là cần thiết cho cả ba phương pháp được thảo luận ở đây và không được bao gồm trong so sánh. Tất nhiên, thuật toán được đề xuất để tính toán tử số cũng có thể được áp dụng để tính toán hiệu quả một tương quan chéo thông thường không chuẩn hóa, nhưng sau đó nỗ lực bổ sung để thiết lập các bảng tổng phải được tính đến so với FFT tiêu chuẩn, không yêu cầu bất kỳ bảng tổng nào.

**Bảng 1. Phân tích độ phức tạp, chỉ tử số.**

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ |
|FFT | $9M_x M_y \log_2(M_x M_y)$ | $6M_x M_y \log_2(M_x M_y)$ |
|Thuật toán mới | $(4K - 1)(M_x - N_x + 1)(M_y - N_y + 1)$ | $K(M_x - N_x + 1)(M_y - N_y + 1)$ |

**Bảng 2. Phân tích độ phức tạp, Ví dụ.**

$M_x = 640, M_y = 480, N_x = N_y = 64, K = 3$

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | 985.5 Triệu | 985.5 Triệu |
|FFT | 50.4 Triệu -->

Sử dụng khai triển tổng của hàm khuôn mẫu $\tilde{t}(x,y)$ cho phép tính toán một xấp xỉ cho tử số của hệ số tương quan chéo

$$\tilde{N}(u,v) = \sum_{i=1}^K k_i \sum_{x=x_l^i+u}^{x_u^i+u} \sum_{y=y_l^i+v}^{y_u^i+v} f(x,y)$$

trong đó K là số lượng hàm cơ sở và $k_i$ là hệ số cho hàm cơ sở i. Nhớ rằng $x_l^i$ và $y_l^i$ biểu thị các chỉ số của các giới hạn dưới của hàm cơ sở i và $x_u^i$ và $y_u^i$ sau đó là các chỉ số của các giới hạn trên. (15) thu được từ (13) sử dụng (14), bởi vì các hàm cơ sở $t_i(x,y)$ hoặc là hằng số bằng một hoặc bằng không. Với bảng tổng chạy qua hàm ảnh (5), đã được tính toán để đơn giản hóa việc xác định mẫu số, mà không thể được tính toán trong miền tần số, tổng kép bên trong trong (15) có thể được tính toán với chỉ 3 phép toán cộng/trừ.

$$\sum_{x=x_l^i}^{x_u^i} \sum_{y=y_l^i}^{y_u^i} f(x,y) = s(x_u^i, y_u^i) - s(x_u^i, y_l^i-1) - s(x_l^i-1, y_u^i) + s(x_l^i-1, y_l^i-1)$$

### 3.3. Phân tích độ phức tạp cho việc tính toán tử số

Số lượng phép tính cần thiết để tính toán tử số của hệ số NCC cho một ảnh có kích thước $M_x * M_y$ và một khuôn mẫu có kích thước $N_x * N_y$ đã được xấp xỉ với K hàm cơ sở hình chữ nhật được đưa ra trong bảng 1. Có thể thấy rằng số lượng phép nhân phụ thuộc tuyến tính vào số lượng hàm cơ sở K cho một hàm ảnh f(x, y) đã cho. Đối với mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa γ(u, v) được đánh giá, K phép nhân được yêu cầu để tính toán tử số, một cho mỗi hàm cơ sở.

Ngược lại, một đánh giá trực tiếp của γ(u, v) yêu cầu $N_x * N_y$ phép nhân tại mỗi điểm (u, v) tại đó γ được đánh giá. Độ phức tạp của FFT phụ thuộc vào kích thước của khuôn mẫu và hàm ảnh. Khi M lớn hơn nhiều so với N, nó thậm chí có thể vượt quá số lượng phép tính yêu cầu bởi phương pháp trực tiếp. Độc lập với M và N, nó cũng có thể vượt xa số lượng phép tính yêu cầu bởi thuật toán được đề xuất tùy thuộc vào số lượng hàm cơ sở K.

Số lượng phép tính cần thiết để thiết lập các bảng tổng và đánh giá mẫu số của hệ số tương quan chéo chuẩn hóa không được đề cập trong bảng 1 và bảng 2. Để tính toán mẫu số, chuẩn hóa hệ số tương quan chéo, phương pháp được đề xuất bởi Lewis hiệu quả hơn nhiều so với một tính toán trực tiếp, đơn giản. Do đó, các phép tính để thiết lập các bảng tổng là cần thiết cho cả ba phương pháp được thảo luận ở đây và không được bao gồm trong so sánh. Tất nhiên, thuật toán được đề xuất để tính toán tử số cũng có thể được áp dụng để tính toán hiệu quả một tương quan chéo thông thường không chuẩn hóa, nhưng sau đó nỗ lực bổ sung để thiết lập các bảng tổng phải được tính đến so với FFT tiêu chuẩn, không yêu cầu bất kỳ bảng tổng nào.

**Bảng 1. Phân tích độ phức tạp, chỉ tử số.**

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ |
|FFT | $9M_x M_y \log_2(M_x M_y)$ | $6M_x M_y \log_2(M_x M_y)$ |
|Thuật toán mới | $(4K - 1)(M_x - N_x + 1)(M_y - N_y + 1)$ | $K(M_x - N_x + 1)(M_y - N_y + 1)$ |

**Bảng 2. Phân tích độ phức tạp, Ví dụ.**

$M_x = 640, M_y = 480, N_x = N_y = 64, K = 3$

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | 985.5 Triệu | 985.5 Triệu |
|FFT | 50.4 Triệut(\frac{1}{N_x N_y} \sum_x \sum_y f(x,y)\right)^2$$ -->

(8) có thể được đơn giản hóa thành

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - \frac{1}{N_x N_y}\left(\sum_x \sum_y f(x,y)\right)^2$$

Các biểu thức tổng trong (10) qua $f^2(x, y)$ và f(x, y) có thể được tính toán hiệu quả sử dụng các bảng tổng chạy (5), (6). Hơn nữa, một phép tính căn bậc hai phải được tính toán cho mỗi điểm (u, v) để xác định mẫu số của (1).

### 3.2. Tính toán tử số

Áp dụng thuật toán được trình bày trong phần trước cho phép tính toán hiệu quả mẫu số, nhưng số lượng phép tính cần thiết để tính toán tử số của hệ số NCC (1) vẫn tương đối cao, ngay cả khi nó được thực hiện trong miền tần số với thuật toán FFT. Do đó, cần đơn giản hóa thêm phép tính này. Tử số có thể được viết lại như

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v) - \bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$$

trong đó $t'(x-u,y-v)$ là hàm khuôn mẫu có giá trị trung bình bằng không được định nghĩa bởi

$$t'(x-u,y-v) = t(x-u,y-v) - \bar{t}$$

Vì $t'(x,y)$ có giá trị trung bình bằng không và do đó cũng có tổng bằng không, nên số hạng $\bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$ cũng bằng không, và tử số của (1) có thể được viết là

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v)$$

Ý tưởng cơ bản để đơn giản hóa việc tính toán tử số là mở rộng hàm khuôn mẫu có giá trị trung bình bằng không $t'(x,y)$ thành tổng có trọng số của K hàm cơ sở hình chữ nhật $t_i$, tạo ra một xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu
<!--  -->
$$\tilde{t}(x,y) = \sum_{i=1}^K k_i t_i(x,y)$$

<!-- Tử số của hệ số tương quan chéo chuẩn hóa sau đó có thể được đánh giá hiệu quả với xấp xỉ này. Hàm cơ sở $t_i(x,y)$ là hằng số bằng 1 bên trong một vùng hình chữ nhật $x_l^i ≤ x ≤ x_u^i \wedge y_l^i ≤ y ≤ y_u^i$ và bằng không nếu khác. Do đó $x_l^i$ và $x_u^i$ là các chỉ số của# Dò Tìm Khuôn Mẫu Sử Dụng Tương Quan Chéo Chuẩn Hóa Nhanh

Kai Briechle và Uwe D. Hanebeck  
Viện Kỹ thuật Điều khiển Tự động, Đại học Kỹ thuật Munich,  
80290 Munich, Đức

## TÓM TẮT

Trong bài báo này, chúng tôi trình bày một thuật toán tính toán nhanh hệ số tương quan chéo chuẩn hóa (NCC) và ứng dụng của nó trong vấn đề dò tìm khuôn mẫu. Với một khuôn mẫu t cần xác định vị trí trong một ảnh f, ý tưởng cơ bản của thuật toán là biểu diễn khuôn mẫu, mà hệ số tương quan chéo chuẩn hóa được tính toán, dưới dạng tổng của các hàm cơ sở hình chữ nhật. Sau đó, phép tính tương quan được thực hiện cho từng hàm cơ sở thay vì toàn bộ khuôn mẫu. Kết quả của phép tính tương quan giữa khuôn mẫu t và ảnh f được tính bằng tổng có trọng số của các hàm tương quan của các hàm cơ sở.

Tùy thuộc vào mức độ xấp xỉ, thuật toán có thể vượt trội hơn nhiều so với các cài đặt dựa trên phép biến đổi Fourier của thuật toán tương quan chéo chuẩn hóa và đặc biệt phù hợp với các bài toán cần tìm nhiều khuôn mẫu khác nhau trong cùng một ảnh f.

**Từ khóa:** Tương quan chéo chuẩn hóa, xử lý ảnh, dò tìm khuôn mẫu, hàm cơ sở

## 1. GIỚI THIỆU

Một vấn đề cơ bản thường gặp trong xử lý ảnh là xác định vị trí của một mẫu đã cho trong một ảnh, hoặc một phần của ảnh, còn gọi là vùng quan tâm. Vấn đề này liên quan chặt chẽ đến việc xác định một tín hiệu số nhận được trong xử lý tín hiệu sử dụng, ví dụ như bộ lọc phù hợp.

Hai trường hợp cơ bản có thể được phân biệt:
- Vị trí của mẫu không xác định
- Có một ước lượng cho vị trí của mẫu

Thông thường, cả hai trường hợp đều phải được xử lý để giải quyết vấn đề xác định vị trí của một mẫu đã cho trong một ảnh. Trong trường hợp sau, thông tin về vị trí của mẫu có thể được sử dụng để giảm đáng kể khối lượng tính toán. Nó cũng được biết đến như theo dõi đặc trưng trong một chuỗi ảnh.

Đối với cả theo dõi đặc trưng và ước lượng ban đầu về vị trí của mẫu đã cho, nhiều thuật toán khác nhau, nổi tiếng đã được phát triển. Một phương pháp cơ bản có thể được sử dụng trong cả hai trường hợp nêu trên là dò tìm khuôn mẫu. Điều này có nghĩa là vị trí của mẫu đã cho được xác định bằng cách so sánh từng điểm ảnh của ảnh với một khuôn mẫu đã cho, chứa mẫu mong muốn. Để thực hiện điều này, khuôn mẫu được dịch chuyển u bước rời rạc theo hướng x và v bước theo hướng y của ảnh, sau đó phép so sánh được tính toán trên vùng của khuôn mẫu cho mỗi vị trí (u, v). Để tính toán phép so sánh này, tương quan chéo chuẩn hóa là một lựa chọn hợp lý trong nhiều trường hợp. Tuy nhiên, nó đòi hỏi nhiều tính toán và do đó một thuật toán tương quan nhanh đòi hỏi ít tính toán hơn phiên bản cơ bản là rất đáng quan tâm.

Trong phần 2, vấn đề được đề cập trong bài báo này được định nghĩa và một tóm tắt ngắn gọn về thuật toán tương quan chéo chuẩn hóa được trình bày. Phần 3 giới thiệu một thuật toán mới, nhanh tính toán tương quan chéo chuẩn hóa một cách hiệu quả cho một xấp xỉ của khuôn mẫu. Hiệu suất của thuật toán mới được so sánh với cách cài đặt tiêu chuẩn đơn giản của tương quan chéo chuẩn hóa và với phép biến đổi Fourier nổi tiếng. Phần 4 mô tả ngắn gọn cách thuật toán có thể được áp dụng đệ quy. Trong phần 5, một ví dụ được trình bày, trong đó thuật toán được đề xuất được áp dụng cho dò tìm khuôn mẫu. Cuối cùng là triển vọng cho các hoạt động nghiên cứu trong tương lai.

## 2. THUẬT TOÁN NCC

Vấn đề được đề cập trong bài báo này là xác định vị trí của một mẫu đã cho trong một ảnh hai chiều f. Gọi f(x, y) là giá trị cường độ của ảnh f có kích thước $M_x × M_y$ tại điểm (x, y); x ∈ {0, ..., $M_x - 1$}, y ∈ {0, ..., $M_y - 1$}. Mẫu được biểu diễn bởi một khuôn mẫu t đã cho có kích thước $N_x × N_y$. Một cách phổ biến để tính toán vị trí $(u_{pos}, v_{pos})$ của mẫu trong ảnh f là đánh giá giá trị tương quan chéo chuẩn hóa tại mỗi điểm (u, v) cho f và khuôn mẫu t, đã được dịch chuyển u bước theo hướng x và v bước theo hướng y. Phương trình (1) đưa ra một định nghĩa cơ bản cho hệ số tương quan chéo chuẩn hóa.

$$\gamma(u,v) = \frac{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})(t(x-u,y-v) - \bar{t})}{\sqrt{\sum_{x,y} (f(x,y) - \bar{f}_{u,v})^2 \sum_{x,y} (t(x-u,y-v) - \bar{t})^2}}$$

Trong (1), $\bar{f}_{u,v}$ biểu thị giá trị trung bình của f(x, y) trong vùng của khuôn mẫu t được dịch chuyển đến (u, v) được tính bằng

$$\bar{f}_{u,v} = \frac{1}{N_x N_y} \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y)$$

Với ký hiệu tương tự, $\bar{t}$ là giá trị trung bình của khuôn mẫu t. Mẫu số trong (1) là phương sai của hàm ảnh có giá trị trung bình bằng không f(x, y) - $\bar{f}_{u,v}$ và hàm khuôn mẫu dịch chuyển có giá trị trung bình bằng không t(x-u, y-v) - $\bar{t}$. Do chuẩn hóa này, γ(u, v) độc lập với sự thay đổi về độ sáng hoặc độ tương phản của ảnh, liên quan đến giá trị trung bình và độ lệch chuẩn.

Vị trí mong muốn $(u_{pos}, v_{pos})$ của mẫu, được biểu diễn bởi t, tương đương với vị trí $(u_{max}, v_{max})$ của giá trị lớn nhất $\gamma_{max}$ của γ(u, v). Do chuẩn hóa, việc sử dụng (1) để tính toán vị trí của mẫu mạnh mẽ hơn các phép đo tương đồng khác, như hiệp phương sai đơn giản hoặc tổng các sai biệt tuyệt đối (SAD). Tuy nhiên nhược điểm chính là việc tính toán (1) đòi hỏi nhiều tính toán. Đối với mẫu số, chuẩn hóa hệ số tương quan chéo, tại mỗi điểm (u, v); u ∈ {0, ..., $M_x - N_x$}, v ∈ {0, ..., $M_y - N_y$} của ảnh, tại đó γ(u, v) được xác định, năng lượng của ảnh có giá trị trung bình bằng không

$$e_f(u,v) = \sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} (f(x,y) - \bar{f}_{u,v})^2$$

và giá trị trung bình của ảnh trong vùng của hàm khuôn mẫu $\bar{f}_{u,v}$ (2) phải được tính toán lại. Nếu phép tính này được thực hiện theo cách đơn giản nhất, theo (1), số lượng phép tính tỷ lệ với $N_x N_y (M_x - N_x)(M_y - N_y)$, mặc dù năng lượng của hàm khuôn mẫu có giá trị trung bình bằng không

$$e_t(u,v) = \sum_{x=1}^{N_x} \sum_{y=1}^{N_y} (t(x,y) - \bar{t})^2$$

và giá trị trung bình của hàm khuôn mẫu $\bar{t}$ chỉ cần được tính toán trước một lần. Khối lượng tính toán này không thể chấp nhận được đối với hầu hết các ứng dụng thực tế. Tử số trong (1) có thể được tính toán trong miền tần số sử dụng phép biến đổi Fourier nổi tiếng, tuy nhiên số lượng phép tính vẫn tương đối cao, và một thuật toán tính toán tương quan chéo chuẩn hóa với ít phép tính hơn rất được quan tâm.

Để khắc phục những vấn đề phức tạp này, một phương pháp hiệu quả để tính toán mẫu số của hệ số tương quan chéo chuẩn hóa được đề xuất bởi Lewis. Ý tưởng chính là tính toán trước các bảng tổng chứa tích phân qua hàm ảnh f(x, y) và hàm ảnh bình phương $f^2(x, y)$ (tổng chạy) một lần cho mỗi ảnh f, và sử dụng các bảng này để tính toán hiệu quả biểu thức $(f(x, y) - \bar{f}_{u,v})^2$ tại mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa được đánh giá. Sử dụng các bảng tổng này, số lượng phép tính kết quả cho mẫu số không còn phụ thuộc vào kích thước của khuôn mẫu $N_x, N_y$ mà chỉ phụ thuộc vào kích thước của hàm ảnh $M_x, M_y$. Một mô tả ngắn gọn về phép tính này được đưa ra trong phần 3.1.

Trong phần 3.2, ý tưởng chính, cho phép tính toán rất hiệu quả tử số của (1) được giải thích chi tiết. Do đó, hệ số tương quan chéo chuẩn hóa (1) có thể được tính toán cho một hàm khuôn mẫu xấp xỉ $\tilde{t}(x, y)$ với số lượng phép tính ít hơn một bậc so với phương pháp FFT tiêu chuẩn, mở ra nhiều ứng dụng mới.

## 3. THUẬT TOÁN NCC NHANH

### 3.1. Tính toán mẫu số

Để đơn giản hóa việc tính toán mẫu số của hệ số tương quan chéo chuẩn hóa, ý tưởng chính là sử dụng hai bảng tổng s(u, v) và $s_2(u, v)$ qua hàm ảnh f(x, y) và năng lượng ảnh $f^2(x, y)$. Bảng tổng của hàm ảnh được định nghĩa đệ quy bởi

$$s(u,v) = f(u,v) + s(u-1,v) + s(u,v-1) - s(u-1,v-1)$$

Một định nghĩa đệ quy tương tự cho bảng tổng qua năng lượng ảnh được đưa ra bởi

$$s_2(u,v) = f^2(u,v) + s_2(u-1,v) + s_2(u,v-1) - s_2(u-1,v-1)$$

với s(u, v) = $s_2(u, v) = 0$ khi u, v < 0. Thuật toán sau đây để tính toán đơn giản hóa mẫu số có thể được áp dụng cho toàn bộ hàm ảnh hoặc bất kỳ ảnh con nào của hàm ảnh f(x, y). Sau đó, các bảng tổng chỉ cần được tính toán cho vùng ảnh con này. Với các bảng này, (2) có thể được tính toán một cách rất hiệu quả, độc lập với kích thước $N_x, N_y$ của khuôn mẫu

$$\sum_{x=u}^{u+N_x-1} \sum_{y=v}^{v+N_y-1} f(x,y) = s(u+N_x-1,v+N_y-1) - s(u-1,v+N_y-1) - s(u+N_x-1,v-1) + s(u-1,v-1)$$

Có thể thấy từ (7), chỉ cần ba phép cộng/trừ để đánh giá tổng kép qua f(x, y) bằng cách đánh giá bảng tổng s(u, v).

Mẫu số của (1) sau đó được đánh giá sử dụng các bảng tổng chạy (5), (6) và

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - 2\bar{f}_{u,v} \sum_x \sum_y f(x,y) + \sum_x \sum_y \bar{f}_{u,v}^2$$

Trong (8) và tất cả các phương trình trong phần còn lại của đoạn này, tổng kép $\sum_x \sum_y$ được đánh giá trên vùng của khuôn mẫu, có nghĩa là u < x < u + $N_x - 1$ và v < y < v + $N_y - 1$. Với

$$\sum_x \sum_y \bar{f}_{u,v}^2 = N_x N_y \left(\frac{1}{N_x N_y} \sum_x \sum_y f(x,y)\right)^2$$

(8) có thể được đơn giản hóa thành

$$\sum_x \sum_y (f(x,y) - \bar{f}_{u,v})^2 = \sum_x \sum_y f^2(x,y) - \frac{1}{N_x N_y}\left(\sum_x \sum_y f(x,y)\right)^2$$

Các biểu thức tổng trong (10) qua $f^2(x, y)$ và f(x, y) có thể được tính toán hiệu quả sử dụng các bảng tổng chạy (5), (6). Hơn nữa, một phép tính căn bậc hai phải được tính toán cho mỗi điểm (u, v) để xác định mẫu số của (1).

### 3.2. Tính toán tử số

Áp dụng thuật toán được trình bày trong phần trước cho phép tính toán hiệu quả mẫu số, nhưng số lượng phép tính cần thiết để tính toán tử số của hệ số NCC (1) vẫn tương đối cao, ngay cả khi nó được thực hiện trong miền tần số với thuật toán FFT. Do đó, cần đơn giản hóa thêm phép tính này. Tử số có thể được viết lại như

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v) - \bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$$

trong đó $t'(x-u,y-v)$ là hàm khuôn mẫu có giá trị trung bình bằng không được định nghĩa bởi

$$t'(x-u,y-v) = t(x-u,y-v) - \bar{t}$$

Vì $t'(x,y)$ có giá trị trung bình bằng không và do đó cũng có tổng bằng không, nên số hạng $\bar{f}_{u,v} \sum_x \sum_y t'(x-u,y-v)$ cũng bằng không, và tử số của (1) có thể được viết là

$$N(u,v) = \sum_x \sum_y f(x,y)t'(x-u,y-v)$$

Ý tưởng cơ bản để đơn giản hóa việc tính toán tử số là mở rộng hàm khuôn mẫu có giá trị trung bình bằng không $t'(x,y)$ thành tổng có trọng số của K hàm cơ sở hình chữ nhật $t_i$, tạo ra một xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu

$$\tilde{t}(x,y) = \sum_{i=1}^K k_i t_i(x,y)$$

Tử số của hệ số tương quan chéo chuẩn hóa sau đó có thể được đánh giá hiệu quả với xấp xỉ này. Hàm cơ sở $t_i(x,y)$ là hằng số bằng 1 bên trong một vùng hình chữ nhật $x_l^i ≤ x ≤ x_u^i \wedge y_l^i ≤ y ≤ y_u^i$ và bằng không nếu khác. Do đó $x_l^i$ và $x_u^i$ là các chỉ số của giới hạn dưới và giới hạn trên của vùng hình chữ nhật bằng 1 hằng số theo hướng x. $y_l^i$ và $y_u^i$ là các chỉ số tương ứng theo hướng y. Hình 1 cho thấy một ví dụ về một hàm cơ sở hình chữ nhật $t_i(x,y)$ trong đó $N_x = N_y = 20$. Nó bằng một hằng số cho $6 ≤ x ≤ 14 \wedge 8 ≤ y ≤ 12$. Chất lượng của xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu có giá trị trung bình bằng không ban đầu $t'(x,y)$ phụ thuộc vào cả việc lựa chọn các hàm cơ sở, có nghĩa là, trên giới hạn dưới và trên của chúng $x_l^i, x_u^i$ và $y_l^i, y_u^i$ và số lượng hàm cơ sở được sử dụng.

Sử dụng khai triển tổng của hàm khuôn mẫu $\tilde{t}(x,y)$ cho phép tính toán một xấp xỉ cho tử số của hệ số tương quan chéo

$$\tilde{N}(u,v) = \sum_{i=1}^K k_i \sum_{x=x_l^i+u}^{x_u^i+u} \sum_{y=y_l^i+v}^{y_u^i+v} f(x,y)$$

trong đó K là số lượng hàm cơ sở và $k_i$ là hệ số cho hàm cơ sở i. Nhớ rằng $x_l^i$ và $y_l^i$ biểu thị các chỉ số của các giới hạn dưới của hàm cơ sở i và $x_u^i$ và $y_u^i$ sau đó là các chỉ số của các giới hạn trên. (15) thu được từ (13) sử dụng (14), bởi vì các hàm cơ sở $t_i(x,y)$ hoặc là hằng số bằng một hoặc bằng không. Với bảng tổng chạy qua hàm ảnh (5), đã được tính toán để đơn giản hóa việc xác định mẫu số, mà không thể được tính toán trong miền tần số, tổng kép bên trong trong (15) có thể được tính toán với chỉ 3 phép toán cộng/trừ.

$$\sum_{x=x_l^i}^{x_u^i} \sum_{y=y_l^i}^{y_u^i} f(x,y) = s(x_u^i, y_u^i) - s(x_u^i, y_l^i-1) - s(x_l^i-1, y_u^i) + s(x_l^i-1, y_l^i-1)$$

### 3.3. Phân tích độ phức tạp cho việc tính toán tử số

Số lượng phép tính cần thiết để tính toán tử số của hệ số NCC cho một ảnh có kích thước $M_x * M_y$ và một khuôn mẫu có kích thước $N_x * N_y$ đã được xấp xỉ với K hàm cơ sở hình chữ nhật được đưa ra trong bảng 1. Có thể thấy rằng số lượng phép nhân phụ thuộc tuyến tính vào số lượng hàm cơ sở K cho một hàm ảnh f(x, y) đã cho. Đối với mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa γ(u, v) được đánh giá, K phép nhân được yêu cầu để tính toán tử số, một cho mỗi hàm cơ sở.

Ngược lại, một đánh giá trực tiếp của γ(u, v) yêu cầu $N_x * N_y$ phép nhân tại mỗi điểm (u, v) tại đó γ được đánh giá. Độ phức tạp của FFT phụ thuộc vào kích thước của khuôn mẫu và hàm ảnh. Khi M lớn hơn nhiều so với N, nó thậm chí có thể vượt quá số lượng phép tính yêu cầu bởi phương pháp trực tiếp. Độc lập với M và N, nó cũng có thể vượt xa số lượng phép tính yêu cầu bởi thuật toán được đề xuất tùy thuộc vào số lượng hàm cơ sở K.

Số lượng phép tính cần thiết để thiết lập các bảng tổng và đánh giá mẫu số của hệ số tương quan chéo chuẩn hóa không được đề cập trong bảng 1 và bảng 2. Để tính toán mẫu số, chuẩn hóa hệ số tương quan chéo, phương pháp được đề xuất bởi Lewis hiệu quả hơn nhiều so với một tính toán trực tiếp, đơn giản. Do đó, các phép tính để thiết lập các bảng tổng là cần thiết cho cả ba phương pháp được thảo luận ở đây và không được bao gồm trong so sánh. Tất nhiên, thuật toán được đề xuất để tính toán tử số cũng có thể được áp dụng để tính toán hiệu quả một tương quan chéo thông thường không chuẩn hóa, nhưng sau đó nỗ lực bổ sung để thiết lập các bảng tổng phải được tính đến so với FFT tiêu chuẩn, không yêu cầu bất kỳ bảng tổng nào.

**Bảng 1. Phân tích độ phức tạp, chỉ tử số.**

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ |
|FFT | $9M_x M_y \log_2(M_x M_y)$ | $6M_x M_y \log_2(M_x M_y)$ |
|Thuật toán mới | $(4K - 1)(M_x - N_x + 1)(M_y - N_y + 1)$ | $K(M_x - N_x + 1)(M_y - N_y + 1)$ |

**Bảng 2. Phân tích độ phức tạp, Ví dụ.**

$M_x = 640, M_y = 480, N_x = N_y = 64, K = 3$

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | 985.5 Triệu | 985.5 Triệu |
|FFT | 50.4 Triệu giới hạn dưới và giới hạn trên của vùng hình chữ nhật bằng 1 hằng số theo hướng x. $y_l^i$ và $y_u^i$ là các chỉ số tương ứng theo hướng y. Hình 1 cho thấy một ví dụ về một hàm cơ sở hình chữ nhật $t_i(x,y)$ trong đó $N_x = N_y = 20$. Nó bằng một hằng số cho $6 ≤ x ≤ 14 \wedge 8 ≤ y ≤ 12$. Chất lượng của xấp xỉ $\tilde{t}(x,y)$ của hàm khuôn mẫu có giá trị trung bình bằng không ban đầu $t'(x,y)$ phụ thuộc vào cả việc lựa chọn các hàm cơ sở, có nghĩa là, trên giới hạn dưới và trên của chúng $x_l^i, x_u^i$ và $y_l^i, y_u^i$ và số lượng hàm cơ sở được sử dụng. -->

Sử dụng khai triển tổng của hàm khuôn mẫu $\tilde{t}(x,y)$ cho phép tính toán một xấp xỉ cho tử số của hệ số tương quan chéo

$$\tilde{N}(u,v) = \sum_{i=1}^K k_i \sum_{x=x_l^i+u}^{x_u^i+u} \sum_{y=y_l^i+v}^{y_u^i+v} f(x,y)$$

trong đó K là số lượng hàm cơ sở và $k_i$ là hệ số cho hàm cơ sở i. Nhớ rằng $x_l^i$ và $y_l^i$ biểu thị các chỉ số của các giới hạn dưới của hàm cơ sở i và $x_u^i$ và $y_u^i$ sau đó là các chỉ số của các giới hạn trên. (15) thu được từ (13) sử dụng (14), bởi vì các hàm cơ sở $t_i(x,y)$ hoặc là hằng số bằng một hoặc bằng không. Với bảng tổng chạy qua hàm ảnh (5), đã được tính toán để đơn giản hóa việc xác định mẫu số, mà không thể được tính toán trong miền tần số, tổng kép bên trong trong (15) có thể được tính toán với chỉ 3 phép toán cộng/trừ.

$$\sum_{x=x_l^i}^{x_u^i} \sum_{y=y_l^i}^{y_u^i} f(x,y) = s(x_u^i, y_u^i) - s(x_u^i, y_l^i-1) - s(x_l^i-1, y_u^i) + s(x_l^i-1, y_l^i-1)$$

### 3.3. Phân tích độ phức tạp cho việc tính toán tử số

Số lượng phép tính cần thiết để tính toán tử số của hệ số NCC cho một ảnh có kích thước $M_x * M_y$ và một khuôn mẫu có kích thước $N_x * N_y$ đã được xấp xỉ với K hàm cơ sở hình chữ nhật được đưa ra trong bảng 1. Có thể thấy rằng số lượng phép nhân phụ thuộc tuyến tính vào số lượng hàm cơ sở K cho một hàm ảnh f(x, y) đã cho. Đối với mỗi điểm (u, v), tại đó hệ số tương quan chéo chuẩn hóa γ(u, v) được đánh giá, K phép nhân được yêu cầu để tính toán tử số, một cho mỗi hàm cơ sở.

Ngược lại, một đánh giá trực tiếp của γ(u, v) yêu cầu $N_x * N_y$ phép nhân tại mỗi điểm (u, v) tại đó γ được đánh giá. Độ phức tạp của FFT phụ thuộc vào kích thước của khuôn mẫu và hàm ảnh. Khi M lớn hơn nhiều so với N, nó thậm chí có thể vượt quá số lượng phép tính yêu cầu bởi phương pháp trực tiếp. Độc lập với M và N, nó cũng có thể vượt xa số lượng phép tính yêu cầu bởi thuật toán được đề xuất tùy thuộc vào số lượng hàm cơ sở K.

Số lượng phép tính cần thiết để thiết lập các bảng tổng và đánh giá mẫu số của hệ số tương quan chéo chuẩn hóa không được đề cập trong bảng 1 và bảng 2. Để tính toán mẫu số, chuẩn hóa hệ số tương quan chéo, phương pháp được đề xuất bởi Lewis hiệu quả hơn nhiều so với một tính toán trực tiếp, đơn giản. Do đó, các phép tính để thiết lập các bảng tổng là cần thiết cho cả ba phương pháp được thảo luận ở đây và không được bao gồm trong so sánh. Tất nhiên, thuật toán được đề xuất để tính toán tử số cũng có thể được áp dụng để tính toán hiệu quả một tương quan chéo thông thường không chuẩn hóa, nhưng sau đó nỗ lực bổ sung để thiết lập các bảng tổng phải được tính đến so với FFT tiêu chuẩn, không yêu cầu bất kỳ bảng tổng nào.

**Bảng 1. Phân tích độ phức tạp, chỉ tử số.**

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ | $N_x N_y (M_x - N_x + 1)(M_y - N_y + 1)$ |
|FFT | $9M_x M_y \log_2(M_x M_y)$ | $6M_x M_y \log_2(M_x M_y)$ |
|Thuật toán mới | $(4K - 1)(M_x - N_x + 1)(M_y - N_y + 1)$ | $K(M_x - N_x + 1)(M_y - N_y + 1)$ |

**Bảng 2. Phân tích độ phức tạp, Ví dụ.**

$M_x = 640, M_y = 480, N_x = N_y = 64, K = 3$

| | Cộng / Trừ | Nhân |
|---|---|---|
|Tính toán trực tiếp | 985.5 Triệu | 985.5 Triệu |
|FFT | 50.4 Triệu