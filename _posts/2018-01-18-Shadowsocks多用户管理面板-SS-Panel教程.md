---
layout: post
title: "Shadowsocks多用户管理面板——SS-Panel教程"
date: 2018-01-18
category: 未分类
---

现在的Shadowsocks站非常的多，很多人看了之后也想做一个，但是苦于没有详细的教程。所以，本教程就诞生啦！！

## 安装环境

PHP版本>5.4，而且需要PDO扩展。

注意：Lnmp一键包是满足所需条件的，如果是用的其他一键包或者面板请注意这两个要求！如果安装完毕，访问页面出现白屏，那就是php或者pdo版本问题。

Web环境我选的是LNMP，用的是lnmp.org军哥的一键安装包！

因为军哥自己写的已经很详细了，所以我具体安装教程看这个吧！——[lnmp](https://lnmp.org/install.html)（注意：输入的MySQL的root密码一定要记住）

安装时间可能会几十分钟到几个小时不等，主要是机器的配置网速等原因会造成影响。安装完成之后就创建虚拟主机。有什么不懂的话就[看这个](https://lnmp.org/faq/lnmp-vhost-add-howto.html)

如果你没有域名，那你可以跳过创建虚拟主机步骤，直接吧sspanel上传到 /home/wwwroot/default 文件夹，把里面除了 phpmyadmin文件夹 以外的文件都删除。这个文件夹是通过IP访问的。

## 数据库配置

先下载SS-Panel面板文件：[自用云](http://45.77.24.141/vps/ss-panel-2.zip)、[github](https://github.com/orvice/ss-panel/tree/v2)（我网盘里不一定是最新版的，可以去这个网站下载最新偶！点击Download ZIP即可下载）

然后解压并打开`sql`文件夹，这时候会看到五个`sql`文件。

然后打开网站：你的`IP/phpmyadmin`这时候你就在phpmyadmin上输入用户名root密码你`安装LNMP时候设置的数据库root密码`

进去之后点击数据库——新建数据库——创建。

[![](http://www.wunail.com/wp-content/uploads/2018/01/2018011803370287.png)](http://www.wunail.com/wp-content/uploads/2018/01/2018011803370287.png)

然后点击左边新建的数据库，点导入，然后选择sql文件夹下的sql文件，点执行。重复五次，把五个文件都导入进去。

[![](http://www.wunail.com/wp-content/uploads/2018/01/2018011803381562.jpg)](http://www.wunail.com/wp-content/uploads/2018/01/2018011803381562.jpg)

导入完成后就是这个样子！

[![](http://www.wunail.com/wp-content/uploads/2018/01/2018011803384429.jpg)](http://www.wunail.com/wp-content/uploads/2018/01/2018011803384429.jpg)

## 网站配置

打开lib文件夹找到`config-simple.php`文件，打开它。
注意：用系统自带的记事本软件会出现编码问题，我推荐使用Notepad++，在编辑代码的时候很方便！）

下图中红框圈中的是必须修改的，至于初始化流量和签到流量，请自己调节！

`DB_USER` 是数据库用户名，`DB_PWD` 是数据库密码，`DB_DBNAME` 是数据库名称（别写错了！），这里可以使用默认的root账户也可以自己创建一个新的数据库用户。

下面的自然就是网站名字和网站域名了（必须加上http://或者https://和最后的“/”，这里请根据自己的网站是否加了ssl来选择http和https，不止一个人问我什么是https，很多人没使用SSL却还是使用默认的https，导致网站的一些链接出错误。）。

然后`$salt` 这个是加密盐值，不要默认，随便填一些，只要不是默认就行。

设置完后保存一下，然后修改config-simple.php文件名为config.php。接下来就是上传文件到vps了！
特别注意：这里是新手问题高发区！config.php里面的数据库配置如果出错，可能会出现内部500错误，无法登录账号（因为没连上/连错数据库），网站白屏等错误问题！

注意！改名字这一步一定要做，否则会导致只有主页能打开，其他页面都是空白！

## 防止搜索引擎收录网站

这里说一下防收录的一种手段，因为SS站比较敏感，为了大家的SS站不被墙发现并封掉，推荐大家做一些防收录措施！

`robots.txt`文件，是屏蔽搜索引擎蜘蛛爬取网站！

```
Disallow: /user
Disallow: /lib
Disallow: /admin
Disallow: /vendor
 
User-agent: Baiduspider
Disallow: /
User-agent: Sosospider
Disallow: /
User-agent: sogou spider
Disallow: /
User-agent: YodaoBot
Disallow: /
```

这里只是把百度、搜狗、有道、SOSO屏蔽了。

一个终极方法，如果你用的是Nginx，以lnmp为例，那就在/usr/local/nginx/conf/vhost文件夹中找到你的域名配置文件，比如：www.baidu.com.conf

然后在里面插入以下代码：

```
if ($http_user_agent ~* (baiduspider|googlebot|soso|bing|sogou|yahoo|sohu-search|yodao|YoudaoBot|robozilla|msnbot|MJ12bot|NHN|Twiceler)) {
return 403;
}
```

然后保存并上传替换，然后ssh链接vps输入lnmp nginx restart（如果提示出错请根据错误修改！）这时候去站长工具测试一下效果，看是不是返回403代码。

nginx配置效果如下

[![](http://www.wunail.com/wp-content/uploads/2018/01/201801180344377.png)](http://www.wunail.com/wp-content/uploads/2018/01/201801180344377.png)

不过我试了试发现没什么用  {懵逼}

## 管理

默认情况下，user表中uid为1的用户为管理员

添加管理员可以在 ‘ss_user_admin’ 表中添加用户UID

默认管理帐号: first@blood.com 密码 1993

当使用新的加密方式带salt的sha256加密，由于每个站点的$salt值都不同，所以初始密码「1993」是没有用的，安装完成后，访问

你的域名`/pwd.php?pwd=1993`

将获得的字符串更新到数据库user表的pass字段。

如果正常安装完毕后只有主页能打开，其他页面都是空白，那就是没有修改config-simple.php文件名为config.php！

$salt 不可随意修改！

由于sspanel面板的前后端用的都是一个用户  所以直接用管理员账户

在`域名/admin`登陆

后台操作教程[逗逼根据地](https://doub.io/ss-jc15/)
