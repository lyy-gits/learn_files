# TCPdump

## **抓包**

默认只抓68个字节

```shell
# tcpdump -i 接口/网卡 -s 抓取数据包的大小（0代表数据包有多大就抓多大） -w 保存到文件
tcpdump -i eth0 -s 0 -w 1.cap
```

![image-20210712152911285](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210712152911285.png)

```shell
# 抓取某个端口上的数据包、也可以指定其他的协议：ICMP、udp..
tcpdump -i eth0 port 22
# 连接目的服务器的22端口
nc -nv 10.0.0.47 22
```

![image-20210712155324843](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210712155324843.png)

![image-20210712155338986](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210712155338986.png)

## **读取抓包文件**

```shell
# tcpdump -r 抓包文件
tcpdump -r file.pcap
# 读取详细信息
tcpdump -A -r file.cap   # 使用ASCII码的形式表现
tcpdump -X -r file.cap	 # 使用十六进制形式表现
```

![image-20210712153605593](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210712153605593.png)

## TCPdump—筛选

```shell
# -n 不对数据包里的ip地址做名称的解析，不会把ip地址解析成相应的域名  sort（排序） -u 剔除重复项
tcpdump -n -r http.cap | awk '{print $3}' | sort -u
# src 源地址 
tcpdump -n src host 10.0.0.47 -r http.cap
#目标地址
tcpdump -n dst host 10.0.0.47 -r http.cap
#对端口进行筛选
tcpdump -n [udp] port 36000 -r http.cap
#十六进制
tcpdump -nX port 36000 -r http.cap
# 显示第13号字节换算成十进制为24的数据包，ACK和PSH为1 其余都为0
tcpdump -A -n 'tcp[13]=24' -r http.cap
```

![image-20210712165800270](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210712165800270.png)

![image-20210712170302900](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210712170302900.png)

# iptables

## iptables概念

netfilter/iptables：IP信息包过滤系统被称为单个实体，但它实际上由两个组件netfilter和iptables组成

> netfilter/iptables关系：

​	netfilter组件也称为内核空间（kernelspace），是内核的一部分，由一些信息包过滤表组成，这些表包含内核用来控制信息包过滤处理的规则集。

​	iptables组件是一种工具，也称为用户空间（userspace），它使插入，修改和除去信息包过滤表中的规则变得容易

​	netfilter/iptables后期简称为：iptables。iptables是基于内核的防火墙，功能非常强大，iptables内置了filter，nat和mangle、raw四张表。所有规则配置后，立即生效。不需要重启服务

==四张表==

+ filter  负责过滤数据包，防火墙；内核模块：iptable_filter  包括的规则链有：input、output和forward；
+ nat  则涉及到网络地址转换；内核模块：iptable_nat 包括的规则链有：prerouting，postrouting和output；
+ mangle  表则主要应用在修改数据包内容上，用来做流量整形的，给数据包打个标识；iptable_mangle 默认的规则链有：INPUT，OUTPUT，NAT，POSTROUTING，PREROUTING
+ raw  关闭nat表上启用的连接追踪机制，用于处理异常，iptable_raw  

==五个链==

+ input 匹配目标IP是本机的数据包
+ output  出口数据包，一般不在次链上做配置
+ forward  匹配流经本机的数据包
+ prerouting  用来修改目的地址，用来做DNAT。如：把内网中的80端口映射到路由器外网端口上
+ postrouting  用来修改源地址用来做SNAT。如：内网通过路由器NAT转换功能实现内网pc机通过一个公网IP地址上网

![image-20210713105111251](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210713105111251.png)

`表->链->规则`

## Iptables过滤封包流程

红色代表五个链

![image-20210713111527315](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210713111527315.png)



总结：==整体数据包分两类：1、发给防火墙本身的数据包；2、需要经过防火墙的软件包，到达内网的==

+ 发给防火墙本身（机器本身）的数据包不经过FORWARD
+ 发给内网的数据包是经过FORWARD 的

![image-20210713112508041](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210713112508041.png)

**表间的优先顺序**

raw——mangle——nat——filter

**链间的匹配顺序**

入站数据：PREROUTING、INPUT

出站数据：OUTPUT、POSTROUTING

转发数据：PREROUTING、FORWARD、POSTROUTING

**链内的匹配顺序**

自上向下按顺序依次进行检查，找到相匹配的规则即停止（LOG选项表示记录相关日志）

若在该链内找不到想匹配的规则，则按该链的默认策略处理（未修改的状况下，默认策略为允许）

## iptables实践

