---
layout: post
title: "Android ScrollView内容改变后自动滚动"
subtitle: "scrollview 自动滚动"
date: 2016-05-05
author: fixxz
category: bug
tags: bug
finished: true
---
`android:focusable="true"    
android:focusableInTouchMode="true" `

因为ScrollView只能有一个子view  
所以只需要在ScrollView的子View下加入上面两行即可
<br>
或者</br>
重写scrollview中的如下方法，并将其返回值设为0即可。
`@Override  
 protected int computeScrollDeltaToGetChildRectOnScreen(Rect rect) {  
    
  return 0;  
 }  `
