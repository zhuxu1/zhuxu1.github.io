---
layout: post
title: "Android wrapcontent随笔"
subtitle: "wrapcontent"
date: 2016-03-28
author: fixxz
category: essay
tags: essay
finished: true
---
`<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:id="@+id/layout_parent"  
    android:layout_width="match_parent"  
    android:layout_height="wrap_content"  
    android:background="@android:color/holo_green_light"  
    android:orientation="horizontal" >  
  
    <ImageView  
        android:id="@+id/img1"  
        android:layout_width="64dp"  
        android:layout_height="64dp"  
        android:src="@drawable/ic_launcher" />  
  
    <ImageView  
        android:id="@+id/img2"  
        android:layout_width="164dp"  
        android:layout_height="164dp"  
        android:background="@android:color/holo_orange_light"  
        android:src="@drawable/ic_launcher" />  
  
    <LinearLayout  
        android:id="@+id/layout1"  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:background="@android:color/holo_purple"  
        android:orientation="vertical" >  
    </LinearLayout>  
</LinearLayout>`


如上  先上结论 id为 layout_parent 的父控件里的wrap_content 是根据子控件的height 来计算的，计算的优先级是如果子控件中设置了固定高度，则计算 "固定高度"，如 64dp 。计算固定高度时会根据最大的高度来计算父控件的高度。如果子控件中没有固定高度，则根据子控件的高度来计算父控件的高度。

如上边的代码，父控件中的height 为 wrap_content，即根据内容设置高度。而它的子控件中img1,img2都为 固定数值 ，即有明确数字，则根据较大的数值，即img2的164dp得到父控件(layout_parent)的高度为164dp。  子控件layout1的高度为match_parent 即填充父类，则layout1的高度也为164dp
<br>计算父控件大小的优先级：(父控件设置为wrap_content的前提下)
<u>子控件设置固定大小的情况下：则根据 “设置最大固定大小的子控件的大小” 来计算父控件的大小，不计算match_parent,fill_parent;
子控件未设置固定大小的情况下：则根据 子控件的大小计算父控件的大小，如子控件为match_parnet，则父控件大大小即为填充屏幕</u>

随笔