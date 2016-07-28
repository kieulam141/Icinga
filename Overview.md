# Tổng quan về Icinga
- Icinga là một hệ thống mã nguồn mở theo dõi các máy chủ và dịch vụ được chỉ định và thông báo cho người quản trị khi có sự cố xảy ra và khi sự cố được khắc phục.
- Icinga được xây dựng dựa trên mã nguồn mở được phát triển từ hệ thống giám sát Nagios, vì vậy nó tương thích hoàn toàn với các phần mềm hỗ trợ của Nagios.
- Đồng thời, phần mềm cũng cung cấp rất nhiều tính năng tùy biến mới, trong đó phải kể đến như giao diện người dùng Web 2.0, hỗ trợ các hệ quản trị cơ sở dữ liệu phổ biến như MySQL, Oracle và PorgreSQL.
- Icinga có thể chạy trên nhiều hệ điều hành nhân Linux: Redhat, Fedora, Debian, openSUSE cũng như các Unix platforms khác (bao gồm Solaris và HP).

## 1.Lịch sử hình thành và phát triển
- Tháng 5 năm 2009, ra mắt phiên bản đầu tiên của Icinga, cung cấp phần nhân, hàm API và giao diện web. Phiên bản đầu tiên đạt mốc 10.000 lượt tải về trong năm đó.
- Năm 2010, ra mắt phiên bản mới, hỗ trợ chuẩn IpV4 và IpV6, cung cấp các tùybiến để truy cập cơ sở dữ liệu, cải thiện giao diện người dùng và một số phần mềm hỗ trợ khác. Phiên bản mới đạt mốc 70.000 lượt tải về.
- Năm 2012, ra mắt phiên bản thử nghiệm Icinga 2, khắc phục những hạn chế về cấu hình và khả năng mở rộng của Icinga khi triển khai hệ thống lớn.
- Tháng 6 năm 2014, ra mắt phiên bản chính thức của Icinga 2 với rất nhiều các tính năng mới và giao diện web theo thiết kế phẳng và tương thích với thiết bị di động.

## 2.Ưu điểm
- Phần mềm được cung cấp miễn phí, hỗ trợ nhiều tùy chọn giao diện quản trị Web.
- Phần mềm cài đặt dễ dàng, hỗ trợ tốt hệ điều hành Linux.
- Giao diện Web thân thiện, dễ sử dụng cho người dùng lần đầu.
- Tương thích với các phần mềm hỗ trợ của Nagios.

## 3.Nhược điểm
- Phần mềm không cung cấp nhiều tùy chọn hiển thị thông tin giám sát bằng đồ thị.

## 4.Các chức năng của Icinga
- Giám sát các dịch vụ mạng (SMTP, POP3, HTTP, NNTP, PING ...).
- Giám sát tài nguyên của máy chủ (Tốc độ xử lý của CPU, khả năng sử dụng của ổ đĩa…).
- Giám sát cảc thành phần của hệ thống mạng (Thiết bị chuyển mạch, thiết bị định tuyến, nhiệt độ, độ ẩm…)
<img src="http://image.prntscr.com/image/a2239fd4d5304631b070e3f866510031.png" />

- Các phần mềm hỗ trợ được thiết kế đơn giản và cho phép người dùng có thể dễ dàng phát triển các dịch vụ để kiểm tra hệ thống.
- Cung cấp các hàm API để người quản trị có thể dễ dàng tùy biến phát triển mà không cần tác động nhiều đến phần nhân của Icinga
- Khả năng kiểm tra, giám sát nhiều dịch vụ cùng một lúc. 
- Khả năng định nghĩa các máy chủ thành một mạng máy tính, phát hiện được máy chủ đang gặp sự cố hay không thể truy cập. 
- Thông báo đến danh sách quản trị viên khi máy chủ hay dịch vụ gặp sự cố thông qua nhiều kênh thông tin như (Thư điện tử, tin nhắn điện thoại…) 
<img src="http://image.prntscr.com/image/555d01b8c05145d2b32bc717c453c1dd.png" />

- Tự động lưu trữ thông tin vào tệp nhật ký (File log) và tích hợp được với các phần mềm ghi log như logstash, graylog ...
<img src="http://image.prntscr.com/image/a2e15a95626a4b8e985eabb35627cb6a.png" />

