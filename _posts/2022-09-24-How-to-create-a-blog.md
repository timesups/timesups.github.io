---
layout: post
title: 第一篇博客|记录这个博客的创建过程
subtitle: Documenting the process of creating this blog
author: Zcx
header-style: text
tags:
  - Jekyll
  - github page
  - Blog
  - 博客创建
published: true
---
## 写在前面
&ensp;&ensp;一直想在CSDN以外的地方写写博客,一开始的想法是买个vps挂个[Wordpress](https://wordpress.org/)在上面.虽说之前已经用过很多次WordPress但一想到VPS繁琐的购置流程,域名的申请,以及诸如是否备案之类的问题,就连第一步也迈不出去了.直到有一天在看[冯乐乐大佬](http://candycat1992.github.io/2017/05/29/update-blog/)的博客时知道了GitHub Pages,算是个轻量级的在线网站的解决方案,瞬间又有动力了.这第一篇博文就用来记录博客搭建的过程(踩过的坑),但毕竟我也是第一次使用GitHub Pages所以我的制作过程不一定能包括所有的特殊情况,所以如果你也想按照我的流程创建个人博客,请务必用好搜索引擎,相信聪明的你一定可以成功的!
## GitHub Pages.
> GitHub Pages 是一项静态站点托管服务，它直接从 GitHub 上的仓库获取 HTML、CSS 和 JavaScript 文件，（可选）通过构建过程运行文件，然后发布网站。 可以在 GitHub Pages 示例集合中看到 GitHub Pages 站点的示例.

&ensp;&ensp;以上就是Github官方关于Github Pages的解释,简单来说,你可以使用github的仓库存放一些静态网页文件,使其他的用户可以访问这些托管在仓库中的静态网页.但显然如果只是静态网页,在更新和维护方面难免会产生很多问题,但不用担心,Github也支持使用静态站点生成器的方式来动态的生成静态站点.需要注意的是,Github page本身并不支持任何服务器端的语言,比如PHP,Ruby,Python.所以每次对网站做出更新都要重新构建静态页面,当然这个过程可以在本地也可以在服务器上完成.

&ensp;&ensp;当然Github Pages在使用上还是有一些限制的,用户只有1Gb的存储容量和每月100GB的带宽,这一点需要在使用的时候注意.
## 搭建过程

&ensp;&ensp;GitHub Pages站点有三种不同类型,分别是项目,用户和组织,因为我只使用了用户站点,所以搭建过程也只针对用户站点.

### 创建存储库
&ensp;&ensp;要使用GitHub Pages首先需要一个存储库
![]({{site.baseurl}}/img/post/2022-09-24-How-to-create-a-blog/pic_1.jpg)
&ensp;&ensp;创建存储库基本没有太多可以设置的,需要注意的是存储库的命名,因为是用户站点,所以存储库的名称必须是"<你的用户名>.github.io",而且必须保持小写,如上图,我的用户名是timesups,所以我的网站域名就是timesups.github.io,用作网站的存储库可以设置为私有或者公开,但设置私有后,其他用户仍然可以方位这个私有库所托管的网站,所以在如果有较隐私的文件不要放在这个托管库中.
### 挑选模板
&ensp;&ensp;我这里直接Copy了[黄玄](https://github.com/Huxpro/huxpro.github.io)大佬的库,当然你也可以自己挑选一些模板,但模板大多需要相对复杂的设置,而且对于Jekyll--这个一会再介绍--版本的支持也不尽相同,为了更快速得到一个相对能用的网站,我就直接在黄玄大佬的网站的基础上做了些更改.
&ensp;&ensp;要使用别人的网站也很简单,你可以直接Fork这个库,直接省去创建库的步骤,也可以向我一样将整个库下载为一个压缩包,在本地更改后上传到之前创建的库中.
### Jekyll
> Jekyll 是一个静态站点生成器，内置 GitHub Pages 支持和简化的构建过程。 Jekyll 使用 Markdown 和 HTML 文件，并根据您选择的布局创建完整静态网站。 Jekyll 支持 Markdown 和 Lick，这是一种可在网站上加载动态内容的模板语言。 

&ensp;&ensp;也就是说Github使用Jekyll将我们上传的文件构建成静态网页,所以同样的我们在本地也可以使用Jekyll构建并预览我们的网站,待满意后在上传到库中,毕竟Github每小时只能构建10次网站,这显然不适合前期网站的修改.
#### Jekyll环境搭建
&ensp;&ensp;因为Jekyll是基于Ruby语言的,所以首先我们需要搭建Ruby环境,访问[ruby](https://rubyinstaller.org/downloads/)的官网,下载并安装ruby,注意你的系统是64还是32位的.
![]({{site.baseurl}}/img/post/2022-09-24-How-to-create-a-blog/pic_2.jpg)

&ensp;&ensp;安装一路下一步即可,安装完成后按下Win+r,输入cmd点击回车,打开命令提示符.输入下面的命令开始安装Jekyll
> gem install jekyll

&ensp;&ensp;不知道为啥,我的cmd经常卡住,需要按几下空格才能继续,安装完成后,解压之前下载的模板文件到你的用作网站的本地库中,在上面的地址栏输入CMD并回车,输入命令:
> bundle install

&ensp;&ensp;然后输入

> bundle exec jekyll serve

&ensp;&ensp;接下来你就可以在[http://127.0.0.1:4000/](http://127.0.0.1:4000/)这个地址上访问你的本地的网站了,可以实时对你的修改做出反馈,如果在这个过程中出现了问题,可以在底部的参考网站中看看有没有解决方案.

### 为你的博客添加评论系统
&ensp;&ensp;模板默认支持Disqus和网易云跟帖,但博主觉得设置麻烦且并不好看,于是同样参考了冯乐乐的博客使用[livere](https://livere.com/)作为评论系统,添加起来也很方便,首先注册并获取用户ID,下图红色部分.

![]({{site.baseurl}}/img/post/2022-09-24-How-to-create-a-blog/pic_7.jpg)

&ensp;&ensp;然后在_config.yml中添加变量livere_uid:,并在后面输入刚才的用户ID,注意!一定要在冒号和ID之间输入一个空格,不然无法构建成功.

&ensp;&ensp;接着将其添加到网页模板中,找到_layouts文件夹下的post.html,找到"<!-- 网易云跟帖 评论框 end -->"在该代码块的下方添加如下代码:
````html
{% if site.livere_uid %}
<!-- livere 评论框 start -->
<div class="comment">
    <div id="lv-container" data-id="city" data-uid="{{ site.livere_uid }}"></div>
</div>
<!-- livere 评论框 end -->
{% endif %}
````
&ensp;&ensp;然后找到"<!-- disqus 公共JS代码 end -->"也在代码块的下方加入如下代码:

````html
{% if site.livere_uid %}
<!-- livere 公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
    (function(d, s) {
        var j, e = d.getElementsByTagName(s)[0];
        if (typeof LivereTower === 'function') { return; }
        j = d.createElement(s);
        j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
        j.async = true;
        e.parentNode.insertBefore(j, e);
    })(document, 'script');
</script>
<!-- livere 公共JS代码 end -->
{% endif %}

````
&ensp;&ensp;这样我们就成功添加了评论系统到我们的博客中,快去试试评论吧.


### 上传到Github存储库中

&ensp;&ensp;在对网站满意后,就可以将其上传到服务器的存储库中了,上传完成后需要在服务端开启Github Page功能,首先在存储库的主页找到Settings,在左侧找到Pages,在其中找到Branch,点击None将其设置为master,然后点击Save

![]({{site.baseurl}}/img/post/2022-09-24-How-to-create-a-blog/pic_3.jpg)

&ensp;&ensp;然后在上方找到Actions并点击,可以看到后台正在构建我们当前的静态网页

![]({{site.baseurl}}/img/post/2022-09-24-How-to-create-a-blog/pic_4.jpg)

&ensp;&ensp;构建完成后就可以使用存储库的名称作为地址访问我们的网站啦!

### 使用[prose](https://prose.io/)写你的第一篇博客
&ensp;&ensp;[prose](https://prose.io/)是一个可以在线编辑Github的网站,支持MarkDown的语法(不知道MarkDown的可以百度一下MarkDown语法),打开prose并关联github找到你用于存放网站的库下的_post文件夹,并点击NEW FILE,就可以开始写博客了,需要注意的是,我们首先需要在右侧的菜单中找到Metadata并在其中加入一些属性

![]({{site.baseurl}}/img/post/2022-09-24-How-to-create-a-blog/pic_5.jpg)

写完后,只需在右边的菜单中找到Change to submit 然后点击SUBMIT CHANGE REQUEST即可.

![]({{site.baseurl}}/img/post/2022-09-24-How-to-create-a-blog/pic_6.jpg)

保存完成后,github会自动构建静态网页,一段时间后就可以在我们的网站上看到文章了.


### 参考
[jekyll本地环境搭建(Windows)](https://www.likecs.com/show-947640.html)

[搭建自己的github.io博客](https://blog.csdn.net/qq_34106574/article/details/82704883)

[安装jekyll及过程中遇到的一点问题](https://blog.csdn.net/flysky_jay/article/details/106397264)

[10分钟搭建一个基于github和jekyll的免费博客](http://cenalulu.github.io/jekyll/how-to-build-a-blog-using-jekyll-markdown/)

[GitHub: 个人博客搭建](https://blog.csdn.net/qq_40928870/article/details/124026389)

[官网的Github Pages教程](https://docs.github.com/cn/pages/getting-started-with-github-pages/about-github-pages)

[关于新的ruby源替换（最新）](https://bbs.pinggu.org/jg/kaoyankaobo_kaoyan_9182823_1.html)
