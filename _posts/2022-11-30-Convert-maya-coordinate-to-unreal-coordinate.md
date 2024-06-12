---
layout: post
title: Unreal学习|将maya坐标转换为UE坐标
subtitle: Convert maya coordinate to unreal coordinate
author: Zcx
header-style: text
tags:
  - unreal engine
  - ue
  - 线性代数
  - python
  - Maya
published: true
---
### 前言
&ensp;&ensp;最近接到一个需求，需要把一个物体在maya中的移动旋转发送到UE中，并且maya中的坐标轴需要能变化一次，UE中则保持原有坐标轴的位置。
### 位移
&ensp;&ensp;因为位移不基于坐标轴，所以直接把maya中在XYZ轴上的坐标值复制到UE中即可，但要注意Maya是Y轴向上的Ue是Z轴向上的，需要把Y轴数据和Z轴数据交换一下位置。
### 旋转
&ensp;&ensp;旋转是最难处理的部分，因为UE是Z轴向上的左手坐标系，Maya是Y轴向上的右手坐标系，一开始我想把Maya的数值直接复制到UE里面，然后交换Y和Z轴并且，翻转Z轴的数值，结果发现在0到90度之间，这个想法都是没有问题的，可一旦超过90°，就完全无法对应了。

&ensp;&ensp;xyz等于15°maya中的效果:
![]({{site.baseurl}}/img/Post/2022-11-30-Convert-maya-coordinate-to-unreal-coordinate/pci_1.png)
&ensp;&ensp;xyz等于15°UE中的效果:
![]({{site.baseurl}}/img/Post/2022-11-30-Convert-maya-coordinate-to-unreal-coordinate/pci_2.png)
&ensp;&ensp;xyz等于90°maya中的效果:
![]({{site.baseurl}}/img/Post/2022-11-30-Convert-maya-coordinate-to-unreal-coordinate/pci_3.png)
&ensp;&ensp;xyz等于90°UE中的效果:
![]({{site.baseurl}}/img/Post/2022-11-30-Convert-maya-coordinate-to-unreal-coordinate/pci_4.png)

&ensp;&ensp;可以看到在xyz为90的时候maya和UE在相同镜头角度下呈现出完全不同的效果，而且无论以何种组合翻转轴向都无法得到正确的结果，试验之后发现在Maya，XYZ都是90度的情况下，UE中正确的旋转角度是0,0，-90，那么如何得到这个值？

