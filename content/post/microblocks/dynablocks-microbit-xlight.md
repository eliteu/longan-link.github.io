---
title: 'MicroBlocks 编程案例 使用 micro:bit 接管 xlight'
date: "2021-12-09"
tags:
  - MicroBlocks
slug: dynablocks-microbit-xlight
---

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#xlight)xlight

@leeyve 前些时候买到 MakeBlock 众筹的项目: [xlight](https://store.makeblock.com/pages/xlight)

[![](https://wwj718.github.io/post/img/xlight.png)](https://wwj718.github.io/post/img/xlight.png)

彩虹灯很好看，小巧而明媚。 但我不想用它的控制盒和编程软件。 不爱用图形化编程领域的大多数软件/硬件。 也许只有 scratch 是例外(硬件的话，micro:bit、树莓派很棒)。大多图形化系统，不是太愚蠢，就是自由度太低，通常，两者兼而有之。

于是我寻思着如何绕开 xlight 的软件和硬件，接管这个彩虹灯。 就像之前接管洛克人的手持装置(@leeyve 从日本带来的）:

<video width="80%" src="https://adapter.codelab.club/video/60022ae5a5b37699168141b8c3ef2b.mp4" controls="controls"></video>

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#hack-it)Hack it!

> 对计算机的访问（以及任何可能帮助你认识我们这个世界的事物）应该是不受限制的、完全的。任何人都有动手尝试的权利！ 黑客们相信，通过将东西拆开，了解它们的工作原理，并根据这种理解创造新奇的甚至更有趣的东西，可以学习到关于系统（关于世界）的重要知识。他们痛恨一切试图阻止他们这么做的人、物理障碍或者法律。 – 《黑客: 计算机革命的英雄》

黑客精神并不总是与破坏、入侵有关，在它诞生之初，更多是一种对系统的好奇，对事物的运行机制的好奇。

我对 xlight 感到好奇，对其虚构出的明媚彩虹 🌈 感到喜悦和好奇。 这好奇和一个六岁孩子面对滴答作响的时钟 ⏰ 的好奇是一样的，它曾经导致我对着散落一屋的零件流泪，现在导致我对这彩虹灯有足够的理解。

当你理解一个东西的时候，你差不多就能 hack 它，进而接管它(如果你乐意的话)，正如大多数黑客所做的那样。

简单观察之后，我猜测 xlight 的灯座里的可编程灯珠是 ws281x。

[![](https://wwj718.github.io/post/img/WechatIMG79.jpeg)](https://wwj718.github.io/post/img/WechatIMG79.jpeg)

如果确实如此，使用 micro:bit 就可以轻松接管。

我使用 microblocks 做了个实验，果真如此。

[![](https://wwj718.github.io/post/img/WechatIMG82.jpeg)](https://wwj718.github.io/post/img/WechatIMG82.jpeg)

> You just do it and it’s done. – Daniel Ingalls 《The Evolution of Smalltalk》

简而言之，我把 xlight(不包括编程板) 接到 micro:bit 上，打开 microblocks 进行编程，顺利接管。进而可以在 [CodeLab Scratch](https://create.codelab.club/) 上对 xlight 进行实时编程，使用到了 CodeLab Adapter 的 micro:bit radio 插件。

以下是一些简单的实验代码。我用杜邦线将 xlight 接到 micro:bit 1 号引脚(扩展版选用了[恩孚](https://learn-cn.readthedocs.io/en/latest/)的，恩孚目前是[英荔](https://aimaker.space/)的合作伙伴)。

[![](https://wwj718.github.io/post/img/WechatIMG78.jpeg)](https://wwj718.github.io/post/img/WechatIMG78.jpeg)

在 microblocks 积木库里打开 NeoPixel 插件库:

[![](https://wwj718.github.io/post/img/01a0adfaff24864b17323376bbe66b43.png)](https://wwj718.github.io/post/img/01a0adfaff24864b17323376bbe66b43.png)

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#john-maloney)John Maloney

> 如果一个系统对于孩子来说是好的，那么对专业人士通常也是好的。 – Alan Kay

MicroBlocks 是 [John Maloney](https://harc.ycr.org/member/john_maloney/) 最新的项目。John Maloney 是[我最喜欢的计算机科学家之一](https://wwj718.github.io/post/%E9%9A%8F%E7%AC%94/aboutme/)。

他是 [Alan Kay 意义上的科学家](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/software-art-engineering-mathematics-or-science/)，而不是我们今天使用这个词所指的。

[![](https://gpblocks.org/images/gallery/Warhol.png)](https://gpblocks.org/images/gallery/Warhol.png)

> 牛顿说他站在巨人的肩膀上看得更远，而计算机科学家却经常站在对方的脚趾上。 – Alan Kay 《The Early History Of Smalltalk》

John Maloney 之前是 Scratch 的首席架构师，对图形/实时交互式编程系统有深入思考，或许是这个领域最出色的思考者之一， 他过去的工作包括:

-   morphic
-   etoys
-   Scratch
-   GPblocks

MicroBlocks 基于 GPblocks，完全在图形化环境里构建，GPblocks 是[自举](https://en.wikipedia.org/wiki/Bootstrapping_(compilers))的图形化编程语言！在图形化环境里实现了自己的编译器！

John Maloney 目前的精力都在 MicroBlocks，他说之后可能会重新回到 GPblocks。

如果你认真看看 John Maloney 的工作，简直要怀疑目前整个领域在瞎搞什么玩意儿(这里包括微软的 MakeCode 和 Google 的 Blockly)。这些项目既没有新的想法，对过去的好想法也是一无所知，结果就是一些乱七八糟的随意拼凑。

今天的编程入门通常与图形化有关，考虑到这个领域的设施是如此之差(主流领域可能只有 Scratch 是例外)。 以至于我们要怀疑孩子们究竟在学些什么东西，我想今天所谓的编程学习，主要是在努力掌握工程师/产品经理们的糟糕想法。这些想法内化在他们的软件平台里、内化学习材料里、内化在考试题目上。

按 Alan Kay 的说法，软件工程领域忙着生产 ‘一次性塑料垃圾’。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#why-microblocks)why Microblocks?

> 现代的编程就如同闭着眼睛去排列符号一样 – Bret Victor
> 
> 在编译和运行一个程序之前，我完全看不到程序会输出一个怎样的结果，我需要在脑内想象并且根据我所想象的图像去修改程序。为了得出我想要的输出我需要一直不停地修改、编译和运行, 反反复复。而且有些时候，当我完全不明白程序为什么没有像我所期望的那样去运行的时候，我就要逐行检查, 使用各种 debug 技巧。 – David Luo

扔掉 [MakeCode](https://makecode.microbit.org/) 之类的东西, 别再闭着眼睛排列积木了。睁开双眼，在 Microblocks 里以可理解的方式实时编程吧！

可悲的是，MakeCode 已经是目前与硬件有关的图形化编程工具里最好的了。

我在[两种硬件编程风格的比较](https://wwj718.github.io/post/%E5%B0%91%E5%84%BF%E7%BC%96%E7%A8%8B/hardware-programming-style/)提到两种典型的风格:

-   灌入式
-   交互式

这两者差异很明显，各自的优点也很明显，所以目前主流领域，两个阵营都有很多拥趸。

John Maloney 的壮举之一是， 在 MicroBlocks 里统一了两者！让交互式的 **可理解性** 和离线的 **实时性** 可以兼得！这是整个领域梦寐以求的特性。 MicroBlocks 通过沿用 Smalltalk 的架构风格实现这一壮举。

MicroBlocks 的另一壮举是为用户提供了一个高度易用和 **自由** 的环境，MicroBlocks 是自支持和可生长的，这是个人计算社区一直以来追求的特质(CodeLab Adapter 也努力追求这一特质)， 在这样的环境中，终端用户拥有高度自由，能够以一致的方式思考系统的各个层次，进而深入理解系统并进行自由创造。 如果你对这种对创造友好的系统感兴趣，可参考[Smalltalk 背后的设计原则](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/design-principles-behind-smalltalk/)

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#hello-microblocks)hello Microblocks

在 Microblocks 中对 micro:bit 编程是非常简易的，就像使用 Scratch 一样简易直观(因为都是 John Maloney 的设计)。

你只需要:

1 使用 Chrome/Edge 浏览器打开 [Microblocks](https://microblocks.fun/) 2 为 micro:bit 刷入固件，也可以下载[固件](https://adapter.codelab.club/hex/dynablocks-microbit.hex)，手动拖到 micro:bit 里

[![](https://wwj718.github.io/post/img/a14605254b94e39613ed35ed01cbb679.png)](https://wwj718.github.io/post/img/a14605254b94e39613ed35ed01cbb679.png)

3 连接 micro:bit

[![](https://wwj718.github.io/post/img/54ab4086970464ca47b68901b8550f42.png)](https://wwj718.github.io/post/img/54ab4086970464ca47b68901b8550f42.png)

4 开始编程！

乍看起来和 makecode 区别不大，而且 makecode 的界面好像还要更好看些(我怀疑 makecode 的大多数精力都集中在这些微创新上)，一旦你完成 hello world 阶段，开始构建真实的项目，就会发现它们的天壤之别。

如果你有过 Scratch 的经验，你对于系统的 ‘活性’ 会有很高期待，你不再乐意闭着眼睛排列东西了，你期待系统给你的实时反馈，这些反馈支持或反对你脑子里的假设(就像物理学家期待在实验中获得物理世界的反馈，以[校准观点](https://wwj718.github.io/post/%E8%AF%BB%E4%B9%A6/the-structure-of-scientific-revolutions/))，如此一来，与系统的每次实时交互。都让你理解系统那些不可见的部分。 microblocks 在硬件编程领域完全提供了和 Scratch 一样的体验，你甚至可以使用消息！它还默认支持多任务！

> The challenge is not buiding it, but understanding it – Bret Victor 《Seeing Spaces》
> 
> We make, not just to have, but to know – Alan Kay 《The Early History Of Smalltalk》

由于系统的’活性’， 在 microblocks 里，对探索式编程提供了绝佳支持，’理解’发生在探索的过程中，而不是理解以后再去编程，这是一种帮助你理解事物的系统，而不只是做出某个东西的系统。

换句话说，它是支持「建构主义」的系统。

编程应该成为理解事物的方式，正如写作是一种理解事物的方式。动态媒介(计算机)最强大的潜力之一在于，对过程中的思考提供强大支持，甚至超越写作(基于静态媒介)所能提供的支持。目前仅有个人计算社区展示了计算机的这种潜力。

我前几天在 microblocks 里玩 micro:bit 的一个下午，比我过去学到的任何关于 micro:bit 的东西都多。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#%E6%8E%A5%E5%85%A5-scratch)接入 Scratch

以下是我把 xlight 接管到 CodeLab Scratch 之后，做的一个演示视频。

<video width="80%" src="https://adapter.codelab.club/video/debfcc908a12e725f314ff360a03a8.mp4" controls="controls"></video>

思路是这样的: 在 microblocks 里为 micro:bit 写一个固件，这个固件让 micro:bit 响应来自外部的 radio 消息，并解释这些消息(Smalltalk风味)。Scratch 通过另一块 micro:bit 使用 radio 给前边的 micro:bit 发送消息。 对 xlight 的控制语义编码在 radio 消息里。

对的，这正是 CodeLab Adapter 的 micro:bit radio 插件的工作原理。 [插件文档](https://adapter.codelab.club/extension_guide/microbit_radio/)有详细说明，过去我们使用 makecode 构建固件(功能板)，现在我们用 microblocks 替代它，所有东西都兼容(中转站(天线)无需任何修改)！因为消息是松耦合的！这是可生长系统的一个例子。可生长的系统能够轻松接入来自未来的新事物，尽管它自己是在过去构建的。

以下是固件代码(当代码达到一定复杂度时，microblocks 的优势就越发明显，因为它提供了强大的抽象构件和交互式支持，编程十分愉快):

[![](https://wwj718.github.io/post/img/60a22ab7bb10756e2cf2ac2dff9aa280.png)](https://wwj718.github.io/post/img/60a22ab7bb10756e2cf2ac2dff9aa280.png)

你也可以自行下载使用: [xlight-node.ubp](https://wwj718.github.io/post/img/xlight-node.ubp)。 下载之后，可以在 microblocks 里打开程序:

[![](https://wwj718.github.io/post/img/607d34a7368e4305459c25d374a2a1e4.png)](https://wwj718.github.io/post/img/607d34a7368e4305459c25d374a2a1e4.png)

Scratch 中的 [demo 链接](https://create.codelab.club/projects/19697/editor/)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#%E9%99%84%E5%BD%95)附录

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#%E5%9C%A8-microblocks-%E9%87%8C%E6%97%A0%E6%B3%95%E8%BF%9E%E6%8E%A5-micro-bit)在 microblocks 里无法连接 micro:bit？

重新插拔 micro:bit

确保只有当前一个 microblocks 页面，关掉可能占用串口的页面: microblocks、MakeCode

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#microblocks-%E4%B8%8E-scratch-micro-bit-radio-%E9%80%9A%E4%BF%A1%E7%9A%84%E5%9F%BA%E7%A1%80%E6%A8%A1%E7%89%88)microblocks 与 Scratch(micro:bit radio) 通信的基础模版

[![](https://wwj718.github.io/post/img/f1f36ea55522f64b35e1f7671fc3d7f2.png)](https://wwj718.github.io/post/img/f1f36ea55522f64b35e1f7671fc3d7f2.png)

[xlight-node.ubp](https://wwj718.github.io/post/img/node-template.ubp)

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#gpblocks-%E7%9A%84%E6%96%87%E6%9C%AC%E5%BD%A2%E5%BC%8F)GPblocks 的文本形式

GPblocks 的文本形式与图形形式是等价的。

GPblocks 的文本代码类似 Smalltalk72， Smalltalk72 参考了 Logo ，而 Logo 来自 LISP。

GPblocks 惊人的可组合性和实现之简单，很大程度与 LISP 的表现力有关。

以下是 microblocks 的编译器代码片段：

[![](https://wwj718.github.io/post/img/ed26faaebf76122e6704f779e8137c23.png)](https://wwj718.github.io/post/img/ed26faaebf76122e6704f779e8137c23.png)

LISP 风味十足！

使用 `to` 定义函数的风格的语法则来自 Smalltalk72

[![](https://wwj718.github.io/post/img/91053287b86d47e304a4d0c68b38c3fd.png)](https://wwj718.github.io/post/img/91053287b86d47e304a4d0c68b38c3fd.png)

Smalltalk72 是从 Logo 里学来的语法。这种定义语法与英语中的动词定义类似: `to speed is to drive fast`。这是 Logo 为可理解性做的努力(符合直觉)，也是个人计算社区一贯的做法。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#%E8%8B%B1%E8%8D%94%E6%AF%94%E7%89%B9)英荔比特

英荔比特里的所有设备在 microblocks 里应该都可用，其中的大多数我都做了测试。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#%E5%85%A5%E9%97%A8%E4%B8%8E%E6%B7%B1%E5%85%A5)入门与深入

[microblocks 官网](https://microblocks.fun/)给出了很多好的材料:

-   [get started](https://microblocks.fun/get-started)
-   [learn](https://microblocks.fun/learn)
-   [wiki](https://wiki.microblocks.fun/)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/#%E5%8F%82%E8%80%83)参考

-   [两种硬件编程风格的比较](https://wwj718.github.io/post/%E5%B0%91%E5%84%BF%E7%BC%96%E7%A8%8B/hardware-programming-style/)
-   [CodeLab 纪事#批评过分看重积木的视角](https://www.codelab.club/blog/2021/06/28/codelab%E7%BA%AA%E4%BA%8B#%E6%89%B9%E8%AF%84%E8%BF%87%E5%88%86%E7%9C%8B%E9%87%8D%E7%A7%AF%E6%9C%A8%E7%9A%84%E8%A7%86%E8%A7%92)
-   [xlight](https://store.makeblock.com/pages/xlight)
-   [gpblocks](https://gpblocks.org/about/)
-   [John Maloney](https://harc.ycr.org/member/john_maloney/)
-   [MicroBlocks](https://microblocks.fun/)
-   [Bootstrapping](https://en.wikipedia.org/wiki/Bootstrapping_(compilers))
-   [软件: 艺术，工程，数学还是科学？](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/software-art-engineering-mathematics-or-science/)
-   [CodeLab Adapter 深度连接 micro:bit （makecode）生态](https://www-old.codelab.club/blog/codelab-adapter-microbit-deep-connect/)
-   [Personal Dynamic Media](http://www.newmediareader.com/book_samples/nmr-26-kay.pdf)
-   [Personal Computing](http://worrydream.com/refs/Kay%20-%20Personal%20Computing.pdf)

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例 使用 microbit 接管 xlight](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-microbit-xlight/)