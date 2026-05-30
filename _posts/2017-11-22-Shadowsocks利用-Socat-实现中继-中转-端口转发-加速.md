---
layout: post
title: "Shadowsocks利用 Socat 实现中继(中转/端口转发)加速"
date: 2017-11-22
category: 未分类
---

## Socat：
优点：支持 TCP/UDP 转发。缺点：不支持端口段（多个端口需要开启多个转发）

安装步骤

Centos 系统：

yum install -y socat

Debian/Ubuntu 系统：

apt-get update

apt-get install -y socat

使用方法

转发TCP

nohup socat TCP4-LISTEN:2333,reuseaddr,fork TCP4:233.233.233.233:6666 >> /root/socat.log 2>&1 &

nohup

指的是 后台运行。

TCP4-LISTEN:2333

指的是 监听ipv4的端口，也就是 转发的端口，后面Shadowsocks链接中继时填写的 端口。

fork TCP4:233.233.233.233:6666

指的是 被转发的 IP 和 端口，也就是你要中继的服务器的 IP 和 端口。

注意：这里的 中继端口(2333) 和 被中继端口(6666) 是可以一样的，我区分开只是为了让你们更好地理解。

/root/socat.log 2>&1 &

指的是 转发日志记录。

转发UDP

nohup socat UDP4-LISTEN:2333,reuseaddr,fork UDP4:233.233.233.233:6666 >> /root/socat.log 2>&1 &

转发UDP很简单，只要把 TCP4 改成 UDP4 就行了！

停止转发

kill -9 $(ps -ef|grep socat|grep -v grep|awk ‘{print $2}’)

卸载方法

Centos系统：

yum remove socat

Debian/Ubuntu系统：

sudo apt-get remove socat

sudo apt-get autoremove

简单解释

注意：假设你的中继服务器也就是现在在操作的服务器 IP 是 1.1.1.1 ，那么你的 中继端口 就是 2333 。你的 被中继服务器的 IP 是 233.233.233.233 ，端口是 6666 。

这时候你的 Shadowsocks客户端 填写信息的时候 IP 就是 1.1.1.1 ，端口 就是 2333 。

所以原理就是：

Shadowsocks客户端通过 1.1.1.1:2333 链接中继服务器 1.1.1.1 ，然后中继服务器把端口 2333 的流量转发到 被中继服务器 233.233.233.233 的端口 6666 上面。然后 被中继服务器 也就是上面的 Shadowsocks服务端，就会去访问你要的数据，然后依次返回 中继服务器 -> Shadowsocks客户端。

防火墙设置

如果你设置后无法链接，那么多半是防火墙 阻拦了，只要开放端口 就行了。以上面的 示例的中继端口 2333 为例。

iptables -I INPUT -m state –state NEW -m tcp -p tcp –dport 2333 -j ACCEPT

iptables -I INPUT -m state –state NEW -m udp -p udp –dport 2333 -j ACCEPT

# 如果要删除端口开放规则，只需要把 -I 改成 -D 即可。

iptables -D INPUT -m state –state NEW -m tcp -p tcp –dport 2333 -j ACCEPT

iptables -D INPUT -m state –state NEW -m udp -p udp –dport 2333 -j ACCEPT

开机启动

因为这个工具并没有开机启动的设定，所以需要设置系统的开机启动。

Centos系统：

chmod +x /etc/rc.d/rc.local

vi /etc/rc.d/rc.local

Ubuntu/Debian系统：

chmod +x /etc/rc.local

vi /etc/rc.local

注意：如果你没有安装 vim 服务或者 不会使用 vim ，那么请看这篇文章：Linux中VIM编辑器的真 · 简单使用教程

输入 I 键 进入编辑模式（如果没反应请看上面的教程安装 vim），然后在打开的文件中的 exit 0 代码前面插入你的 socat 命令代码（就是上面 nohup socat…的代码）。

然后再 按 ESC 键 退出编辑模式，然后输入 :wq 退出并保存。

另外的端口转发教程：https://doub.io/ss-jc26/#服务器中继（国内中转）

其他的优化方案：https://doub.io/ss-jc26/#三、优化Shadowsocks

转载    [逗逼](https://doub.io/ss-jc40/)
**鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用[BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/)协议进行授权 , 转载请注明[Shadowsocks利用 Socat 实现中继(中转/端口转发)加速](http://www.wunail.com/archives/488)！		      

			[喜欢 (0)](javascript:;)赏[aa@qq.com]![](http://www.wunail.com/wp-content/uploads/2016/08/zfb.jpg)**分享 (0)[](#)[](#)[](#)[](#)[](#)[](#)[](#)
