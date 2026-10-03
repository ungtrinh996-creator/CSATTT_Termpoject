# CHƯƠNG 1. TỔNG QUAN VỀ TẤN CÔNG VÀ PHÒNG CHỐNG DoS/DDoS

## 1.1 Khái niệm cơ bản về DoS và DDoS

### 1.1.1 Định nghĩa tấn công từ chối dịch vụ (DoS)

Tấn công từ chối dịch vụ (Denial of Service, DoS) ngăn người dùng hợp lệ truy cập một tài nguyên hệ thống, hoặc làm chậm các hoạt động của hệ thống. DoS nhắm vào tính sẵn sàng (Availability) của dịch vụ. Kẻ tấn công không cần chiếm quyền hay lấy dữ liệu, chỉ cần làm cạn hoặc làm hỏng thứ mà dịch vụ dựa vào. Mỗi kỹ thuật tấn công nhắm vào một loại tài nguyên: băng thông, tài nguyên giao thức (như hàng đợi kết nối) hoặc năng lực xử lý của ứng dụng.

DoS có hai hướng thực hiện:

- **Khai thác lỗ hổng:** Vài gói tin chế tạo đặc biệt đủ làm phần mềm hoặc hệ điều hành treo. Ping of Death gửi gói ICMP vượt kích thước tối đa. Teardrop và Land khai thác hai lỗ hổng trong ngăn xếp TCP/IP.
- **Làm ngập tài nguyên:** Gửi lượng yêu cầu vượt năng lực xử lý, hoặc giữ nhiều kết nối ở trạng thái chưa hoàn tất. Hướng này không phụ thuộc lỗ hổng cụ thể nên áp dụng được cho hầu hết dịch vụ.

### 1.1.2 Tấn công từ chối dịch vụ phân tán (DDoS) và mạng botnet

DDoS (Distributed Denial of Service) là DoS với nhiều nguồn tấn công, thường là các máy nằm trong một botnet. Sự khác biệt này kéo theo chênh lệch về quy mô, khả năng chặn và khả năng truy vết:

| Tiêu chí | DoS | DDoS |
|---|---|---|
| Số nguồn tấn công | Một máy hoặc một vài máy | Nhiều máy, thường thuộc một botnet; Mirai đạt đỉnh khoảng 600.000 thiết bị nhiễm |
| Quy mô lưu lượng | Bị giới hạn bởi các máy tấn công | Lớn hơn đáng kể; Cloudflare ghi nhận 935 đợt tấn công tầng mạng vượt 1 Tbps trong nửa đầu 2026 |
| Khả năng chặn | Chặn IP nguồn thường đủ | Khó, vì nguồn phân tán và có thể bị giả mạo IP |
| Khả năng truy vết | Tương đối dễ | Khó, vì nguồn phân tán và được che giấu bằng giả mạo IP, botnet |

Botnet là mạng máy tính bị xâm nhập và bị kẻ tấn công điều khiển. Một botnet gồm botmaster (người điều hành), máy chủ chỉ huy và điều khiển (C&C) chuyển lệnh xuống các bot, và bản thân các bot (máy tính, camera IP, router... bị nhiễm). Vòng đời có ba giai đoạn: lây nhiễm, chờ lệnh và tấn công. Mirai lây nhiễm thiết bị IoT nhờ thông tin đăng nhập mặc định, rồi các bot nhận lệnh từ C&C để tấn công.

Botnet truyền thống có một vị trí C&C tập trung. Botnet ngang hàng (P2P) không có máy chủ C&C nên khó bị vô hiệu hóa hơn.

Mirai là ví dụ điển hình. Botnet này gồm chủ yếu thiết bị nhúng và IoT, đạt đỉnh khoảng 600.000 thiết bị nhiễm. Ngày 21/10/2016, một đợt tấn công vào hạ tầng DNS của Dyn dùng lưu lượng TCP và UDP giả dạng trên cổng 53. Dyn xác nhận Mirai là nguồn chính, với ước tính ban đầu tới 100.000 điểm cuối độc hại. Nhiều website lớn không truy cập được. Kẻ tấn công cũng không cần tự dựng botnet, vì có thể mua tấn công từ các dịch vụ DDoS-as-a-Service (booter).

### 1.1.3 Hậu quả và thiệt hại

