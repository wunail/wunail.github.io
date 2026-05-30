---
layout: post
title: "树莓派使用虚拟终端Screen,不再惧怕SSH中断"
date: 2016-12-12
category: 未分类
---

Screen怎么理解呢？理解为虚拟终端管理器吧。我们可以用它在后台管理终端界面，这样SSH断开后就不用怕正在进行的操作中断了。

**一、安装：**

```
sudo apt-get update
sudo apt-get install screen
```

**二、使用：**

1、创建一个虚拟终端，

树莓派登陆SSH后执行

```
screen -S t   #这样就创建好一个名为t的终端了
```

此时我们可以随便执行操作了，

比如执行 sudo apt-get upgrade，或者其它消耗时间比较长的工作，像编译内核等等。

按 ctrl+a 后再按 d 这样就保存好一个虚拟终端了，系统会提示deatached

我们的SSH什么的可以完全断开不管了，让虚拟终端自己运行去吧。

2、访问已经创建好的终端

```
screen -ls           #可以列出已经创建的正在后台运行的终端
screen -r            #终端名称就可以了
示例：
screen -r t       #列出正在运行的t
```

3、彻底退出

如果一个虚拟终端中的程序执行完毕了，screen -r 进入这个终端后再执行 exit 就完全退出了。

这样以后通过SSH编译内核之类的长时间工作时，再也不怕因为断网造成的操作中断了。

在任何linux设备上都能安装Screen，操作也是一样的
**鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用[BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/)协议进行授权 , 转载请注明[树莓派使用虚拟终端Screen,不再惧怕SSH中断](http://www.wunail.com/archives/220)！		      

			[喜欢 (1)](javascript:;)赏[aa@qq.com]![](http://www.wunail.com/wp-content/uploads/2016/08/zfb.jpg)**分享 (0)[](#)[](#)[](#)[](#)[](#)[](#)[](#)
