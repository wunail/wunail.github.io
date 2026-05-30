---
layout: post
title: "ubuntu下配置lamp"
date: 2016-08-13
category: 未分类
---

**1**首先配置apache

```
sudo apt-get install apache2
```

 配置完成后检查是否配置成功。通过浏览器打开locallhost    (若在服务器上即通过浏览器打开”ip“)  如果出现Apache欢迎界面，表示安装成功，如下所示：[![http://img0.imgtn.bdimg.com/it/u=2216208700,2020871444&fm=21&gp=0.jpg](http://www.ahlinux.com/uploadfile/2015/0313/20150313101119918.png)](http://www.ahlinux.com/uploadfile/2015/0313/20150313101119918.png)

**2**修改/var/www/html 下的文件**修改时会出现权限不够的情况。 ls一下会出现这种情况

```
drwxrwxr-x 3 root root 4096 Dec 24 01:35 www
```

drwxrwxr-x，从左到右第一个字母表示文件系统对象的类别，这里d表示为目录(文件夹)。其它文件系统对象:
-(常规文件)、d(目录)、l(符号链接)、c(字符特殊设备)、b(模块特殊设备)、p(FIFO)、s(套接字)
drwxrwxr-x 除出去第一个字母d后的rwxrwxr-x表示的是三种用户关系对文件或文件夹的操作权限。从左到右每三个一组，依次表示所有者权限、组权限、其他用户权限。每组的顺序均为rwx，如果用户有相应的操作权限就用相应的字母表示，如果不具有相应的操作权限就用-表示。比如:rwxrwxr-x表示文件或文件夹的所有者具有rwx(可读，可写，可执行)的操作权限，组用户也具有rwx(可读，可写，可执行)的权限，其他用户具有r-x(可读，可执行，没有可读)的操作权限。
 需要修改一下用户权限

```
sudo chmod 777  /var/www/
sudo chmod 777  /var/www/html
sudo chmod 777  /var/www/html/index.html
```

修改/var/www/html 下的index.html后即可访问
现在通过浏览器打开locallhost    (若在服务器上即通过浏览器打开”ip“) 访问服务器上的内容了，只不过这个时候只能访问服务器上的静态文件（html） 所以下边我们就要安装 php环境了。
2**安装php环境

```
sudo apt-get install php5
```

（因为之前我先安装了php所以在安装apache后配置出了些问题，看明天能不能解决，不能解决的话只能重新再来了）
