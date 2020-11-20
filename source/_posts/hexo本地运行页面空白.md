---
title: hexo本地运行页面空白
abbrlink: 6303
date: 2018-12-23 18:10:17
tags:
---





# hexo本地测试运行重启后页面空白,提示 : WARN No layout: index.html?



最近捣鼓hexo+github，刚写好一篇博客，准备在本地运行看看效果的时候，发现“localhost:4000” 出现的页面是空白，不知什么原因，

后来找根目录的_config.yml 看到配置的主题的名字。去themes文件夹中找，发现对应主题的文件夹是空的，后来从网上重新clone了主题，

运行成功。

所以，其他的小伙伴如果遇到类似的问题后，可以查看主题是_config.yml中设置的主题是否存在