# Icinga2 Features
<img src="http://image.prntscr.com/image/f9a170537a8848cd831174eeaa83d1fd.png" >

- Các feature nằm trong đường dẫn `etc/icinga2/feature-available`.

## 1. Logging
- Support 3 kiểu log # nhau:File logging, Syslog  (OS Unix), Console logging, (STDOUT on tty).
|	Feature		|	Description |
| ------------  | ------------- |
|	debuglog	| 	Debug log (path: /var/log/icinga2/debug.log, severity: debug or higher) |	
|	mainlog		|	Main log (path: var/log/icinga2/mainlog, severity: information or higher) |
|	syslog		|	Syslog (severity: warning or higher) |
- Mặc định thằng mainlog được enable.khi severity "information" hoặc higher -> console.

## 2. DB IDO
- Xuất tất cả cấu hình và thông tin trạng thái vào cơ sở dữ liệu.
- hỗ trợ High Availability -> Icinga2 cluster.

## 3. External Commands
- Cung cấp các lệnh bên ngoài để xử lý các câu lệnh kích hoạt các tác vụ đặc biệt.
- Path mặc định: /var/run/icinga2/cmd/icinga2.cmd

## 4. Perfomance Data
- Khi kiểm tra 1 host hay service bằng việc thực thi các plugins thì được gọi là Perfomance Data.
- Nó có thể được lấy ra từ việc sử dụng icinga2 runtime macros như việc kiểm tra độ trễ hoặc trạng thái service hiện tại.
- Nó có thể được chuyển qua các ứng dụng bên ngoài để có thể tập hợp và lưu trữ tại backend.Những tool này tạo ra biểu đồ cho việc báo cáo lịch sử và xu hướng.
- Các addon : PNP4Nagios, Graphite or OpenTSDB.

### 4.1 Writing Perfomance Data File
- PNP4Nagios và Graphios dùng các perfdata collector deamon để lấy các file perfomance hiện tại -> backend update.
- Feature PerfdataWriter cho phép qui định format của template output cho các host + service
- Command:	`icinga2 feature enable perfdata`
- Path: /var/spool/icinga2/perfdata/
- 

### 4.2 Graphite Carbon Cache Writer
- Khi thằng graphios dành cho icinga 1.x thì để phát triển hơn, Graphite deamon tcp socket được tạo ra cho icinga2.
- Command:	`icinga2 feature enable graphite`
- Listen at 127.0.0.1 và port 2003

### 4.3 GELF Writer
- Gửi các log của ứng dụng trực tiếp tới TCP socket.
- Command:	`icinga2 feature enable gelf`
- Listen at 127.0.0.1 port 12201
- Các events sau được xử lý:Check results, State changes, Notifications.
	
### 4.4 OpenTSDB Writer
- 1 dạng collector scripts and deamons
- Thằng icinga1.x có tcollector thì TSDB tcp socket -> icinga2
- Command:	`icinga2 feature enable opentsdb`
- Listen at 127.0.0.1 and port 4242

## 5. Livestatus
- MK Livestatus thực hiện các phương thức truy vấn cho users truy vấn về thông tin trạng thái.Nó cũng có thể được dùng để gửi command.
- Chỉ enable thằng này khi có yêu cầu.
- Command:	`icinga2 feature enable livestatus`
- Sau đó restart icinga2.
- Livestatus socket mặc định: /var/run/icinga2/cmd/livestatus.
- Tiếp theo để queries và commands hoạt động. add user query (vd user web server) -> icingacmd group:
- Command:	`usermod -a -G icingacmd apache`
- Thằng Debian sẽ sử dụng "nagios" thay cho "icingacmd"
- Đổi "apache" thành user muốn nó chạy queries.
- Tiếp theo để sử dụng bảng lịch sử cung cấp bởi livestatus (vd bảng log) bạn cần enable feature "CompatLogger". Path mặc định: /var/log/icinga2/compat . Tùy chỉnh path = set đặc tính cấu hình "compat_log_path"
- Command:	`icinga2 feature enable compatlog`
	
### 5.1 Livestatus sockets
- Hỗ trợ 2 kiểu socket: Unix socket (mặc định), TCP socket.

### 5.2 Livestatus GET Queries

### 5.3 Livestatus Command Queries

### 5.4 Livestatus Filters
- Bộ lọc sử dụng các toán tử: And, or, negate.
| Operator | Negate | Description |
| -------  | ------ | ----------- |
| =	| != | Equality |
| ~ | !~ | Regex Match |
| < | -- | 	Less than |
| > | -- | 	Greater than |
| <= | -- | Less than or equal |
| >= | -- | Greater than or equal |


### 5.5 Livestatus Stats
- Schema: "Stats: aggregatefunction aggregateattribute"
- aggregatefunction: sum, min, max, avg (sum/count), std (standard deviation), suminv (sum (1/value)), avginv (suminv/count), count.

### 5.6 Livestatus Output
- CSV 
- JSON

### 5.7 Livestatus Error Codes
- 200: OK
- 404: Tables does not exist
- 452: query ngoại lệ.

### 5.8 Livestatus Tables

## 6. Status Data files
- Icinga 1.x  viết dữ liệu cấu hình object và trạng thái vào 1 khỏang time theo chu kì vào các file objects.cache và status.dat.
- Icinga2 có StatusDataWriter => cấu hình objects, update trạng thái trong 1 khoảng time đều đặn
- Command	`icinga2 feature enable statusdata`
	
## 7. Compat Log Files
- CompatLogger tạo ra các file log có format tương thích với icinga1.x
- Phân tích Log và show trên giao diện web bên ngoài.
- Tạo ra các báo cáo SLA (Service-Level Agreement) và xu hướng trong icinga 1.x Classic UI.
- Hơn nữa, Feature Livestatus dùng thằng này để trả lời truy vấn cho bảng lịch sử.
- Command	`icinga2 feature enable compatlog`
- Path: /var/log/icinga2/compat	

## 8. Checker
- Có trách nhiệm lập kế hoạch kiểm tra hoạt động

## 9. Notify
- 

	