- **Gián đoạn dịch vụ và thiệt hại tài chính:** Website, ứng dụng hoặc email ngừng hoạt động, tổ chức mất thời gian và tiền bạc để xử lý.
- **Mất uy tín:** Tổ chức chịu thiệt hại danh tiếng khi dịch vụ không truy cập được.
- **Màn khói (smokescreen):** Khảo sát của Kaspersky Lab và B2B International năm 2016 cho thấy 56% doanh nghiệp tin rằng DDoS từng bị dùng để đánh lạc hướng trước một cuộc xâm nhập khác.
- **Lan sang bên thứ ba:** Khi nhà cung cấp DNS bị tấn công, các dịch vụ dựa vào họ cũng ngừng, như vụ Dyn năm 2016.

## 1.2 Phân loại DoS/DDoS theo mô hình OSI

Báo cáo chọn cách chia theo tầng OSI vì mỗi tầng gắn với một loại tài nguyên bị khai thác, nên cũng gắn với một hướng phòng thủ riêng. CISA, FBI và MS-ISAC chia kỹ thuật DoS/DDoS thành ba loại: volumetric (làm cạn băng thông), protocol (khai thác giao thức, tầng 3 và 4) và application (nhắm vào ứng dụng, tầng 7). Các loại này không loại trừ nhau. Báo cáo thêm nhóm khuếch đại/phản xạ vì nhóm này dùng một kỹ thuật riêng để nhân lưu lượng.

| Nhóm | Tầng OSI | Tài nguyên bị nhắm tới | Ví dụ |
|---|---|---|---|
| Volumetric | 3, 4 | Băng thông | UDP Flood, ICMP Flood |
| Protocol | 3, 4 | Bảng trạng thái, tài nguyên thiết bị | SYN Flood |
| Application layer | 7 | CPU, RAM, số kết nối của ứng dụng | HTTP Flood, Slowloris, Slow POST |
| Amplification/Reflection | 3, 4, 7 | Băng thông (nhờ khuếch đại) | DNS, NTP, Memcached Amplification |

### 1.2.1 Tấn công tầng mạng/vận chuyển (Volumetric và Protocol)

**Volumetric:** Lấp đầy đường truyền của nạn nhân bằng lượng dữ liệu lớn.

- **UDP Flood:** Gửi dồn dập gói UDP tới các cổng ngẫu nhiên. Với mỗi gói, máy đích kiểm tra xem có ứng dụng nào nghe ở cổng đó không. Nếu không có, nó trả gói ICMP "Destination Unreachable". Xử lý và trả lời hàng loạt như vậy làm cạn tài nguyên máy đích.
- **ICMP Flood (Ping Flood):** Gửi nhiều gói ICMP Echo Request, buộc đích trả Echo Reply cho từng gói, nên tốn băng thông cả chiều vào lẫn chiều ra. Lưu lượng đối xứng: băng thông nạn nhận bằng tổng lưu lượng các bot gửi.

**Protocol:** Khai thác cách giao thức hoạt động để làm cạn tài nguyên trạng thái của máy chủ hoặc thiết bị mạng.

- **SYN Flood:** Bình thường, client gửi SYN, server trả SYN-ACK rồi chờ ACK để hoàn tất kết nối. Kẻ tấn công gửi hàng loạt SYN mà không hoàn tất bắt tay, nên các kết nối half-open chiếm hàng đợi (backlog) của cổng đích. Khi hàng đợi đầy, kết nối hợp lệ không vào được. Tấn công này nhắm vào hàng đợi, không nhằm làm quá tải mạng hay bộ nhớ của máy chủ.

### 1.2.2 Tấn công tầng ứng dụng (Layer 7)

Loại này nhắm thẳng vào web server, cơ sở dữ liệu hoặc API. Lưu lượng thường nhỏ nhưng khó phân biệt với người dùng thật vì yêu cầu trông hợp lệ.

- **HTTP Flood:** Gửi nhiều yêu cầu GET hoặc POST. POST thường tốn tài nguyên hơn vì máy chủ phải xử lý và ghi dữ liệu vào cơ sở dữ liệu.
- **Slowloris:** Mở nhiều kết nối, gửi tiêu đề HTTP chưa hoàn chỉnh, rồi định kỳ gửi thêm tiêu đề để kết nối không bị đóng. Khi mọi luồng phục vụ bị chiếm, người dùng thật không được phục vụ. Cách này dùng rất ít băng thông.
- **Slow POST (R.U.D.Y.):** Gửi yêu cầu POST khai báo trước lượng dữ liệu sẽ gửi, rồi gửi dữ liệu rất chậm. Máy chủ phải giữ kết nối để chờ.

### 1.2.3 Tấn công khuếch đại (Amplification/Reflection)