- Cung cấp tùy chọn Giao diện web cổ điển cho phép hiển thị các thông tin như tình trạng của mạng, các thông báo, danh sách lịch sử các sự cố, tệp nhật ký… 
- Cung cấp tùy chọn Giao diện web mới sử dụng giao diện hiện đại của Web 2.0 để hiện thị trạng thái, thông tin lịch sử, sử dụng các bộ lọc thông tin mới, và hỗ trợ tạo các báo cáo, hỗ trợ đa ngôn ngữ. 
- Chức năng báo cáo của Icinga được xây dựng dựa trên phần mềm Jasper Reports hỗ trợ cả hai giao diện Icinga web cổ điển và Giao diện Icinga web mới.

## 5.Kiến trúc Icinga
<img src="http://i.imgur.com/whsMvep.png" />

- Icinga được viết trong C và có kiến trúc kiểu module với 1 core riêng biệt, giao diện người dùng và CSDL để người dùng có thể tích hợp các add-on và plug-in

## Icinga Core
### Icinga 1.x
- Icinga1.x  dựa trên Nagios.
- Quản lý các giám sát tác vu, nhận các kiểm tra kết quả từ các plug-in.
- Sau đó kết nối những kết quả này tới IDODB (icinga Data Out Database) qua giao diện IDOMOD (Icinga Data Out Module) và daemon IDO2DB (Icinga Data Out to Database) over SSL mã hóa TCP sockets.
- Mặc dù 2 thằng trên được đóng gói thành IDOUtils ở trong Core, nhưng nó vẫn là 2 thành phiên riêng biệt, có thể được tách ra để phân phối dữ liệu và làm việc với nhiều server trong trường hợp Giám sát các hệ thống phân phối.
- Giao diện người dùng cũ của Icinga cũng được đóng gói trong Core.

### Icinga2
- Icinga2 có thêm các tính năng mới.
- Quản lý các giám sát tác vụ, running checks, gửi thông báo về cảnh báo.
- Có các feature mặc định như "checker", "notification".Các feature có thể được enable theo yêu cầu.
- Tích hợp được với các phần mềm ghi log như logstash, graylog ...
- HỖ trợ hầu hết các distribution chính.
- CLI mạnh mẽ.
- Bao gồm các thư viện mẫu sâu rộng.
- Giao diện bên ngoài tương thích với Icinga1.x và chính giao diện người dùng của nó.
- Được xây dựng trên cluster stack bảo mật bởi chứng chỉ SSL x509 giúp cho việc cài đặt các giám sát phân phối dễ hơn.
- Cú pháp cấu hình cũng # so với Icinga1.x và Nagios/

## Icinga's User Interfaces
### Icinga Classic UI (Classic Web)
- Dựa trên Nagios và giữ lại format của nó.
- Thêm các tính năng mới như đánh số trang, ouput JSON và export CSV.
- Được tích hợp sẵn trong Core (Icinga1.x).
- Nhận dữ liệu từ cache và gửi câu lệnh qua pipe tới các file lệnh.

### Icinga Web (New Web)
- Dựa trên Agavi và PHP.
- Sử dụng Cronks (widget) => kéo thả để tùy chỉnh dashboard.
- Khác với thằng Classic, thằng này đứng riêng biệt.
- Nó kết nối tới core, CSDL và các add-on bên thành phần thứ 3.
- Nó có 3 lớp: lớp trừu tượng Doctrine (đầu vào/CSDL), lớp REST API (chạy các script bên ngoài) và lớp Command Control (thực thi các câu lệnh).
- Cả 2 thằng đều hiện thị thông tin trên host và service về: trạng thái, lịch sử, thông báo, biểu đồ trạng thái của hệ thống mạng trong thời gian thực.Đều hỗ trợ IPv4 + IPv6.

## Icinga Data Out Database
- Nơi lưu trữ dữ liệu về lịch sử giám sát để các add-on hoặc giao diện Web truy cập vào.
- Khác với Nagios, thằng này hỗ trợ thêm CSDL PostgreSQL và Oracle.




