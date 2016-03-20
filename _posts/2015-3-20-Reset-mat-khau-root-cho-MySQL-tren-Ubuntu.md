---
layout: post
title: Reset mật khẩu root cho MySQL trên Ubuntu
---

Đôi khi cài đặt `MySQL` trên `Ubuntu` hoặc trên các HĐH khác, trình cài đặt sẽ tự động set một mật khẩu ngẫu nhiên cho tài khoản `root`, nếu không để ý thì sau khi cài đặt bạn sẽ không biết mật khẩu `root` để login vào mysql là gì hoặc một ngày đẹp trời bạn bỗng dưng quên mất mật khẩu `root` để login vào `mysql`. Vậy có cách nào để đặt lại mật khẩu cho tài khoản `root` hay không? Quá đơn giản, bạn chỉ cần làm theo các bước sau đây:

1.  Tạm dừng mysql service

	```
	$ sudo service mysql stop
	```

2. Khởi động lại mysql với `--skip-grant-tables`  để login vào `mysql` mà không cần mật khẩu

	```
	$ sudo mysqld_safe --skip-grant-tables &
	$ mysql -u root -p
	```

3. Chạy các lệnh sau trong mysql để reset lại mật khẩu cho tài khoản root

	```
	mysql> use mysql;
	mysql> update user set password=PASSWORD("Mật_khẩu_mới") where user='root';
	mysql> flush privileges;
	mysql> quit
	```
    	
4. Sau đó, khởi động lại `mysql` và bạn có thể login vào mysql với mật khẩu mới 

	```
	$ sudo service mysql stop
	$ sudo service mysql start
 	$ mysql -u root -p
 	```

Ngoài ra, bạn có thể xem lịch sử reset mật khẩu của `mysql` bằng cách xem file `/home/$USER/.mysql_history`của người đã thực hiện reset mật khẩu.


