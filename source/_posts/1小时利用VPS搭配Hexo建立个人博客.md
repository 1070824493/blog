---
title: 1小时利用VPS搭配Hexo建立个人博客
date: 2016-09-24 11:21:59
categories: 折腾
---
有了VPS，除了可以满足科学上网的需求，最简单实用的就算弄个小博客了，可以供学习和总结。

本文也是我这个小白自个爬帖总结出来的，适应范围也是同等级的小白。先把效果放在前面：[个人博客地址](http://tangyi.ml)
#### 一、准备条件
- VPS
> 这总得有一个吧，最低配置都行。实在没有的话可以换个组合，网上很多Hexo或者WordPress搭配Github啥的建立博客的，随便一搜都是一大堆，这里就不赘述了。
- 域名
> 这个也是硬性基本要求，不多说。免费的，收费的都行。

#### 二、环境要求
介绍下我的一些安装环境，其他环境没试过，就不打包票了：
- VPS：搬瓦工VPS，要求Centos6操作系统。（其他VPS没试过，无关广告）
- Mac：这个无影响，终端方便而已。
- 域名：国外ML免费的域名，同类型还有tk,gq等等结尾的域名。冷门域名一般免费一年。地址：[域名注册入口](http://www.freenom.com)

#### 三、安装一（VPS部分）
- **1. 登录VPS**
- **2. 更新一次系统**
```
yum update -y
```
- **3. 安装软件**
```
yum -y install wget;
wget http://download.kanglesoft.com/easypanel/ep.sh -O ep.sh;
sh ep.sh
```

<!--more-->


> 解释一下上面三行命令：
第一行是安装wget，用于文件下载，已安装的可以忽略。
第二行是从给定地址下载软件，输出保存为ep.sh。安装的软件是kangle，web服务器和反向代理服务器。
第三行就是执行了。
然后就是静等上面走完。

- **4. 修改mysql的密码**
```
mysqladmin -u root password 你的密码
```
>数据库目前没用上，评论或者其他高级功能可能会用得上。先把密码改了备份，防以后不时之需。

- **5. 登录后台管理面板**
```
http://VPS地址:3312/admin/
```
>第一次登录的用户名为admin,密码kangle,登录成功后可在面板里自行修改密码。

- **6. 初始化web服务器**
![配置web服务器](http://upload-images.jianshu.io/upload_images/1723686-0eaccfb22b7762bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![初始化web服务器](http://upload-images.jianshu.io/upload_images/1723686-26491ac684bbf767.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>配置下数据库，其他全部默认，第一次初始化服务器三个选项全选，以后再有初始化需求的时候默认会勾选后两个，如上图所示。

- **7. 建立网站**
![新建网站需要的设置](http://upload-images.jianshu.io/upload_images/1723686-f4a4ab89b16c84f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>网站名和密码用于登录FTP，名字随意。最好能区分你的网站，因为你可以同理建立你的多个子站。

- **8. 登录网站**
```
http://VPS地址:3312/vhost
```
> 当然这也可以从后台管理面板中网站管理→所有网站进入。帐号密码即第7步设置的帐号密码。

- **9. 绑定域名**
![域名绑定](http://upload-images.jianshu.io/upload_images/1723686-f96c976d1930c717.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> 接下来就是最简单的域名解析了，先进入你购买的域名对应管理页面，一般都有管理dns的，添加A记录到你的VPS地址。然后再回来把你域名添加上，如上图所示。

- **10. VPS配置到此为止，剩下的就是将你的静态网页放入wwwroot文件夹。静态网页在接下来的Hexo部分将会生成在你的电脑本地，至于VPS文件和你电脑端网页同步的问题，因为服务器具备FTP功能，可以使用FTP管理工具，我这里推荐Yummy FTP，当然你也可以使用控制面板中的“在线文件”功能在线上传，如果你不觉得每次更新网站都得全部上传覆盖的话。**

#### 四、安装二（本地Hexo部分）
- **1.安装Node.js**，去[Node.js官网](https://nodejs.org)下载对应系统版本。
- **2.安装Git**，安装过Xcode的可以跳过，[Git下载地址](https://git-scm.com/downloads)。
- **3.安装Hexo**
```
sudo npm install -g hexo
```
- **4.生成静态网页**。在你要存储静态网页的路径下创建文件夹，打开终端cd到这个文件夹，执行如下命令，生成初始静态网页。
```
hexo init
```
执行完效果应该下图所示：
![初始效果图](http://upload-images.jianshu.io/upload_images/1723686-0c87181cf6a4d844.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> 常用文件夹说明：
> ①_config.yml：站点配置文件，里面配置你网站名字，网站标题，作者信息，语言，主题等信息。具体如下：
Site               ##修改以适应搜索引擎的收录
title:       ##定义网站的标题
subtitle:            ##定义网站的副标题（不一定会显示）
description:         ##定义网站的描述
author:            ##定义网站的负责人
email:               ##定义网站负责人的电子邮件
language:            ##定义网站的语言，默认zh-Hans
url:                   ##定义访问的域名
root:  				##定义所在Web文件夹的哪一个目录
permalink: :year/:month/:day/:title/      ##定义时间格式
source_dir: source           ##定义从哪个文件夹获取博客资料              
public_dir: public           ##定义生成静态网站到哪个文件夹
> ②source：存放需要发布的文章。默认生成的文件为md格式，支持markdown语法。
> ③themes：里面默认有一个主题，丑得很一般。

- **5.常用命令**
```
hexo new 名字 //新建文章，会在source/_posts下生成一个.md文件，不可直接复制生成新文件，否则部署生成会报错。
hexo new page 页面名字 //生成新的页面，比如标签页默认没有，会显示404。
hexo deploy 或 hexo d //部署到localhost，用于本地调试预览。
hexo server 或 hexo s //启动本地预览服务，地址为http://localhost:4000/
hexo generate 或 hexo g //会在你init的文件夹下生成public文件夹，里面包含全套静态网页代码。这里面的全部文件（不包括public文件夹）放入你服务器的wwwroot文件夹里就可以通过域名访问了。
hexo clean //清空缓存，删除配置数据库及public文件夹。
```
- **6.更换主题**
```
cd themes
git clone 需要下载的主题地址
```
[Hexo官方主题地址](https://github.com/hexojs/hexo/wiki/Themes)在这里，我使用的是next主题，简单好看又容易弄。
下载完后修改根目录站点配置文件_config.yml里面的theme: 为主题文件夹的名字来启用主题。注意冒号后有一个空格，注意和themes对应主题文件夹下的_config.yml主题配置文件进行区分~

- **7.自定义主题**，修改成自己想要的样子。每个主题都有相关的配置文档，比如[Next使用文档](http://theme-next.iissnan.com/getting-started.html)，这里特别详细，我也不赘述了。

- **8.更新文章**，只需将public文件夹文件全部同步到VPS的wwwroot文件夹就行了，我用的FTP工具，方法比较笨，没办法，懒。附上我的FTP连接图。登录帐号密码即VPS部分第7步的帐号密码。
![FTP连接图](http://upload-images.jianshu.io/upload_images/1723686-2f68eae2286df49a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>**开始折腾吧~**