![ShadowsocksR](https://github.com/uncia/shadowsocksr_install/raw/master/shadowsocks.png)
# 自动安装  ShadowsocksR Server 无日志
# Auto install ShadowsocksR Server No Log

shadowsocksR.sh
===============
* Description: Auto Install ShadowsocksR Server for CentOS/Redhat/Debian/Ubuntu
* Intro: https://shadowsocks.be/9.html

使用方法：
使用root用户登录，运行以下命令：
```
wget --no-check-certificate https://raw.githubusercontent.com/uncia/shadowsocksr_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```
安装完成后，脚本提示如下：
```
Congratulations, ShadowsocksR install completed!
Server IP:your_server_ip
Server Port:your_server_port
Password:your_password
Local IP:127.0.0.1
Local Port:1080
Protocol:origin
obfs:plain
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

Copyright (C) 2014-2999
