# MỞ ĐẦU

Đảm bảo tính sẵn sàng (Availability) cho máy chủ và ứng dụng web là mục tiêu cốt lõi của an toàn thông tin. Các cuộc tấn công từ chối dịch vụ (Denial of Service, DoS) và từ chối dịch vụ phân tán (Distributed Denial of Service, DDoS) trực tiếp làm tê liệt hoạt động của dịch vụ trực tuyến. Khác với các đợt tấn công bão hòa băng thông tầng mạng (Volumetric DoS), hình thức tấn công cạn kiệt tài nguyên tầng ứng dụng (Layer 7) như tấn công tốc độ chậm (Slow HTTP Attack) nhắm vào cơ chế quản lý phiên của máy chủ. Cụ thể, công cụ Slowloris duy trì các kết nối HTTP dở dang với lưu lượng rất thấp để chiếm dụng toàn bộ bảng kết nối đồng thời, khiến máy chủ không thể tiếp nhận người dùng hợp lệ.

Báo cáo bài tập lớn tập trung nghiên cứu cơ chế phát động của Slowloris, xây dựng mô hình thử nghiệm cô lập và triển khai giải pháp phòng thủ hai tầng phối hợp giữa máy chủ web Nginx và tường lửa Linux iptables.

Nội dung báo cáo gồm ba chương:

* **Chương 1. Tổng quan về tấn công và phòng chống DoS/DDoS:** Trình bày khái niệm, cơ chế mạng botnet, phân loại kỹ thuật tấn công theo mô hình OSI và các nguyên lý phòng thủ theo chiều sâu.
* **Chương 2. Phân tích công cụ tấn công Slowloris và giải pháp phòng thủ:** Làm rõ cơ chế giữ kết nối dở dang của Slowloris, đặc điểm nhận diện trên hệ thống và thiết kế mô hình phòng thủ kết hợp Nginx với iptables.
* **Chương 3. Thực nghiệm và đánh giá kết quả:** Triển khai mô hình thử nghiệm cô lập, thực nghiệm tấn công bằng SlowHTTPTest, kích hoạt cấu hình phòng thủ hai tầng và đối chiếu định lượng hiệu năng hệ thống.

---

# KẾT LUẬN

## Kết quả đạt được

Đề tài đã hoàn thành việc nghiên cứu lý thuyết, thực nghiệm tấn công và đánh giá phương án phòng thủ với các kết quả cụ thể:

* Hệ thống hóa kiến thức nền tảng về tấn công DoS/DDoS theo mô hình OSI và nguyên lý phòng thủ theo chiều sâu từ tầng mạng đến tầng ứng dụng.
* Làm rõ cơ chế hoạt động của Slowloris trong việc khai thác chuẩn HTTP/1.1 (RFC 7230): duy trì kết nối bằng tiêu đề dở dang để làm cạn kiệt bảng trạng thái kết nối máy chủ mà không tiêu tốn nhiều tài nguyên CPU hay băng thông mạng.
* Thiết lập môi trường thử nghiệm cô lập (Host-only), tái hiện hiện tượng máy chủ Nginx bị tê liệt hoàn toàn khi đạt trần 500 kết nối `ESTABLISHED` từ công cụ SlowHTTPTest.
* Triển khai và kiểm chứng giải pháp phòng thủ hai tầng kết hợp giữa Nginx (`client_header_timeout 10s`, `limit_conn addr 20`) và iptables (`connlimit 20`):
  * Giảm 97.9% số kết nối chiếm dụng đồng thời (từ trung bình 441.6 xuống 9.1 kết nối).
  * Giảm 80.8% tổng số socket mở trên cổng 80, giải phóng áp lực lưu vết phiên trên nhân Linux.
  * Tự động đóng 120 kết nối vi phạm thời gian chờ và chặn 380 kết nối vượt trần tại tầng mạng.
  * Duy trì tỷ lệ phục vụ 100% đối với các yêu cầu hợp lệ từ bên ngoài với độ trễ phản hồi ổn định (trung vị 1.53 ms).

## Hướng phát triển

Từ các kết quả đạt được, đề tài có thể tiếp tục mở rộng theo các hướng sau:

* Tự động hóa phòng thủ bằng Fail2ban: phân tích nhật ký truy cập Nginx để phát hiện các địa chỉ IP có hành vi treo phiên và tự động bổ sung luật chặn vào iptables.
* Đánh giá kịch bản tấn công phân tán (Distributed Slowloris): kiểm thử trường hợp nhiều máy tấn công phát động đồng thời nhằm xác định ngưỡng chịu tải khi luật giới hạn kết nối theo IP đơn lẻ (`connlimit`) bị phân tán.
* Mở rộng phạm vi thử nghiệm đối với các biến thể tấn công cạn kiệt tầng ứng dụng khác như Slow POST (chậm thân gói tin) và Slow Read (chậm nhận phản hồi).
