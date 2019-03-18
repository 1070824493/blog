---
title: 1小时利用VPS搭建SVN
date: 2016-10-27 09:10:18
categories: 折腾
---
SVN的搭建可以用独立主机安装，也可以apache为基础安装，以便于web访问，基于懒人的原则，本文介绍的是基于独立主机安装的。


#### 一、用途
项目开发中，总会碰到需要回退版本或者打包阶段性版本的情况，公司的项目还好，可以利用公司的SVN或者git中进行管理，但如果是自己的项目，除了挂在github上进行版本管理，最方便的就是利用手头的VPS主机搭建一个私人的SVN了。

#### 二、安装环境
下面是我的一些安装环境，其他环境没试过，基本通用。

- VPS：Centos6 64位操作系统。
- Mac：这个无影响，终端方便而已。

#### 三、具体步骤
- **1、终端登录VPS**

- **2、安装SVN**

```
yum install subversion
```

<!--more-->

- **3、创建SVN版本库**

随便找个自己习惯的路径建立版本库，本文使用usr/local路径。
在usr/local文件夹下新建svn文件夹

```
mkdir svn 
```
建立工程文件夹，project随意取名。

```
svnadmin create /usr/local/svn/project
```

- **4、修改svnserve.conf文件**

打开下面的三行注释。

```
anon-access = read
auth-access = write
password-db = passwd
```
*注意：默认无用户名认证登录的也会有只读权限，建议将anon-access 设为 none，禁止访问。*

- **5、修改authz文件**

```
[groups]
group1 = test1，test2，test3 
[/foo/bar]
test1 = rw
test2 = r
* =
```
创建一个名为group1的组，并指定三个用户test1，test2，test3。test1可读可写，test2仅读，test3和其他用户无任何权限。结合里面原有的注释应该很好理解。

- **6、修改passwd文件**

创建或修改用户密码。

```
[users]
test1 = 123456
test2 = 123456
test3 = 123456
```

- **7、启动SVN服务**

```
svnserve -d -r /usr/local/svn
```
要设置开机启动的话就编辑/etc/rc.local文件，将上条命令添加进去就OK了。

- **8、访问SVN**

懒人的话使用SVN客户端连接，比如Cornerstone啥的，附上一张Cornerstone连接图，SVN默认端口为3690。
![连接SVN配置图](http://upload-images.jianshu.io/upload_images/1723686-33312aa7a0604570.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 四、总结
整个流程较为简单，因为受限于不同VPS访问速度以及硬盘大小，首次上传项目时长可能和公司独立主机要稍长，但不影响代码同步操作。实测大约200M的项目首次import大概5分钟，当然这个会因VPS和本地带宽而异。
> Have Fun~