---
layout: post
title: “中国传统色”应用使用帮助
category: project
description: 姗姗来迟的使用帮助。多谢厚爱！
---

这是个什么应用？
-----------
“中国传统色”是一个色谱/色卡应用。

什么是色谱/色卡呢？在我的理解里，认识植物需要植物图鉴，理解单词需要词典，那么，了解颜色就需要色谱。它是颜色的图鉴，是颜色的字典。

为什么要开发这个应用？
------------
大千世界中有那么多美丽的颜色，而我每天只能面对着电脑屏幕。所以，我想把美丽的颜色也搬上屏幕，给我，给屏幕那端的你。

这个应用的创意来源于一个非常惊艳的网站[“日本传统色”](www.nipponcolors.com)。自从我知道这个网站后，每次做设计时都会泡在上面，汲取灵感。照这样说，我第一个开发的应用，应该是“日本传统色”。

我当然这样想过……不过，既然有“日本传统色”，自然也有“中国传统色”咯。把《红楼梦》中出现的颜色都展示出来，该是多么有趣的事情啊！

从网上爬了下资源，再一边翻书一边翻Stackoverflow，自学了c#和json，就像完成一个大作业一样，半生不熟地把这个应用开发出来了。

从此，我也是上过“热门应用”官方榜单的独立开发者啦！

应用里每个东西都是干嘛的？
-----------------
在应用列表中点击“中国传统色”条目，不出意外的话它应该稳稳地呆在所有应用的最下面：

![应用列表](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XRy9aUStIVkNNbGhOL3BuQmlvVVc5UURNNm1JQ25EUTdBPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

然后，你会看到经过N次迭代后被红淹没~~不知所措~~的闪屏界面；
![一堆胭脂变成的low poly](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XSitQOVJXdXhMcVRoTGcwVFI0WmVoVWJZelVvcTVCam5RPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

进入应用后，你首先看到的大概是这样一个页面：
![第一个色谱](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XSk1oeG96R2RINUZFSmx1NkloQ2xJcERCb0VjYmJGZzBnPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

它是一个颜色列表，搓一搓就可以滚动了。并且，它是一个很长很长的颜色列表，搓到最后你的屏幕一定无比干净w

你以为这就是全部了么？Naive！左右搓搓试试看！

右搓搓：

![第二个色谱](http://imglf1.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XUG0xNHRqRzJsWjFUV2VOSTIxTDB3UUNmMDZncnEzdnZ3PT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

哈！出现了第二个列表！和上个列表完全没有重叠的哦！

再右搓搓：

![空白色谱](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZGFwOTBJalRvdWdaVTZsbVpYUkVYNkJudnNZYUpyQnhiMXJHenYzZ0hzdkR3PT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

这个是为了让大家能够去掉旁边色条的影响，完完全全地沉浸进去欣赏一种颜色而设置的(●'◡'●)

点击任何一个颜色条目，都会更换背景色+解说文字。那么……

##右边这些文字究竟是什么意思呢？##

嗯，就让我们用以上颜色为例，依次说明：

![第二个色谱样式解说](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XRlpNY25lZ2FIVGx4NG9kbnpFb3gxNTMrQXhvc2pYQU5nPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

那么，就会有小朋友举手提问了：

##背景颜色太浅看不清字怎么办？##

像这样：
![一片精白](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZFl0VW9WOTB5dWxNK21FNmExUlBzQWZuUXJIUGN5K0xJTEh4M1dBYkFXVmNnPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

……请允许我做个悲伤的表情……

不要慌不要急，我们摸一下屏幕右下角的“...”，在浮出的菜单中选择“深色”（就是那个<u>A</u>）：
![深色](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZFl0VW9WOTB5dWxNK0lVb0ViZGRRTjhvcGM4a2ZLMFJuU253NWRaVWxTbkNnPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

哈！就可以变成这样子了！q(≧▽≦q)
![深色精白](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZFl0VW9WOTB5dWxNK01yZkVHVDBsMmJqWktlUmhPeGNZSW80ZytMNlFNVmNBPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

不过，由于某种很呆(lan)的原因，字的颜色并不能自动变化。不过私觉得，深色的字也一样美丽呢~~~

##我想保存当前美丽的颜色！##

这大概是窝的邮箱里收到的最多用户请求了。

于是在右下角的“...”选单里，就有了这个“保存”按钮：

它的作用是，保存当前的颜色。在默认状态下，它仅仅保存当前颜色，对，就是一张纯纯的纯色图片，像这样：

![纯色图片](http://imglf0.nosdn.127.net/img/UlJvNXBPT29FZFl0VW9WOTB5dWxNL1NjK3dwZ1ZsdEpydkcvVEMxK1JZUmxoS3E5bWNjY3VnPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

但是，窝觉得字也很好看啊（就是很好看！），那怎么把配文也保存下来呢？

摸一下有字的地方，就会变成这样：

![无字颜色](http://imglf0.nosdn.127.net/img/UlJvNXBPT29FZGFwOTBJalRvdWdaUWcxTXlwMEVWS3Brd0xmRlllZDJSSmcwODdpM2JVaHZnPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

这个时候点击“保存”图片，就可以得到带配文的图啦：
![有字图片](http://imglf0.nosdn.127.net/img/UlJvNXBPT29FZFl0VW9WOTB5dWxNOWg5VEh4UFZ0dVhsUU9CeHlxUUNudVdQUHNjQVZQVlJnPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

保存完图片后，会弹出这样一个对话框：

![保存图片消息](http://imglf1.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XR2g0MXpsYUNUcDZXRGZrd0hMZDIyK2lMenJLRXNxbVVBPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

它的本意是提醒你：保存成功啦！不过每次保存完后都会弹出确认一下，有点烦。所以，当你熟悉到不需要提示的时候，点击“不再提醒”，就好了~

保存的图片可以在相册里看到。

![相册查看](http://imglf.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XQ0pTMWdBZDFZRkVTaDE2NXovVVphcjdVSWZjK2NuU2RBPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

你还有啥想说的？
------------------
另外，作为一个闷骚的家伙，我当然要在这个应用里藏彩蛋咯~

比如有机智的亲们发现有一天磁贴忽然变成一朵透明的莲花了：
![大概是这个样子的透明版](http://imglf1.nosdn.127.net/img/UlJvNXBPT29FZGJPaEM1aldVa05XSGExMnE3Uk9XcEZwSmdmYXBSTDlzY0g3aUZGcm1jSkhnPT0.png?imageView&thumbnail=1680x0&quality=96&stripmeta=0&type=jpg)

我花了不计其数的闲暇时间去打磨这个应用的细节，希望你能感到惊喜(●'◡'●)

开发者我爱你！我要包养你么么哒！
------------------
窝也爱你(づ￣ 3￣)づ

给这个应用点赞请直接到应用商店里刷五星；

给这篇文章点赞请在下面留言/加赞b(￣▽￣)d；

包养我请直接往下面的二维码打钱；
![支付宝二维码](/images/alipay.png)

想从我这里拿红包请自己想办法加微信吧，备注“Mukosame我找到你了!”