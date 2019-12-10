# linux
## Linux-user: **linux中一切皆文件**
### 系统安装
 1. 系统centos: 自带有yum ：
 2. **安装软件命令**：
	1. wget: yum install wget
	2. vim：yum -y install vim-enhanced
	3. docker:  
 3. **查看系统内核版本**
	1. uname -a  
	2. cat /proc/version
 	
### 系统磁盘，CPU，内存
1. 查看磁盘 
	```
	df 查看磁盘使用情况
	df -h 查看所有磁盘信息
	du 查看文件大小
	du -sh [file] 查看某个文件的大小
	du -h 查看该目录下所有文件夹的大小
	du -h file 查看该文件的大小
	du -h --max-depth=1 /usr/local/mongodb  查看mongodb文件夹下一级目录文件大小
	du -h --max-depth=0 dev 查看当前目录下的dev文件大小

	du -sm * | sort -n 统计当前目录下的所有文件大小并排序
	du -sk * | sort -n

	du --help  查看命令帮助

	```
2. 查看cpu
	```
	top 
		```
		-p 进程号 产看某个进程的CPU，内存的使用情况
		-i n 表示时间间隔
		-u userNmame 表示某个用户的进程使用情况
		进入后：
		b ： 高亮显示当前线程高亮
		x : 使得排序列列高亮（默认cpu）
		shift > |<  切换排序的列
		c ：显示进程完整命令
		```
	uptime   命令的执行结果
		```
		1. 21:04:19: 当前系统时间；
		up 129 days, 20:31: 系统已经运行了129天20小时31分钟（这期间系统没有重启）；
		1 users: 当前有1个用户登录系统；
		load average: 58.32, 57.85, 57.50: load average 后面的三个数分别是1分钟、5分钟、15分钟的负载情况（**这个数除以逻辑 CPU 的数量，结果高于5的时候就表明系统在超负荷运转**）
		```

	sar命令,命令不存在时需要安装sysstat包，这个包很有用。
	sar -u 1 5  每1秒采集一次CPU使用率，共采集5次。
	sar -P 0 -u 1 5 查看某颗cpu的使用可以用-P参数 cpu==0
	1 进程队列长度和平均负载状态
	sar -q 1 5 
	runq-sz：运行队列的长度（等待运行的进程数）
	plist-sz：进程列表中进程（processes）和线程（threads）的数量
	ldavg-1：最后1分钟的系统平均负载（System load average）
	ldavg-5：过去5分钟的系统平均负载
	ldavg-15：过去15分钟的系统平均负载
	2 进程创建的平均值和上下文切换的次数
	sar -w 1 5 

	3 iostat 方便查看 CPU、网卡、tty设备、磁盘、CD-ROM 等等设备的活动情况，负载信息。
	iostat -c 1 2   查看io使用情况

	```
3. 查看内存
	```
	free 
	
	```
### Linux 命令
4. **启动程序命令**
	nohup bin/kafka-server-start.sh config/server2.properties >nohup2.out 2>&1 &
	nohup & : 表示后台执行进程
	>nohup2.out: 表示输出重定向到 nohup2.out
	2>&1: 表示错误重定向到标准输出 nohup2.out
	>/dev/null: 表示空文件，不输出任何信息
5. **history 命令**
	1. history  n : 表示查看最近n条命令
	2. !命令序号 : 表示执行该条命令
	3. history -c ： 表示清楚所有的命令
6. **ps命令“**
	1. 
7. **Linux 快捷键**
	1. Ctrl + c  退出程序
	2. Ctrl + a  光标放到输入头
	3. Ctrl + e  光标放到输入尾
	4. Ctrl + k	 删除从光标所在位置到行末
	5. Tab 单词补全
	6. 方向上键↑ 可以查看历史命令

8. **Linux 通配符**
	1. * 匹配 0 或多个字符
		1. touch test{1..10}.txt
		2. rm test*.txt
	2. ? 匹配任意一个字符
		1. rm test?.txt
	3. Linux 帮助手册
		1. man java<command>
		2. 