```shell
# 查看iptables是否安装
rpm -qf `which iptables`
# 安装
rpm -ivh iptables-1.4.21-16.tl2.1.x86_64
# 查看iptables的配置文件
ls /etc/sysconfig/iptables
# 启动iptables,默认开机自启动
/etc/init.d/iptables start
# 检查确认开机是否启动
chkconfig --list iptables / systemctl status iptables
```

![image-20210713162358356](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210713162358356.png)

```shell
# iptables命令的语法格式
iptables [-t 表名] 管理选项 [链名][条件匹配] [-j 目标动作或跳转]
#注意事项
不指定表名时，默认表示filter表
不指定链名时，默认表示该表内所有链
除非设置规则链的缺省策略，否则需要制定匹配条件
```

iptables语法总结

![image-20210713162833077](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210713162833077.png)

```shell
-p	协议
-s	源ip地址
-d	目标地址
-i
-o
--sport	源地址端口	
--dport	目标端口


```

### iptables命令使用方法

```shell
#iptables [-t 要操作的表]
#		<操作命令>  （-A、-I、-D、-P、-F）
#		[要操作的链]
#		[规则号码]
#		[匹配条件]
#		[-j 匹配到以后的动作]

# 查看命令(-[vnx]L)

# 保存iptables规则
/etc/init.d/iptables save
```

#### 选项

**-A <链名>	APPEND,追加一条规则（放到最后）**

```shell
# 在filter表的INPUT链里追加一条规则（作为最后一条规则） 匹配所有访问本机IP的数据包，匹配到的丢弃
iptables -t filter -A INPUT -j DROP		# 拒绝所有人访问服务器
```

**-D <链名> <规则号码 | 具体规则内容>  DELETE，删除一条规则**

```shell
-D <链名> <规则号码 | 具体规则内容>  DELETE，删除一条规则
iptables -L # 查看规则
# 删除INPUT里的第一条规则
iptables -D INPUT 1（按号码匹配）
# 注意：
	1.若规则列表中有多条相同的规则时，按内容匹配只删除序号最小的一条
	2.按号码匹配删除时，确保规则号码 <= 已有规则数，否则报错
	3.按内容匹配删除时，确保规则存在，否则报错
# 删除filter表INPUT链中内容为“192.168.0.1”的规则
iptables -D INPUT -s 192.168.0.1 -j DROP（按内容匹配）
```

**-P <链名><动作>    POLICY，设置某个链的默认规则**

```shell
iptables -P INPUT DROP

Chain INPUT (policy DROP)  # 修改为拒绝
target     prot opt source               destination
```

**-F [链名]     FLUSH，清空规则**

```shell
iptables -F INPUT	# 清空INPUT链上的规则
iptables -F			# 清空filter表中所有规则
iptables -t nat -F PREROUTING	# 清除nat表PREROUTING链上的规则

# 注意
	1.-F仅仅是清空链中规则，并不影响-P设置的默认规则
	2.-P设置了DROP后，使用-F一定要小心
	配置crontab
	*/15 * * * * iptables -P INPUT ACCEPT
	*/15 * * * * iptables -F
	3.如果不写链名，默认清空某表里所有链里的所有规则
```

**-L [链名]	LIST，列出规则**

```shell
v：显示详细信息，包括每条规则的匹配包数量和匹配字节数
x：在v的基础上，禁止自动单位换算（K、M）
n：只显示IP地址和端口号码，不显示域名和服务名称

iptables -t nat -vnL   # 其他的选项必须要在L前

# 例如：
	iptables -L	# 粗略列出filter表所有链及所有规则
	iptables -t nat -vnL	# 用详细方式列出nat表所有链的所有规则，只显示IP地址和端口号
	iptables -t nat -vxnL PREROUTING	# 用详细的方式列出nat表PREROUTING链的所有规则以及详细数字，不反解
```

#### **匹配条件**

```shell
流入、流出接口(-i、-o)
来源、目的地址(-s、-d)
协议类型	(-p)
来源、目的端口(--sport、--dport)
```

==按网络接口匹配==

```shell
-i <匹配数据进入的网络接口>	# 此参数主要用用于nat表，例如目标地址转换
-i eth0		# 匹配是否从网络接口eth0进来
-i ppp0		# 匹配是否从网络接口ppp0进来

-o 匹配数据流出的网络接口
-o eth0
-o ppp0

iptables -t nat -o eth0 条件 动作
```

==按来源目的地址匹配==

