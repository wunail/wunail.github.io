---
layout: post
title: "wordpress主题Git-alpha的使用技巧"
date: 2019-09-04
categories: [各种搞]
source_id: 690
---

本网站一直以来就用的这个主题 不过开始是免费的 后来就收费了 
 这是网站出问题时 花了1元从作者那里买了一份指导文档

 

## 文档

 

### 安装主题启用之后空白了

 

主题集成了多种常用插件，所以和一些插件冲突，禁用全部插件，主要是谷歌字体插件，头像插件，no-category 插件，本地头像插件等等

 

## 给网站菜单前面添加各种个性图标

 

### 给网站菜单前面添加各种个性图标

 

首先打开这个网站：http://www.fontawesome.com.cn/faicons/，然后是不是看到了很多图标，随便点击一个，比如我点击一个五角星吧 
 ![星星](https://img-blog.csdn.net/20180923204810746?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3ODE1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 
 然后看到里面的那个 i 标签没？就是他 
 `` 
 复制他，然后进入网站后台——外观——菜单里面，打开想要添加图标的菜单，打开菜单标签，然后将刚刚复制的代码粘贴在标签文字的前面

 

### 插入图片没法弹出来，没有弹出效果

 

首先更新主题到最新版，然后这个需要 a 标签的支持，只需要插入图片的时候，链接到媒体文件就好了，看下图就好了 
 ![](https://img-blog.csdn.net/20180923205157356?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE3ODE1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 

### 主题缩略图无法显示图片

 

首先需要检查下主机是否开启了 GD 库

 

在和timthumb.php同一个目录下新建一个cache文件夹用来存储生成的小图片，设置 cache 文件夹为 755 或 777 权限

 

1、主机需要支持 GD 库 
 2、原图片是否坏了 
 3、WordPress 文件下是否有一个 cache 文件夹 
 4、cache 文件夹是否给予 777 或者 755 权限

 

提示：如果缩略图不显示，浏览器右键，将缩略图图片新标签打开就可以发现问题所在了

 

### 修改头部文字 logo 字体

 

 

---

 ** 鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用 [BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/) 协议进行授权 , 转载请注明 [wordpress主题Git-alpha的使用技巧](http://www.wunail.com/archives/690) ！