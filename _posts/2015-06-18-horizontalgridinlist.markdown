---
layout: post
title: "自定义无滚动条GridView"
subtitle: "随笔啦"
date: 2015-06-18
author: fixxz
category: essay
tags: essay
finished: true
---

## 自定义纵向无滚动条的GridView 

此处关键就是重写onMeasure 

 `public class HorizontalGridView extends GridView{  
     public HorizontalGridView(Context context, AttributeSet attrs) {  
         super(context, attrs);  
     }  
     @Override  
     protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {  
         int measureSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2,  
                 MeasureSpec.AT_MOST);  
         super.onMeasure(widthMeasureSpec, measureSpec);  
     }  
 }  `

## 配置gridView
在这里用到了LayoutInflator  
在实际开发中LayoutInflater这个类还是非常有用的，它的作用类似于findViewById()。
不同点是LayoutInflater是用来找res/layout/下的xml布局文件，并且实例化；而findViewById()是找xml布局文件下的具体widget控件(如Button、TextView等)。

LayoutParams params = new LinearLayout.LayoutParams(1,LinearLayout.LayoutParams.MATCH_PARENT);
// 此处的 1  ,表示Gridview的宽度  可以在此处直接指定  也可在稍后更改
`viewHolder.gridView.setColumnWidth(150); // 设置列宽  为150   可根据需要自行更改  
viewHolder.gridView.setStretchMode(GridView.NO_STRETCH); // 设置为禁止拉伸模式  
viewHolder.gridView.setNumColumns(GridView.AUTO_FIT); // 设置列数  每一列的item数目 设置为自动  
viewHolder.gridView.setHorizontalSpacing(2); // 两列之间的间距   
viewHolder.gridView.setLayoutParams(params); // 设置GirdView布局参数,横向布局的关键  `

## baseAdapter
[adapter](http://www.cnblogs.com/xiaowenji/archive/2010/12/08/1900579.html)
` // 然后系统调用getView()方法，根据getCount的长度逐一绘制ListView的每一行。  
    @Override  
    public View getView(int position, View convertView, ViewGroup parent) {  
        ViewHolder viewHolder = null;  
            if (convertView == null) {  
            // 将 list_item 转换为View "LayoutInflater的作用"  
             convertView = LayoutInflater.from(context).inflate(  
                     R.layout.list_item, null);  
             viewHolder = new ViewHolder(convertView);  
             // setTag()会把View与ViewHolder绑定，形成一一对应的关系，拖动listview的时候会重新绘制每一条item  
             convertView.setTag(viewHolder);  
         } else {  
             // 但是那些已经取得绑定的View，会调用getTag()方法，就不会重新绘制，而是拿到内存中已经取得的资源，提高了效率  
             viewHolder = (ViewHolder) convertView.getTag();  
         }  
         viewHolder.head_text.setText(title_list.get(position) + "");  
 }  `

## tips
setColumnWidth  设置列宽</br>
setStretchMode   设置模式</br>
setNumColumns   设置每一列Item的数目</br>
setHorizontalSpacing    设置两列之间的间距</br>
setLayoutParams    设置Gridview布局参数