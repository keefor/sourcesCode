---
title: Unity图片资源加载
date: 2017-09-04 12:30:13
tags: Unity
---
分为File读取和WWW异步读取。以及Assetbundle加载。
	
### 准备工作

修改了图片后缀，二者加载正常，忘记当时为什么要修改后缀了，可能是测试后缀时候影响加载把。
不过也说明，文件加载是通过文件内部所携带文件格式数据判断的。
一张图效果不明显，共准备了9张256*256的jpg图。

### 方法

使用Stopwatch监测加载时长。
注意：不要使用UnityEngine.Time，否则你可能会发现时间都差不多。在因为在加载图片时主线程阻塞，Time是不走的。

### File读取图片

这里分为两步，
1）File.ReadAllBytes读取文件字节，
2）使用的Texture2D.LoadImage将字节转换为图片。

###### 经多次测试，加载平均时长是11。然而，只是读取字节的话，速度为0。
 
个人猜测，这里主要是对图片数据进行解压，也就是为什么有jpg、png、BMP多种格式存在原因。

### WWW读取图片

只需要一步。

###### 经多次测试，平均加载时长25。

在测试中发现，直接WWW（file+路径）即可，不需要使用异步加载。
该测试为加载小图片，其他资源类型未测试，PC、Android测试可行，暂未测试iOS。

### assetbundle读取

另外，还测试了一种将图片打包成assetbundle后加载，

###### 经多次测试，平均加载时长4.
	
### 童话项目

加载外部多张图片，
1）、首先使用的是File加载，卡顿。
2）、后来改用WWW异步加载，卡顿。
3）、使用队列依次异步加载，间断卡顿。
经过上述测试，未找到可行方法。尝试修改图片大小，缩小图片一倍，加载速度明显提升。无卡顿。