---
layout: post
title: "ShadowsocksR 多用户版服务端安装教程（SS-Panel后端）"
date: 2018-01-18
category: 未分类
---

注:多用户版需配合ss-panel等前端使用，查看SS-Panel教程。

## 基本库安装

Centos系统执行这个：

```
yum update
yum install git -y
```

Ubuntu/Debian系统执行这个（推荐这两个系统，对新手友好.测试用的这个成功了）：

```
apt-get update
apt-get install git -y
```

如果要使用 salsa20 和 chacha20 加密方式，请安装 libsodium：**我这里暂时用不上就没安装  安装的话看链接：
[ShadowsocksR 安装libsodium 以支持 Chacha20/Chacha20-ietf 加密方式](https://doub.io/ss-jc51/)
如果曾经安装过旧版本，亦可重复用以上步骤更新到最新版，仅1.0.4或以上版本支持chacha20-ietf

## 获取源代码

```
git clone -b manyuser https://github.com/ToyoDAdoubi/shadowsocksr.git
#备用地址
git clone -b manyuser https://github.com/wehaha/shadowsocksr.git
```

执行完毕后此目录会新建一个shadowsocksr目录，其中根目录的是多用户版（即数据库版），子目录中的是单用户版。
子目录中的是单用户版(即 shadowsocksr/shadowsocks)。
以下是相对路径，比如你在 /root 目录下执行上面的代码，那你的SS根目录就是 /root/shadowsocksr 。
根目录即 shadowsocksr
子目录即 shadowsocksr/shadowsocks

## 安装依赖（cymysql）

```
cd shadowsocksr
# 进入ShadowsocksR根目录
bash setup_cymysql.sh
# 安装Cymysql 依赖
bash initcfg.sh
# 初始化ShadowsocksR服务端
```

## 服务端配置

shadowsocksr 根目录内，打开文件nano `usermysql.json`：

```
"host": "127.0.0.1", //前端mysql域名/IP
"port": 3306, //mysql端口
"user": "ss", //mysql用户名
"password": "pass", //mysql密码
"db": "shadowsocks", //数据库名
```

注意：这里的数据库信息除了 host ，其他的必须和SS-Panel完全一致，服务端启动的时候会读取数据库信息！至于 host ，如果你的服务端和前端（SS-Panel）在一个VPS上，那就写localhost或者127.0.0.1，如果不在一起，请写前端（SS-Panel）所在VPS的ip或者域名！
还有一条要注意的，如果服务端和前端（SS-Panel）不在一个VPS上，那么数据库链接就属于远程链接了，这时候需要开启数据库用户的远程链接功能！
开启方法：在数据库——用户——编辑权限——登录信息中修改Host为任意主机“%”
[![丢失了](https://img.doub.pw/ss-jc14-01.jpg)](https://img.doub.pw/ss-jc14-01.jpg)shujuk

## 配置文件config.json：

```
# 一般情况下不需要编辑，除非你需要修改 加密方式/协议/混淆等参数。
nano user-config.json
```

```
"method":"aes-128-ctr", //修改成您要的加密方式的名称
"protocol": "auth_aes128_md5", //修改成您要的协议插件名称
"obfs": "tls1.2_ticket_auth_compatible", //修改成您要的混淆插件名称
```

前端用的sspanel的话是不支持ssr 所以再添加节点后需要根据这里的设置 在节点信息处添加上协议和混淆  然后ssr手动设置
id //用户id数据库字段说明:

```
email //用户邮箱
pass //用户密码
passwd //ss密码
t //最后使用的时间
u //已上传流量
d //已下载流量
transfer_enable //可用流量（总量）
port //ss端口
switch //保留字段
enable //启用或禁用ss帐号（1启用，0禁用）
type //保留字段
last_get_gift_time //保留字段
last_rest_pass_time //保留字段
```

ShadowsocksR多用户板服务端默认开启UDP的**

## 服务端运行与停止

```
python server.py
```

显示如下即为成功**[![](http://www.wunail.com/wp-content/uploads/2018/01/2018011803020268.png)](http://www.wunail.com/wp-content/uploads/2018/01/2018011803020268.png)

## 通过脚本运行

脚本位于 shadowsocksr 根目录，如果你没有在这个目录，请先进入根目录cd shadowsocksr
请分清楚，根目录 shadowsocksr 和子目录 shadowsocksr/shadowsocks ！
赋予脚本执行权限（执行一次就好）

```
chmod +x *.sh
```

后台运行 但不记录日志（ssh窗口关闭后也继续运行）

```
./run.sh
```

后台运行 且 记录日志（ssh窗口关闭后也继续运行）

```
./logrun.sh
```

查看 SS日志（用 logrun.sh 脚本启动才会打开日志）

```
./tail.sh
```

停止运行

```
./stop.sh
```

注：通过脚本运行默认日志会保存在根目录的ssserver.log，可手动查看。
如果日志文件太大，需要清理，可以用下面这个命令 清空 日志文件。

```
cat /dev/null > ssserver.log
```

## 其他

### 限制设备连接数

```
nano /root/shadowsocksr/user-config.json
```

找到协议参数（参数为空`""`时，默认限制 64个设备数）

```
"protocol_param": "",
```

在协议参数中设置你要限制 每个端口最大设备连接数（建议最少2个），比如 限制最大 5个设备同时链接，那么改为：

```
"protocol_param": "5",
```

注意：协议参数仅在服务端 协议设置(protocol)为 非原版(origin)协议并不兼容原版(_compatible) 时才有效！
如果你服务端 协议设置(protocol)的是 原版(origin) 时，设备数限制无效。
如果你服务端 协议设置(protocol)的是 协议兼容原版 ，那么当用户使用原版协议(origin)连接Shadowsocks账号时，设备数限制无效。

### 更新源代码

如果代码有更新可用本命令更新代码
进入shadowsocksr目录
cd shadowsocksr
执行
git pull
成功后重启ss服务
这个功能以后就用不上了。很多大神都迫于压力 退圈了。。。。。

### 开机启动

一些人可能需要开机启动，我就一起写上吧。
首先设置开机启动文件的权限，并打开该文件。
Centos系统：**

```
chmod +x /etc/rc.d/rc.local
nano /etc/rc.d/rc.local
```

**Ubuntu/Debian系统：**

```
chmod +x /etc/rc.local
nano /etc/rc.local
```

如果是debian9的话 请看[Debian 9解决/etc/rc.local开机启动问题](http://www.wunail.com/archives/628)

然后在`exit 0`这一句代码（只有ubuntu/debian有这个 exit 0）的前面加上 下面这句代码(如果你的Shadowsocksr文件夹不在root目录下，请自行修改路径)。

是使用不记录日志的 run.sh 脚本还是记录日志的 logrun.sh 自己看着改。

```
bash /root/shadowsocksr/run.sh
```

### 远程链接数据库

默认lnmp安装后，会封闭3306的端口，不允许外部链接，所以删掉这个规则重新添加开放规则就好了。

删掉原来的3306 DROP规则，然后加上开放3306端口规则。

查看iptables规则

```
#--line-number可以显示规则序号，在删除的时候比较方便 
iptables -L -n --line-number |grep 21
```

修改规则

```
#将规则3改成DROP  
iptables -R INPUT 3 -j DROP
```

删除iptables规则

```
打印?
[root@linux ~]# iptables -D INPUT 3  //删除input的第3条规则  
  
[root@linux ~]# iptables -t nat -D POSTROUTING 1  //删除nat表中postrouting的第一条规则  
  
[root@linux ~]# iptables -F INPUT   //清空 filter表INPUT所有规则  
  
[root@linux ~]# iptables -F    //清空所有规则  
  
[root@linux ~]# iptables -t nat -F POSTROUTING   //清空nat表POSTROUTING所有规则
```

保存规则

```
iptables-save > /etc/sysconfig/iptables
```

参考

[逗逼根据地](https://doub.io/ss-jc14/)
**鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用[BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/)协议进行授权 , 转载请注明[ShadowsocksR 多用户版服务端安装教程（SS-Panel后端）](http://www.wunail.com/archives/632)！		      

			[喜欢 (26)](javascript:;)赏[aa@qq.com]![](http://www.wunail.com/wp-content/uploads/2016/08/zfb.jpg)**分享 (0)[](#)[](#)[](#)[](#)[](#)[](#)[](#)
