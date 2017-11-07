title: 'Github Pages配合Hexo3搭建个人网站'
date: 2015-8-31 14:06:37
tags: [Github Pages, Hexo]

---

这是一篇关于使用Github Pages和Hexo3搭建网站并绑定独立域名的个人总结，目录顺序为个人实际操作顺序。

<!--more-->

## 目录

- [1. 购买域名](#1)
- [2. 环境安装](#2)
- [3. 配置和使用Github](#3)
	+ [3.1 配置SSH keys](#3.1)
	+ [3.2 添加SSH keys到Github](#3.2)
	+ [3.3 设置用户信息](#3.3)
- [4. 使用Github Pages](#4)
- [5. 使用Hexo](#5)
	+ [5.1 安装](#5.1)
	+ [5.2 配置与部署](#5.2)
	+ [5.3 主题克隆](#5.3)
- [6. 绑定域名](#6)
	+ [6.1 Github上的设置](#6.1)
	+ [6.2 DNS设置](#6.2)
- [7. 其他](#7)
	+ [7.1 新建文章及页面](#7.1)
	+ [7.2 配置主题](#7.2)
	+ [7.3 评论](#7.3)
	+ [7.4 RSS订阅](#7.4)
	+ [7.5 Sitemap网站地图](#7.5)
	+ [7.6 404页面](#7.6)
	+ [7.7 添加小图标](#7.7)
	+ [7.8 robots.txt](#7.8)
	+ [7.9 图床](#7.9)
- [8. 参考资料](#8)

<h2 id="1">1. 购买域名</h2>

域名并非为必选项。此前从网上收集后的信息表明，大家都推荐到godaddy上购买，并且支持支付宝。但是如果不喜欢麻烦或者国外域名注册商的话，那就在万网购买吧。以后只是为了安安静静的更新个个人网站的话，域名在哪里购买都差不多。

我选择了万网，就拿万网来说吧。首先查询自己期望的域名是否可用，个人倾向选择.com，.cn，.net等结尾的后缀，待加入清单结算并填写相关的个人信息后就已经购买成功了。在填写个人信息的时候不必太过纠结，后续都是可以更改并且可以选择隐私保护的。购买之后的.com，.net之类的国际域名不需要再实名认证是可以使用的，不需要担心所提示的红色警告。

<h2 id="2">2. 环境安装</h2>

首先下载所需软件并安装。

* <a href="//nodejs.org" target="_blank">Node.js</a>
* <a href="//git-scm.com" target="_blank">Git</a>

<h2 id="3">3. 配置和使用Github</h2>

在Github注册完账号后，注意用户名（可作为登陆账号）和昵称的区别，另外这里的邮箱很重要。

<h3 id="3.1">3.1 配置SSH keys</h3>

这里需要用到刚才下载好的Git，打开Git Bash。这里不得不提一下，在Git中有一个和Linux中相同的原则，如果输入一串指令而没有任何提示，是表明成功的预兆，不要怀疑没有反应或者失败了。

首先检查SSH key。

```
$ cd ~/.ssh  //检查本机是否有SSH key,弹出No such file or directory说明是第一次使用。
```

生成新的SSH key，注意大小写和空格。

```
$ ssh-keygen -t rsa -C "邮箱地址"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):  //回车
Created directory '/c/Users/your_user_directory/.ssh'.
```

然后设置密码并确认，这里的密码用来以后提交项目使用，输入时无*号提示，如果为空，则以后提交项目时无需验证。

```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

之后看到以下图案就成功啦。

![](//7xlivs.com1.z0.glb.clouddn.com/2015/08/31/build-Github-Pages-with-Hexo3/SSH-key-for-github.jpg)

<h3 id="3.2">3.2 添加SSH keys到Github</h3>

在本机设置SSH key成功后，会在`\users\your_user_directory\.ssh`目录下生成`id_rsa`和`id_rsa.pub`两个文件。

1. 打开`ssh.pub`文件并拷贝其中的内容。
2. 打开Github登陆后找到*Settings*中的*SSH keys*，选择*Add SSH key*，输入*Title*，并粘贴所复制的内容到*Key*中，确认即可。

最后来测试一下。

```
$ ssh -T git@github.com
```

会得到如下反馈：

![](//7xlivs.com1.z0.glb.clouddn.com/2015/08/31/build-Github-Pages-with-Hexo3/SSH-key-for-github-test.jpg)

输入yes后弹出如下代码即代表成功。

```
Hi 账户名! You've successfully authenticated, but GitHub does not provide shell access.
```

<h3 id="3.3">3.3 设置用户信息</h3>

现在本机已经可以通过SSH链接到Github了接下来完善一下个人信息。

```
$ git config --global user.name "用户名"
$ git config --global user.email  "邮箱地址"
```

Github会通过这些信息来做权限处理。


以上所有步骤可能遇到的错误：

<a href="//help.github.com/articles/generating-ssh-keys" target="_blank">GitHub Help - Generating SSH Keys</a>

<a href="//help.github.com/articles/error-permission-denied-publickey" target="_blank">GitHub Help - Error Permission denied (publickey)</a>

<h2 id="4">4. 使用Github Pages</h2>

基础的环境和配置都设置完后，接下来就是利用Github的Github Pages这个功能来搭建自己的静态页面。

首先登陆Github并且新建一个Repository，项目名称要输入“**username.github.io**”这样的形式，其中username替换为自己的用户名即可，接下来一步步往下填写相关信息即可创建完成。到此个人的Github Pages就创建完成额，每个用户只能创建一个这个项目。

此刻，已经可以自己编辑网站的样式了，只要在master分支下拥有index.html文件即可通过申请的域名进行访问了。为了方便和快捷，接下来使用一些框架来进行搭建。

<h2 id="5">5. 使用Hexo</h2>

<a href="https://github.com/tommy351/hexo" target="_blank">Hexo</a>是一款便捷的博客发布框架。正如作者描述的一样，很给力。

>A fast, simple & powerful blog framework, powered by Node.js.

<h3 id="5.1">5.1 安装</h3>

在Git中输入指令。

```
$ npm install -g hexo-cli
```

如果在这里卡住没有反应，可以尝试把官方源替换为<a href="https://npm.taobao.org/" target="_blanK">淘宝npm源</a>后再安装Hexo，注意安装<a href="https://npm.taobao.org/" target="_blanK">淘宝npm源</a>后，与`npm`有关的指令要替换为`cnpm`操作才有效。

```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

在Hexo3中，包括hexo-server等一些模块已经独立了出来，需要先安装。可以到`node_modules`目录下对照着安装：

```
$ npm install hexo-generator-index --save
$ npm install hexo-generator-archive --save
$ npm install hexo-generator-category --save
$ npm install hexo-generator-tag --save
$ npm install hexo-server --save
$ npm install hexo-deployer-git --save
$ npm install hexo-renderer-marked@0.2 --save
$ npm install hexo-renderer-stylus@0.2 --save
$ npm install hexo-generator-feed@1 --save
$ npm install hexo-generator-sitemap@1 --save
```

<h3 id="5.2">5.2 配置与部署</h3>

安装完成后运行如下指令。`<folder>`的格式为`/e/name`，随便选择一个位置即可。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

之后生成静态文档，并开启本地server调试。

```
$ hexo generate
$ hexo server
```

此时在浏览器中打开“local:4000”可以再本地看到生成的页面，回到git中操作“ctrl + c”即可关闭server。

<h3 id="5.3">5.3 主题克隆</h3>

因为默认的主题实在是有点丑，于是还是换上其他的<a href="https://github.com/hexojs/hexo/wiki/Themes" target="_blank">Hexo主题</a>来美化一下自己即将搭建起来的网站吧。这里我使用了<a href="https://github.com/litten/hexo-theme-yilia" target="_blank">Yilia</a>的主题，按照作者的方法进行安装和配置。

* 安装

```
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

* 使用

修改目录`hexo`下的`_config.yml`文件，将其中的`theme`项设置为`yilia`。

* 更新

```
$ cd themes/yilia
$ git pull
```

* 部署

部署到Github前需要配置目录`hexo`下的`_config.yml`文件。

```
$ deploy:
$ type: git
$ repository:    //Github上自己username.github.io项目下的地址
$ branch: master
```

然后就可以部署了。

```
$ hexo clean
$ hexo generate
$ hexo deploy
```

注意，如果出现错误，请先检查对应文件夹`hexo\node_modules`中的模块是否完全。

<h2 id="6">6. 绑定域名</h2>

上述所有步骤完成之后，已经可以发布文章，并且可以访问*username.github.io*了。现在来设置前面已经准备好的个人域名。

<h3 id="6.1">6.1 Github上的设置</h3>

在这里设置其实很简单，只要在Repository的根目录下面，新建一个名为CNAME的文本文件，里面写入你要绑定的域名地址就可以了。

但是为了防止以后通过Git部署到Github上的时候把CNAME给覆盖掉，建议在目录`hexo`下的文件夹`source`中直接新建一个CNAME的纯文本文件。

<h3 id="6.2">6.2 DNS设置</h3>

这里使用<a href="https://www.dnspod.cn/" target="_blank">DNSPod</a>来解析。注册登录后，添加域名并如下设置。

![](//7xlivs.com1.z0.glb.clouddn.com/2015/08/31/build-Github-Pages-with-Hexo3/DNSPod-settings.jpg)

其中A的两条记录指向的IP地址是Github Pages提供的IP，如有改动，在<a href="" target="">Github Pages</a>查看最新IP。其中www指定的记录是个人的Github上的Repository。

做完这一步后，登录万网，找到自己购买的域名并更改DNS服务器，对应上图设置中的两条NS记录。

这时已经可以成功访问到自己的域名了。

<h2 id="7">7. 其他</h2>

以下内容是进阶篇，用于改造主题并加入一些新功能，暂时用到的只有这么多。

<h3 id="7.1">7.1 新建文章页面</h3>

#### 新建文章

Hexo使用Markdown语法的纯文本存放文章，后缀为`.md`，可以手动在`hexo\source\_post`文件夹下新建UTF-8编码的文档后来编写文章。也可以输入一下指令来自动生成。

```
$ hexo new post <title>
```

新建文章的内部格式如下。

```
title: title #文章标题
date: 2015-08-31 00:00:00 #文章生成时间
categories: #文章分类目录，可以省略
tags: #文章标签，可以省略。
description: #本页描述，可以省略
---   
这里开始使用markdown格式输入你的正文。
```

多标签语法格式。

```
tags:
- 标签1
- 标签2
- 标签3
- ...
```

正文中设置文章在网站首页的摘要而不全部显示。

```
以上是文章摘要
<!--more-->
以下是余下全文
```

#### 自定义页面

以*关于*页面为例。

```
$ hexo new page "about"
```

编辑`source\about\index.md`。

```
title: About
date: 2015-08-31 00:00:00
---
About Me
```

在主题的配置文件中添加入口。

```
menu:
  About: /about
```

<h3 id="7.2">7.2 配置主题</h3>

关于Hexo的结构及一些指令如下。

#### 目录结构

```
.
├── .deploy       #待部署文件
├── node_modules  #插件
├── public        #生成的静态网页文件
├── scaffolds     #工具模板
├── source        #博客正文和其他源文件，404、favicon、CNAME等都放在这里
|   ├── _drafts   #草稿
|   └── _posts    #文章
├── themes        #主题
├── _config.yml   #全局配置文件
└── package.json
```

#### 全局配置 _config.yml

```
# Hexo Configuration
## Docs: //hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title:   #网站标题
subtitle:   #网站副标题
description:   #网站描述
author: John Doe  #作者
email:   #邮箱地址
language:   #语言，简体中文：zh-CN
timezone:

# URL
## If your site is put in a subdirectory, set url as '//yoursite.com/child' and root as '/child/'
url: //yoursite.com   #网站地址
root: /	 #网站根目录
permalink: :year/:month/:day/:title/  #网站url地址结构
permalink_defaults:

# Directory
source_dir: source  #源文件目录
public_dir: public	#生成网页的文件目录
tag_dir: tags  #标签目录
archive_dir: archives  #存档目录
category_dir: categories  #分类目录
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md  #新文章标题
default_layout: post  #默认的模板，包括 post、page、photo、draft（文章、页面、照片、草稿）
titlecase: false  #标题转换成大写
external_link: true  #在新选项卡打开链接
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:  #语法高亮
  enable: true  #是否启用
  line_number: true  #显示行号
  auto_detect: true  #自动对齐
  tab_replace:

# Category & Tag
default_category: uncategorized  #默认分类
category_map:
tag_map:

# Archives
2: 开启分页
1: 禁用分页
0: 全部禁用
archive: 2
category: 2
tag: 2

# Server #本地服务器
port: 4000  #端口号
server_ip: localhost  #IP地址
logger: false
logger_format: dev

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## //momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10  #每页文章数，设置成 0 禁用分页#
pagination_dir: page


# Disqus  #disqus评论，多说在主题中配置
disqus_shortname:

# Extensions  #扩展插件
## Plugins: //hexo.io/plugins/
## Themes: //hexo.io/themes/
theme: landscape  #主题
exclude_generator:

plugins:   #插件，例如生成 RSS 和站点地图的
- hexo-generator-feed
- hexo-generator-sitemap

# Deployment  #部署
## Docs: //hexo.io/docs/deployment.html
deploy:
  type: git
  repository:   #自己Github的项目地址
  branch: master
```

#### 命令行

* 常用命令

```
$ hexo help  //查看帮助
$ hexo init  //初始化一个目录
$ hexo new "postName"  //新建文章
$ hexo new page "pageName"  //新建页面
$ hexo generate  //生成网页，可以在 public 目录查看整个网站的文件
$ hexo server  //本地预览，'Ctrl+C'关闭
$ hexo deploy  //部署.deploy目录
$ hexo clean  //清除缓存，**强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹**
```

* 复合命令

```
$ hexo deploy -g
$ hexo server -g
```

* 简写

```
$ hexo n == hexo new
$ hexo g == hexo generate
$ hexo s == hexo server
$ hexo d == hexo deploy
```

* 安装插件

```
$ npm install <plugin-name> --save
$ npm update #升级
$ npm uninstall <plugin-name>  #卸载
```

* 安装主题

```
$ git clone <repository> themes/<theme-name>
```

#### 主题优化

可以从Hexo的<a href="https://github.com/hexojs/hexo/wiki/Themes" target="_blank">主题列表</a>中找到自己喜欢的主题。

下载主题并修改全局配置文件`_config.yml`中*theme*一行应用主题。

```
$ git clone <repository> themes/<theme-name>
```

也可以手动下载后解压到`themes`目录中。

* 主题目录结构

```
.
├── languages          #国际化
|   ├── default.yml    #默认
|   └── zh-CN.yml      #中文
├── layout             #布局
|   ├── _partial       #局部的布局
|   └── _widget        #小挂件的布局
├── script             #js脚本
├── source             #源代码文件
|   ├── css            #CSS
|   |   ├── _base      #基础CSS
|   |   ├── _partial   #局部CSS
|   |   ├── fonts      #字体
|   |   ├── images     #图片
|   |   └── style.styl #style.css
|   ├── fancybox       #fancybox
|   └── js             #js
├── _config.yml        #主题配置文件
└── README.md          #主题介绍
```

* <a href="https://github.com/litten/hexo-theme-yilia" target="_blank">Yilia</a>的主题配置文件

```
# Header
menu:
  主页: /
  所有文章: /archives
  # 随笔: /tags/随笔

# SubNav
subnav:
  github: "#"
  weibo: "#"
  rss: "#"  #域名地址/atom.xml
  zhihu: "#"
  #douban: "#"
  #mail: "#"  #mailto:邮箱地址
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml

# Content
excerpt_link: more
fancybox: true
mathjax: true

# Miscellaneous
google_analytics: ''
favicon: /favicon.png

#你的头像url
avatar: ""
#是否开启分享
share: true
#是否开启多说评论，填写你在多说申请的项目名称 duoshuo: duoshuo-key
#若使用disqus，请在博客config文件中填写disqus_shortname，并关闭多说评论
duoshuo: true  #这里使用多说直接改为多说的项目名称即可
#是否开启云标签
tagcloud: true

#是否开启友情链接
#不开启——
#friends: false
#开启——
friends:
  奥巴马的博客: //localhost:4000/
  卡卡的美丽传说: //localhost:4000/
  本泽马的博客: //localhost:4000/
  吉格斯的博客: //localhost:4000/
  习大大大不同: //localhost:4000/
  托蒂的博客: //localhost:4000/

#是否开启“关于我”。
#不开启——
#aboutme: false
#开启——
aboutme: 我是谁，我从哪里来，我到哪里去？我就是我，是颜色不一样的吃货…
```

<h3 id="7.3">7.3 评论</h3>

这里使用的是<a href="//duoshuo.com/" target="_blank">多说</a>的评论，登陆后在*后台管理*找到*工具*可以看到一些代码，在代码中找到**short_name:"XXX"**。然后打开主题文件下的`_config.yml`，替换以下代码即可。

```
duoshuo: true
==>
duoshuo: XXX
```

<h3 id="7.4">7.4 RSS订阅</h3>

安装插件。

```
$ npm install hexo-generator-feed
```

并在全局配置文件中开启插件。

```
plugins:
- hexo-generator-feed
```

在主题配置文件中添加入口。

```
rss: /atom.xml
```

开启server可以本地测试*//localhost:4000/atom.xml*是否生效。

<h3 id="7.5">7.5 Sitemap网站地图</h3>

安装插件。

```
$ npm install hexo-generator-sitemap
```

开启插件。

```
plugins:
- hexo-generator-sitemap
```

浏览*//localhost:4000/sitemap.xml*查看是否生效。

<h3 id="7.6">7.6 404页面</h3>

添加`source\404.html`文件。

404页面不需要Hexo解析。

```
layout: false
--------
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>404</title>
    <link rel="icon" href="/favicon.ico">
  </head>
  <body>
    <div align="center">
      <p>Page 404</p>
    </div>
  </body>
</html>
```

<h3 id="7.7">7.7 添加小图标</h3>

将`favicon.ico`文件放在`source`目录下,配置主题里的全局设置文件。

```
favicon: /favicon.ico
```

<h3 id="7.8">7.8 robots.txt</h3>

`source`目录下添加`robots.txt`。

```
User-agent: Baiduspider
Disallow:
User-agent: Googlebot
Disallow:
User-agent: *
Disallow: /
```

<h3 id="7.9">7.9 图床</h3>

网站中不免会使用到一些图片，这里推荐使用<a href="https://portal.qiniu.com/signup?code=3ln23ux9ydhzm" target="_blank">七牛</a>的来进行存储。

<h2 id="8">8. 参考资料</h2>

1. <a href="//cnfeat.com/blog/2014/05/10/how-to-build-a-blog/" target="_blank">如何搭建一个独立博客——简明Github Pages与Hexo教程</a>
2. <a href="//segmentfault.com/a/1190000002538363" target="_blank">Hexo静态博客使用指南</a>
3. <a href="//forsweet.github.io/hexo/%E7%94%A8Hexo%E6%90%AD%E5%BB%BAGithub%E5%8D%9A%E5%AE%A2/" target="_blank">用Hexo 3 搭建github blog</a>
4. <a href="//blog.lmintlcx.com/post/blog-with-hexo.html" target="_blank">使用Hexo搭建博客 </a>
5. <a href="//segmentfault.com/a/1190000002398039" target="_blank">更换博客系统——从jekyll到hexo</a>
6. <a href="//ibruce.info/2013/11/22/hexo-your-blog/" target="_blank">hexo你的博客</a>
7. <a href="//segmentfault.com/a/1190000002632530" target="_blank">hexo常用命令笔记</a>
