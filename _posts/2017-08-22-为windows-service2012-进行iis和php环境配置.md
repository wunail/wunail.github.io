---
layout: post
title: "为windows service2012 进行iis和php环境配置"
date: 2017-08-22
category: 未分类
---

适用对象：Windows Server 2012 R2, Windows Server 2012

**1.1. 安装 IIS**

你可以使用 Web 平台安装程序 (Web PI) 安装 IIS 和在 IIS 上运行的应用程序。 Web PI 安装了最新版本的可用 Web 平台产品，因此只需单击几下即可。 使用 Web PI，可以下载并安装任何新的工具或更新，包括 PHP。 若要了解有关 Web PI 的详细信息，请参阅了解详细信息并安装 Web PI。

        1.如果不使用 Web PI 安装 IIS，则可以手动安装 IIS。 要手动安装 IIS，请使用以下步骤：

          2.在 Windows Server 2012 上安装 IIS

          在“开始”页面上，单击“服务器管理器”磁贴，然后单击“确定”。

在“服务器管理器”中，选择“仪表板”，然后单击“添加角色和功能”。

在“添加角色和功能向导”中的“开始之前”页面上，单击“下一步”。

在“选择安装类型”页上，选择“基于角色或功能的安装”，然后单击“下一步”。

在“选择目标服务器” 页上，选择“从服务器池中选择服务器”，选择你的服务器，然后单击“下一步”。

在“选择服务器角色” 页上，选择“Web 服务器 (IIS)”，然后单击“下一步”。

在“选择功能”页上，注意默认情况下安装的预先选择的功能，然后选择“CGI”。 选择此选项还会安装 FastCGI，建议用于 PHP 应用程序。

单击“下一步”。

在“Web 服务器角色 (IIS)”页面上，单击“下一步”。

在“选择角色服务” 页上，注意默认情况下安装的预先选择的角色服务，然后单击“下一步”。
**鸿鹄博客 , 版权所有丨如未注明 , 均为原创丨本网站采用[BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/3.0/)协议进行授权 , 转载请注明[为windows service2012 进行iis和php环境配置](http://www.wunail.com/archives/361)！		      

			[喜欢 (0)](javascript:;)赏[aa@qq.com]![](http://www.wunail.com/wp-content/uploads/2016/08/zfb.jpg)**分享 (0)[](#)[](#)[](#)[](#)[](#)[](#)[](#)
