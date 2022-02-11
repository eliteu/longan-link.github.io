---
title: 'MicroBlocks 编程案例: MQTT 库'
date: "2022-02-08"
tags:
  - MicroBlocks
slug: microblocks-mqtt-library
---

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E5%89%8D%E8%A8%80)前言

CodeLab 候车群里吸引了一些「个人计算」爱好者，他们容易被「个人计算」气质的项目吸引。从 Scratch、Smalltalk 到 MicroBlocks.

最近群里讨论 MicroBlocks 十分起劲，过年期间，也玩得不亦乐乎， @kwyjibo 做了很棒的[梳理](https://kwyjibo.notion.site/MicroBlocks-f8af6fd0a8ea4786bf267bf948ae2ea1)。

年前大家在「CodeLab 候车群」聊到对 MicroBlocks 有一些疑问，其中一个问题是，MicroBlocks 何时支持 MQTT？我把问题转给了 @John Maloney，他说在他那边 MQTT 优先级比较低，问我是否有兴趣来做。我答应他试试看。

我担心超出我的能力范围，结果发现，这工作比我预期的简单，假期里完成了 MQTT 库，目前已提交给官方并合并到了 dev 分支。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E6%80%9D%E8%B7%AF)思路

为 MicroBlocks 构建 MQTT 库，比我构建之前的几个库来得复杂:

-   [MicroBlocks 编程案例: 创建 Sphero RVR 代码库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/)
-   [MicroBlocks 编程案例: 创建哪吒扩展板代码库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/)
-   [MicroBlocks 编程案例: 创建 AI 摄像头代码库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/)

