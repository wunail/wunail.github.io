---
layout: post
title: "Linux 使用iptables进行shadowsocks流量统计"
date: 2017-08-17
category: 未分类
---

通过ifconfig只能看到所有的流量总和

如果想实时统计某个端口上用了多少流量，最简单的方法便是通过iptables

通过这个方法也可以去统计shadowsocks的每个账号用了多少流量

shadowsocks多用户版为每个用户分配了不同的服务器连接端口号，服务器对该用户的所有流量均是通过这个端口发出的

只需要以这个端口为源端口，统计OUTPUT流量，就可以精确统计shadowsocks的单用户流量

统计5902端口上的出网流量（这里统计的是用户下载流量）:

```
iptables -A OUTPUT -p tcp --sport 5902  
#-A OUTPUT  表示在OUTPUT上增加一条规则
#-D OUTPUT  表示在OUTPUT上删除一条规则
#-p tcp  表示指定tcp协议    
#–sport 5902  表示出网的端口号为5902

统计5902端口上的进网流量（这里统计的是用户上传流量）:

iptables -A INPUT -p tcp --dport 5902  
-A INPUT表示在INPUT上增加一条规则
-D INPUT表示在INPUT上删除一条规则
-p tcp  表示指定tcp协议    
–dport 5902  表示入网的端口号为5902
```

添加完成之后就可以通过下面命令查看流量信息

```
iptables -vnL  
iptables -vnL>/home/iptables.log  #输出内容到文件
```

在INPUT下面的就是入网流量，OUTPUT里面的是出网流量，默认是使用易读的单位，也就是自动转化成M，G。如过需要Bytes做单位，则增加一个-x参数：

iptables -n -v -L -t filter -x

流量信息自添加规则之后开始统计，无法显示之前的流量信息。 重启防火墙，流量统计数据将会被重置

iptables

pkts一列是包的数量 bytes一列是流量统计结果

最后：

一般重启后iptables规则会丢失，因此需要进行保存操作。

service iptables save  #保存防火墙配置

vi /etc/sysconfig/iptables  #编辑防火墙配置

service iptables restart  #重启防火墙  

类似于top一样的流量检测工具  [iftop](https://linux.cn/article-1843-1.html)

 [另一种](https://jeffhughlee.blogspot.jp/2017/03/shadowsocks.html)
