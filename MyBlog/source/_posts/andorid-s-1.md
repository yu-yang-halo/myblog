---
title: Bitmap在Intent中传递的问题
date: 2016-08-29 13:59:10
tags: Android开发
categories:  Android
---
##### JavaBinder: !!! FAILED BINDER TRANSACTION !!! Bitmap太大无法在Intent中传递

一般的解决办法是对Bitmap进行缩放
```java
   // 对于网络下载转化的bitmap只能使用Bitmap自带的转化方法
   Bitmap.createScaledBitmap(bitmap,w,h,true);//这种方式会带来很多意想不到的bug，建议使用画布canvas来缩放
   或者
   BitmapFactory.Options opts = new BitmapFactory.Options();
   // 设置为ture只获取图片大小
   opts.inJustDecodeBounds = true;
   opts.inPreferredConfig = Bitmap.Config.ALPHA_8;
   // 返回为空
   BitmapFactory.decodeFile(path, opts);
   opts.inSampleSize = getScaleSize(opts,w,h);
   // 加载到内存
   opts.inJustDecodeBounds = false;
   Bitmap bm= BitmapFactory.decodeFile(path, opts);
```
另外的解决方式就是缓存(只说明一下解决思路)

本地缓存
在跳转Intent之前，将Bitmap缓存到到本地文件中，然后再取出来。

内存缓存
使用单例模式的方式缓存（不建议）


#####  Bitmap.createScaledBitmap使用注意
Bitmap.createScaledBitmap 该方法会创建一个新的Bitmap，而且不会按照比例拉伸
它直接会按照你输入的Width和Height输出,这样的输出会导致



总结一下如何解决Intent传输Bitmap crash或者无法传递的问题
1.Bitmap缩小使用Canvas来缩放
2.如果Bitmap本身很小就无需缩放，否则会导致很奇怪的crash