构建 MQTT 库与构建 传感器/执行器 库(通常是 I2C 协议)的区别在于: 后者只需要在 IDE 里编程(摆弄硬件协议，通常是 I2C)，换句话说，只需要拼搭积木即可，而构建 MQTT 库则需要在[虚拟机(vm)](https://bitbucket.org/john_maloney/smallvm/src/master/vm/)层面编程。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#microblocks-%E8%99%9A%E6%8B%9F%E6%9C%BA)MicroBlocks 虚拟机

MicroBlocks 虚拟机是 MicroBlocks 如此卓越的主要原因之一。了解 Smalltalk 架构的朋友，可能会在这里看到 Smalltlak 的影子。 这并不偶然，MicroBlocks 的创始人之前也是 Squeak 的核心开发者(Morphic 的发明者)。

MicroBlocks 虚拟机主要使用 C/C++ 构建，但并不意味着你有丰富的 C 经验，就能很好理解它，因为

> 架构主宰材料. – Alan Kay

[The MicroBlocks Virtual Machine](https://wiki.microblocks.fun/virtual_machine) 是了解 MicroBlocks 虚拟机设计思路的好材料。当我们使用 MicroBlocks 对硬件编程时，我们处在 MicroBlocks IDE 环境里，这 IDE 环境由 GPblocks 构建(@John 的另一个项目)，编程过程看不见虚拟机，也不需要关心它，它只是默默地解释来自 IDE 的消息。IDE 与 MicroBlocks 虚拟机实时通信（串行协议），通信协议细节参考 [SERIAL\_PROTOCOL](https://bitbucket.org/john_maloney/smallvm/src/master/misc/SERIAL_PROTOCOL.md)。虽然看起来像是 Scratch 一样的交互式编程，但实时烧录，可以离线运行！

MicroBlocks IDE 只是 MicroBlocks 虚拟机的一个客户端，理论上我们可以构建任何兼容 [SERIAL\_PROTOCOL](https://bitbucket.org/john_maloney/smallvm/src/master/misc/SERIAL_PROTOCOL.md) 的客户端/编程环境。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E6%8A%95%E6%9C%BA%E5%8F%96%E5%B7%A7)投机取巧

我并不打算完全弄懂 MicroBlocks 虚拟机，再来构建 MQTT 库，那样耗费的时间太多了。

MicroBlocks 中已经有那么多代码库，找一个最接近 MQTT 库的东西来参考，依葫芦画瓢不是更简单吗？

MicroBlocks 中代码库的分类十分清晰，由于 MQTT 库与网络有关，我自然去寻找网络相关的库，看看它们在虚拟机里的部分是怎么写的，很快我发现 WebSocket Server 库正是我需要的东西。

[![](https://wwj718.github.io/post/img/1644310978158.jpg)](https://wwj718.github.io/post/img/1644310978158.jpg)

粗略翻了下代码, 感觉相当简单啊！ 即使我几乎不写 Arduino 代码，而且多年不写 C 代码(可能只是入门水平)，感觉也没什么压力。毕竟编程主要与想法有关，其他缺啥补啥就好。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E5%8A%A8%E6%89%8B)动手

先抛出[相关源码](https://bitbucket.org/john_maloney/smallvm/commits/0f8ac4b9638ca935e815b102c2e4570e670f1cc9)，再细聊我的具体做法。

通过阅读 WebSocket Server 库的实现，我了解到大约需要做以下几件事:

-   在 Arduino 生态里找一个 MQTT 库(不就是在 Github 搜一搜吗？)
-   在 MicroBlocks 虚拟机里([netPrims.cpp](https://bitbucket.org/john_maloney/smallvm/src/master/vm/netPrims.cpp)) 提供 MQTT 相关的 primitives 方法( Smalltalk 虚拟机也是这么做的 )
-   在 IDE 里构建 MQTT 积木， 调用虚拟机里相关的 primitives 方法

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E5%9C%A8-arduino-%E7%94%9F%E6%80%81%E9%87%8C%E6%89%BE%E4%B8%80%E4%B8%AA-mqtt-%E5%BA%93)在 Arduino 生态里找一个 MQTT 库

这事儿简单，很快我就找到一个好东西: [256dpi/arduino-mqtt](https://github.com/256dpi/arduino-mqtt), 不止兼容 Arduino，而且可在 [PlatformIO](https://platformio.org/) 里使用, 而 MicroBlocks 虚拟机正是使用 PlatformIO 构建的，PlatformIO 或许是硬件领域最好的编程平台。

[![](https://wwj718.github.io/post/img/1644303641425.jpg)](https://wwj718.github.io/post/img/1644303641425.jpg)

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E5%9C%A8-microblocks-%E8%99%9A%E6%8B%9F%E6%9C%BA%E9%87%8C%E6%8F%90%E4%BE%9B-mqtt-%E7%9B%B8%E5%85%B3%E7%9A%84-primitives-%E6%96%B9%E6%B3%95)在 MicroBlocks 虚拟机里提供 MQTT 相关的 primitives 方法

这是整件事情中最难的一步。

但好在 MicroBlocks 是一个持续几年的项目了，成熟度已经很高。大多数工作，都可以学习项目里之前的工作来开始。这正是开源革命带来的最大好处之一: 从别人的代码里快速学习和成长。

通过学习 @John 在 WebSocket Server 库上的工作，我用了很短时间就完成了可以工作的第一个原型，当时还是年前，我告知了 @John 最新进展，他也很开心:

> Wow, that was fast work! Enjoy your New Year’s holiday! Happy Year of the Tiger! – @John Maloney

这期间，我甚至是一边翻[C 语言文档](https://learnxinyminutes.com/docs/c/), 一边编程的。说实话，我相当不喜欢 C 里边的一些设计决策，一种你一直在适应机器的不愉快感受。

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E5%90%90%E6%A7%BD)吐槽

我在 C 语言上其实是投入过一些时间的，抛开大学扯淡的 C 语言入门和计算机二级考试不算(他妈简直是一场灾难)，我也阅读 C 语言作者写的《The C Programming Language》，读过 Unix 文化的很多东西(包括《Unix 编程艺术》)，甚至读过 C++ 社区推荐的一些书。但如果你接触过 LISP/Smalltalk 之类的东西，尤其是接触过个人计算社区里的人写的东西(Alan Kay、Dan Ingalls 之类)，你就很难有耐心去阅读主流计算机世界里这些据说写得挺好的书 :(

> 语言并不是你学到的东西，而是你参与的东西 – Arika Okrent

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E5%9C%A8-ide-%E9%87%8C%E6%9E%84%E5%BB%BA-mqtt-%E7%A7%AF%E6%9C%A8-%E8%B0%83%E7%94%A8%E8%99%9A%E6%8B%9F%E6%9C%BA%E9%87%8C%E7%9B%B8%E5%85%B3%E7%9A%84-primitives-%E6%96%B9%E6%B3%95)在 IDE 里构建 MQTT 积木， 调用虚拟机里相关的 primitives 方法

最后一步十分简单，现在还要更简单！

我当时的做法是在文本环境里编程，因为之前没有直接调用 primitives 方法的积木。但现在有了！在 MicroBlocks 图形 IDE 里可以直接调用虚拟机里的 primitives 方法！参考[这个 issue](https://bitbucket.org/john_maloney/smallvm/issues/246/is-there-any-example-of-call-block-to-call)

具体做法参考[相关源码](https://bitbucket.org/john_maloney/smallvm/commits/0f8ac4b9638ca935e815b102c2e4570e670f1cc9)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E4%BD%BF%E7%94%A8)使用

如果你想要开箱即用，可以使用 [CodeLab 部署的版本](https://microblocks.codelab.club/), 如果你之前打开过，请刷新缓存。

由于 MQTT 库 还未经过充分测试(@John 提到网络相关的库，因其动态性，相当不好测试)，目前官方虚拟机还未默认包含 MQTT 支持。 @John 说:

> I’m being conservative because RAM is a scare resource. On the ESP32, the MicroBlocks VM is stored in RAM (including all libraries that are linked in), so even if the MQTT primitives are not used, the C++ MQTT library code consumes RAM. When RAM gets too low on the ESP32, then network operations can become unreliable and MicroBlocks itself can crash…  
> It is difficult to know how much RAM a complex network package needs because they allocate memory dynamically and the amount needed depends on the usage patterns and load.

目前你需要自己 [build 固件](https://bitbucket.org/john_maloney/smallvm/src/0d4abb14608a836c8fb0c2d1b20045f7388ff929/platformio.ini#lines-121)。但放心它相当容易: `pio run -e esp32-mqtt` 。 如果要上传到板子上(目前仅支持 ESP32)，只需: `pio run -e esp32-mqtt -t upload`。 @Tom Ming 提到他在 esp8266 下测试也是正常的。他这个学期计划在教学中使用。

官方 IDE 里已经提供了 MQTT 积木:

[![](https://wwj718.github.io/post/img/1644310626832.jpg)](https://wwj718.github.io/post/img/1644310626832.jpg)

我还添加了一个 Demo:

[![](https://wwj718.github.io/post/img/1644310656103.jpg)](https://wwj718.github.io/post/img/1644310656103.jpg)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/#%E6%80%BB%E7%BB%93)总结

为 MicroBlocks 虚拟机引入新的 primitives 方法十分简单！一旦掌握这种做法，你就可以轻松将 Ardoino 生态里的海量库轻松引入 MicroBlocks！


---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: MQTT 库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-mqtt-library/)