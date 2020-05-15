---
title: 使用hexo和git搭建个人博客
abbrlink: 11351
date: 2017-03-01 11:48:58
tags:
---







## 一、准备工作

### 1、了解Hexo

[Hexo](https://github.com/hexojs/hexo)是高效的静态站点生成框架，她基于Node.js。 通过 Hexo 你可以使用 Markdown 编写文章

### 2、搭建node.js环境

<!--more-->

搭建博客网站首先需要安装Node.js环境。

下载地址：http://nodejs.cn/download

测试安装：命令行使用node -v 、npm -v，查看显示版本号即成功。

Ps:大部分同学装的node版本是之前提供的5.0.0多的 hexo的初始化步骤中会出现报错，需要重新装最新的版本（10.0.0多的）

### 3、安装Hexo博客框架工具

Hexo是一个建站工具，可以帮助我们快速生成基本的博客文件，安装它需要在

控制台下（cmd）使用如下命令：

`npm install hexo`-cli-g

### 4、安装git版本工具

Git是目前世界上最流行的分布式版本控制系统

使用Git可以帮助我们把本地的网页和文章等内容提交到Github上，实现同步。

下载地址：https://git-scm.com/downloads

测试安装:右击鼠标，如果有git bash等两个选项 即安装成功。

### 5、注册github账号

gitHub是一个面向开源及私有软件项目的托管平台，因为只支持git 作为唯一的版本库格式 进行托管，故名gitHub。这里用到Github，是因为我们需要通过Github得到自己的博客网站域名，而且需要使用gitHub同步我们个人博客的相关文件。

注册地址：[https://github.com](https://github.com/)

## 二、开始搭建博客

### 1、开启github服务

通过Github Pages获得一个免费使用的域名，这需要我们在Github上新建一个仓库，如下：/n

![](/使用hexo和git搭建个人博客/1.png)



新仓库的名字，最好是是UserName+“github.io”的形式。这也是之前强调的要起一个好的用户名的原因。这样之后我们最后的博客网站的链接就会是：[https://UserName.github.io的形式。](https://username.github.xn--io-7j3dxl300h./)

注意：固定新仓库的名字格式并非必须，只是这样操作生成的博客域名比较短小简洁，另起他名生成博客域名会很冗长

点击Create Repository之后，随后选择Setting进入设置，找到Github Pages如下：

![](/使用hexo和git搭建个人博客/2.png)

这里我们需要点击Choose a theme任意选择一个选择主题，然后界面会跳转到仓库，我们看到有两个文件如下：

![](/使用hexo和git搭建个人博客/3.png)

再查看setting，我们会看到开启GitHub Pages之后得到的域名如下：

![](/使用hexo和git搭建个人博客/4.png)

现在，可以使用[https://UserName.github.io](https://dreamcoffeezs.github.io/)，访问自己的博客网站了，打开链接我们会看到默认主题的个人博客样式如下(虽然点丑)：

![](/使用hexo和git搭建个人博客/5.png)

### 2、创建本地博客站点：

上述的步骤相当于我们使用Github，创建了一个默认的博客页，并且得到了一个可外部访问的域名。但是这个博客页很丑。我们的目的是创建自己个性化的博客网站，所以我们使用Hexo在本地先创建一个本地博客站点，优化后再把它部署到github上。接下来我们使用控制台(cmd)命令在本地一个合适的位置创建博客站点文件夹如下：

**hexo init myHexoBlog** **//myHexoBlog\****是项目名**

![](/使用hexo和git搭建个人博客/6.png)

测试本地博客站点，在本地博客根目录(git bash)下使用控制台命令：

hexo g *//g**是generetor的缩写，生成博客*

hexo s *//s**是server的缩写，启动服务*

此时打开浏览器，输入 http://localhost:4000/，我们将会看到Hexo自带默认主题显示的博客样式如下:

![](/使用hexo和git搭建个人博客/7.png)

### 3.同步Github,允许公共访问

初次安装git需要配置用户名和邮箱，否则git会提示：please tell me who you are.

![](/使用hexo和git搭建个人博客/9.png)

你需要运行命令来配置你的用户名和邮箱：

$ git config –global user.name “superGG1990”

$ git config –global user.email “[superGG1990@163.com](mailto:superGG1990@163.com)“

**注意：（引号内请输入你自己设置的名字，和你自己的邮箱）**此用户名和邮箱是git提交代码时用来显示你身份和联系方式的，并不是github用户名和邮箱

### **git配置SSH Key**

1、 打开git bash.exe

2、检查是否已经有SSH Key `$ cd ~/.ssh`

```
3、生成SSH Key  $  ssh-keygen -t rsa -C "youremail"
```

第一次生成的话，直接一路回车，不需要输入密码。不是第一次生成的话，会提示 overwrite (y/n)? 问你是否覆盖旧的 SSH Key ，直接填 y ，然后一直回车就行了，最后得到了两个文件：id_rsa和id_rsa.pub。

![](/使用hexo和git搭建个人博客/10.png)

4、记事本打开/C/Users/Administrator/.ssh/下id_rsa.pub文件，复制该段信息；登录github账户，点击头像进入Settings -> SSH and GPG keys -> New SSH key，将复制的信息粘贴到该处。

![](/使用hexo和git搭建个人博客/12.png)

5、测试是否成功`$ssh -T git@github.com`

提示“Hi xxx! You’ve successfully authenticated, but GitHub does not provide shell access.”说明添加成功。

在本地我们已经搭建了博客，但是还只能自己本地访问。若要别人也能看到，那就需要我们将其同步部署到GitHub上了。首先找到我们的博客仓库，并拷贝仓库地址：
![](/使用hexo和git搭建个人博客/13.png)

然后修改本地博客目录的配置：
修改本次博客根目录下的_config.yml文件，修改deploy下的配置如下：

![](/使用hexo和git搭建个人博客/14.0.png)

hexo d *//**部署到github*

再次访问链接：

[https://userName.github.io](https://dreamcoffeezs.github.io/)

，就会发现这里的界面和本地的一样了。如此一来我们搭建的个人博客网站就基本完成了。

## 三、发布博客

可以发布自己的第一篇博客了。来尝试一下以下的步骤：
在本地博客文件夹根目录(git bash)输入：

hexo new “我个人博客的第一篇博客”

hexo g *//**生成网页*

hexo d *//**部署到远端(github)*

![](/使用hexo和git搭建个人博客/14.png)

现在打开我们的博客网站：[http://UserName.github.io](http://username.github.io/),会看到网页

（显示可能有延迟 所以可以采用以下方法(git bash)：

hexo clean *//**清理缓存*

hexo g *//**重新生成博客代码*

hexo d *//**部署到本地*

*稍等即可*

*//////////////////////////////**顺便一提* *hexo s* *启动本地服务器，用于预览*

）

## 四、更换主题

为了让它看起来更美观一些，我们可以为其更换主题（当然也可以自己在默认主题下自己编写美化博客界面）。这里以使用github上的next主题为例：

#### 1.创建next文件夹

切换到本地博客根目录下，在主题文件thems下创建一个新文件夹next存放即将下载的next主题(git bash)

$ git clone https://github.com/iissnan/hexo-theme-next themes/next

*//**下载主题*

下载成之后我们会看到next的主题已经存在thems里了如下：



![](/使用hexo和git搭建个人博客/15.png)

#### 2.修改博客配置文件，更换主题配置

修改博客根目录(不是next主题)下的_config.yml文件，搜索theme字段，并将其值修改为next

![](/使用hexo和git搭建个人博客/16.png)

然后在控制台（git bash）下输入如下命令：

hexo clean *//**清理缓存*

hexo g *//**重新生成博客代码*

hexo d *//**部署到本地*

再次打开我们的博客网站[https://UserName.github.io](https://dreamcoffeezs.github.io/)，将会看到更换主题后

![](/使用hexo和git搭建个人博客/17.png)

更多主题美化：

https://blog.csdn.net/qq_32454537/article/details/79482896



## 五、注意

==这边最好把本地的源码保存一份到，github上，防止，以后换电脑时，继续更新博客==

