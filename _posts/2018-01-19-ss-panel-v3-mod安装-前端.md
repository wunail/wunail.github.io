---
layout: post
title: "ss-panel-v3-mod安装 （前端）"
date: 2018-01-19
category: 未分类
---

## 前言

前一篇文章[Shadowsocks多用户管理面板——SS-Panel教程](http://www.wunail.com/archives/639)因为功能单一所以相对好弄些。只需要

> 安装Web环境（LNMP/LAMP） → 创建数据库并导入SQL文件 → 修改SSPanel的config.php文件并全部上传到网站根目录 → 做一些防收录措施（可选）。

比较简单

但想要更方便些就要用`ss-panel-v3-mod`相对会好点。

本教程用的是 使用赵大的魔改版(new_master分支) 

不过此人因为被人人肉已经退坑了。说句题为话：任何人都搞不爬中国人，除了中国人自己。国人就是喜欢窝里斗！！！

演示环境 : VirMach 1GRAM机子 debian8

## 1.安装 LNMP

```
screen -S lnmp
wget http://soft.vpser.net/lnmp/lnmp1.4-full.tar.gz && tar xvzf lnmp1.4-full.tar.gz
cd lnmp1.4-full && ./install.sh
```

安装过程要求输入MySQL密码, 选择MySQL版本>=5.5, PHP版本>7.0.

安装大概需要半小时, 如果中途 ssh 断线, 输入 `screen -r lnmp`

## 2.设置虚拟主机

```
lnmp vhost add
```

要求输入你的域名, 然后其余项默认即可, SSL 开启

接着修改下`nginx`

编辑 /usr/local/nginx/conf/vhost/你的域名.conf

然后添加下面内容到 server

```
location / 
{
    try_files $uri $uri/ /index.php$is_args$args;		                
}

location ~ [^/]\.php(/|$)
{
    # comment try_files $uri =404; to enable pathinfo
    try_files $uri =404;
    fastcgi_pass  unix:/tmp/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
    #include pathinfo.conf;
}
```

修改 root 那一行为

```
root /home/wwwroot/你的域名/public;
```

## 3.下载 sspanel 代码

```
cd /home/wwwroot/你的域名
apt-get install git -y
git clone -b new_master https://github.com/glzjin/ss-panel-v3-mod.git tmp && mv tmp/.git . && rm -rf tmp && git reset --hard
#自己fork了一份
#git clone -b new_master https://github.com/wehaha/ss-panel-v3-mod.git tmp && mv tmp/.git . && rm -rf tmp && git reset --hard  
chown -R root:root *
chmod -R 755 *
chown -R www:www storage
chattr -i .user.ini
mv .user.ini public
cd public
chattr +i .user.ini
service nginx restart
```

LNMP1.4需要修改防跨目录的设置, 否则肯定报错

```
cd ~/lnmp1.4/tools/
./remove_open_basedir_restriction.sh 
# 然后输入 /home/wwwroot/你的域名/public
```

因为我并不打算用vpn 中间就跳过一部安装radius

## 5.配置数据库

浏览器打开 `http://你的vps ip/phpmyadmin`

用户 : root

密码 :安装 lnmp 时设置的

需要创建一个数据库和一个访问这个数据库的用户

点击 `账户` -> `新建` -> `添加用户`

- 登录信息 :

Username 选择 使用文本域 , 填写你的用户名 如 sspanel

Host 选择任意主机 %

密码 选择使用文本域 填写密码

- 用户数据库 :

勾选 创建与用户同名的数据库并授予所有权限

- 全局权限 :

全选

接着按执行 选择刚刚新建的数据库 sspanel 导入程序目录下的 `glzjin_all.sql`

下载sql文件夹下的`glzjin_all.sql`然后登陆`http://你的vps ip/phpmyadmin`导入自己刚才建立的那个数据库

## 6.配置 sspanel

```
cd /home/wwwroot/你的域名
php composer.phar install
cp config/.config.php.example config/.config.php
# 编辑以下文件 建议使用 FTP 下载到本地修改
nano config/.config.php
```

由于配置太多 这里只说重点

```
$System_Config['key'] = '';			//修改此key为随机字符串确保网站安全
$System_Config['appName'] = '';             //站点名称
$System_Config['baseUrl'] = 'https://zhaojin97.cn';            // 站点地址
$System_Config['timeZone'] = 'PRC';        // RPC 天朝时间  UTC 格林时间
$System_Config['pwdMethod'] = 'sha256';       // 密码加密   可选 md5,sha256
$System_Config['salt'] = '';               // 密码加密用，从旧版升级请留空
$System_Config['authDriver'] = 'cookie';   // 登录验证存储方式,推荐使用Redis   可选: cookie,redis

$System_Config['mailDriver'] = 'mailgun';   // 邮件 可选 mailgun or smtp 需要支持qq邮箱的选 smtp

$System_Config['checkinMin'] = '100';       // 签到最少流量 单位MB
$System_Config['checkinMax'] = '500';       // 签到最多流量
$System_Config['defaultTraffic'] = '100';      // 用户初始流量 单位GB
$System_Config['inviteNum'] = '0';			// 注册后获得的邀请码数量

# database 数据库配置
$System_Config['db_driver'] = 'mysql';
$System_Config['db_host'] = 'localhost';		// 数据库地址
$System_Config['db_database'] = '';				// 数据库名称 sspanel
$System_Config['db_username'] = '';				// 数据库用户 sspanel
$System_Config['db_password'] = '';				// sspanel用户的密码
$System_Config['db_charset'] = 'utf8';
$System_Config['db_collation'] = 'utf8_general_ci';
$System_Config['db_prefix'] = '';

# redis
$System_Config['redis_scheme'] = 'tcp';			// 登录验证存储方式选了 redis 的话需要配置
$System_Config['redis_host'] = '127.0.0.1';
$System_Config['redis_port'] = '6379';
$System_Config['redis_database'] = '0';
$System_Config['redis_password']="";

# smtp
$System_Config['smtp_host'] = ''; //邮箱服务商，如：smtp.qq.com、smtp.163.com
$System_Config['smtp_username'] = ''; //邮箱登陆账号（有的邮箱只需要填写 @之前的内容）
$System_Config['smtp_port'] = '25'; //邮箱服务器端口，根据各邮箱要求自行填写
$System_Config['smtp_name'] = ''; //自定义发信人名字
$System_Config['smtp_sender'] = ''; //发信邮箱
$System_Config['smtp_passsword'] = ''; //邮箱密码，一般不同于邮箱密码，在邮箱开启 smtp 服务后会提示
$System_Config['smtp_ssl'] = 'false'; //如果使用 ssl，请把 false 改成 true

#功能开关  需要用到的才开 建议先别动
$System_Config['enable_wecenter']='false';
$System_Config['enable_radius']='false';		// 配置了 radius 的话就开
$System_Config['enable_cloudxns']='false';
$System_Config['enable_duoshuo']='false';
$System_Config['enable_rss']='true';
$System_Config['enable_paymentwall']='false';

#Radius数据库设置
$System_Config['radius_db_host']='';		// 跟 上面 database 数据库配置差不多 换成radius即可
$System_Config['radius_db_database']='';
$System_Config['radius_db_user']='';
$System_Config['radius_db_password']='';

#Radius连接密钥
$System_Config['radius_secret']='';			// 这个重要 必须设

#端口池
$System_Config['min_port']='10000';			// SSR 分配端口号范围
$System_Config['max_port']='65535';

#两种方式相对于ss端口的偏移
$System_Config['pacp_offset']='-20000';		// PAC+ 和 PAC++ 用到
$System_Config['pacpp_offset']='-20000';

#测速周期/h
$System_Config['Speedtest_duration']='6';	// 对应后端 SSR 的 userapiconfig.py 里的 SPEEDTEST

#随机分组，注册时随机分配到的分组，多个分组请用英文半角逗号分隔。
$System_Config['ramdom_group']='0';			// 组别用于区分用户组 对应组只能访问对应组和0组的服务器 明白后再修改 

#充值返利百分比
$System_Config['code_payback']='20';		// 用户充值后 给邀请他注册的人返利多少%

#注册时的流量重置日以及需要重置的流量,0不重置
$System_Config['reg_auto_reset_day']='0';
$System_Config['reg_auto_reset_bandwidth']='100';	// 单位G
```

以上为 `config`部分配置 完成后保存并上传到原目录下

切换到 ssh 窗口, 在你的网站目录下执行以下命令创建管理员

php xcat createAdmin

按照提示, 输入管理员邮箱密码等信息, 然后执行以下命令同步用户

```
php xcat syncusers
```

如果输入错误的话可以按着`CTRL+back`删除

此时管理员创建完成

接下来需要对服务器进行计划任务的设置,执行 crontab -e 命令, 添加以下五段

```
30 22 * * * php /home/wwwroot/站点文件夹/xcat sendDiaryMail 
*/1 * * * * php /home/wwwroot/站点文件夹/xcat synclogin
*/1 * * * * php /home/wwwroot/站点文件夹/xcat syncvpn
0 0 * * * php /home/wwwroot/站点文件夹/xcat dailyjob
*/1 * * * * php /home/wwwroot/站点文件夹/xcat checkjob    
*/1 * * * * php /home/wwwroot/站点文件夹/xcat syncnas
```

重启Crontab

```
/etc/init.d/cron restart
```

## 7.注意事项

检查时间是否为天朝时间

如果VPS默认是非中国时区的话, 如下命令可以用来更改为中国时区

```
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

为了后端的安装所以需要检查防火墙是否屏蔽数据库端口

```
# 允许本机访问
iptables -A INPUT -s 127.0.0.1/32 -p tcp -m tcp --dport 3306 -j ACCEPT
# 允许节点访问
iptables -A INPUT -s 节点IP -p tcp -m tcp --dport 3306 -j ACCEPT
# 允许所有IP访问
iptables -A INPUT -p tcp -m tcp --dport 3306 -j ACCEPT
```

查看防火墙规则

```
查看已添加的iptables规则
iptables -L -n --line-numbers
删除已添加的iptables规则
iptables -D INPUT line-numbers
```

规则保存

```
# Ubuntu
iptables-save > /etc/iptables.rules
# CentOS
service iptables save
```

参考

[github](https://github.com/iMeiji/ss-panel-v3-mod/wiki/%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E-lnmp1.4)

[详细安装ss-panel-v3魔改版前端+后端教程](http://www.gblm.net/487.html)

[从master分支升级到new_master分支](https://github.com/iMeiji/ss-panel-v3-mod/wiki/%E4%BB%8Emaster%E5%88%86%E6%94%AF%E5%8D%87%E7%BA%A7%E5%88%B0new_master%E5%88%86%E6%94%AF)

一个新的面板  还没看 应该会不错吧  先收藏

[ssrpeanl](https://github.com/ssrpanel/SSRPanel)
**鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用[BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/)协议进行授权 , 转载请注明[ss-panel-v3-mod安装 （前端）](http://www.wunail.com/archives/645)！		      

			[喜欢 (2)](javascript:;)赏[aa@qq.com]![](http://www.wunail.com/wp-content/uploads/2016/08/zfb.jpg)**分享 (0)[](#)[](#)[](#)[](#)[](#)[](#)[](#)
