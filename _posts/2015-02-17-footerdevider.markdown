---
layout: post
title: "Android footerDeviersEnabled设置无效"
subtitle: "bug"
date: 2015-02-17
author: fixxz
category: bug
tags: bug
finished: true
---

ListView中每个Item项之间都有分割线，设置Android:footerDividersEnabled表示是否显示分割线，此属性默认为true。
设置android:footerDividersEnabled运行没有效果
只需要将listView的 height  改成match_parent 即可
