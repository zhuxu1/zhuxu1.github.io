---
layout: post
title: "ADB启动失败或者无法识别设备的问题"
subtitle: "bug"
date: 2015-02-13
author: fixxz
category: bug
tags: bug
finished: true
---

今天用Eclipse开发Android时，USB插入设备后电脑有响应，相关驱动也已经安装好，但是Eclipse无法识别到Nexus7
后来通过端口查看，发现ADB被占用
最后发现是"猎豹免费wifi"占用了
 
关闭“猎豹免费wifi”，并重启Eclipse，在Eclipse右上角选择DDMS，在Devices中查看是否发现设备
如未发现，点击Devices右上角倒立三角，选择Reset ADB
OK，问题解决
 
其它可能占用ADB端口的还有各种手机助手等
可从端口列表中自行查看

1.打开命令行
    {% highlight ruby %} -ano|findstr 端口 {% endhighlight %}
2.找到占用端口的<b>进程ID</b>(最后一组纯数字)
<br>
3.然后根据进程ID找到进程并结束掉
 {% highlight ruby %} tasklist|findstr "进程ID" {% endhighlight %}