```shell
# -s <匹配来源地址>
# 	可以是IP、NET、DOMAIN ，也可空（任何地址）
例如：
-s 192.168.0.1 		匹配来自192.168.0.1的数据包
-s 192.168.1.0/24	匹配来自192.168.1.0/24网络的数据包
-s 192.168.1.0/16	匹配来自192.168.1.0/16网络的数据包

# -d <匹配目的地址>
#	可以是IP、NET、DOMAIN，也可以空
例如：
	-d 192.168.0.1	匹配去往192.168.0.1的数据包
	-d 192.168.0.0/16	匹配去往192.168.0.0/16网络的数据包
	-d www.abc.com	匹配去往域名www.abc.com的数据包
```

==按协议类型匹配==

```shell
-p <匹配协议类型>
	可以是TCP、UDP、ICMP等，也可为空
例如：
	-p tcp
	-p udp
	-p icmp --icmp-type 类型
	ping:type 8		pong:type 0
```

==按来源目的端口匹配==

```shell
--sport <匹配源端口>
	可以是个别端口，可以是端口范围
例如：
	--sport 1000		匹配源端口是1000的数据包
	--sport 1000:3000	匹配源端口是1000-3000的数据包（包含1000、3000）
	--sport:3000		匹配源端口是3000以下的数据包（含3000）
	--sport 1000:		匹配源端口是1000以上的数据包（含1000）
	
--dport <匹配目的端口>
	可以是个别端口，也可以是端口范围
例如：
	--sport 80			匹配目的端口是80的数据包
	--sport 1000:3000	匹配目的端口是1000-3000的数据包（包含1000、3000）
	--sport:3000		匹配目的端口是3000以下的数据包（含3000）
	--sport 1000:		匹配目的端口是1000以上的数据包（含1000）

# 注意：--sport和--dport必须配合 -p 参数使用

# 匹配应用举例
# 1、端口匹配
-p udp --dport 53
匹配网络中目的端口是53的UDP协议数据包

# 2、地址匹配
-s 10.1.0.0/24 -d 172.17.0.0/16
匹配来自10.1.0.0/24去往172.17.0.0/16的所有数据包

# 3、端口和地址联合匹配
-s 192.168.0.1 -d www.abc.com -p tcp --dport 80
匹配来自192.168.0.1，去往www.abc.com 的80端口的TCP协议数据包
```

![image-20210716153733918](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210716153733918.png)

```shell
# 注意：
	--sport、--dport必须联合-p使用，必须指明协议类型是什么
	条件写的越多，匹配越细致，匹配范围越小
```

动作（处理方式）

+ ACCEPT			放行

  ```shell
  -j ACCEPT
  	通过，允许数据包通过本链而不拦截它
  
  # 例如：允许所有访问本机IP的数据包通过
  	iptables -A INPUT -j ACCEPT
  ```

+ DROP               丢弃，不给对端任何回应

  ```shell
  -j DROP
  	丢弃，阻止数据包通过本链而丢弃它
  	
  # 阻止来源地址为192.168.80.39的数据包通过本机
  	iptables -A FORWARD -s 192.168.80.39 -j DROP 
  ```

+ REJECT             拒绝，拒绝后给对端回应

+ SNAT                源地址转换，解决内网用户用同一个公网地址上网的问题

  ```shell
  -j SNAT --to IP[-IP][:端口-端口](nat表的PREROUTING链)
  源地址转换，SNAT支持转换为单IP，也支持转换到IP地址池（一组连续的IP地址）
  
  # 将内网192.168.0.0/24的原地址修改为公网IP地址：1.1.1.1，
  iptables -t nat -A PREROUTING -s 192.168.0.0/24 -j SNAT --to 1.1.1.1
  # 修改成一个地址池里的IP
  iptables -t nat -A PREROUTING -s 192.168.0.0/24 -j SNAT --to 1.1.1.1-1.1.1.10
  # 扩展：一个IP地址做SNAT转换：可以让内网多少台PC实现上网？100台  65535  1-1024端口
  ```

+ DNAT                目标地址转换

  ```shell
  -j DNAT --to IP[-IP][:端口-端口](nat表的PREROUTING链)
  目的地址转换，DNAT支持转换为单IP，也支持转换到IP地址池（一组连续的IP地址）
  
  # 把从eth0进来的要访问TCP/80的数据包目的地址改为192.168.0.1
  iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j DNAT --to 192.168.0.1
  iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 81 -j DNAT --to 192.168.0.1:81
  # 把从eth0进来的要访问TCP/80的数据包目的地址改为192.168.0.1-192.168.0.10
  iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 81 -j DNAT --to 192.168.0.1-192.168.0.10
  ```