#### 旋转公式
&ensp;&ensp;首先想到的是Maya和UE的旋转角公式不同，所以同样的旋转要用不同的角度描述，用两个旋转公式按照各自的顺序旋转一个相同的向量，应该可以根据已知的角度，求出另一个角度，但查阅了有关文档，UE的旋转角公式和顺序几乎没有，只查到了[连接](https://forums.unrealengine.com/t/euler-rotations-and-matrix-questions/4450/2)而且作者似乎也不是很确定，而且这个方法似乎也有一些问题。
#### 四元数
&ensp;&ensp;找了半天，终于发现了一个如何把Houdini的旋转转换到UE中的回答，他是先把Maya中的[[欧拉角]]旋转，转换成[[四元数]]旋转，然后翻转这个旋转中的W，并且交换Z和Y的值。正好在UE中提供了四元数和欧拉角转换的API
````python
    # NOTE Split Maya data
    Rotate = TransfromData['Rotate'] 
    rotationQuat = unreal.Quat()
    # Convert to unreal Aixs
    # Convert to Quat  
    rotationQuat.set_from_euler(unreal.Vector(Rotate[0],Rotate[1],Rotate[2]))
    # SWAP Z and Y
    temp = rotationQuat.z
    rotationQuat.z = rotationQuat.y
    rotationQuat.y = temp
    # inverse W value
    rotationQuat.w = -1 * rotationQuat.w
````
&ensp;&ensp;实验之后发现，只有在Z轴上的旋转是正确的，在XY上的旋转都无法对应，看来是Maya和Houdini的坐标系也有区别，不过试了几次之后发现，似乎XY上的旋转都是反的只要加一个负号就可以得到正确的结果了，于是改了改代码
````python
    # NOTE Split Maya data
    Rotate = TransfromData['Rotate'] 
    rotationQuat = unreal.Quat()
    # Convert to unreal Aixs
    # Convert to Quat  
    rotationQuat.set_from_euler(unreal.Vector(-Rotate[0],-Rotate[1],Rotate[2]))
    # SWAP Z and Y
    temp = rotationQuat.z
    rotationQuat.z = rotationQuat.y
    rotationQuat.y = temp
    # inverse W value
    rotationQuat.w = -1 * rotationQuat.w
````
&ensp;&ensp;这次测试过后发现终于完美实现了旋转信息的传递，接下来就是坐标轴变化的部分了
### 轴心点变换
&ensp;&ensp;因为旋转是基于坐标轴位置的，但无论在什么坐标轴下旋转，旋转后得到的物体的角度是相同的，也就是说，只要求出不同坐标轴在旋转之后所产生的的偏移，然后抵消掉这个偏移就可以了。



````python
    Pivot = TransfromData['Pivot']
    # 排除本来偏移对坐标轴位置的影响
    for i in range(0,len(Pivot)):
        Pivot[i] = Pivot[i]-Translate[i]
    # 把旋转角度转换为弧度制
    XRadAngle = Rotate[0]/180.0*math.pi
    YRadAngle = Rotate[1]/180.0*math.pi
    ZRadAngle = Rotate[2]/180.0*math.pi
    # 计算三角函数值
    XCosa = math.cos(XRadAngle)
    XSina = math.sin(XRadAngle)
    YCosa = math.cos(YRadAngle)
    YSina = math.sin(YRadAngle)
    ZCosa = math.cos(ZRadAngle)
    ZSina = math.sin(ZRadAngle)
    # 这里写了一个矩阵的类，也可以用UE的类
    pivotOver = Mat.Mat([[Pivot[0]],[Pivot[1]],[Pivot[2]]])
    # 写出x，y，z的旋转矩阵
    XAxiRotate = Mat.Mat([[1,0,0],[0,XCosa,-XSina],[0,XSina,XCosa]])

    YAxiRotate = Mat.Mat([[YCosa,0,YSina],[0,1,0],[-YSina,0,YCosa]])

    ZAxiRotate = Mat.Mat([[ZCosa,-ZSina,0],[ZSina,ZCosa,0],[0,0,1]])
    # 按顺序应用旋转矩阵
    PivotNew = ZAxiRotate * YAxiRotate * XAxiRotate * pivotOver
    # 计算之前坐标轴和新坐标轴的偏移
    Offset = pivotOver - PivotNew
    #最后把这个偏移加上原有的位移即可
````

### 相机旋转

&ensp;&ensp;以上的方式在传递一般物体的旋转时是没有问题的,但是在处理相机时却会得到不正确的结果,反复实验之后发现maya相机朝向的是-z轴,而ue的相机朝向的是+x,所以在处理相机的旋转数据时,需要首先进行局部旋转,沿着y轴旋转-90度然后,将得到的数据再进行上述的转换即可.
这里用蓝图实现这个过程

![]({{site.baseurl}}/img/Post/2022-11-30-Convert-maya-coordinate-to-unreal-coordinate/pci_5.png)

### 谢谢观看!
![]({{site.baseurl}}/img/GoodMorning.jpg)


### 参考
------------------------------------------------------------------------------------------------


[3D-app Euler-Rotations to UE4 Euler-Rotations](https://www.gamedev.net/forums/topic/703263-3d-app-euler-rotations-to-ue4-euler-rotations/)

[Houdini rotation to UE4 rotation?](https://www.sidefx.com/forum/topic/69197/?page=1#post-304968)

------------------------------------------------------------------------------------------------

