# Cài đặt snmp-plugins và cấu hình monitor SNMP.
## Mục lục
[1.Giới thiệu và chú thích] (#1)

[2.Cài đặt snmp-plugins] (#2)

[3.Cấu hình monitor SNMP] (#3)

<a name="1"></a>
### 1.Giới thiệu và chú thích
- Icinga2 Core
<ul>
	<li>OS: CentOS7</li>
	<li>Ip: 192.168.100.10</li>
</ul>
- Switch cần monitor
<ul>
	<li>Model: Cisco 3550</li>
	<li>Ip: 192.168.100.220</li>
	<li>Interface: FastEthernet0/1-24</li>
</ul>

<a name="2"></a>
### 2.Cài đặt snmp-plugins
- Cài đặt các gói cần thiết:
```sh
yum -y install net-snmp net-snmp-utils net-snmp-perl perl-CPAN
```
- Chạy perl để install Net::SNMP:
```sh
perl -MCPAN -e shell // enter mặc định
cpan> install Net::SNMP
```
- Tải nagios-snmp-plugins về:
```sh
wget http://nagios.manubulon.com/nagios-snmp-plugins.1.1.1.tgz
```
- Giải nén:
```sh
tar -xzvf nagios-snmp-plugins.1.1.1.tgz
```
- Vào thư mục đã giải nén ra chạy file : ./install.sh.
- Khi được hỏi install các file plugin ".pl" để chạy snmp, bạn có thể tùy chọn đường dẫn.Tôi sẽ để chung với các plugins trước đấy: /usr/lib64/nagios/plugins

<a name="3"></a>
### 3.Cấu hình monitor SNMP
- Trước khi cấu hình monitor SNMP, set các view table cho các đối tượng cần monitor ở trong file /etc/snmp/snmpd.conf.
- Link về các view table: http://nagios.manubulon.com/index_info.html
- Bắt đầu thêm host,service và command để monitor switch, có thể khai báo các hàm vào các file host.conf, service.conf, command.conf có sẵn ở đường dẫn /etc/icinga2/conf.d. Nhưng để dễ quản lí và fix lỗi, tôi sẽ tạo riêng các file config host.conf, service.conf, command.conf để dễ quản lý tại đường dẫn :/etc/icinga2/repository.d/hosts/Switch (Lưu ý đường dẫn tự tạo, ko có sẵn).
- Khai báo host tại file host.conf: có 2 cách
#### 3.1.Cách 1: (Khi bạn đã biết trước Switch có bao nhiêu cổng)
##### a.Cấu hình cho host Switch
```sh
object host Switch {
	import "generic-host"
	address = "192.168.100.220"
	// Các tham số sau phục vụ cho việc khai báo các interface được monitor.Bạn có thể thêm các interface khác tùy ý với cú pháp tương tự.
	var num = 1
	while (num < 25) {		// FastEthernet0/1-24
		vars.int["f0/" + String(num)] = {
			int = "FastEthernet0/" + String(num)
		}
		num += 1
	}
	vars.os = "Switch"	// add vào group host.
}
```

##### b.Cấu hình Service cho host Switch
###### Check-Load
- Bạn có thể vào đường dẫn /usr/lib64/nagios/plugins và chạy file *./check_snmp_load.pl -h* để xem các tham số cần có.
- Mẫu vd: http://nagios.manubulon.com/snmp_load.html
- Trước khi chạy plugins với icinga2, bạn có thể chạy plugins riêng với câu lệnh:
```sh
./check_snmp_load.pl -H 192.168.100.220 -C public -2 -T cisco -w 3,2,1 -c 3,3,2 -f
```

- Khai báo service vào trong file Service.conf
```sh
object Service "snmp-load" {
    host_name           = "Switch"
    // Set the type of load check to use.
    vars.snmp_load_type = "cisco"
    // Set the Load Average warning threshold.
    vars.snmp_warn      = "3,2,1" // 1min: 2*(Number of Cores)+1, 5min: (Number of Cores)+1, 15min: (Number of Cores)
    // Set the Load Average critical threshold.
    vars.snmp_crit      = "3,3,2" // 1min: 3*(Number of Cores), 5min: 2*(Number of Cores)+1, 15min: (Number of Cores)+1
    check_command       = "snmp-load"
	vars.community = "public"
}
```

###### Check-Memory
- Tương tự chạy file *./check_snmp_mem.pl -h* để xem các tham số cần có.
- Mẫu vd: http://nagios.manubulon.com/snmp_mem.html
- Tương tự check plugins riêng:
```sh
./check_snmp_mem.pl -H 192.168.100.220 -C public -2 -w 99 -c 100 -I -f
```

- Khai báo service vào tiếp trong file Service.conf
```sh
apply Service "switch-memory" {
	import "generic-service"
	// Set the Memory warning for Ram and swap Respectively.
	 // Uses percents.
	 check_command  = "check_memory"
	// Gọi đến address của switch.
	assign where host.vars.address
}
```

###### Check-Interface
- Tương tự chạy file *./check_snmp_int.pl -h* để xem các tham số cần có.
- Mẫu vd: http://nagios.manubulon.com/snmp_int.html
- Check plugins riêng:
```sh
./check_snmp_int.pl -H 192.168.100.220 -C public -n "Fast.*0.1[1234]" 
```

- Khai báo service vào tiếp trong file Service.conf
```sh
apply Service "Switch-interface: " for (int => config in host.vars.int){
	import "generic-service"
	check_command = "check_interface"
	vars += config
	assign where host.vars.int
}
```

##### c.Cấu hình Command cho host Switch
- Khai báo command vào trong file Command.conf

###### Command Memory
```sh
object CheckCommand "check_memory" {
	import "plugin-check-command"
	command = [ PluginDir + "/check_snmp_mem.pl", "-2", "-f", "-I"]
	arguments = {
		"-H" = "$address$"
		"-C" = "$community$"
		"-w" = "99"
		"-c" = "100"
	}
	vars.community = "public"
}
```

###### Command interface
```sh
object CheckCommand "check_interface" {
	import "plugin-check-command"
	command = [ PluginDir + "/check_snmp_int.pl", "-2", "-r", "-f", "-k", "-Y"] 

	arguments = {
	"-H" = "$address$"
	"-C" = "$community$"
	"-n" = "$int$"
	"-w" = "90,90"
	"-c" = "99,99"
	}
	vars.community = "public"
}
```

#### 3.2.Cách 2: (Khi bạn đã chưa biết Switch có bao nhiêu cổng)
- Có thể do tôi chưa tìm được cách discover port Switch nên tôi đã tự viết các script discover port Switch và script Auto add các port Switch vào file host.conf mà không cần nhập bằng tay như cách 1.
- Bạn có thể down về tại [đây] (https://github.com/kieulam141/Icinga/tree/master/Script)
- Bạn chạy file discoverSwitchPort trước để discover xem Switch đang có những port nào, sau đó chạy file AutoAddPortSNMP để add các tham số vào file host.conf.
- Add service và command giống như cách 1.
- Cuối cùng reload lại icinga2 và enjoy.
