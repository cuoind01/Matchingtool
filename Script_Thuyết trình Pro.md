### Slide01:
- Thưa các Sếp sau đầu em xin được trình bày dự án Phát triển chương trình Vision ALign cho thiết bị SVI khu vực V2-3F

### Slide02:
- Dự án được triển khai tại công đoạn SVI V2-3F, thời gian thực hiện từ tháng 04.2024~09.2025

### Slide03:
- Báo cáo gôm 4 nội dung chính sau: 
1. Phân tích vấn đề nguyên nhân 
2. Ý tưởng nội dung cải tiến
3. Action items
4. Lịch trình thực hiện
5. Hiệu quả dự án

### Slide04:
Bối cảnh đưa ra là mục tiêu hiệu suất năm 2024 là 90%, theo phân tích hiệu suất 10.2023 thì thiết bị AFTP có Hiệu suất tổng hợp là 87.4%.

Trong đó HS thời gian 96.1%, HS tính năng là 90.9%
HS thời gian đang ở mức khá cao nên em sẽ tập trung phân tích HS tính năng

- Phân tích: Qua phân tích thấy được đang có sự chênh lệch tacttime  giữa công đoạn AFTP 6.0s và công đoạn SVI 6.17s. Do các khối hoạt động đọc lập nhưng ràng buộc lẫn nhau( không có LD/ULD) 
→ Dẫn đến ảnh hưởng đến HS tính năng. Những vấn đề đang gặp phải ở SVI như sau: 
+ Bin2 ảo cap 25.4%
+ Alarm liên quan đến Vision chiếm 34.3%
+ Tacttime cao: 6.17s

### Slide05:
- Đầu tiên về vấn đề Bin2 ảo cao: đa phần là do việc bị việc center ở vị trí inspection ngoại quan. Thuật toán Align không ổn định dễ bắt sai.
- Thứ 2 vấn đề về Alarm, lỗi Alarm liên quan đến vision là lỗi Vision Shuttle Align Fail. Nguyên nhân do đèn chiếu sáng từ trên xuống dễ gây lóa sáng làm phát sinh ALarm
- Thứ 3: Sau khi phân tích tacttime bằng E-Mas MCC, thì nhận thấy phần Shuttle liên quan đến Vision Align đang hơn các cụm còn lại
    + Phân tích kỹ hơn thấy cột ALign_Process chiếm 2.73s cao hơn so với các cụm Align ở các máy công đoạn khác.(chương trinh vision của vendor chưa tối ưu, việc dùng 2 TemplateMatch OpenCV làm tăng thời gian xử lý cho CPU)
- Cuối cùng, hiện tại đang có 3 chương trình vision của 3 vendor khác nhau gây khó khăn trong việc Jobchange, Teaching cũng như vẫn hành sửa chữa Alarm.

=> Từ các lý do trên nên em đang quyết định phát triển một chương trình Vision nội chế cho thiết bị SVI với các nội dung cải tiến sau:
### Slide06:
- Giảm Alarm: thay đổi Optic system: thay thế đèn chiếu sáng trực tiếp sang chiếu sáng ngược(Backlight) để loại bỏ lóa sáng và nhiễu
- Giảm Bin2 ảo, Tacttime: thay vì sử dụng 2 TemplateMatch thì bên em chỉ dùng 1 TemplateMatch kết hợp với các tool FindLine và IntersectLine để tìm được điểm Master Point → tăng tính chính xác giảm tacttime
- Cuối cùng: Thay thế 3 chương trình thành 1 chương trình nội chế duy nhất để giảm bắt gánh nặng cho Teaching, jobchange cũng như giảm độ phức tạp khi vận hành sửa chữa thiết bị.

### Slide07:
- Em xin phép sang phần 3 Action items: 
- Các nội dung thay đổi gồm 5 phần như sau: 
  + Cải tiến hardware quang học
  + Cải tiến software
  + Phát triển chương trình Robot
  + Phát triển truyền thông
  + Runtest
