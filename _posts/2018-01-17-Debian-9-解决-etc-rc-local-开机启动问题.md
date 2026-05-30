---
layout: post
title: "Debian 9 解决 /etc/rc.local 开机启动问题"
date: 2018-01-17
category: 未分类
---

由于某些软件并没有增加开启启动的服务，很多时候需要手工添加，一般我们都是推荐添加命令到`/etc/rc.local` 文件，但是 Debian 9 默认不带 `/etc/rc.loca`l 文件，而 rc.local 服务却还是自带的。

不过默认下是关闭的

为了解决这个问题，我们需要手工添加一个 /etc/rc.local 文件

```
nano  /etc/rc.local 

#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

exit 0
```

然后赋予权限

```
chmod +x /etc/rc.local
```

接着启动`rc-local` 服务

```
systemctl start rc-local
```

再次查看状态

```
root@debian9 ~ # systemctl status rc-local
● rc-local.service - /etc/rc.local Compatibility
   Loaded: loaded (/lib/systemd/system/rc-local.service; static; vendor preset: enabled)
  Drop-In: /lib/systemd/system/rc-local.service.d
           └─debian.conf
   Active: active (exited) since Thu 2017-08-03 09:41:18 UTC; 14s ago
  Process: 20901 ExecStart=/etc/rc.local start (code=exited, status=0/SUCCESS)

Aug 03 09:41:18 xtom-proxy systemd[1]: Starting /etc/rc.local Compatibility...
Aug 03 09:41:18 xtom-proxy systemd[1]: Started /etc/rc.local Compatibility.
```

然后你就可以把需要开机启动的命令添加到`/etc/rc.local`文件，丢在 exit 0 前面即可，并尝试重启以后试试是否生效了

参考[烧饼博客](https://sb.sb/debian-9-rc-local/)
