#net-speeder OpenVZ vps的安装及使用

net-speeder的安装
=================
安装net-speeder（CentOS6 64bit）
```
wget --no-check-certificate https://gist.github.com/LazyZhu/dc3f2f84c336a08fd6a5/raw/d8aa4bcf955409e28a262ccf52921a65fe49da99/net_speeder_lazyinstall.sh
sh net_speeder_lazyinstall.sh
nohup /usr/local/net_speeder/net_speeder venet0 "ip" >/dev/null 2>&1 &

echo 'nohup /usr/local/net_speeder/net_speeder venet0 "ip" >/dev/null 2>&1 & ' >> /etc/rc.local
```

安装net-speeder （Debian/Ubuntu）
wget --no-check-certificate https://raw.githubusercontent.com/tennfy/debian_netspeeder_tennfy/master/debian_netspeeder_tennfy.sh
chmod a+x debian_netspeeder_tennfy.sh
bash debian_netspeeder_tennfy.sh

查看net-speeder是否运行

```
ps aux|grep net_speeder|grep -v grep
```

停止net-speeder

```
killall net_speeder
```

启动net-speeder（OPENVZ环境）

```
nohup /root/net_speeder venet0 "ip" >/dev/null 2>&1 &
```

设置net-speeder定时开关

net-speeder实际上是颇有争议的，双倍发包会导致网络拥堵，有点损人利己的感觉。因此，tennfy给出一个折中的方案，就是在晚上高峰期的时候开启net-speeder,空闲时间关闭。

1、设置时区

由于美国的VPS时区跟中国是不一致的，因此需要给VPS设置一下时区。

```
echo "Asia/Shanghai" >/etc/timezone
```

2、设置net-speeder定时开关

我们设定19点开启，24点关闭。执行以下命令：
```
echo '0 19 * * * root nohup /root/net_speeder venet0 "ip" >/dev/null 2>&1 &' >>/etc/crontab
echo "0 0 * * * root killall net_speeder" >>/etc/crontab
/etc/init.d/cron restart
```
