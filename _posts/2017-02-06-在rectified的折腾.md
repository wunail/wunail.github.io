---
layout: post
title: "在rectified的折腾"
date: 2017-02-06
category: 未分类
---

刚买了一年的洛杉矶 机器  先来试一试

本次测试环境：

系统：Debian8 64X

内存：256M

## 纯ss

单独安装ss使用的脚本如下

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

[在vultr上搭建梯子](http://www.wunail.com/2016/12/31/%E5%9C%A8vultr%E4%B8%8A%E6%90%AD%E5%BB%BA%E6%A2%AF%E5%AD%90/)

由于是在家试的 速度感人 youtube只有30几。。。

ping平均314

## ss+锐速

锐速使用的脚本如下

```
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh
```

装了即便一直显示NOT run

后来重启一遍机器后没问题了  感觉可能是内存不足的原因

速度不稳定  youtube可能有8。9百左右

ping平均199

总体来说稍微好一点了
本来还想在它上边搭建ss站点 但无奈内存太小，根本没有成功，后来在搬瓦工上边成功过一次  不过由于vps太菜  网页根本就反应不过来   后来还是不折腾了 单纯弄个ss吧  后来一想还是弄ssr好点  不仅支持ss  还有可能通过设置实现免流

## 单纯弄个ssr吧

ShadowsocksR 多用户版安装教程

以下命令均以root用户执行，或sudo方式执行

基本库安装

centos：

yum install python-setuptools && easy_install pip

yum install git

ubuntu/debian：

apt-get install python-pip

apt-get install git

获取源代码

git clone -b manyuser https://github.com/shadowsocksr/shadowsocksr.git

执行完毕后此目录会新建一个shadowsocks目录，其中根目录的是多用户版（即数据库版），子目录中的是单用户版。

根目录即 ./shadowsocksr

子目录即 ./shadowsocksr/shadowsocks

服务端配置

进入根目录初始化配置(假设根目录在~/shadowsocksr，如果不是，命令需要适当调整)：

cd ~/shadowsocksr

bash initcfg.sh

shadowsocksr目录内，对userapiconfig.py里以下内容进行相应修改：

API_INTERFACE = ‘mudbjson’ #修改接口类型

接着，通过使用脚本mujson_mgr.py添加端口及相应的加密、协议、混淆等配置，具体方法通过执行以下命令查看该脚本的说明及提示：

python mujson_mgr.py

服务端运行与停止

后台运行（无log，ssh窗口关闭后也继续运行）

sh ./run.sh

后台运行（输出log，ssh窗口关闭后也继续运行）

sh ./logrun.sh

后台运行时查看运行情况

sh ./tail.sh

停止运行

sh ./stop.sh

注：通过脚本运行默认日志会保存在根目录的ssserver.log，可手动查看。

更新源代码

如果代码有更新可用本命令更新代码

进入shadowsocksr目录

cd shadowsocksr

执行

git pull

成功后重启ssr服务

其它异常

如果你的服务端python版本在2.6以下，那么必须更新python到2.6.x或2.7.x版本

其它参见 [github](https://github.com/breakwa11/shadowsocks-rss/wiki/ulimit)

命令 详解

操作：

  -a添加/编辑用户

  -d DELETE删除用户

  -e编辑用户

  -c CLEAR将u / d设置为零

  -l LIST显示用户信息或所有用户信息

选项：

  -u USER用户名

  -p PORT服务器端口（仅添加用户时，必须设置此选项）

  -k PASSWORD密码

  -m METHOD加密方法，默认值：aes-128-ctr

  -O协议协议插件，默认：auth_aes128_md5

  -o OBFS obfs插件，默认值：tls1.2_ticket_auth_compatible

  -G PROTOCOL_PARAM协议插件参数

  -g OBFS_PARAM obfs插件参数

  -t TRANSFER最大传输G字节，默认值：8388608（8 PB或8192 TB）

  -f FORBID设置禁止端口。实施例（禁用1〜79和81〜100）：-f“1-79,81-100”

  -i MUID设置要显示的子标识（仅与-l一起使用）

一般选项：

  -h，–help显示此帮助消息并退出

 python mujson_mgr.py -a -p 9002 -k 9002 -O origin(协议) -o plain(混淆)
**鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用[BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/)协议进行授权 , 转载请注明[在rectified的折腾](http://www.wunail.com/archives/282)！		      

			[喜欢 (0)](javascript:;)赏[aa@qq.com]![](http://www.wunail.com/wp-content/uploads/2016/08/zfb.jpg)**分享 (0)[](#)[](#)[](#)[](#)[](#)[](#)[](#)
