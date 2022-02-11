---
title: 'MicroBlocks 编程案例: 创建 Sphero RVR 代码库'
date: "2022-01-24"
tags:
  - MicroBlocks
slug: microblocks-sphero-rvr-library
---

[![](https://adapter.codelab.club/img/rvr_bar.jpeg)](https://adapter.codelab.club/img/rvr_bar.jpeg)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E5%89%8D%E8%A8%80)前言

由于科技节项目和 @yinxi 的演示项目都使用到了 Sphero RVR, 于是我想接管 RVR.

CodeLab Adapter 之前已经[接入了 RVR (通过蓝牙)](https://adapter.codelab.club/extension_guide/spheroRVR/)，windows 下连接蓝牙有时会连不上(跟 RVR 的蓝牙服务本身也有关)，体验不好。最近我们都很喜欢 MicroBlocks， 于是我想在 MicroBlocks 里实现 Sphero RVR 驱动.

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E6%80%9D%E8%B7%AF)思路

查看 [Sphero RVR 的官方文档](https://sdk.sphero.com/microbit), 了解到 Sphero RVR 已经有 [MicroPython 驱动](https://github.com/sphero-inc/sphero-sdk-microbit-python/blob/master/sphero.py)和 [MakeCode 驱动](https://github.com/sphero-inc/sphero-sdk-microbit-makecode)了。

于是我打算将 [MicroPython 驱动](https://github.com/sphero-inc/sphero-sdk-microbit-python/blob/master/sphero.py) 翻译到 MicroBlocks，然后作为一个库(library) 提交给官方。

阅读 [MicroPython 驱动](https://github.com/sphero-inc/sphero-sdk-microbit-python/blob/master/sphero.py)代码，可以发现是通过 UART 来通信。我试着使用 MicroBlocks 的串口积木来实现目标。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E5%9B%B0%E5%A2%83)困境

结果遇到了困难，在 MicroBlocks 对 micro:bit 编程, 使用串口积木发送的数据，和通过 USB 端口收到的数据([comtool](https://github.com/Neutree/COMTool))不一致。 我[将问题提交到了 MicroBlocks 项目 issue](https://bitbucket.org/john_maloney/smallvm/issues/235/a-problem-about-serial-write) ，得到 MicroBlocks 几位作者的热心帮助。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E7%83%AD%E5%BF%83%E5%B8%AE%E5%8A%A9)热心帮助

从 @Turgut Guneysu 那儿了解到 USB 端口服务于 MicroBlocks IDE 本身的功能([协议细节](https://bitbucket.org/john_maloney/smallvm/src/master/misc/SERIAL_PROTOCOL.md))。 @John Maloney 提议将 micro:bit V2 的 0/1 pin 用于 UART 引脚，然后直接用引脚接管 RVR(因为树莓派就是这样驱动 RVR 的)。

我采取了 @John Maloney 的建议，顺利在 MicroBlocks 里完成了 Sphero RVR 的驱动。并提了 [Pull Request](https://bitbucket.org/john_maloney/smallvm/pull-requests/16/add-sphero-rvr-library-complete-all), @John Maloney 说下个 MicroBlocks 版本(Pilot release)会内置我的 RVR 驱动。

意外的是，使用这种方式，比使用 UART over USB 更优！因为不需要反复插拔 USB 来调试，可以一直将板子连在电脑上，实时编程！当然，需要把 RVR 轮子垫高，不然它会跑掉：）

在 MicroBlocks 的下个版本发布之前，如果你想提前使用 Sphero RVR 库，可手动下载: [Sphero-RVR.ubl](https://wwj718.github.io/post/img/Sphero-RVR.ubl)

可以从这里加载自定义的代码库:

[![](https://wwj718.github.io/post/img/1643008474077.jpg)](https://wwj718.github.io/post/img/1643008474077.jpg)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E7%A1%AC%E4%BB%B6%E8%BF%9E%E6%8E%A5)硬件连接

硬件连接比较简单，将控制板（我是用的是 micro:bit V2）和 RVR 的 UART 引脚连在一起即可(RX/TX交叉), 可参考以下信息

-   [special pins](https://wiki.microblocks.fun/special_pins)
-   [Extra Notes on Using RVR’s UART](https://sdk.sphero.com/docs/getting_started/before_you_start/uart_disclaimer?_ga=2.142002423.456035690.1642714022-290583108.1638474910)

[![](https://wwj718.github.io/post/img/WechatIMG166.jpeg)](https://wwj718.github.io/post/img/WechatIMG166.jpeg)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#sphero-rvr-%E4%BB%A3%E7%A0%81%E5%BA%93)Sphero RVR 代码库

我将 Sphero RVR 驱动构建为一个 [MicroBlocks 代码库(library)](https://microblocks.zhubai.love/posts/2088166608488710144) 。

如果你之前用过 MakeCode 或 Scratch，可以把一个 「MicroBlocks 代码库」看作一个 MakeCode/Scratch 扩展(extension)。

但不同在于， MicroBlocks 代码库是在 MicroBlocks 平台上构建的！不需要绕到底层去实现！这是[受 Smalltalk 影响产生特质](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/swimming-with-the-fish/)。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E6%8E%A5%E5%85%A5%E6%9B%B4%E5%A4%9A%E7%A1%AC%E4%BB%B6)接入更多硬件

由于接入新硬件工作非常典型(为新的硬件写驱动库)，围绕这块我想多写几句。

MicroBlocks 支持主流的硬件协议: I2C、UART、SPI 等 ，所以我们可以把海量硬件接入 MicroBlocks 中。

MicroBlocks 平台还有个非常有趣的特性，大多数驱动是通用的！在无需修改任何代码的情况下，可以运行在多种硬件上！诸如我构建的 Sphero RVR 驱动，不仅可以用于 micro:bit, 也可用于任何已接入 MicroBlocks 且带有 UART 支持的板子。而大多数板子都有 UART 支持！

如果你打算基于这些主流硬件协议，将更多硬件接入到 MicroBlocks，可展开[Sphero-RVR.ubl](https://wwj718.github.io/post/img/Sphero-RVR.ubl)积木块，查看它的定义(无需进入「下层」)。这是在 MicroBlocks 深入学习的极佳方式:

[![](https://wwj718.github.io/post/img/1643008303372.jpg)](https://wwj718.github.io/post/img/1643008303372.jpg)

以上代码翻译自 [MicroPython 驱动](https://github.com/sphero-inc/sphero-sdk-microbit-python/blob/master/sphero.py#L25)

我在 MicroBlocks 里的大多数技能都是这样学会的，通过理解正在使用的功能块的原理(展开定义)来学习。在同个编程环境中，可以进入到不同的抽象层里学习！这同样源自[Smalltalk 的理念](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/design-principles-behind-smalltalk/):

> 如果一个系统要发挥创造精神，那么个体必须完全可以理解该系统。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E5%B0%8F%E6%8A%80%E5%B7%A7)小技巧

MicroBlocks 代码的积木形式和文本形式可以精确地双向转化。所以你愿意，也可以使用代码编辑器编辑文本代码。

当我想导出 [Sphero-RVR.ubl](https://wwj718.github.io/post/img/Sphero-RVR.ubl) 时，想调整积木顺序，发现目前积木 IDE 里做不到。我是在文本代码里做的。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E6%BC%94%E7%A4%BA)演示

我们来看看使用 Sphero RVR 代码库驱动的 demo:

<video width="80%" src="https://adapter.codelab.club/video/1642742641707728.mp4" controls="controls"></video>

@John Maloney 很喜欢这个演示，把视频转给了 Micro:bit 基金会的 Katie Henry。

> Katie loved the video and asks if it would be okay to share it with other at the Micro:bit Foundation and Sphero team. Is that okay? She can cc you if you like. – John Maloney

很开心大家喜欢这些工作 :)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/#%E7%82%AB%E8%80%80)炫耀

@John Maloney 是我最喜欢的计算机科学家之一。

> 对计算机抱有跟当前领域不同的愿景, 追随 个人计算(Personal Computing) 的理想, 希望成为 Alan Kay、Daniel Ingalls、John Maloney 和 Bret Victor 的后继者 – 种瓜 [aboutme](https://wwj718.github.io/post/%E9%9A%8F%E7%AC%94/aboutme/)

我跟 John 分享了我在 MicroBlocks 上做的更多项目，很开心得到他的认同。

> Your videos are really amazing! Are you a teacher? If so, what ages do you teach? Your blog posts are really amazing! – John Maloney

也很开心 @Bernat Romagosa 喜欢「玩给你看」里的所有视频

> WOW! I’ve watched all of your videos and I loved them all. – Bernat Romagosa

几年前设计 CodeLab Adapter 时，想法来源之一就是 @Bernat Romagosa 的 S4A 项目

-   [How to create a library](http://wiki.microblocks.fun/create_a_library)
    -   [如何在 MicroBlocks 中创建代码库](https://microblocks.zhubai.love/posts/2088166608488710144)
-   [My Blocks](https://wiki.microblocks.fun/reference_manual/myblocks)
    -   [MicroBlocks 积木块使用说明 – 自定义积木](https://microblocks.zhubai.love/posts/2094173059597713408)
-   [special pins](https://wiki.microblocks.fun/special_pins)
-   [Extra Notes on Using RVR’s UART](https://sdk.sphero.com/docs/getting_started/before_you_start/uart_disclaimer?_ga=2.142002423.456035690.1642714022-290583108.1638474910)

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: 创建 Sphero RVR 代码库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/)