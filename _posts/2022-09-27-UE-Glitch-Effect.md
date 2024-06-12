---
layout: post
title: Unreal学习|UE故障艺术效果效果(更新中)
subtitle: UnrealEngine Glitch Effect Test
author: Zcx
header-style: text
tags:
  - unreal engine
  - ue
  - Glitch
  - 故障艺术
  - UE学习
published: true
---
### 前言
&ensp;&ensp;最近想要制作一些蒸汽波啊,赛博朋克啊之类的个人作品,在制作过程中找到了一些制作故障(Glitch)艺术的教程,决定把他们在Unrel中都实现一遍,这里基于的是unrealEngine5,在其他版本也都一样.
### 错位图块故障
![]({{site.baseurl}}/img/Post/2022-09-27-UE-Glitch-Effect/PIC_1.gif)
&ensp;&ensp;简单来说错位图块故障就是生成在屏幕上随机抖动的块,让这些块影响UV,然后来采样图片,还可结合色调分离一类的元素来提升表现,所以首先我们需要在屏幕空间生成一些随机闪烁的方块.,呀要生成随机闪烁的方块首先我们需要一个基础的Random函数,由于在UE中没有相关的函数,而且为了保证输出结果的可控性,我们可以自己写一个,使用下面的代码,其中vt是一个随机向量,coord是二维坐标的输入,time是时间的输入.
````cpp
float2 vt = float2(12.9898,78.233);
return float(frac(sin(dot(coord,vt))*(43758.5453123+time)));
````
&ensp;&ensp;我们可以吧这个代码封装成一个材质函数方便以后的调用.
![]({{site.baseurl}}/img/Post/2022-09-27-UE-Glitch-Effect/PIC_2.jpg)

&ensp;&ensp;从图中可以看出,纹理坐标输入后会得到一张随机的Noise图片,要想得到随机的方块效果,我们可以将坐标乘上一个数字,然后使用Floor函数向下取整,然后输入到RandomNoise中,最后使用Power函数增加对比度去掉一些方块,当然也可以使用不同的对比值然后将结果相乘以增加更多的随机性.
![]({{site.baseurl}}/img/Post/2022-09-27-UE-Glitch-Effect/PIC_3.jpg)
![]({{site.baseurl}}/img/Post/2022-09-27-UE-Glitch-Effect/PIC_4.jpg)

&ensp;&ensp;其实对于错位图块效果,到这里原理部分基本就说完了,下面就是增加更多元素以增强表现的部分了,比如我们可以用两组大小不同的方块相乘,已得到更随机,更细碎的效果.

![]({{site.baseurl}}/img/Post/2022-09-27-UE-Glitch-Effect/PIC_5.jpg)

&ensp;&ensp;碎块与坐标的融合也很简单,只需要相加或者相减即可,在下图中我还加了一些RGBSplit的效果以及使用不同的随机色块采样贴图不同的通道,使色块在实际表现中呈现色彩变化以增强表现.
![]({{site.baseurl}}/img/Post/2022-09-27-UE-Glitch-Effect/PIC_6.jpg)


### 谢谢观看!
![]({{site.baseurl}}/img/GoodMorning.jpg)


### 参考
------------------------------------------------------------------------------------------------


[高品质后处理：十种故障艺术(Glitch Art)算法的总结与实现](https://zhuanlan.zhihu.com/p/148256756)

[The Book of Shaders-Generative designs](https://thebookofshaders.com/10/)

------------------------------------------------------------------------------------------------


