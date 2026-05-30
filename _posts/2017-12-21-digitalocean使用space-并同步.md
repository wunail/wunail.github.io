---
layout: post
title: "digitalocean使用space 并同步"
date: 2017-12-21
category: 未分类
---

## 前言

因为github学生认证的原因获得了50美刀 digitalocean的使用券 于是在do上开了台旧金山的机子。开之前我试了一下学校的网来说还是旧金山的相对好一点。 然后这几天do又刚推出了免费spaces两个月使用的活动  于是就尝试一下。他的这个spaces和国内的oss储存应该是一个东西（还不是太懂）。刚一开始不知道有什么实际的用处  后来想用oddfs挂载一下  不过看digitalocean文档的时候发现了他有官方提供的参考文档

   用的是亚马逊的上cmd 做的修改。

s3cmd 是一款 Amazon S3 命令行工具。它不仅能上传、下载、同步，还能设置权限，下面是完整的安装使用指南。

## 一下载安装

cenos好像直接能通过yum安装  不过我这次用的debian就没有试

方法一：（Debian/Ubuntu ）

```
wget -O- -q http://s3tools.org/repo/deb-all/stable/s3tools.key | sudo apt-key add -
wget -O/etc/apt/sources.list.d/s3tools.list http://s3tools.org/repo/deb-all/stable/s3tools.list
apt-get update && sudo apt-get install s3cmd
```

具体的可去官网看看在最下边  [官网](http://s3tools.org/repositories)

方法二：

```
wget http://nchc.dl.sourceforge.net/project/s3tools/s3cmd/1.0.0/s3cmd-1.0.0.tar.gz
tar -zxf s3cmd-1.0.0.tar.gz -C /usr/local/
mv /usr/local/s3cmd-1.0.0/ /usr/local/s3cmd/
ln -s /usr/local/s3cmd/s3cmd /usr/bin/s3cmd
```

这个方法是直接下载下来安装的 具体两者有什么差别我也不是很清楚  不过后者能自己选择安装版本

在下载压缩包的时候可以自己选择  [下载地址](https://sourceforge.net/projects/s3tools/files/s3cmd/)

不过之一定要找到直连下载地址。

刚一开始使用的是第一种方法下载下来是1.8的版本  但digitalocean给的文档是给的s3cmd 2.0的 在使用过程中一直出现302定向错误  后来在安装2.0后配置时发现一句话    “通过HTTPS连接        接下来，我们被提示通过HTTPS进行连接，HTTPS保护数据在传输过程中不被读取。DigitalOcean Spaces不支持未加密的传输，所以我们将按ENTER接受默认值YES：”

按理说只要配置好哪个版本都行

## 二、配置

1、配置，主要是 Access Key ID 和 Secret Access Key

```
s3cmd --configure
```

输入 在api页面设置的 key

和 Secret Access Key
参数                                   说明                                                                               是否必填

Access Key                        OBS访问密钥的AK。                                                                     必填

Secret Key                         OBS访问密钥的SK。                                                                     必填

Default Region              默认区域，即桶将存储在哪个区域。                                                  必填

Encryption password     当使用客户端加密方式传输数据时，需要用到的加密密码。                选填

Path to GPG program    当使用客户端加密方式传输数据时，GPG加密软件所在的路径。         选填

Use HTTPS protocol        是否启用HTTPS协议。l 是，配置为Yes。l 否，配置为No。              选填

HTTP Proxy server name     在不启用HTTPS协议时，选择是否启用HTTP代理，如果配置为空，则表示不使用HTTP代理。   选填

其中在1.8和2.0在配置时的内容是不一样的  不过都可以通过nao修改 ~/.s3cfg

把              s3.amazonaws.com              改为    nyc3.digitaloceanspaces.com

把  “%(bucket)s.s3.amazonaws.com”    改为   %(bucket)s.nyc3.digitaloceanspaces.com

## 使用方法

一些常见命令

1、列举所有 Buckets。（bucket 相当于根文件夹）

s3cmd ls

2、创建 bucket，且 bucket 名称是唯一的，不能重复。

s3cmd mb s3://my-bucket-name

3、删除空 bucket

s3cmd rb s3://my-bucket-name

4、列举 Bucket 中的内容

s3cmd ls s3://my-bucket-name

5、上传 file.txt 到某个 bucket，

s3cmd put file.txt s3://my-bucket-name/file.txt

6、上传并将权限设置为所有人可读

s3cmd put –acl-public file.txt s3://my-bucket-name/file.txt

7、批量上传文件

s3cmd put ./* s3://my-bucket-name/

8、下载文件

s3cmd get s3://my-bucket-name/file.txt file.txt

9、批量下载

s3cmd get s3://my-bucket-name/* ./

10、删除文件

s3cmd del s3://my-bucket-name/file.txt

11、来获得对应的bucket所占用的空间大小

s3cmd du -H s3://my-bucket-name

[华为官方文档](http://static.huaweicloud.com/upload/files/pdf/20170519/20170519170342_77767.pdf)

[digitalocean文档](https://www.digitalocean.com/community/tutorials/how-to-configure-s3cmd-2-x-to-manage-digitalocean-spaces#configure-s3cmd)

[s3cmd官网](http://s3tools.org/s3cmd)

[s3cmd 中文帮助](http://www.yunvm.com/blog/static/archives/48069)

[s3cmd 安装使用指南](http://blog.csdn.net/pkufergus/article/details/50034309)