### **Linux 用户和权限管理**
1. who am i  | who -a | who -u |who -q
2. 用户
	1. root  
		1. 超级管理员 ; 可以操作任何的用户和文件
	2. 普通用户
		1. sudo 命令
			1. 必须知道自己的密码
			2. 必须在sudo用户组
			3. **添加sudo权限**(不需要密码)
				1. touch  	/etc/sudoers.d/taoge
					taoge ALL=(ALL)   NOPASSWD:ALL
					Defaults:taoge !requiretty
				2. 修改用户权限sudo usermod
					1. sudo usermod -G sudo taoge
					2. groups taoge
						taoge: taoge sudo

		2. su <user> 切换用户
			1. 需要知道<user>的密码
		3. su -l <user>
			1. 需要目标<user>密码
			2. 同时环境变量会转变为目标用户
	3. 添加用户和切换用户
		1. sudo adduser lilei
			1. 需要输入用户密码
			2. su -l lilei 
				1. 输入密码
		2. 	删除用户
			1. sudo deluser taoge --remove-home
				删除用户，并删除其home文件夹

	4. 用户组
		1. 查看用户组
			1. groups taoge 
				1. taoge : taoge  用户名：组名
			2. cat /etc/group | sort（ cat /etc/group | grep -E "taoge"）
				1.  cat /etc/group | grep -E "shiyanlou"
					如果用户主用户组，即用户的 GID 等于用户组的 GID，那么最后一个字段 user_list 就是空的，
		2. 修改用户用户组
			1. sudo usermod -G sudo taoge
3. **adduser和useradd的区别**
	1. useradd 只创建用户，创建完了用 passwd taoge 去设置新用户的密码; adduser 会创建用户，创建目录，创建密码

4. 用户权限命令			
	1. adduser  添加用户
	2. deluser  删除用户
	3. usermod  修改用户组等等
	4. 

### linux 文件
1. **Linux 文件权限**
	1. **查看文件权限及其拥有者**
		1. ls -l
		2. ls -A  可以查看隐藏文件
		3. ll file 可以指定文件的权限  == ls -l

	2. ![文件权限图](../images/file_permission.png)
		1. 	drwxr-xr-x :表示是文件，拥有者有rwx权限，拥有组有r-x权限，其他组r-x权限
	3. **一个目录同时具有读权限和执行权限才可以打开并查看内部文件，而一个目录要有写权限才允许在其中创建其它文件**
	4. linux 中  . ：表示当前目录； .. ：表示父级目录 ；.开头的文件为隐藏文件
	5. 修改文件
		1. 变更文件所有者
			1. sudo chown taoge file -> 将file的所有者转给taoge
		2. 修改文件权限
			1. chmod 600 file 更改文件只能是拥有者读写
			2. chmod go-rw file 收回group,other对文件的rw权限
			3. chmod go+rw file 授予group,other对文件的rw权限
			4. g、o 还有 u 分别表示 group、others 和 user，+，- 分别表示增加和去掉
			5. 
	6. 常用的文件权限
		1. 默认新建文件权限
			拥有者和组都是读写权限，other是读权限
		2. 600
			1. 拥有者有读写的权限，拥有者和其他没有权限
		3. 755 
			拥有者可以读写执行，拥有组可以读执行，other可执行

	100. 命令<command>
		1. chown  文件所有者变更
		2. chmod 文件权限变更 
2. 文件的追加和覆盖
	1.  \>> ： 追加
	2. \> : 覆盖
	
3. **Linux 环境变量设置**
	1. 变量
		1. 普通变量的声明和使用
			1. declare temp
			2. temp=shiyanlou
			3. echo $temp
		2. 环境变量
			1. 临时性修改为环境变量
				1. export temp (将temp普通变量定义为环境变量)
			2. 永久性环境变量
				1. 对当前用用户永久生效
					1. cd /home/用户名（taoge）
					2. ls -a  可以看到一个.profile文件
					3. 在.profile中添加的环境变量
						1. vim /home/username/.profile
						2. export key=value
						3. source /home/username/.profile
						4. echo $key

				2. 对所有用户永久生效
					1. 在 /etc/profile中添加的环境变量
						1. vim /etc/profile
						2. export key=value
						3. source /etc/profile  
						4. echo $key
		3. 删除普通变量
			1. unset key
			2. 
		4. 让环境变量生效
			1. export  本次登录
			2. 修改私有profile 或者 root profile（/etc/profile）
				1. source /etc/profile 使之生效

#### **命令的查找路径和顺序**
1. shell中的命令都是通过PATH 环境变量进行搜索的（windows中的环境变量）
2. 添加path
	PATH=$PATH:路径
			