+ MASQUERADE   伪装一个公网ip地址

  ```shell
  -j MASQUERADE
  动态源地址转换（动态IP的情况下使用）比如ADSL
  
  # 将源地址是192.168.0.0/24的数据包进行地址伪装，转换成eth0上的IP地址。eth0为路由器外网出口IP地址
  iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE
  ```

+ LOG                  记录日志信息

+ REDIRECT         在本机做端口映射

==附加模块==

+ 按包状态匹配（state）

  ```shell
  -m state --state状态
  状态：NEW、RELATED、ESTABLISHED、INVALID
  	NEW：有别于tcp的syn
  	ESTABLISHED：连接态
  	RELATED：衍生态，与conntrack关联（FTP）
  	INVALID：不能被识别属于哪个连接或没有任何状态
  
  # 例如：允许已经建立的链接和TCP衍生出来的链接放行
  iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
  ```

+ 按来源MAC匹配（mac）

  ```shell
  -m mac --mac-source MAC
  匹配某个MAC地址
  例如：
  iptables -A FORWARD -m mac --mac-source xx:xx:xx:xx:xx:xx -j DROP 
  阻断来自某MAC地址的数据包，通过本机
  
  # 注意：
  报文经过路由后，数据包中源有的mac信息会被替换，所以在路由后的iptables中使用mac模块是没有意义的
  iptables  -A FORWARD -m mac --mac-source 52:54:00:c4:06:5e -j DROP
  ```

+ 按包速率匹配（limit）

  ```shell
  -m limit --limit 匹配速率 [--burst缓冲数量]
  	用一定速率去匹配数据包
  例如：
  	iptables -A FORWARD -d 192.168.0.1 -m limit --limit 50/s -j ACCEPT
  	iptbales -A FORWARD -d 192.168.0.1 -j DROP
  # 注意：
  limit英语上看是限制的意思，但实际上只是按一定速率去匹配而已，50/s表示1秒钟转发50个数据包，要想限制的话后面要再跟一条DROP
  ```

+ 多端口匹配（multiport）

```shell
-m multiport <--sports|--dports|--ports> 端口1[,端口2,...，端口n]
一次性匹配多个端口，可以区分源端口，目的端口或不指定端口

# 例如：允许所有客户端访问本服务器的21,22,25,80,110端口上的服务
iptables -A INPUT -p tcp -m multiport --dports 21,22,25,80,110 -j ACCEPT 

注意：
必须与 -p 参数一起使用
```

### 实战举例

#### 例1

使用iptables防火墙保护公司web服务器

分析：单服务器的防护
	1、弄清对外服务对象
	2、书写规则



网络接口lo的处理
协议+端口的处理
状态监测的处理



具体配置如下：

web服务器端：xuegod63

客户端：xuegod64

配置xuegod63防火墙



硬件防火墙拓扑图

![image-20210716173129209](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210716173129209.png)

```shell
# 配置web服务器xuegod63防火墙：

iptables -A INPUT -i lo -j ACCEPT	# 放行回环口所有数据
iptables -A INPUT -p tcp -m multiport --dports 22,80 -j ACCEPT
# 允许已经建立tcp连接的包以及该连接相关的包通过。状态防火墙能识别TCP或者UDP会话。非状态防火墙只能根据端口识别，不能识别会话
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -P INPUT DROP

# 注意：一般iptables，OUTPUT出口一般都放行，不需要在出口上做限制。这样允许服务器主动访问外网所有数据

测试：
在xuegod63上安装一个vsftpd服务器，看看是否会禁止用户访问
yum -y install vsftpd	# 安装ftp服务器并开启
/etc/init.d/vsftpd start
xuedog64安装：
yum -y install elinks -y	# elinks是一个纯文本格式的浏览器
elinks 192.168.1.63 # 测试web访问正常
rpm -ivh lftp-4.0.9-1.el6.x86_64.rpm
lftp 192.168.1.63	# 可以看到连接断开了
```

**查看配置结果`-n`的作用**

![image-20210719152705421](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719152705421.png)

![image-20210719152807098](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719152807098.png)

> 如何知道tcp的端口是22

会读取/etc/services配置文件

![image-20210719153133377](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719153133377.png)

#### 例2

使用iptables搭建路由器，通过SNAT功能，使用内网PC机，可以上网

==分析==

+ 弄清网络拓扑

`扩展：ip地址命名技巧：方法一，以两个设备名称定义相领的网段。适用于做多个网段的实验`

![image-20210719154333646](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719154333646.png)

