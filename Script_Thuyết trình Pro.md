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
- Giảm Bin2 ảo, Tacttime: thay vì
### Slide07:

### Slide08:

### Slide09:

### Slide10:

### Slide11:

### Slide12:

### Slide13:


### Slide14:

### Slide15:

### Slide16:

### Slide17:

### Slide18:

### Slide19:

### Slide20: