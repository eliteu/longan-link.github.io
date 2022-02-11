---
title: 'MicroBlocks 编程案例: 正中靶心！'
date: "2021-12-14"
tags:
  - MicroBlocks
slug: hit-the-bullseye
---

> 历史人物胡乱射出一箭, 历史学家在箭的落点画个圈说: 看！他正中靶心！

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/#%E5%89%8D%E8%A8%80)前言

@leeyve 最近买了两个机器人，问我能否接管它们，使其可编程。我拿来玩了会儿，十分喜欢，于是便着手试着用蓝牙接管它们。

蓝牙黑客的工作主要集中在处理 bytes，只要愿意投入时间，总是可以弄懂传输的信息，更何况身后有伟大的开源社区。

在汇景上课，课间十分钟，完成了第一次控制。

以下是接管之后，在 Python 里对其编程的小例子(近期我们也会将其接入 Scratch)

<video width="80%" src="https://adapter.codelab.club/video/93a6505c3c8ea33485381eb852abda.mp4" controls="controls"></video>
<video width="80%" src="https://adapter.codelab.club/video/8c0bfc6508a66d2e5c2ea588bd3dab.mp4" controls="controls"></video>

然而，我们今天并不打算讨论蓝牙黑客的技巧，玩一些更有趣的东西。

ps: 如果你对蓝牙黑客感兴趣, 可能会喜欢 [btlejack](https://github.com/virtualabs/btlejack)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/#%E7%8C%9C%E6%83%B3%E4%B8%8E%E5%8F%8D%E9%A9%B3)猜想与反驳

> 知识，特别是我们的科学知识，是通过未经证明的（和不可证明的）预言，通过猜测，通过对我们问题的尝试性解决，通过猜想而进步的。 – 波普尔《猜想与反驳》

在瞎折腾的过程中，我们发现这两机器人有一种对战模式: 当用户驾驶机器人对战时，如果按下发射按钮，击中对方，另一个机器人会被击败。

机器人并没有真的发出的子弹，另一个机器人如何知道被击中呢？ 我猜测，机器人身上带有红外发射器和红外接收器。

猜想的具体内容是: 红外基本沿直线传播，如果一个机器人的红外发射器发出红外线，正好被另一个机器人的红外接收器收到，就产生了 “击中” 事件。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/#%E9%AA%8C%E8%AF%81)验证

我想验证这个想法，于是找来 micro:bit， 翻箱倒柜，从 CodeLab 旧物箱里找到 m5stack 的红外传感器:

[![](https://wwj718.github.io/post/img/WechatIMG115.jpeg)](https://wwj718.github.io/post/img/WechatIMG115.jpeg)

在旧物箱里又找到一个不知哪个公司的红外遥控器:

[![](https://wwj718.github.io/post/img/WechatIMG116.jpeg)](https://wwj718.github.io/post/img/WechatIMG116.jpeg)

我将红外传感器接到 micro:bit 里，之前没有对红外设备进行编程的经验，但因为有 Microblocks，信心十足，感觉可以搞定它，因为 Microblocks 是一个放大你心智力量的编程环境，对探索和理解事物提醒了绝佳支持。

我将 m5stack 的红外传感器(IN pin)连到 micro:bit 1 号引脚，通过使用 Microblocks 内置的 IR Remote (在 积木库/Other 分类里):

[![](https://wwj718.github.io/post/img/WechatIMG117.png)](https://wwj718.github.io/post/img/WechatIMG117.png)

现在，可以实时观察到接受自红外遥控器的信号。

[![](https://wwj718.github.io/post/img/WechatIMG118.jpeg)](https://wwj718.github.io/post/img/WechatIMG118.jpeg)

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/#%E6%8C%AB%E6%8A%98)挫折

> 对我们猜想的批判极为重要：通过指出我们的错误，使我们理解我们正试图解决的那个问题的困难。就这样我们越来越熟悉我们的问题，并可能提出越来越成熟的解决：对一个理论的反驳——即对问题的任何认真的尝试性解决的反驳——始终是使我们接近真理的前进的一步 – 波普尔《猜想与反驳》

我开心地跑到隔壁办公室准备拿小黄人机器人试试。

@leeyve 和 @Leon 正在折腾它，我让他们操控机器人朝我的红外接收器发射，结果一无所获，我们的猜想似乎被实验反驳了。

[![](https://wwj718.github.io/post/img/6d39fa4a548187caeb841019047125a3.png)](https://wwj718.github.io/post/img/6d39fa4a548187caeb841019047125a3.png)

我猜想，有两种可能:

1.  机器人对战根本不是基于红外线
2.  机器人确实发射了红外线，但目前 Microblocks 插件对红外的解码机制与机器人发射红外时的编码机制不同，所以无法得到有意义的内容。

如何验证「猜想 2」呢？我需要拆开红外积木，看看它是如何处理红外信号的，和大多数积木化平台不同， Microblocks 对于深入理解事物的机制，提供了绝佳的支持，你可以在其中自由探索。

于是我打开积木的定义:

[![](https://wwj718.github.io/post/img/WechatIMG119.png)](https://wwj718.github.io/post/img/WechatIMG119.png)

[![](https://wwj718.github.io/post/img/WechatIMG119.png)](https://wwj718.github.io/post/img/WechatIMG119.png)

[![](https://wwj718.github.io/post/img/WechatIMG121.png)](https://wwj718.github.io/post/img/WechatIMG121.png)

通过查看积木定义，大致了解了其工作原理。回到我们的 「猜想 2」:

> 2 机器人确实发射了红外线，但目前 Microblocks 插件对红外的解码机制与机器人发射红外时的编码机制不同，所以无法得到有意义的内容。

为了绕开红外的解码机制，我打算直接使用定义里看到的 `读取数字引脚`。 如果小黄人确实发射红外，那么我们就会收到红外，并显示被枪击中(用于实时调试)。

[![](https://wwj718.github.io/post/img/WechatIMG123.png)](https://wwj718.github.io/post/img/WechatIMG123.png)

可行！

[![](https://wwj718.github.io/post/img/WechatIMG122.png)](https://wwj718.github.io/post/img/WechatIMG122.png)

这段代码对我们就够用了，因为我们并不关心红外信号里的内容，所以不必去解码红外信号，只关心是否有被红外射中（检测 有/无红外线 射过来）。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/#%E5%B0%84%E5%87%BB%E9%9D%B6%E5%BF%83)射击靶心 🎯

> 我们可以从他们的做法中知道他们的想法. —- Richard Hamming

现在我们已经可以通过 micro:bit 和红外传感器，接收到机器人的射击信号了。

我立即想做这样一个好玩的项目: 射击靶心 🎯

想法是这样的，驾驶机器人，调整其方位，看谁能够射中靶心。

个想法很容易扩展成各种形式的对抗赛。

我打算使用使用 舵机+红外传感器 来制作 **靶**， 当 红外传感器(放在靶心) 被击中时，舵机带动红外传感器转动，于是 **靶** 就被击倒了。

为了增加表现力，@leeyve 建议把我们[上次实验用的彩虹灯 🌈](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/)加上。 我还想把声音效果也加上， 于是我们写出了这样的代码:

[![](https://wwj718.github.io/post/img/WechatIMG125.png)](https://wwj718.github.io/post/img/WechatIMG125.png)

值得注意的是消息积木，它们是并发的！这位构建大型项目提供了强大的支持。随着项目的生长，基于消息的编程风格不会导致耦合度和复杂性骤增。

对应的 micro:bit 代码为: [gun-demo.ubp](https://wwj718.github.io/post/img/gun-demo.ubp)

项目效果如下:

<video width="80%" src="https://adapter.codelab.club/video/2c56299427a9a3e2a259e243961cb3.mp4" controls="controls"></video>

ps: 这个项目里拼凑了来自 6 家不同公司的零件！

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/#%E7%88%86%E7%82%B8-%E6%95%88%E6%9E%9C)爆炸 💥 效果

后来我想增强子弹击中的效果，Scratch 非常适合制作效果动画！ 于是我使用 Adapter 的 [micro:bit radio](https://adapter.codelab.club/extension_guide/microbit_radio/)插件，将 Microblocks 平台与 Scratch 连在一起, 相关原理我们在[上篇文章](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#%E6%8E%A5%E5%85%A5-scratch)里做了详细介绍。

接着我改编了之前[洛克人的项目](https://adapter.codelab.club/video/60022ae5a5b37699168141b8c3ef2b.mp4)。 完成后效果如下:

<video width="80%" src="https://adapter.codelab.club/video/1e171833e78af4dae3d637cc721a58.mp4" controls="controls"></video>

对应的 micro:bit 代码为: [gun-radio.ubp](https://wwj718.github.io/post/img/gun-radio.ubp)

对应的 Scratch 项目代码为: [demo](https://create.codelab.club/projects/14639/editor/)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/#%E6%80%BB%E7%BB%93)总结

从猜想、探索、实验到做出预期的项目，我们前后只花了 1 小时不到，中间有遇到困难，但没有迈不过的。我将这视为: 精心设计的创作平台增强用户心智和能力的一个例子。它提供的自由度，让我可以深入到系统的不同层面，进而可以使用多种方式来验证我的想法，当一种想法卡住时，可以快速切到另一种。它为增强可理解性而做的设计，让我在探索事物的同时，得以深入理解它们的运作机制；而当我有了新想法，它的实时交互性又鼓励我随时开始，不必想清楚再做，在编程的过程中深入自己的想法，实际上，它增强了我心智的力量。最终抵达的东西，并不是我最初想好的，在行进中抵达意料之外的境地。通常是意料之外的欢喜。

这里涉及的创作环境包括 Microblocks、Scratch、CodeLab Adapter，它们都是[个人计算](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/personal-computing/)的产物, 这个社区为支持用户更好理解世界，几十年来付出了不懈努力。

致敬个人计算的先驱们， 尤其是致敬 Alan Kay。

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: 正中靶心！](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/hit-the-bullseye/)