3. **shell 种类**
	1. sh
	2. bash
	3. **zsh  推荐去使用**
		1. 兼容 bash
		2. 强大的历史纪录功能，输入 grep 然后用上下箭头可以翻阅你执行的所有 grep 命令。
		3. 智能拼写纠正，输入gtep mactalk * -R，系统会提示：zsh: correct ‘gtep’ to ‘grep’
		4. ......
	4. ......
4. shell
	1. **sh脚本**
		1. #!/bin/bash 开头
	
#### 文件搜索  
1. whereis 	who
	1. 快，单不全
	2. 直接从数据库中查询，但是只查询二进制文件（比如可执行文件）
2. locate
	1. 全面，快
	2. 数据库搜索所有满足的
3. which
	1. 只从PATH去搜索某个可执行文件（软件）
	2. which man
	3. **用于查看是否可以直接执行某个可执行文件**
4. find
	1. find 命令的路径是作为第一个参数的， 基本命令格式为 find [path] [option] [action]
	1. sudo find /etc/ -name interfaces  ：查询某个文件夹下文件名为interfaces的文件
	2. 与时间相关的参数
		1. -atime	最后访问时间
		2. -ctime	最后修改文件内容的时间
		3. -mtime	最后修改文件属性的时间
	3. 
#### 文件的压缩和解压
1. zip 命令
	1. $ cd /home/shiyanlou
	2. $ zip -r -q -o shiyanlou.zip /home/shiyanlou/Desktop
	3. $ du -h shiyanlou.zip
	4. $ file shiyanlou.zip
	5. zip -r -1 -q -o shiyanlou_1.zip /home/shiyanlou/Desktop -x ~/*.zip
		1.  -r 参数表示递归打包包含子目录的全部内容， **压缩目录**
		2. -q 参数表示为安静模式，即不向屏幕输出信息，
		3. -o，表示输出文件，需在其后紧跟打包输出文名。
		4. -[1-9] 表示压缩的体积大小
		5. -x 表示剔除某个文件 
	6. 创建加密压缩包
		1. zip -r -e -o shiyanlou_encrypjtion.zip /home/shiyanlou/Desktop
			1. -e ： 表示对压缩包进行加密
	7. 让文件压缩后在windows 环境可以正确换行
		1. zip -r -l -o shiyanlou.zip /home/shiyanlou/Desktop
			1. -l : 表示压缩后可以在windows上正确换行
2. unzip 解压
	1. unzip shiyanlou.zip
	2. 将文件解压到指定目录	
		1. unzip -q shiyanlou.zip -d ziptest
			1. -d : 表示指定解压的目录   **解压目录**
	3. 不进行解压，只是查看压缩包中的内容 **查看压缩包**
		1. unzip -l shiyanlou.zip
			1. -l : 表示查看压缩文件
	3. 解压编码问题
		1. 一般在windows上默认为GBK， Linux上默认是UTF-8
			1. unzip -O GBK 中文压缩文件.zip  **解压编码**
				1. -O 编码格式 ： 表示使用指定编码进行解码  
2. tar 命令
	1. 压缩
		1. tar -cf shiyanlou.tar /home/shiyanlou/Desktop
			1. -c : 表示创建压缩
			2. -f : 表示压缩后文件名
	2. 解压
		1. tar -xf shiyanlou.tar -C tartest
			1. -x : 表示解压
			2. -f : 表示解压的文件
			3. -C ： 表示解压后文件夹  **解压目录**
	3. 查看tar 压缩包  **查看压缩包**  
		1. tar -tf shiyanlou.zip
			1. -t ： 表示查看压缩包
	4. tar -czf shiyanlou.tar.gz /home/shiyanlou/Desktop
		1. -z : 表示使用gzip 压缩
		2. tar -xzf shiyanlou.tar.gz 解压
	5. **压缩文件类型**
	| 文件后缀名 | 说明|
	|--- |--- |
	| *.zip |	zip 程序打包压缩的文件|
	| *.rar |	rar 程序压缩的文件|
	| *.7z |	7zip 程序压缩的文件|
	| *.tar |	tar 程序打包，未压缩的文件|
	|*.gz |	gzip 程序（GNU zip）压缩的文件|
	|*.xz |	xz 程序压缩的文件|
	|*.bz2 |	bzip2 程序压缩的文件|
	|*.tar.gz |	tar 打包，gzip 程序压缩的文件|
	|*.tar.xz |	tar 打包，xz 程序压缩的文件|
	|*tar.bz2 |	tar 打包，bzip2 程序压缩的文件|
	|*.tar.7z |	tar 打包，7z 程序压缩的文件|

#### 磁盘大小，磁盘分区，格式化，挂载
1. 查找一个目录下最大的是个文件
	1. du -a | sort -n -r | head -n 10
	2.  du -hsx * | sort -rh | head -10 : 查找当前目录最大的是个文件
2. 查看文件系统磁盘的容量
	1. df -h 
3. 查看文件大小
	1. du -h -d 1 ~
		1. -h : 表示易看的 单位换算为M，G
		2. -d : 表示目录深度
		3. ~ ： 表示显示文件位置
	2. du -a | sort -n -r | head -n 10
		1. -a  表示目录下所有文件
		2. sort -n -r	
			1. -n :  依照数值的大小排序；
			2. -r :  以相反的顺序来排序；
		3. head : 用于控制显示的内容
			1. -n : 表示显示的行数
			2. -c : 表示显示的字符数
4. 磁盘的管理
	1. 创建虚拟磁盘
		1.  dd if=/dev/stdin of=test bs=10 count=1 ： 表示标准输入到test文件，大小10 byte,数据1个
	2. 格式化磁盘
		1. sudo mkfs.ext4 test :  使用ext4 文件系统格式化 test文件
	3. 对磁盘进行分区
		1. sudo fdisk -l ： 查看磁盘分区表信息
		2. sudo fdisk test : 对磁盘test 进入分区模式
			1. 

	
	4. 对磁盘或者分区进行挂载
		1. sudo mount ： 查看当前主机已经挂载的文件系统
		2. mount [-o [操作选项]] [-t 文件系统类型] [-w|--rw|--ro] [文件系统源] [挂载点]
			1. 以只读方式挂载 ： mount -o loop --ro test /mnt
			2. mount -o loop -t ext4 virtual.img /mnt  : 以ext4 文件类型挂载
		3. sudo umount /mnt : 对/mnt 挂载点进行卸载
### 帮助命令
1. 命令形式
	1. 内部命令
		1. shell 运行时加入了内存 ： 执行速度快
		2. 比如： history、cd、exit 
	2. 外部命令
		1. 执行的时候加入内存 
		2. vim ， ls
	3. 查看命令来源
		1. type ls/vim/.....
2. 帮助命令
	1. man
		1. man ls
		2. q : 表示退出产看信息
	2. help
		1. zsh ： 没有help
		2. bash : ls/vim/... --help
	3. info
		1. info ls : 外部插件 ： 需要安装  
		2. q : 表示退出查看信息


7. **服务器 防火墙命令**
	1. yum install firewalld  //安装firewalld 防火墙
	2. 开启服务: systemctl start firewalld.service
	3. 关闭防火墙: systemctl stop firewalld.service
	4. 开机自动启动: systemctl enable firewalld.service
	5. 关闭开机制动启动: systemctl disable firewalld.service
	6. 查看状态: systemctl status firewalld
	7. 服务网页需要开启防火墙组：


## 附加：
1. VPN 搭建
	1. v2ray:
		安装包：cd /usr/bin/v2ray
2. 练习题
	1. 目标
		1. 找到 sources.list 文件
		2. 把文件所有者改为自己（shiyanlou）
		3. 把权限修改为仅仅只有自己可读可写
	2. 思考与实现
		1. 查找该文件位置 ： find /etc/ -name sources.list
		2. 查看该文件的属主 ：ll sources.list
		3. 修改文件属主 ： sudo chown taoge  sources.list(需要进入目录)
		4. 修改文件权限 ：sudo chmod 600 sources.list 
		5. 如果不清楚命令使用 
			1. command --help
			2. man command

3. xshell 上传windows文件到linux
	1. yum  install lrzsz
		2. rz，即是接收文件（上传到Linux上）
		1. sz file 就是发文件到windows上

### linux有效命令
1. free
	1. 可用内存(free + buffers + cached)
	2. 实际使用内存(used – buffers – cached)
	3. 因为linux在管理内存的时候会先将写入的数据写入到内存中缓存,这些数据随时可以回收

2. 进程相关命令
	2.1 ps -ef | grep nginx 查看nginx相关的命令
	2.2 kill -9 [pid] : 杀进程  -9/15