Kiểu này kết hợp hai ý tưởng. Với phản xạ, kẻ tấn công gửi yêu cầu tới máy chủ trung gian công khai, giả mạo IP nguồn thành IP nạn nhân, nên phản hồi đổ về nạn nhân. Với khuếch đại, phản hồi lớn hơn yêu cầu nhiều lần. Hệ số khuếch đại băng thông (BAF) là tỷ lệ giữa số byte tải UDP trong phản hồi và số byte tải UDP của yêu cầu.

| Kỹ thuật | Giao thức/cổng | Cách khuếch đại | BAF |
|---|---|---|---|
| DNS Amplification | UDP/53 | Truy vấn nhỏ tới máy chủ DNS công khai, nhận phản hồi lớn | 28 đến 54 |
| NTP Amplification | UDP/123 | Lạm dụng lệnh monlist | 556,9 |
| Memcached Amplification | UDP/11211 | Yêu cầu nhỏ để lấy dữ liệu lớn trong bộ nhớ đệm | Tới 51.200 |

GitHub nhận một đợt tấn công memcached đạt đỉnh 1,35 Tbps (126,9 triệu gói/giây). Nguyên nhân gốc là các máy chủ memcached vô tình mở ra Internet với UDP bật, cộng với việc giả mạo IP nguồn.

Ba biện pháp giảm rủi ro:

1. Lọc gói giả mạo IP ở phía nhà cung cấp mạng (ingress filtering, BCP 38).
2. Tắt UDP của memcached khi không cần và cách ly máy chủ khỏi Internet.
3. Cập nhật cấu hình dịch vụ để hạn chế bị lạm dụng.

> **Hình 1.5.** Mô hình tấn công khuếch đại/phản xạ.

## 1.3 Tổng quan các nguyên lý và kỹ thuật phòng chống DoS/DDoS

Các kỹ thuật tấn công có thể kết hợp với nhau, nên một biện pháp đơn lẻ không đủ. CISA khuyến nghị kết hợp giám sát mạng, IDS, lọc bằng firewall và giới hạn tốc độ, dịch vụ giảm thiểu DDoS, cân bằng tải và dự phòng. Báo cáo gọi cách tiếp cận này là phòng thủ nhiều lớp (defense in depth) và gom kỹ thuật thành ba lớp: hạ tầng mạng, máy chủ dịch vụ, hệ thống phát hiện/ngăn chặn xâm nhập.

> **Hình 1.6.** Mô hình phòng thủ nhiều lớp chống DoS/DDoS.

### 1.3.1 Phòng thủ tại hạ tầng mạng

- **Firewall và ACL:** Lọc gói theo IP, cổng, giao thức, kèm giới hạn tốc độ. Khi nguồn tấn công phân tán, việc chặn từng địa chỉ IP trở nên khó.
- **Chống giả mạo IP:** Ingress filtering (BCP 38) ngăn gói giả mạo IP đi ra từ phía sau điểm tập trung của nhà cung cấp mạng. BCP 84 mở rộng cách này cho mạng multihomed và mô tả kiểm tra đường đi ngược (uRPF). Vì tấn công phản xạ cần giả mạo IP nguồn, lọc giả mạo làm giảm hiệu quả của nó.
- **Blackholing (RTBH):** Nhà vận hành quảng bá một route BGP cho địa chỉ đang bị tấn công, với next-hop trỏ vào giao diện discard. Mọi lưu lượng tới địa chỉ đó bị bỏ ngay ở biên mạng. Hạ tầng còn lại được bảo vệ, nhưng chính dịch vụ bị nhắm tới cũng không còn truy cập được.
- **BGP Anycast và scrubbing:** Cùng một địa chỉ được quảng bá từ nhiều node. Mỗi node hấp thụ lưu lượng tấn công phát sinh trong vùng của nó, và với nguồn phân tán rộng thì việc xử lý được chia đều cho các node. Dịch vụ giảm thiểu DDoS và CDN lọc lưu lượng độc hại trước khi chuyển về mạng của tổ chức. Cloudflare, chẳng hạn, xử lý ICMP echo ngay tại biên mạng Anycast của họ.

### 1.3.2 Phòng thủ tại máy chủ dịch vụ

- **Củng cố nhân hệ điều hành:** Linux có ba tham số liên quan đến SYN Flood:
  - `tcp_syncookies`: Gửi syncookie khi hàng đợi SYN của socket tràn.
  - `tcp_max_syn_backlog`: Đặt số yêu cầu kết nối tối đa ở trạng thái SYN_RECV.
  - `tcp_synack_retries`: Đặt số lần gửi lại SYN-ACK (mặc định là 5, tương ứng khoảng 63 giây cho đến khi hết thời gian chờ).

  Tài liệu kernel coi syncookies là cơ chế dự phòng, vì nó vi phạm đặc tả TCP và có thể làm giảm chất lượng một số dịch vụ. RFC 4987 xếp SYN cache và SYN cookies là hai kỹ thuật khả thi phía máy chủ.

