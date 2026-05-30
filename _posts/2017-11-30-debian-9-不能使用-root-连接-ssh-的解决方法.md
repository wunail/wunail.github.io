---
layout: post
title: "debian 9 不能使用 root 连接 ssh 的解决方法"
date: 2017-11-30
category: 未分类
---

debian9因为ssh的版本比较高，默认情况下，在ssh的配置文件中，禁用了root登录，我们只需要改ssh配置文件，即可，此方法同样也适合于 ubuntu 的系统

**1修改/etc/ssh/sshd_config**

```
nano /etc/ssh/sshd_config
```

找到PermitRootLogin  并修改

```
ctrl+w  
将
#PermitRootLogin prohibit-password
修改为
PermitRootLogin yes
```

**2重启ssh即可**

service sshd restart

或

/etc/init.d/ssh restart

到这就可以以root用户ssh登录了。