`方法二“从左到右，依次命名。地址1留给核心设备如下：`

实验环境：

xuegod63路由器，xuegod64做客户端

![image-20210719155309494](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719155309494.png)==需求：使xuegod64可以通过xuegod63上网==

```shell
# 配置：xuegod63
	添加两个网卡，配置eth0为桥接，eth1为vmnet4模式（模拟模式）
	配置eth1的IP地址为：192.168.2.1/24
	重启网络：systemctl restart network
	启用内核路由转发功能：
	echo "1" > /proc/sys/net/ipv4/ip_forward
	或
	vim /etc/sysctl.conf
	net.ipv4.ip_forward = 1
	sysctl -p	# 使配置生效
# 配置SNAT
iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -j SNAT --to 192.168.1.63
```

```shell
# 配置客户端xuegod64
修改xuegod64 eth0网卡模式为vmnet4
配置eth0 IP，网关，DNS：
```

![image-20210719162304256](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719162304256.png)

```shell
测试：ping网关是否能ping通（在63上已经将192.168.2.0网段转发到192.168.1.63上）
能ping通代表64可以通过63上网
ping 192.168.1.1
```

#### 例3

拒绝访问服务器本身和拒绝通过服务器访问别的机器。（主要看是否明白iptables每个链的作用）

+ 过滤位置filer表FORWARD链
+ 匹配条件 -s   -d    -p    --s/dport
+ 处理动作ACCEPT DROP

```shell
实验环境：
配置好网络和对应的IP地址：
xuegod64：IP：192.168.2.2		网卡属于：Vmnet4
xuegod63：eth1：192.168.2.1		网卡属于：Vmnet4
xuegod64：eth0：192.168.1.63
网络拓扑图如下
```

![image-20210719155309494](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719155309494.png)

```shell
# 设置INPUT链为允许
iptables -P INPUT ACCEPT

# 测试
xuegod64上执行
ping 192.168.2.1
ping 192.168.1.1
都可以ping通

# 禁止xuegod64通过xuegod63访问192.168.2.1。即不允许访问服务器本身
xuegod63设置
iptables -A INPUT -s 192.168.2.2 -j DROP
测试：
xuegod64 ping xuegod63（192.168.2.1） ping不通

iptables -A FORWARD -s 192.168.2.2 -j DROP
测试：
还是可以ping通192.168.2.1
```

==注意：直接ping服务器本身，数据流是不经过FORWARD链的，所以规则要添加在INPUT链上==

![image-20210713111527315](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210713111527315.png)

```shell
# 禁止192.168.2.2这台机器通过服务器上网
xuegod63设置
iptables -A OUTPUT -s 192.168.2.1 -j DROP  # 数据包流出去是通过192.168.2.1出去的，所以这里禁止192.168.2.1通过
iptables -A FORWARD -s 192.168.2.2 -j DROP
```

iptables -A INPUT -s 192.168.2.2 -j DROP  

或
iptables -A OUTPUT -d 192.168.2.2 -j DROP  （直接在input链上拒绝即可，减少服务器资源开销）

==总结==

+ 添加规则，要添加在最靠近数据流源的链上，减少服务器不必要的资源开销

#### 例4

使用DNAT功能，把内网web服务器端口映射到路由器外网

![image-20210719155309494](C:\Users\v_lyyoli\Desktop\learn_files\typora-user-images\image-20210719155309494.png)

```shell
实验环境;
xuegod64:192.168.2.2
xuegod63:eth1:192.168.2.1
```

80端口映射

```shell
# xuegod64，开启httpd服务，首页内容为192.168.2.2
xuegod64：
yum -y install httpd -y
/etc/init.d/httpd start
echo "192.168.2.2" > /var/www/html/index.html

# 测试：192.168.1.1通过xuegod63访问xuegod64问不通，因为这里还没有配置端口映射
在192.168.1.1访问192.168.1.63

# xuegod63:任选两种都可
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j DNAT --to 192.168.2.2:80
iptables -t nat -A PREROUTING -d 192.168.1.63 -p tcp --dport 80 -j DNAT --to 192.168.2.2:80

# 测试：192.168.1.1访问192.168.1.63是可以正常访问的

```

## iptables命令使用总结

+ 所有链名必须大写
  INPUT/OUTPUT/FORWARD/PREROUTING/POSTROUTING
+ 所有表名必须小写
  filter/nat/mangle/raw
+ 所有动作必须大写
  ACCEPT/DROP/SNAT/DNAT/MASQUERADE
+ 所有匹配必须小写
  -s/-d/-m <module_name>/-p

