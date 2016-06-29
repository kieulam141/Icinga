# Tổng quan Icinga
## Icinga là gì?
- Icinga là 1 hệ thống giám sát check hosts và các services mà bạn chỉ định hoặc thông báo cho bạn khi có lỗi gì đó và khi chúng được phục hồi.
- Nó có thể chạy trên nhiều Linux distributions (bao gồm Fedora, Ubuntu, openSUSE) cũng như các Unix platforms khác (bao gồm Solaris và HP).
- Hệ thống có thể giám sát bất kì cái gì có kết nối mạng.
- Các đặc trưng của Icinga gồm:
<ul>
	<li>Giám sát các dịch vụ mạng (SMTP, POP3, HTTP, NNTP, PING ...)</li>
	<li>Giám sát tài nguyên của host (CPU load, disk usage ...)</li>
	<li>Thiết kế các plugin đơn giản cho phép người dùng dễ dàng phát triền cách check services theo ý của họ.</li>
	<li>Check các service 1 cách song song.</li>
	<li>Có thể định nghĩa hệ thống mạng các host bằng cách sử dụng các host "parent", cho phép bảo vệ và phân biệt giữa các host khi nó bị down và các host đó sẽ không bị động đến.</li>
	<li>Gửi thông báo khi service hoặc host gặp vấn đề hoặc được giải quyết vấn đề (qua email,sms hoặc tùy người dùng)</li>
	<li>Có thể định nghĩa cách xử lý sự kiện trong khi service đang chạy để có thể giải quyết vấn đề chủ động.</li>
	<li>Tự động quay vòng file log.</li>
	<li>Hỗ trợ cho việc thực hiện giám sát các host dư thừa.</li>
	<li>Giao diện web cổ điển để xem trạng thái mạng,lịch sử thông báo và vấn đề, file log...</li>
	<li>Giao diện web mới dựa trên Icinga Core, IDOUtils, API sử dụng GUI Web 2.0 mới mẻ hơn có thể show trạng thái hiện tại, thông tin lịch sử, sử dụng cronks và bộ lọc, tạo thông báo với hỗ trọ đa ngôn ngữ.</li>
</ul>

### Yêu cầu hệ thống
- Bạn cí thể muốn có cấu hình TCP/IP để có thể checks nếu muốn truy cập qua network.
- Bạn không bắt buộc phải sử dụng web interfaces, nếu muốn dùng thì bạn phải có các phần mềm kèm theo
<ul>
	<li>Web:httpd</li>
	<li>DB:mysql</li>
	<li>Scripting Language(for nagios-plugins):perl</li>
	<li>Scripting Language(for icinga-web):php</li>
	<li>icinga core:icinga</li>
	<li>plugin:nagios-plugins</li>
	<li>icinga frontend:icinga-web</li>
	<li>icinga report:icinga-reports</li>
	<li>reporting engine(for icinga-reports):jasperreports-server-cp</li>
	<li>graph(nagios addon):pnp4nagios-0.6.16</li>
</ul>