- **Giới hạn tốc độ và kết nối:** Nginx giới hạn tốc độ yêu cầu theo khóa, chẳng hạn theo IP, bằng phương pháp leaky bucket (`limit_req`). Nó cũng giới hạn số kết nối đồng thời của mỗi khóa (`limit_conn`). Hai tham số `client_header_timeout` và `client_body_timeout` kết thúc yêu cầu bằng lỗi 408 khi tiêu đề không gửi đủ đúng hạn hoặc thân yêu cầu ngừng nhận dữ liệu. Nhờ đó cửa sổ tấn công của Slowloris và Slow POST thu hẹp lại.
- **Reverse proxy:** Đặt trước máy chủ ứng dụng và đệm yêu cầu của client trước khi chuyển vào trong. Cloudflare dùng cách này để đối phó Slowloris.
- **Fail2ban:** Đọc log và cập nhật luật firewall để chặn tạm thời các IP đăng nhập thất bại nhiều lần hoặc có dấu hiệu độc hại.

### 1.3.3 Hệ thống phát hiện/ngăn chặn xâm nhập (IDS/IPS, WAF)

- **IDS và IPS:** IDS giám sát và cảnh báo, còn IPS có thể chủ động ngăn chặn sự cố. Hai phương pháp phát hiện chính là theo chữ ký và theo bất thường. Snort và Suricata là hai công cụ mã nguồn mở phổ biến; Suricata chạy ở chế độ IDS (nghe thụ động) hoặc IPS (inline). Phương pháp chữ ký khó phát hiện tấn công gồm nhiều sự kiện riêng lẻ nếu không sự kiện nào mang dấu hiệu rõ ràng.
- **WAF (Web Application Firewall):** Lọc yêu cầu và phản hồi HTTP/HTTPS. ModSecurity là engine WAF mã nguồn mở, thường dùng với bộ luật OWASP CRS. Khi lưu lượng làm đầy bảng trạng thái của firewall, biện pháp ở máy chủ không đủ vì nghẽn xảy ra phía trước thiết bị đó.

**Bảng so sánh nhanh các giải pháp phòng chống:**

| Giải pháp | Phù hợp nhất với | Hạn chế chính |
|---|---|---|
| Firewall/ACL | Lọc theo IP, cổng, giao thức | Khó khi nguồn phân tán |
| Chống giả mạo IP (BCP 38, uRPF) | Tấn công phản xạ/khuếch đại | Do nhà cung cấp mạng triển khai, không phải nạn nhân |
| Blackholing (RTBH) | Lưu lượng vượt ngưỡng chịu đựng | Địa chỉ bị tấn công không còn truy cập được |
| BGP Anycast/Scrubbing | Tấn công phân tán quy mô lớn | Phạm vi bảo vệ tùy nhà cung cấp |
| SYN cookies | SYN Flood | Là cơ chế dự phòng, có thể làm giảm chất lượng một số dịch vụ |
| Rate limiting/Reverse proxy | Tấn công tầng 7 | Yêu cầu vượt ngưỡng bị trì hoãn hoặc từ chối |
| IDS/IPS | Dấu hiệu đã biết và bất thường | Chữ ký khó bắt tấn công gồm nhiều sự kiện riêng lẻ |
| WAF | HTTP/HTTPS tầng 7 | Không thay được năng lực hấp thụ lưu lượng lớn ở tầng mạng |

## 1.4 Kết chương

Chương 1 trình bày cơ sở lý thuyết về DoS/DDoS: định nghĩa, vai trò của botnet và hậu quả đối với tổ chức. Các dạng tấn công được chia theo mô hình OSI thành tấn công băng thông/giao thức ở tầng 3 và 4, tấn công tầng ứng dụng, và tấn công khuếch đại/phản xạ. Mỗi nhóm nhắm vào một loại tài nguyên nên cần biện pháp riêng.

Về phòng chống, báo cáo trình bày phòng thủ nhiều lớp ở ba mức: hạ tầng mạng, máy chủ dịch vụ, và hệ thống phát hiện/ngăn chặn. Chương 2 sẽ chọn một công cụ tấn công cụ thể để phân tích và xây dựng giải pháp phòng chống tương ứng.