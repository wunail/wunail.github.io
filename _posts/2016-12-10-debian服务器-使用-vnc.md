---
layout: post
title: "debian服务器 使用  vnc"
date: 2016-12-10
category: 未分类
---

第一、确定Debian版本和组件系统是最新

```
apt-get update
apt-get upgrade
apt-get dist-upgrade
```

在进程过程中如果有出现任何Do you want to continue [Y/n]，我们肯定是输入y然后回车继续

第二、安装LXDE环境

```
apt-get install xorg lxde-core tightvncserver
```

同样的，在进程过程中如果有出现任何Do you want to continue [Y/n]，我们肯定是输入y然后回车继续。

在过程中有一个机器地区选择，我们默认回车就可以
第三、启动VNC/输入密码

```
tightvncserver :1
```

然后需要我们输入VNC的登录密码，输入密码要注意输入的时候是不可见的，我们要看清楚自己键盘然后输入回车后再输入一次。

我们会看到有这一行”Would you like to enter a view-only password (y/n)”问我们是否需要一个只可读的账户密码，我们可以输入y输入密码，也可以输入n不输入，这个不要紧。
第四、暂停VNC

```
tightvncserver -kill :1
```

第五、配置xstartup系统文件

```
vi ~/.vnc/xstartup
```

然后在文件的最后添加下面的脚本代码

```
lxterminal &
/usr/bin/lxsession -s LXDE &
```

保存后退出。

第六、重启VNC

重启之后，我们就可以用RealVNC软件登录桌面环境，地址可以用ip:5901登录，密码是我们上面设置的VNC密码。
第七、安装支持简体中文
**鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用[BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/)协议进行授权 , 转载请注明[debian服务器 使用  vnc](http://www.wunail.com/archives/204)！		      

			[喜欢 (0)](javascript:;)赏[aa@qq.com]![](http://www.wunail.com/wp-content/uploads/2016/08/zfb.jpg)**分享 (0)[](#)[](#)[](#)[](#)[](#)[](#)[](#)
