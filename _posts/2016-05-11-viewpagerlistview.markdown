---
layout: post
title: "Android Viewpage与ListView焦点冲突问题"
subtitle: "Viewpage与ListView"
date: 2016-05-11
author: fixxz
category: bug
tags: bug
finished: true
---
{% highlight java %}
float start_x = 0, start_y = 0, end_x = 0, end_y = 0;   
viewPager.setOnTouchListener(new View.OnTouchListener() {  
  
            @Override  
            public boolean onTouch(View v, MotionEvent event) {  
                int action = event.getAction();  
                if (action == MotionEvent.ACTION_DOWN) {  
                    // toast("down");  
                    // Log.i("zhuxu", "down");  
                    start_x = event.getX();  
                    start_y = event.getY();  
                } else if (action == MotionEvent.ACTION_UP) {  
                    // toast("up");  
                    // Log.i("zhuxu", "up");  
                    end_x = event.getX();  
                    end_y = event.getY();  
                } else if (action == MotionEvent.ACTION_MOVE) {  
                    // Log.i("zhuxu", "move : " + "start_x : " + start_x  
                    // + "start_y : " + start_y + "end_x : " + end_x  
                    // + "end_y : " + end_y);  
                    // toast("move");  
                    // if (Math.abs(end_x - start_x) > 50 && Math.abs(end_y -  
                    // start_y) < 100) {  
                    if (Math.abs(end_x - start_x) > Math.abs(end_y - start_y)) {  
                        // Log.i("zhuxu", "横向移动！");  
                        //此句代码是为了通知他的父View 现在进行的是本控件的操作，不要对我的操作进行干扰  
                        v.getParent().requestDisallowInterceptTouchEvent(true);  
                        return false; // 是否需要View继续处理  
                    } else {  
                        //接近垂直滑动，交给父控件处理  
                        v.getParent().requestDisallowInterceptTouchEvent(false);  
                        // Log.i("zhuxu", "纵向移动！");  
                        return true;  // 是否需要View继续处理  
                    }  
                }  
                return false;  
            }  
        });  
{% endhighlight %}
viewPager的横向滑动焦点 与ListView的纵向滑动焦点会有冲突<br>
造成的问题是在viewPager上纵向滑动时无法滑动listView  严重影响用户体验<br>
要做的其实很简单   在Viewpager中检测手势是否为纵向滑动  如果不是就拦截
