---
layout: post
title: "Android ttesseract识别数字demo"
subtitle: "-"
date: 2015-02-06
author: fixxz
category: demo
tags: demo
finished: true
---
最近接触到Tesseract，有个感触就是人与人之间的差距可能就在 Google和Baibu 之间<br>
好了，话不多说，关于Tesseract背景什么的我不介绍了<br>
入正题:

## step1: 下载
首先去<a href="http://code.google.com/p/tesseract-ocr/downloads/list">点击</a> 下载Tesseract,目前版本是3.02，我用的3.01 。//自行翻墙<br>
//关于如何在Windows以及其它非Android环境下使用 Tesseract ，请自行百度谷歌<br>
另外記得在項目主頁下載相关的語言包，想想如果没有语言包你拿什么识别<br>
以Android端为例，如果需要识别中文，则找到一个  Chinese(simple) 开头  ，如果识别 英文就是 English，其它语言同理<br>
//Android 的JAR包   http://pan.baidu.com/s/1eQjcjse    (好像没有密码了) gcvh<br>

## step2 配置
 将你下载的语言包解压，得到一个traineddata类型的文件，以中文为例，文件名为  "chi_sim.traineddata"   <br>
 放到对应目录下的tessdata目录下<br>
比如我放的目录为  /mnt/sdcard/tesseract<br>
<b> // 如果这一步没做好   会报错  “Java.lang.IllegalArgumentException: Data path must contain subfolder tessdata”</b><br>
如果下载正确应该是下面这样:<br>
![Example]({{ site.url }}/img/2015-04-25-androidtesseract.png "Example")

## step3 使用
`public String doOcr(Bitmap bitmap, String language) {  
            TessBaseAPI baseApi = new TessBaseAPI();  
            baseApi.init("/mnt/sdcard/tesseract", language);  
            int w = bitmap.getWidth();  
            int h = bitmap.getHeight();  
            Log.e("client_img", w+":"+h);  
            //灰度化  
            bitmap = convertToGrayscale(bitmap);  
            bitmap = Bitmap.createBitmap(bitmap, 130, 70, w-260, h-140);  
              
            // 必须加此行，tess-two要求BMP必须为此配置  
            //copy改为图的大小产生一个新位图，图像的大小为256位图。true表示产生的图片可以切割。  
            bitmap = bitmap.copy(Bitmap.Config.ARGB_8888, true);       
            baseApi.setImage(bitmap);  
            String text = baseApi.getUTF8Text();  
            baseApi.clear();  
            baseApi.end();  
            return text;                  
        }  `

## step4 tips
`String text_result = new LicensePlate().doOcr(bitmap, "chi_sim");`<br>
此处是识别为  "chi_sim"  即  Chinese  simple  简体中文<br>
<b>如果需要识别英文   把"chi_sim"改为"eng"   其它同理  </b><br>

這差不多是基本步驟   國內百度的文檔 不是很多   也有幾篇值得借鑒的參考資料   各位可自行百度<br>
  另外可以查看 它的源碼  源碼都自帶說明  仔細看看會帶給你很大提升<br>

如果實在有問題可以  去Google搜英文    看不懂的Google翻譯  <br>

另外給自己留一句話  <b>“保持本心”</b>