---
layout: post
title: "手把手教你用 GitHub Pages 创建个人博客 —— 零基础完整教程"
date: 2026-05-29
category: 技术教程
tags: [GitHub, 博客, 教程, Jekyll, GitHub Pages]
description: "从零开始，手把手教你如何在 GitHub 上免费搭建个人博客，包含详细步骤、代码示例和常见问题解答。"
---

# 手把手教你用 GitHub Pages 创建个人博客

## 📋 目录

1. [什么是 GitHub Pages](#一什么是-github-pages)
2. [准备工作](#二准备工作)
3. [创建 GitHub 账号](#三创建-github-账号)
4. [创建博客仓库](#四创建博客仓库)
5. [配置 GitHub Pages](#五配置-github-pages)
6. [安装和配置 Jekyll](#六安装和配置-jekyll)
7. [使用 Jekyll 主题](#七使用-jekyll-主题)
8. [自定义博客配置](#八自定义博客配置)
9. [编写第一篇文章](#九编写第一篇文章)
10. [本地预览博客](#十本地预览博客)
11. [推送到 GitHub](#十一推送到-github)
12. [自定义域名（可选）](#十二自定义域名可选)
13. [进阶技巧](#十三进阶技巧)
14. [常见问题解答](#十四常见问题解答)

---

## 一、什么是 GitHub Pages

### 1.1 简介

**GitHub Pages** 是 GitHub 提供的免费静态网站托管服务。你可以直接从 GitHub 仓库中托管网站，支持：

- **完全免费** —— 不需要购买服务器和域名
- **自动部署** —— 推送代码后自动更新网站
- **自定义域名** —— 可以绑定自己的域名
- **HTTPS 支持** —— 自动启用 SSL 证书
- **Jekyll 集成** —— 原生支持 Jekyll 静态网站生成器

### 1.2 适用场景

- ✅ 个人博客
- ✅ 项目文档
- ✅ 技术笔记
- ✅ 作品集展示
- ✅ 教程分享

### 1.3 限制

- 仓库大小建议不超过 1GB
- 网站大小不超过 1GB
- 每月带宽限制 100GB
- 每小时最多构建 10 次

---

## 二、准备工作

### 2.1 检查工具

在开始之前，确保你的电脑已安装以下工具：

#### 检查 Git 是否安装

```bash
# Windows（打开命令提示符或 PowerShell）
git --version

# macOS（打开终端）
git --version

# Linux（打开终端）
git --version
```

如果显示版本号（如 `git version 2.43.0`），说明已安装。否则：

- **Windows**: 下载 https://git-scm.com/download/win
- **macOS**: 下载 https://git-scm.com/download/mac
- **Linux**: `sudo apt install git`（Ubuntu/Debian）或 `sudo yum install git`（CentOS）

#### 检查 Node.js 是否安装（可选，用于本地预览）

```bash
node --version
npm --version
```

如果需要本地预览，建议安装 Node.js：https://nodejs.org/

### 2.2 注册 GitHub 账号

如果你还没有 GitHub 账号：

1. 访问 https://github.com/
2. 点击右上角 **Sign up**
3. 填写邮箱、密码、用户名
4. 完成验证
5. 验证邮箱

**用户名建议**：
- 使用英文（如 `zhangsan`）
- 简短好记
- 避免特殊字符

---

## 三、创建 GitHub 账号

### 3.1 登录 GitHub

1. 访问 https://github.com/
2. 点击右上角 **Sign in**
3. 输入用户名和密码
4. 点击 **Sign in**

### 3.2 创建 Personal Access Token（重要）

GitHub 已经不支持密码认证，需要使用 Personal Access Token (PAT)：

1. 访问 https://github.com/settings/tokens
2. 点击 **Generate new token** → **Generate new token (classic)**
3. 配置令牌：
   - **Note**: 填写 `blog-token`（或其他名称）
   - **Expiration**: 选择 `90 days`（或自定义）
   - **Select scopes**: 勾选以下权限：
     - ✅ `repo`（完全仓库访问权限）
     - ✅ `workflow`（GitHub Actions 权限）
     - ✅ `read:org`（读取组织信息，可选）
4. 点击 **Generate token**
5. **立即复制** 令牌（`ghp_` 开头，只显示一次！）

⚠️ **重要**：令牌只显示一次，请立即保存到安全的地方！

---

## 四、创建博客仓库

### 4.1 仓库命名规则

GitHub Pages 博客有两种仓库命名方式：

#### 方式一：用户站点（推荐）

```
<用户名>.github.io
```

例如：用户名是 `zhangsan`，则仓库名为 `zhangsan.github.io`

访问地址：`https://zhangsan.github.io`

#### 方式二：项目站点

```
<任意名称>
```

访问地址：`https://<用户名>.github.io/<仓库名>/`

### 4.2 创建仓库

1. 登录 GitHub
2. 点击右上角 **+** → **New repository**
3. 填写信息：
   - **Repository name**: `你的用户名.github.io`
   - **Description**: `我的个人博客`（可选）
   - **Visibility**: 选择 **Public**（必须公开才能启用 GitHub Pages）
   - ✅ 勾选 **Add a README file**
4. 点击 **Create repository**

### 4.3 配置 Git（首次使用）

打开终端（Windows 打开 Git Bash），执行以下命令：

```bash
# 设置用户名（与 GitHub 用户名一致）
git config --global user.name "你的用户名"

# 设置邮箱（与 GitHub 注册邮箱一致）
git config --global user.email "你的邮箱@example.com"

# 设置凭证存储
git config --global credential.helper store
```

### 4.4 克隆仓库到本地

```bash
# 克隆仓库
git clone https://github.com/你的用户名/你的用户名.github.io.git

# 进入仓库目录
cd 你的用户名.github.io
```

输入用户名和令牌（粘贴时不会显示，这是正常的）：

```
Username: 你的用户名
Password: 粘贴你的令牌（ghp_xxx）
```

---

## 五、配置 GitHub Pages

### 5.1 启用 GitHub Pages

1. 进入你的仓库页面
2. 点击 **Settings**（设置）
3. 左侧菜单找到 **Pages**
4. 配置 Source：
   - **Source**: 选择 **Deploy from a branch**
   - **Branch**: 选择 `main`，目录选择 `/ (root)`
5. 点击 **Save**

### 5.2 配置构建和部署

在 **GitHub Pages** 设置页面：

- **Build and deployment**:
  - **Source**: GitHub Actions
  - 或者选择 **Legacy**（使用 Jekyll 自动构建）

### 5.3 等待构建

- 配置后，GitHub 会自动构建你的网站
- 等待 1-5 分钟
- 构建完成后，页面顶部会显示绿色提示和访问链接

---

## 六、安装和配置 Jekyll

### 6.1 什么是 Jekyll

**Jekyll** 是一个静态网站生成器，GitHub Pages 原生支持。你只需编写 Markdown 文件，Jekyll 会自动转换为 HTML 网页。

### 6.2 安装 Jekyll（本地预览用）

如果你需要在本地预览博客，需要安装 Jekyll：

#### Windows

```bash
# 安装 Ruby（Jekyll 依赖）
# 下载 https://rubyinstaller.org/，选择带 DevKit 的版本

# 安装 Jekyll
gem install jekyll bundler
```

#### macOS

```bash
# 安装 Ruby（macOS 自带，但建议更新）
brew install ruby

# 安装 Jekyll
gem install jekyll bundler
```

#### Linux（Ubuntu/Debian）

```bash
# 安装依赖
sudo apt install ruby-full build-essential zlib1g-dev

# 安装 Jekyll
gem install jekyll bundler
```

### 6.3 创建 Jekyll 项目（如果仓库是空的）

```bash
# 在仓库目录执行
jekyll new .

# 这会创建以下文件：
# _config.yml - Jekyll 配置文件
# _posts/     - 文章目录
# index.md    - 首页
# about.md    - 关于页面
```

---

## 七、使用 Jekyll 主题

### 7.1 选择主题

GitHub Pages 支持多种 Jekyll 主题，推荐几个常用的：

| 主题名称 | 特点 | 适合场景 |
|---------|------|---------|
| **minima** | 简洁、官方默认 | 个人博客 |
| **cayman** | 美观、响应式 | 项目文档 |
| **minimal** | 极简、快速 | 技术笔记 |
| **midnight** | 暗色主题 | 代码分享 |
| **slate** | 灰色主题 | 专业展示 |

### 7.2 在 GitHub 上选择主题

1. 进入仓库 **Settings** → **Pages**
2. 点击 **Choose a theme**
3. 浏览并选择喜欢的主题
4. 点击 **Select theme**

### 7.3 手动配置主题

编辑 `_config.yml` 文件：

```yaml
# 使用 minima 主题
theme: minima

# 或者使用远程主题
# remote_theme: pages-themes/cayman@v0.2.0
# plugins:
#   - jekyll-remote-theme
```

### 7.4 安装主题（本地预览）

```bash
# 编辑 Gemfile
# 添加主题
gem "minima", "~> 2.5"

# 安装依赖
bundle install
```

---

## 八、自定义博客配置

### 8.1 配置文件 `_config.yml`

这是 Jekyll 的核心配置文件，完整示例：

```yaml
# 站点信息
title: "我的博客"
description: "一个热爱技术的程序员的个人博客"
author: "张三"

# 站点 URL（部署后修改为实际地址）
url: "https://zhangsan.github.io"
baseurl: ""

# 构建设置
markdown: kramdown
highlighter: rouge
permalink: /:title/

# 主题配置
theme: minima

# 插件
plugins:
  - jekyll-feed
  - jekyll-seo-tag

# 分页设置
paginate: 10
paginate_path: "/page:num/"

# 社交链接（可选）
social:
  github: zhangsan
  twitter: zhangsan
```

### 8.2 配置文件详解

#### 基础配置

```yaml
# 站点标题（显示在浏览器标签和页面顶部）
title: "我的博客"

# 站点描述（用于 SEO）
description: "分享技术、记录生活"

# 作者名称
author: "张三"

# 站点语言
lang: "zh-CN"
```

#### URL 配置

```yaml
# 完整 URL（部署后修改）
url: "https://zhangsan.github.io"

# 如果是项目站点，需要设置 baseurl
# baseurl: "/my-blog"
```

#### 构建配置

```yaml
# Markdown 渲染器
markdown: kramdown

# 代码高亮
highlighter: rouge

# 文章 URL 格式
permalink: /:year/:month/:day/:title/

# 或者简洁格式
# permalink: /:title/
```

### 8.3 自定义页面

#### 创建关于页面

创建 `about.md` 文件：

```markdown
---
layout: page
title: 关于我
permalink: /about/
---

# 关于我

你好！我是一个热爱技术的程序员。

## 技能

- Python
- JavaScript
- Linux

## 联系方式

- Email: zhangsan@example.com
- GitHub: [github.com/zhangsan](https://github.com/zhangsan)
```

#### 创建归档页面

创建 `archive.md` 文件：

```markdown
---
layout: page
title: 文章归档
permalink: /archive/
---

{% for post in site.posts %}
- {{ post.date | date: "%Y-%m-%d" }} - [{{ post.title }}]({{ post.url }})
{% endfor %}
```

---

## 九、编写第一篇文章

### 9.1 文章文件格式

文章存放在 `_posts/` 目录下，文件名格式：

```
YYYY-MM-DD-title.md
```

例如：`2026-05-29-hello-world.md`

### 9.2 文章结构

```markdown
---
layout: post
title: "文章标题"
date: 2026-05-29
category: 分类
tags: [标签1, 标签2]
description: "文章摘要，显示在首页列表中"
---

这里是文章正文内容...

## 二级标题

### 三级标题

**加粗文字**

*斜体文字*

`代码`

[链接文字](https://example.com)

![图片描述](图片URL)

- 列表项 1
- 列表项 2

1. 有序列表项 1
2. 有序列表项 2

> 引用文字

```python
# 代码块
def hello():
    print("Hello, World!")
```

| 表头1 | 表头2 |
|-------|-------|
| 内容1 | 内容2 |
```

### 9.3 文章 Front Matter

文章开头的 `---` 之间是 Front Matter，用于配置文章属性：

```yaml
---
# 布局模板
layout: post

# 文章标题（必填）
title: "我的第一篇文章"

# 发布日期（必填）
date: 2026-05-29

# 分类（可选）
category: 技术

# 标签（可选）
tags: [GitHub, 博客, 教程]

# 摘要（可选，不填则自动截取前 200 字）
description: "这是一篇关于创建博客的教程"

# 是否发布（可选，默认 true）
published: true
---
```

### 9.4 Markdown 语法速查

#### 标题

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

#### 文本格式

```markdown
**加粗文字**
*斜体文字*
~~删除线~~
`行内代码`
```

#### 列表

```markdown
- 无序列表项 1
- 无序列表项 2
  - 嵌套列表项

1. 有序列表项 1
2. 有序列表项 2
```

#### 链接和图片

```markdown
[链接文字](https://example.com)
[带标题的链接](https://example.com "链接标题")

![图片描述](https://example.com/image.jpg)
![带标题的图片](image.jpg "图片标题")
```

#### 引用

```markdown
> 这是引用文字
> 
> 可以多段
```

#### 代码块

````markdown
```python
def hello():
    print("Hello!")
```
````

#### 表格

```markdown
| 列1 | 列2 | 列3 |
|-----|-----|-----|
| 内容1 | 内容2 | 内容3 |
| 内容4 | 内容5 | 内容6 |
```

#### 分割线

```markdown
---
```

---

## 十、本地预览博客

### 10.1 使用 Jekyll 本地预览

```bash
# 进入仓库目录
cd 你的用户名.github.io

# 启动本地服务器
bundle exec jekyll serve

# 或者
jekyll serve
```

访问 http://localhost:4000 预览博客

### 10.2 自动刷新

Jekyll 服务器支持自动刷新，修改文件后会自动重新构建。

### 10.3 常用命令

```bash
# 启动服务器（生产模式）
bundle exec jekyll serve --port 8080

# 启动服务器（包含草稿）
bundle exec jekyll serve --drafts

# 构建静态文件（不启动服务器）
bundle exec jekyll build
```

### 10.4 不想安装 Jekyll？

如果不想在本地安装 Jekyll，可以：

1. 直接编辑 Markdown 文件
2. 推送到 GitHub
3. 等待 GitHub 自动构建

---

## 十一、推送到 GitHub

### 11.1 添加文件到 Git

```bash
# 添加所有文件
git add .

# 或者添加指定文件
git add index.md
git add _posts/
```

### 11.2 提交更改

```bash
# 提交（需要引号包裹提交信息）
git commit -m "第一篇文章"
```

### 11.3 推送到 GitHub

```bash
# 推送到远程仓库
git push origin main
```

如果提示输入用户名和密码，输入：

```
Username: 你的用户名
Password: 粘贴你的令牌
```

### 11.4 验证部署

1. 推送后，访问仓库页面
2. 点击 **Actions** 查看构建状态
3. 等待绿色 ✓ 表示构建成功
4. 访问 `https://你的用户名.github.io` 查看博客

---

## 十二、自定义域名（可选）

### 12.1 购买域名

推荐的域名注册商：

- [阿里云](https://wanwang.aliyun.com/)
- [腾讯云](https://dnspod.cloud.tencent.com/)
- [Namesilo](https://www.namesilo.com/)（便宜）

### 12.2 配置 DNS

以阿里云为例：

1. 登录阿里云控制台
2. 进入 **域名解析**
3. 添加两条记录：

| 记录类型 | 主机记录 | 记录值 |
|---------|---------|--------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | 你的用户名.github.io |

### 12.3 在 GitHub 配置域名

1. 进入仓库 **Settings** → **Pages**
2. 在 **Custom domain** 输入你的域名
3. 点击 **Save**
4. 勾选 **Enforce HTTPS**（推荐）

### 12.4 验证域名

```bash
# 检查 DNS 解析
dig 你的域名

# 或者
nslookup 你的域名
```

---

## 十三、进阶技巧

### 13.1 使用 GitHub Actions 自动部署

创建 `.github/workflows/jekyll.yml` 文件：

```yaml
name: Deploy Jekyll site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4

      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 13.2 添加评论系统

#### 使用 Giscus（推荐）

1. 访问 https://giscus.app/
2. 按照指引配置
3. 在文章页面底部添加评论代码

```html
<script src="https://giscus.app/client.js"
        data-repo="你的用户名/你的用户名.github.io"
        data-repo-id="..."
        data-category="Announcements"
        data-category-id="..."
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="top"
        data-theme="preferred_color_scheme"
        data-lang="zh-CN"
        crossorigin="anonymous"
        async>
</script>
```

### 13.3 添加统计访问量

#### 使用不蒜子

在页面底部添加：

```html
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>

<span id="busuanzi_value_site_pv"></span> 次访问
<span id="busuanzi_value_site_uv"></span> 位访客
```

### 13.4 SEO 优化

在 `_config.yml` 添加：

```yaml
# SEO 设置
title: "我的博客"
description: "一个分享技术的个人博客"
url: "https://zhangsan.github.io"

# 社交媒体
social:
  github: zhangsan
```

在页面 `<head>` 中添加：

```html
<meta name="description" content="{{ page.description | default: site.description }}">
<meta property="og:title" content="{{ page.title }}">
<meta property="og:description" content="{{ page.description }}">
<meta property="og:url" content="{{ page.url | absolute_url }}">
<meta property="og:image" content="{{ page.image | default: '/assets/images/default.png' }}">
```

---

## 十四、常见问题解答

### Q1: 推送后网站没有更新？

**A**:
1. 检查 **Actions** 页面，查看构建是否成功
2. 等待 5-10 分钟
3. 清除浏览器缓存（Ctrl + F5）
4. 检查 `_config.yml` 配置是否正确

### Q2: 出现 404 错误？

**A**:
1. 检查仓库名是否正确（必须是 `<用户名>.github.io`）
2. 检查分支设置是否正确（main 分支，/root 目录）
3. 确认 GitHub Pages 已启用

### Q3: 本地预览正常，线上不正常？

**A**:
1. 检查 `url` 配置是否正确
2. 检查 `baseurl` 是否为空（用户站点）
3. 检查文件路径是否正确（区分大小写）

### Q4: 如何修改主题颜色？

**A**:
创建 `_sass/custom.scss` 文件：

```scss
// 自定义颜色
$brand-color: #E34141;
$text-color: #333;
$background-color: #fff;

// 覆盖主题变量
$link-color: $brand-color;
$headings-color: $brand-color;
```

### Q5: 如何添加图片？

**A**:

```markdown
# 方式一：使用相对路径
![图片描述](/assets/images/photo.jpg)

# 方式二：使用绝对 URL
![图片描述](https://example.com/photo.jpg)
```

图片存放在 `assets/images/` 目录。

### Q6: 如何使用代码高亮？

**A**:

````markdown
```python
def hello():
    print("Hello!")
```
````

支持的语言：python, javascript, html, css, ruby, go, java, c, cpp 等。

### Q7: 如何设置中文？

**A**:

在 `_config.yml` 添加：

```yaml
lang: "zh-CN"
```

在 HTML 模板中：

```html
<html lang="zh-CN">
```

---

## 🎉 总结

恭喜你！你已经成功创建了自己的 GitHub Pages 博客。回顾一下我们做了什么：

1. ✅ 注册了 GitHub 账号
2. ✅ 创建了博客仓库
3. ✅ 配置了 GitHub Pages
4. ✅ 安装了 Jekyll 主题
5. ✅ 编写了第一篇文章
6. ✅ 部署了博客

### 下一步

- 继续写更多文章
- 自定义博客样式
- 添加更多功能（评论、统计、搜索）
- 绑定自定义域名

### 参考资源

- [GitHub Pages 官方文档](https://docs.github.com/en/pages)
- [Jekyll 官方文档](https://jekyllrb.com/docs/)
- [Markdown 语法](https://www.markdownguide.org/)
- [GitHub Pages 主题](https://pages.github.com/themes/)

---

**最后更新**: 2026-05-29

如果这篇文章对你有帮助，欢迎点赞 ⭐ 收藏 📁 分享给更多人！
