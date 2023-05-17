---
layout: post
title: Unreal学习|关于半透明物体的排序问题,混合模式为半透明的物体出现破碎的现象
subtitle: ConvertTranslucency object crush problem
author: Zcx
header-style: text
tags:
  - unreal engine
  - ue
  - 深度测试
  - 半透明混合模式
published: true
---
### 前言
&ensp;&ensp;最近一段时间长期处于拖延症的状态下,博客一直也没有更细,今天终于打起精神更新一篇

### 问题
&ensp;&ensp;如果我们要在UE中制作一个诸如纱布的效果,可能会用到UE中的半透明材质,但是,有时半透明材质发生重叠的时候会出现类似破碎的感觉.

![]({{site.baseurl}}/img/Post/2022-05-17-Translucency-object-crush/Pasted_image_20230516235831.jpg)


&ensp;&ensp;这点在开启材质的双面后更为明显(为了更明显的看到破碎,此时的不透明度为1.0)

![]({{site.baseurl}}/img/Post/2022-05-17-Translucency-object-crush/Pasted_image_20230516235911.jpg)

### 原因分析

&ensp;&ensp;这种现象产生的原因主要是,半透明物体实际上不参与深度测试,对于分开的static mesh actor我们还可以通过调整actor上的Translucency Sort Priority的值来获得正确的半透明物体的排序,但是对于同一个actor上的物体,如果有重叠的面UE就无法正确的判断像素的前后关系,从而导致后面的像素叠到了前面的像素的上面,从而产生这种类似破碎的效果.

&ensp;&ensp;可以看到在Scene Depth 中实际上是看不到半透明物体的深度的,即使该物体的不透明度为1.


![]({{site.baseurl}}/img/Post/2022-05-17-Translucency-object-crush/Pasted_image_20230517000345.jpg)


### 解决方案

&ensp;&ensp;要解决这个问题我们需要借助自定义深度通道,首先在模型上开启Render Custom Depth Pass,然后在材质上开启Allow Custom Depth Write.此时在自定义深度Buffer中就可以看到半透明物体的深度了,因为是单独的pass所以即使是半透明物体也可以产生正确的深度.

![]({{site.baseurl}}/img/Post/2022-05-17-Translucency-object-crush/Pasted_image_20230517000621.jpg)

&ensp;&ensp;接下来我们可以通过对比像素深度和自定义深度来决定每个shader point 的透明度.下图就是像素深度的效果,可以看到也出现了排序错误问题.

![]({{site.baseurl}}/img/Post/2022-05-17-Translucency-object-crush/Pasted_image_20230517001709.jpg)

&ensp;&ensp;通过对比像素深度和自定义深度的大小,如果像素深度等于或者小于自定义深度那么证明该点的深度正确,那么就使用原有的不透明度值,如果像素深度大于自定义深度那么使用0作为不透明度值.但需要注意的是,像素深度默认情况下就大于自定义深度(不知道具体原因),我们在使用像素深度时需要减去一个较小值.

![]({{site.baseurl}}/img/Post/2022-05-17-Translucency-object-crush/Pasted_image_20230517002447.jpg)

&ensp;&ensp;此时,半透明物体就不会出现破碎的现象了.

![]({{site.baseurl}}/img/Post/2022-05-17-Translucency-object-crush/Pasted_image_20230517002510.jpg)

### 谢谢观看!
