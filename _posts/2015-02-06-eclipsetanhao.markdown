---
layout: post
title: "Eclipse新建工程后的红色叹号"
subtitle: "修复方法"
date: 2015-02-06
author: fixxz
category: bug
tags: bug
finished: true
---

今个碰到一个小问题，new Java project后工程上出现一红色叹号。

分享一下解决方案。

 

1.工程右键  →Build Path  → Configure Build Path

2.在打开的Properties for SortDemo窗口中，点击Libraries找到出问题的jar包

3.发现dns_sd.jar上有一红色叉号  后面的括号里提示(missed)..想必不知道是什么问题导致dns_sd.jar丢失了

4.在http://www.java2s.com/Code/Jar/d/Downloaddnssdjar.htm  这个网站里搜索dns_sd.jar 并下载，将jar文件放到你的JAVA路径下,我的路径是D:\Program Files\Java\jre8\lib\ext

5.返回Eclipse，刷新工程    ok