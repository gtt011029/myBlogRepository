---
title: hexo 更换电脑迁移博客
abbrlink: 25530
date: 2018-12-15 18:28:10
tags:
---









# hexo 更换电脑 ---- 迁移博客

hexo是一个快速、简洁、高效的博客框架。hexo可以使用markdown解析文章，操作简单方便，在几秒钟就可以利用靓丽的主题生成静态网页。

但是，其有一个比较致命的缺点，就是其仓库是在本地的，github上存储的是编译后的代码，所以只有在当前的电脑上才可以编写博客，但是如果作者想要跟换电脑或者之前的电脑已坏、本地代码丢失等，想要更新博客就是一件比较麻烦的事。

<!--more-->

解决方案：

把本地的源码也存一份在github上，最好保持随该随存。

当你跟换了电脑，在新电脑上拉，之前存在github上的源码

git clone XXX

在本地新拷贝的文件夹下通过Git bash依次执行下列指令：

npm install hexo

npm install

npm install hexo-deployer-git

（记得，不需要hexo init这条指令）。



此时直接执行hexo g 、hexo s 本地localhost:4000 就可以看见你的博客啦，然后hexo d推到远端就好了。

记得：跟换了新电脑后也要继续把本地仓库推到github哦



