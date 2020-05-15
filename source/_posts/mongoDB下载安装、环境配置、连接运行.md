---
title: mongoDB下载安装、环境配置、连接运行
abbrlink: 37507
date: 2019-03-10 11:30:08
tags:
---







初时MongoDB，一些安装配置方法，怕日后忘记，现总结如下：



MongoDB是一个基于分布式文件存储的数据库，由c++语言编写，旨在为WEB应用提供可扩展到的高性能数据存储解决方案。MongoDB是一个介于关系型数据库和非关系型数据库之间的产品，是非关系型数据库中功能最丰富，最像关系数据库的。

 <!--more-->

### 一、下载和安装

 1：windows的 64 位系统的预编译二进制包安装下载
https://www.mongodb.com/download-center/community 

2：下载之后点击安装

 ![img](/mongoDB下载安装、环境配置、连接运行/安装一) 

3：可以选择custom进行自定义位置安装

 ![img](/mongoDB下载安装、环境配置、连接运行/安装2) 

 ![img](/mongoDB下载安装、环境配置、连接运行/安装3) 

4：记住自己的安装路径

```
C:\Program Files\MongoDB\Server\4.0\
```

 ![img](/mongoDB下载安装、环境配置、连接运行/安装4) 

5：安装mongodb

 ![img](/mongoDB下载安装、环境配置、连接运行/安装5) 

 6：点击下一步，安装完成
现在让我们创建一个 data 的目录然后在 data 目录里创建 db 目录。 

### 二、MongoDB运行数据库和日志的配置

1:打开cmd(右键管理员身份打开)，进入安装路径底下，新建一个data文件夹

```
mkdir c:\ data\db
mkdir c:\ data\log
```

 于是在c盘底下简历了数据库和日志 

 ![img](/mongoDB下载安装、环境配置、连接运行/安装6) 

 ![img](/mongoDB下载安装、环境配置、连接运行/安装7) 

 ![img](/mongoDB下载安装、环境配置、连接运行/安装8) 

 2：运行：返回上一级，找到安装路径 

 ![img](/mongoDB下载安装、环境配置、连接运行/9) 

 3：从 MongoDB 目录的 bin 目录中执行 mongod.exe 文件。
通过 window 的资源管理器中找到一开始安装的路径 

 ![img](/mongoDB下载安装、环境配置、连接运行/10) 

 4：通过cmd进入这个路径、 

```
C:\Program Files\MongoDB\Server\4.0\bin
```

 ![img](/mongoDB下载安装、环境配置、连接运行/11) 

```css
mongod.exe –dbpath c:\data\db
```

5：成功界面
服务器搭建完毕，成功提示，注意data的文件位置是`c:\data\db`




 ![img](/mongoDB下载安装、环境配置、连接运行/12) 

 ![img](/mongoDB下载安装、环境配置、连接运行/13) 

#### 开始连接连接MongoDB

1：之前的窗口不变
再打开一个cmd窗口(右键以管理员身份)来运行mongo.exe。
同样打开bin文件，执行mongo.exe

```bash
cd\
cd Program Files\MongoDB\Server\4.0\bin
```

 ![img](/mongoDB下载安装、环境配置、连接运行/14) 

2：输入连接命令



```undefined
mongo
```

 ![img](/mongoDB下载安装、环境配置、连接运行/15) 

 我们的连接链接：
connecting to: mongodb://127.0.0.1:27017
来到浏览器测试一下 

 ![img](/mongoDB下载安装、环境配置、连接运行/16) 

 到这一步数据库已经成功跑起来了，接下来就是操作一些命令向数据库里面插入数据等并且可以看到自己对数据库的一系列操作的结果了。 

 ![img](/mongoDB下载安装、环境配置、连接运行/17) 

3：OK
完全安装并可以运行MongoDB了
我们可以看到创建的数据库文件夹里面自动生成的文件





这边把数据库单独抽出来的步骤有可能会出现错误，这边当然也可以不用抽出来，直接在里面就好了，不用创建data文件夹，因为你下载的那些里面会默认有这些，mongo连接成功就ok

