---
layout: post
title: "Ubuntu 18.04 手动安装 qBittorrent 并使用 nginx 进行反代"
date: 2019-10-12
categories: [linux服务器]
tags: [linux]
source_id: 704
---

qBittorrent 是一款开源免费的种子和磁力链接下载工具（项目地址点此直达），支持 Windows、Mac 和 Linux，且功能非常强大。qBittorrent 支持使用种子文件和磁力链接下载，包括了做种、tracker 编辑、下载优先级设置、RSS 订阅等功能非常丰富。本文详细介绍如何在 Ubuntu 18.04 上手动安装最新版 qBittorrent 并使用 nginx 进行反代。注意，以下操作是在 root 账号下进行的，非 root 账号需提升到 root 权限。

 

安装 qBittorrent 
 添加软件源 
 apt update && apt install software-properties-common -y && add-apt-repository ppa:qbittorrent-team/qbittorrent-stable -y 
 安装 qBittorrent 
 apt update && apt install qbittorrent-nox -y 
 配置 qBittorrent 
 添加 qBittorrent 运行账户 
 使用如下命令为 qBittorrent 运行添加账户，使用 –system 创建系统用户而不是普通用户，系统用户没有密码，无法登录：

 

adduser –system –group qbtuser 
 为 qBittorrent 创建 systemd 服务 
 使用如下命令为 qBittorrent 创建 systemd 服务：

 

cat > /etc/systemd/system/qbittorrent.service << EOF
[Unit]
Description=qBittorrent Daemon Service
After=network.target

[Service]
User=qbtuser
Group=qbtuser
ExecStart=/usr/bin/qbittorrent-nox
ExecStop=/usr/bin/killall -w qbittorrent-nox

[Install]
WantedBy=multi-user.target
EOF
启动服务并设置开机自启：

systemctl start qbittorrent && systemctl enable qbittorren

 

---

 ** 鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用 [BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/) 协议进行授权 , 转载请注明 [Ubuntu 18.04 手动安装 qBittorrent 并使用 nginx 进行反代](http://www.wunail.com/archives/704) ！