### Slide08:
- Về tổng quan chương trình em tiến hành xây dựng chương trình gồm các module: UI, Process, Data, Interface
### Slide09:
- Đầu tiên tiến hành cải tiến cụm quang học Align: thay thế đèn directlight sang backlight. Từ đó xây dựng module truyền thông với cụm đèn này thông qua RS232 với các phương thức bao gồm: Open, SetChannel, Set Light On, Set Light Off, Close
### Slide10:
- Tiếp theo xây dựng thuật toán cho chương trình Vision: 
  + Đầu tiên thuật ALign: bắt đầu → tiến hành lấy ảnh từ Camera → sau đó đi qua tool Image Processing để xử lý ảnh → rồi tìm Matching → nếu tìm được thì sẽ Findline để tìm được điểm Center Point, nếu không tìm được Matching thì sẽ tiến hành retry lại 3 lần.
  + Thứ 2 xây dựng thuật toán Calib:        
    * Để chuyển đổi hệ toạn độ từ Vision → Robot: chạy chương trình Calib 9 điểm từ đó tìm được ma trận Homography để quy đổi hệ tọa độ từ pixel sang mm, chiều của hệ tọa độ cũng như xử lý méo ảnh.
    * Tiếp theo chạy chương trình tìm kiếm tâm quay gồm tập hợp n điểm để tìm tâm xoay bằng các giải tập hợp điểm trong phương trình đường tròn
### Slide11:
- Sau đó Xây dựng Toolist cho thuật toán Align: 
1. Tool ImagePrcessing: sử dụng phép biến đổi tuyền tính( Linear Transform) để tăng độ tương phản
2. Tool DeepSegmentation: sử dụng kiến trúc U2-Net để phân tách vật thể với nền( để loại bỏ glare và noise)
3. Tool TemplateMatching: xây dựng tool TemplateMaching bằng việc sử dụng kỹ thuật Image Pyramid Technique và Normalized Cross Corelation(NCC) 
### Slide12:
4. Tool Findline: Xây dựng tool FindLine bằng việc sử dụng kernel Prewitt cho các Caliper để tìm được điểm biên phân tách rõ nhất. Sau đó từ tập hợp điểm này sẽ tìm được đường Line tốt nhất.
5. Tool IntersectLine: từ các đường line trên tìm giao điểm bằng cách giải hệ phương trình đường thẳng
6. Tool MakeLine: cuối cùng tìm điểm Center áp dụng công thức tính giá trị trung bình và ArcTan để tìm ra được tọa độ x, y, theta
7. Từ đó tính được Offset bằng các sử dụng các phương pháp biến đổi theo ma trận góc xoay và ma trận tịnh tiến để tìm được offset x,y,t để gửi cho main Control
### Slide13: 
- Tiếp đến là xây dựng giao diện: bao gồm Auto, Setting, Teaching, Calibration

### Slide14:
- Cuối cùng là xây dựng chương trình truyền thông
  + đầu tiên là truyền thông với Main Control thông qua TCP/IP: 
    * Khi Main Control phát truyền tín hiệu Align 
    * Vision nhận lệnh sau đó tiến hành trigger đưa ra kết quả và gửi về cho Main Control giá trị XYT và lệnh Compelete
    * Hoặc gửi về Error khi phát sinh Alarm
### Slide15:
+ Tiếp theo là xây dựng chương trình AutoCalib với Robot:
+ Chương trình Vision sẽ tiến hành Start Calib để bắt đầu, sau đó sẽ gửi đi giá trị Offset theo tọa độ điểm đề ra
+ Cuối cùng là lệnh EndCalib để kết thúc quá trình này
  
### Slide16:
- Hiện nay dự án đã được áp dụng 21/24 Line khu vực V2-3F
### Slide17:
- Về phần hiệu quả: 
  + Hiệu suất từ 87.4 đã lên 90% đạt KPI đề ra 
  + Alarm liên quan đến lỗi Vision giảm từ 53 vụ xuống 1 vụ/tháng
  + Tacttime giảm từ 6.17s → 6.0s, Capa tacttime ở cụm Shuttle từ 6.15s → 5.05s
  + Chi phí costdown giảm 409,868$ ~ 5 억 Won
### Slide18:

### Slide19:

### Slide20: