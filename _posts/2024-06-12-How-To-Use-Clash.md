---
layout: post
title: 上网技巧|如何基于clash科学上网
subtitle: How To Use Clash
author: Zcx
header-style: text
tags:
  - 科学上网
  - Clash
  - 梯子
published: true
---
### 前言
&ensp;&ensp;emm距离上次更新Blog已经过去一年多了,终于又再次克服拖延症更新Blog了,因为经常有小伙伴问我怎么优雅的科学上网,或者想要使用国外的一些AI产品无奈大陆地区无法访问,所以这次简单写一下,当然我也不可能对这个有多么全面的了解,就是个人使用的一点经验的总结罢了,权当参考.

### 原理
&ensp;&ensp;科学上网的发展了这么多年,衍生出的方案不计其数,但万变不离其宗,本质上还是在国外连接一台电脑(当然不一定是物理意义上的电脑,主要是虚拟机),将你的流量在国外的电脑上进行中转,因为GFW(防火墙)不可能封禁所有的国外IP地址,所以这样就可以成功绕过GFW的阻拦,想要了解这些方案的细节推荐电丸科技AK的[硬核翻墙](https://www.youtube.com/watch?v=XKZM_AjCUr0&list=PLqybz7NWybwUgR-S6m78tfd-lV4sBvGFG)系列,基本把翻墙的所有方案的原理都讲解了一遍.非常硬核.    

&ensp;&ensp;我这里讲的原理主要是想纠正一个很多小伙伴在翻墙时的误区,就是将翻墙服务的提供商和翻墙服务所使用的软件混为一谈,所以在问我的时候总说我这个软件不好使了怎么办,实际上很多所谓的翻墙软件都是购买的翻墙服务提供商的线路然后包装一下再卖给你,这种翻墙服务相当于多了一层中间节点,有时甚至是好几层,这样无形中就提高了价格,和信息的不安全性,当然并不是说直接从提供商那里购买服务就一定安全,但是中间商越少,能够篡改你信息的人就越少,从而也就相对安全一些.所以以下我说的科学上网的教程都是基于直接向提供商购买节点这一方案展开的.

### 提供商的选择
&ensp;&ensp;有关提供商的选择更是一个纷繁复杂的过程,我这里不会给出太多的参考,只说一下我目前使用的一家:[AnyLand](https://any602.cc/#/register?code=qq8tHvxH)(可能大陆地区没法访问,需要的话可以联系我帮忙注册),相对来说这家的价格还是比较便宜的,一年也就260,支持Disney+和网飞的解锁,当然问题也是有的,使用下来主要是节点不稳定,然后应该是有人经常用节点跑爬虫,导致某些网站经常需要认证以下你不是机器人,比如下面这种:

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_1.jpg)

&ensp;&ensp;访问上面的链接,和所有其他网站一样都需要注册一个账号这里不多赘述,常规的注册流程就好,需要注意不推荐使用qq邮箱,   

&ensp;&ensp;登录之后在左侧的导航栏中找到购买订阅,在这里选择你需要的套餐,这边推荐"BASICS年套餐(特别优惠)",然后点击订阅,然后再右上角优惠码那里输入"JUNE"可以有9折优惠.

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_2.png)

&ensp;&ensp;接下来就是正常的付款流程,购买完成后,回到仪表盘,点击"查看教程",下载对应的客户端,其实就可以正常使用了,但是,一般服务商提供的方案都不怎么优雅,如果想要优雅的翻墙,还得看下面的内容!

### 优雅的翻墙

&ensp;&ensp;开头我也说过,服务商和软件实际上是不同的,当然很多服务提供商也会有自己的软件,但本质上他也会提供节点的订阅地址让你用其他的软件来翻墙,这里我推荐Clash,对翻墙稍微了解的小伙伴一定说了,我知道!Clash的作者删库跑路了,所以Clash现在已经不安全了,这么说当然没问题,但我个人实测下来,比Clash好用的目前也还没发现,不如在成功翻墙之后,再去找找看适合你的哪一款软件.

&ensp;&ensp;因为Clash的作者已经删除跑路了,所以并不能从Github上直接下载到Clash,我这边上传了蓝奏云:[地址](https://www.ilanzou.com/s/xHU3BWW)下载完成后,解压双击打开"Clash for Windows.exe",然后回到刚才网站的仪表盘页面,点击一键订阅在弹出的窗口中点击"导入到Clash For Windows"如果网页提示是否允许打开Clash点击允许就可以,接着Clash的窗口会弹出来如下图

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_3.png)

&ensp;&ensp;切换到Proxies标签,可以看到所有的节点,点击红圈所示的图标可以测速,

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_4.png)

&ensp;&ensp;点击自动选择就可以了,一般程序会自动选择一个可以链接的节点,当然一也可以手动选择你想要的节点,注意节点后面的数字表示延迟,越低越好,Timeout代表超时不可以选择

&ensp;&ensp;节点选择完成后,我们来配置浏览器,通常服务商提供的软件默认是全局模式,但一般情况下我们翻墙只是用浏览器就可以了,别的软件还是希望走我们国内的ip,所以这里我们需要对浏览器进行配置,这里我以Chrome为例,Chrome浏览器下载也很方便,直接百度Chrome就可以了,安装完成后,我们先将Clash切换为系统代理,保证我们可以访问Chrome的应用商店,在任务栏中找到Clash的图标,一个蓝色的猫头,右键选择System Proxy就可以了,此时猫头的颜色会变成金色,此时我们可以访问以下[谷歌的网址](https://www.google.com/),测试以下翻墙是否成功.

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_5.png)

&ensp;&ensp;在地址栏输入"chrome://extensions/"打开Chrome的扩展程序页面,在左侧找到"在 Chrome 应用商店 中发现更多扩展程序和主题",如下图

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_6.png)

&ensp;&ensp;找到搜索栏,输入"proxy omega"并按下回车,如下图,找到第一个并点击进入proxy omega的页面

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_7.png)

&ensp;&ensp;后点击添加至Chrome,等待安装,安装完成后找到拼图一样的图标,会弹出下面的窗口,可以先找到Proxy SwitchyOmega一列右边的图钉一样的图标将其固定

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_8.png)


&ensp;&ensp;然后找到Proxy SwitchyOmega一列右边的三个点,在弹出的列表中点击选项,接下来会弹出配置页面

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_9.png)

&ensp;&ensp;点击新建情景模式,输入一个情景模式名称,其他不用改,这里建议使用Clash作为名称,点击创建,代理服务器配置就按照我上面的图中那样配置,分别选择代理协议,代理服务器,代理端口,填写完成后点击应用选项,此时Chrome的配置就完成了,此时需要在Chrome右上角的插件区域找到一个圆形的图标,根据状态的不同图标的形状会有一些区别,大体如下图

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_10.png)

&ensp;&ensp;点击该图标,并在弹出的列表中选择我们刚才创建的情景模式Clash,选择完成后,需要关闭Clash的System Proxy模式,在任务栏中找到Clash的图标,一个金色的猫头,右键再次点击System Proxy,此时猫头的颜色又会切换为蓝色,此时基本的配置就完成了回到浏览器,试试能不能访问[油管](https://www.youtube.com/),[谷歌](https://www.google.com/)把,当然TikTok也是可以访问的(注意不要使用香港节点),

&ensp;&ensp;翻墙成功后,玩法就多了,当然Clash本身还有很多玩法,这就需要聪明的你不断的探索了,下次克服拖延症,写写怎么用油猴,可以说是浏览器必备的神器了,可以看下我目前使用的油猴脚本

![]({{site.baseurl}}/img/Post/2024-06-12-How-To-Use-Clash/pic_11.png)



&ensp;&ensp;最后感谢你能够看到这里~


![]({{site.baseurl}}/img/GoodMorning.jpg)



