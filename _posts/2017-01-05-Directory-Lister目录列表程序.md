---
layout: post
title: "Directory Lister目录列表程序"
date: 2017-01-05
category: 未分类
---

一直想弄个自己的私有云  但现在条件不成份  时间不太多  只好临时弄个这了

## 简单介绍

Directory Lister是一个目录列表程序，基于PHP和一些脚本文件，可以列出目录的内容，在众多的目录列表程序中一直保持简洁。

  这个修改版，主要是对谷歌字体和一些七七八八的国外CDN下载到本地了，并且做了兼容中文处理。

首先安装[军哥lnmp](https://lnmp.org/install.html)

然后下载

文件下载**  文件名称：DirectoryLister**  文件大小：1M**  下载声明：本站文件大多来自于网络，仅供学习和研究使用，不得用于商业用途，如有版权问题，请联系博猪！**  下载地址：[DirectoryLister](http://45.77.24.141/vps/%E6%9C%AC%E7%AB%99%E7%94%A8%E7%9A%84%E6%A8%A1%E6%9D%BF/DirectoryLister-master.zip)

把里边的文件 放到主目录下就好了

这个就很简单，直接把下载好的压缩包解压然后把解压后的文件上传到你的虚拟主机目录就好了。

标题是在 resources\themes\bootstrap\index.php 中修改，底部文件则是同目录的 default_footer.php 文件。

然后把你要列出显示的目录和文件上传到和 resources 文件夹平级的位置，也就是同一个文件夹中。

## 注意事项

我发现部分人安装的LNMP一键包，使用DL后无法显示文件和文件夹，那是因为LNMP默认禁用了一些影响安全性的PHP函数，其中 scandir 就是导致无法显示的函数，所以解除禁用就可以了。

依次执行以下代码：

```
sed -i 's/,scandir//g' /usr/local/php/etc/php.ini
/etc/init.d/php-fpm restart
```

转载 [逗比](https://www.dou-bi.co/dbrj-2/)
