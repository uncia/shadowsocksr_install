![ShadowsocksR](https://github.com/uncia/shadowsocksr_install/raw/master/shadowsocks.png)
# 自动安装  ShadowsocksR Server 无日志
# Auto install ShadowsocksR Server No Log

客户端下载
https://github.com/shadowsocksrr/shadowsocksr-csharp/releases

全局代理端
https://www.sockscap64.com/sstap/

shadowsocksR.sh
===============
* Description: Auto Install ShadowsocksR Server for CentOS/Redhat/Debian/Ubuntu
* Intro: https://shadowsocks.be/9.html & https://teddysun.com/489.html BBR
* Intro: https://teddysun.com/448.html & https://teddysun.com/444.html 
* Intro: https://github.com/fatedier/frp

Linux VPS/服务器一键检测硬件配置、节点下载和IO读写脚本

使用方法：
```
命令1:
wget -qO- bench.sh | bash
或者

curl -Lso- bench.sh | bash

命令2:

wget -qO- 86.re/bench.sh | bash
或者

curl -so- 86.re/bench.sh | bash
```
```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/unixbench.sh
chmod +x unixbench.sh
./unixbench.sh
```

```
wget http://soft.laozuo.org/scripts/bench.sh
sh bench.sh
```

###使用方法：


清理查找清理旧版本与添加开放端口
```
find / -name 'shadowsocks*'
iptables -I INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
iptables -I INPUT -m state --state NEW -m udp -p udp --dport 8080 -j ACCEPT
/etc/init.d/iptables save
/etc/init.d/iptables restart
```

使用root用户登录，运行以下命令：
```
yum install -y wget && 
wget -N --no-check-certificate https://raw.githubusercontent.com/uncia/shadowsocksr_install/master/shadowsocksR.sh && bash shadowsocksR.sh
rm -rf shadowsocksR.sh
```
或者
```
wget --no-check-certificate https://raw.githubusercontent.com/uncia/shadowsocksr_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```
再或者
```
wget --no-check-certificate https://raw.githubusercontent.com/uncia/shadowsocksr_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh
rm -rf shadowsocksR.sh
```
安装完成后，脚本提示如下：
```
Congratulations, ShadowsocksR install completed!
Server IP:your_server_ip
Server Port:your_server_port
Password:your_password
Local IP:127.0.0.1
Local Port:1080
Protocol:auth_sha1_v4_compatible
obfs:http_simple_compatible
Encryption Method:aes-256-cfb

Welcome to visit:https://shadowsocks.be/9.html
If you want to change protocol & obfs, reference URL:
https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup
Enjoy it!
```
卸载方法：

使用 root 用户登录，运行以下命令：
```
./shadowsocksR.sh uninstall
```
安装完成后即已后台启动 ShadowsocksR ，运行：
```
/etc/init.d/shadowsocks status
```
可以查看 ShadowsocksR 进程是否已经启动。

本脚本安装完成后，已将 ShadowsocksR 自动加入开机自启动。

使用命令：
```
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status

配置文件路径：/etc/shadowsocks.json
日志文件路径：/var/log/shadowsocks.log
代码安装目录：/usr/local/shadowsocks
```

多端口配置

如果要多个用户一起使用的话，请写入以下配置：
```
    "port_password":{
        "${shadowsocksport}":"${shadowsockspwd}",
        "25":"${shadowsockspwd}"
    },
```
```
{
    "server":"0.0.0.0",
    "server_ipv6": "[::]",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "80":"password1",
        "443":"password2"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "protocol": "auth_sha1_compatible",
    "protocol_param": "",
    "obfs": "http_simple_compatible",
    "obfs_param": "",
    "redirect": "",
    "dns_ipv6": false,
    "fast_open": false,
    "workers": 1
}
```
按照格式修改端口和密码：
```
    "port_password":{                  
        "80":"password1",       //端口和密码1
        "443":"password2"       //端口和密码2 
    },
```
如果要为每个端口配置不同的混淆协议，请写入以下配置：
```
{
    "server":"0.0.0.0",
    "server_ipv6":"::",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "8388":{"protocol":"auth_simple", "password":"abcde", "obfs":"http_simple", "obfs_param":""},
        "8389":{"protocol":"origin", "password":"abcde"}
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "protocol": "auth_sha1_compatible",
    "protocol_param": "",
    "obfs": "http_simple_compatible",
    "obfs_param": "",
    "redirect": "",
    "dns_ipv6": false,
    "fast_open": false,
    "workers": 1
}

"port_password":{
    "${shadowsocksport}":"${shadowsockspwd}",
    "25":{"protocol":"auth_aes128_sha1_compatible", "password":"3XWswUhRJL52InvP", "obfs":"tls1.2_ticket_auth_compatible", "obfs_param":""}
    },
```
注：客户端的protocol和obfs配置必须与服务端的一致，除非服务端配置为兼容插件。

redirect参数说明：

值为空字符串或一个列表，若为列表示例如

"redirect":["bing.com", "cloudflare.com:443"],

作用是在连接方的数据不正确的时候，把数据重定向到列表中的其中一个地址和端口（不写端口则视为80），以伪装为目标服务器。

dns_ipv6参数说明：

为true则指定服务器优先使用IPv6地址。仅当服务器能访问IPv6地址时可以用，否则会导致有IPv6地址的网站无法打开。

一般情况下，只需要修改以下五项即可：
```
"server_port":8388,        //端口
"password":"password",     //密码
"protocol":"origin",       //协议插件
"obfs":"http_simple",      //混淆插件
"method":"aes-256-cfb",    //加密方式
```
按格式修改端口、密码以及混淆协议。也可以和以前的格式混合使用，如果某个端口不配置混淆协议，则会使用下面的默认"obfs"配置。

如果你想修改配置文件，请参考：
https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup

参考链接：
https://github.com/breakwa11/shadowsocks-rss/wiki/ulimit

Ubuntu 优化

```
echo "* soft nofile 512000
* hard nofile 1024000" >> /etc/security/limits.conf
echo "ulimit -SHn 1024000" >> /etc/profile
echo "fs.file-max = 1024000" >> /etc/sysctl.conf
echo "session required pam_limits.so" >> /etc/pam.d/login
```

```
echo "* soft nofile 51200
* hard nofile 51200" >> /etc/security/limits.conf
echo "ulimit -SHn 51200" >> /etc/profile
echo "fs.file-max = 51200
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.core.rmem_default = 65536
net.core.wmem_default = 65536
net.core.netdev_max_backlog = 250000
net.core.somaxconn = 4096
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_mem = 25600 51200 102400
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1" >> /etc/sysctl.conf
echo "session required pam_limits.so" >> /etc/pam.d/login
```

锐速优化
```
vi /serverspeeder/etc/config
把rsc="0"改成rsc="1"，切换新网卡驱动
修改gso="0"改成gso="1"
推荐修改的内容为：
advinacc="1"  （高级入向加速开关；设为 1 表示开启，设为 0 表示关闭；开启此功能可以得到更
好的流入方向流量加速效果）
maxmode="1"  （最大传输模式；设为 1 表示开启；设为 0 表示关闭；开启后会进一步提高加速效
果，但是可能会降低有效数据率）"如果测试无效果请不要开启此功能"
其它设置，如果不能直接操作到总服务器的话，不推荐修改，保留默认即可。
按下esc退出编辑
输入:wq保存退出
最后输入下面命令，重启软件即可。
/serverspeeder/bin/serverSpeeder.sh restart
注：如果提示内存不足无法启动的话，请释放点内存后在执行启动。
或者设置engineNum="1"（只启用1个加速引擎“单核心才能更稳定”，默认CPU多少线程就启用多少个）
卸载方法：/serverspeeder/bin/serverSpeeder.sh uninstall
以上注意区分大小写，否则提示找不到文件
停止命令
/serverspeeder/bin/serverSpeeder.sh stop
启动命令
/serverspeeder/bin/serverSpeeder.sh start
方便对比测试效果
```

```
vi /appex/etc/config
1.	所需要设置的各项参数如下
2.	acc="1"
3.	advacc="1"
4.	advinacc="1"
5.	maxmode="1"
6.	initialCwndWan="44"
7.	l2wQLimit="256 2048"
8.	w2lQLimit="256 2048"
9.	shaperEnable="0"
10.	SmBurstMS="25"
11.	rsc="1"
12.	gso="1"
13.	engineNum="0"
14.	shortRttMS="60"

详细设置：

initialCwndWan="60"
平均ping ms÷3=数值

l2wQLimit="256 2048"
w2lQLimit="256 2048"
VPS内存	内存MB值	缓存KB值
256M	256	2048
512M	512	4096
1G	1024	8192
2G	2048	16384
4G	4096	32768

VPS内存MB×8=缓存数值

SmBurstMS="15"
平均ping ms÷9=数值

engineNum="0"
CPU核心 0=1核 1=2核 2=3核 3=4核，你的VPS是多少核心的就按这样以此类推。

shortRttMS="0"
平均ping ms÷3=数值，最高100，再高也没啥效果了。
```

Centos7基礎網絡優化

後綴為純粹是為了在編輯器下代碼顯示更好看而設置

以下過程切記不要少回車

連接至服務器，複製以下內容並回車

```
yum clean all
yum install screen -y
screen -S temp
```
然後複製以下內容並回車

```
yum update -y
yum install epel-release -y
yum install net-tools wget vim unzip zip python-pip iptables -y
yum -y groupinstall "Development Tools"
echo "* soft nofile 51200
* hard nofile 51200" >> /etc/security/limits.conf
echo "ulimit -SHn 51200" >> /etc/profile
echo "fs.file-max = 51200
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.core.netdev_max_backlog = 250000
net.core.somaxconn = 4096
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_mem = 25600 51200 102400
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1
net.ipv4.tcp_congestion_control = hybla" >> /etc/sysctl.conf
echo "session required pam_limits.so" >> /etc/pam.d/login
rpm -ivh https://buildlogs.centos.org/c7.1511.00/kernel/20151119220809/3.10.0-327.el7.x86_64/kernel-3.10.0-327.el7.x86_64.rpm --force
wget -4qO- softs.pw/Bash/Get_Out_Spam.sh|bash
reboot
```

稍等片刻，鏈接後複製以下內容並回車

```
wget --no-check-certificate -O appex.sh https://raw.githubusercontent.com/0oVicero0/serverSpeeser_Install/master/appex.sh && chmod +x appex.sh && bash appex.sh install
pip install --upgrade pip
pip install speedtest-cli
speedtest-cli
cd
rm -rf *
curl myip.ipip.net
echo -ne "Finish,Please engoy it."
```

https://github.com/uncia/serverSpeeder_Install

中转SS，加速Linode上的ss

一般来说，机房的网络相比民用网络，有更高的QoS级别，出口质量会相对高一些。经朋友推荐，入手了阿里云ECS（云服务器），用其中转原本直接到Linode的流量。经过几番折腾，总结出设置如下（以下均在 Ubuntu 14.04 64-bit 下操作）：


1、开启IP_FORWARD
```
vi /etc/sysctl.conf
#在文件末添加以下一行（如已有则不必添加）
net.ipv4.ip_forward = 1
```
2、使用IPTABLES，转发TCP、UDP流量
```
iptables -t nat -A PREROUTING -p tcp --dport 12XXX -j DNAT --to-destination 106.186.XX.XX:12XXX
iptables -t nat -A POSTROUTING -p tcp -d 106.186.XX.XX --dport 12XXX -j SNAT --to-source 139.XX.XX.XX
iptables -t nat -A PREROUTING -p udp --dport 12XXX -j DNAT --to-destination 106.186.XX.XX:12XXX
iptables -t nat -A POSTROUTING -p udp -d 106.186.XX.XX --dport 12XXX -j SNAT --to-source 139.XX.XX.XX

其中 106.186.XX.XX:12XXX 是ss服务器的IP与端口，139.XX.XX.XX是阿里云ECS的公网IP。
```

Azure.腾讯云：

Azure在外层有一层NAT，所给虚拟机的IP虽然是公网IP，但是不是绑定在虚拟机网卡上的IP。Azure的防火墙在收到数据包后进行一次NAT，转发给内部虚拟机。出去的数据包也经过一次NAT，之后才进行发送。

虽然在出咱虚拟机网卡的包的目标地址被正确的修改，指向了SS-vps，但是源地址是该虚拟机的外网IP（Azure叫他：公用虚拟 IP (VIP)地址），这个包在经国Azure的外围防火墙的时候被丢弃，因为认为这个包的源IP不是内部的服务器的。

```
iptables -t nat -A PREROUTING -p tcp --dport 8388 -j DNAT --to-destination SS_VPS_IP:8388
iptables -t nat -A PREROUTING -p udp --dport 8388 -j DNAT --to-destination SS_VPS_IP:8388
iptables -t nat -A POSTROUTING -p tcp -d SS_VPS_IP --dport 8388 -j SNAT --to-source Azure内部 IP 地址
iptables -t nat -A POSTROUTING -p udp -d SS_VPS_IP --dport 8388 -j SNAT --to-source Azure内部 IP 地址

iptables -t nat -A PREROUTING -p tcp --dport 233:666 -j DNAT --to-destination SS_VPS_IP
iptables -t nat -A PREROUTING -p udp --dport 233:666 -j DNAT --to-destination SS_VPS_IP
iptables -t nat -A POSTROUTING -p tcp -d SS_VPS_IP --dport 233:666 -j SNAT --to-source SS_VPS_IP
iptables -t nat -A POSTROUTING -p udp -d SS_VPS_IP --dport 233:666 -j SNAT --to-source SS_VPS_IP

iptables -t nat -A PREROUTING -p tcp --dport 233:666 -j DNAT --to-destination 103.75.117.179
iptables -t nat -A PREROUTING -p udp --dport 233:666 -j DNAT --to-destination 103.75.117.179
iptables -t nat -A POSTROUTING -p tcp -d 103.75.117.179/32 --dport 233:666 -j SNAT --to-source 117.174.59.81
iptables -t nat -A POSTROUTING -p udp -d 103.75.117.179/32 --dport 233:666 -j SNAT --to-source 117.174.59.81
```
3、保存IPTABLES，重启阿里云ECS
```
#这里使用 iptables-persistent 保存iptables配置，也可以使用其他方法保存
apt-get install iptables-persistent
netfilter-persistent save
 
reboot
```

OK，这时将ss客户端的IP改为阿里云ECS的公网IP，再去连接，ss流量就会通过阿里云中转，从此卡顿不再有（阿里云 ￥40 + ￥0.8/GB，所以钞票也不再有）。

需要注意的是，并非所有机房都支持UDP转发，各个机房网络环境也不同，具体操作过程中需要根据实际情况，找到合适的线路。

一键封禁 BT PT SPAM（垃圾邮件）
```
wget -4qO- softs.pw/Bash/Get_Out_Spam.sh|bash
```
备用下载地址（上面的链接无法下载，就用这个）：
```
wget -4qO- raw.githubusercontent.com/ToyoDAdoubi/doubi/master/Get_Out_Spam.sh|bash
```

Linux中利用 iptables string模块 屏蔽泛域名(匹配字符串)

一般 iptables 自带的都有 string 模块，这个模块的作用就是匹配字符串，匹配到泛域名的URL，然后就把数据包丢弃，就实现了屏蔽泛域名的功能。

示例：

以下规则是屏蔽以 youtube.com 为主的所有一级 二级 三级等域名。
```
iptables -I OUTPUT -m string --string "youtube.com" --algo bm --to 65535 -j DROP
# 添加屏蔽规则
 
iptables -D OUTPUT -m string --string "youtube.com" --algo bm --to 65535 -j DROP
# 删除屏蔽规则，上面添加的代码是什么样，那么删除的代码就是把 -I 改成 -D 
```
解释：

-I 是插入iptables规则；

-D 是删除对应的iptables规则；

-m string 是指定模块；

–string “youtube.com” 是指定要匹配的字符串(域名)；

–algo bm 是指定匹配字符串模式/算法；

–to 65535 是指定端口，这里代表所有端口；

-j DROP 是指匹配到数据包后处理方式，这里是丢弃数据包。

这个模块的作用就是匹配字符串，这个字符串可以是URL，也可以是普通文本，也可以是文件后缀，

比如： .zip ，就会把包含 .zip 的数据库丢弃，这样就会无法下载 .zip 类型的文件了！

Copyright (C) 2014-2999
