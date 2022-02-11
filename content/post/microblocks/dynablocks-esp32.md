---
title: 'MicroBlocks 编程案例: ESP32'
date: "2022-01-08"
tags:
  - MicroBlocks
slug: dynablocks-esp32
---

> 使用 smalltalk 编程，不需要掉头发和眼泪 –Alan Kay

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#esp32)ESP32

手头有几块 ESP32 板子（恰好都是[ESP-WROOM-32 表面贴装模块](https://en.wikipedia.org/wiki/ESP32)）

[![](https://wwj718.github.io/post/img/WechatIMG5263.jpeg)](https://wwj718.github.io/post/img/WechatIMG5263.jpeg)

> ESP32 是一系列低成本，低功耗的单片机微控制器，集成了 Wi-Fi(2.4G) 和双模蓝牙。 – [维基百科](https://en.wikipedia.org/wiki/ESP32)

ESP32 在 maker 群体中广受欢迎，有庞大的生态(在 Github 搜索 ESP 试试)。它也深受硬件公司的欢迎，近年推出的编程或智能设备，如果它们带有蓝牙或 WIFI 功能，那么很有可能采用了 ESP32。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#microblocks-%E6%94%AF%E6%8C%81-esp32)Microblocks 支持 ESP32

Microblocks 支持 ESP32, 我尤其关注 Microblocks 对 ESP32 WIFI 能力的利用 (目前还不支持蓝牙)，至于 I2C、SPI 等硬件领域主流协议，Microblocks 当然也都是支持的。

一些有趣的用例是:

-   接管市面上的 WIFI 智能家居设备([esphome](https://esphome.io/)梳理了一些内置 esp32 控制器的智能家居设备，Sonoff 等，价格便宜)
-   使用 WebThings 库，构建兼容 [wot 协议](https://iot.mozilla.org/wot/)的 Things (wifi)，继而允许自下而上地自定义和扩展[可编程空间](https://codelab.club/blog/2020/04/29/%E5%8F%AF%E7%BC%96%E7%A8%8B%E7%A9%BA%E9%97%B4/)。
-   使用 HTTP/WebSocket server 库，构建简易灵活的 WIFI 设备，诸如我们在[谁先拔枪](https://www.codelab.club/projects#%E8%B0%81%E5%85%88%E6%8B%94%E6%9E%AA)中的灯带装置(当时直接使用了淘宝上的 WIFI 驱动器)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#%E4%B8%80%E4%BA%9B%E4%BE%8B%E5%AD%90)一些例子

分享几个我这几天折腾的东西。

如果你对 microblocks 不熟悉，可通过[官网资料](https://microblocks.fun/learn)学习。 中文资料推荐阅读 kwyjibo 的 [翻译文章](https://microblocks.zhubai.love/)。

ESP32 的使用文档: [microblocks wiki ESP32](https://wiki.microblocks.fun/boards/ESP32)

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#bpi-bit)BPI-Bit

BPI-Bit 是香蕉派推出的一款 STEAM 教育开发板, 采用 ESP32 作为主控模块。

由于使用「掌控板」和 「m5stack」 的体验 **非常糟心** ，我这几天大多在使用 BPI-Bit 和淘宝上的廉价 ESP32 开发板做实验。

虽然 microblocks 内置 ESP32 固件，适用于大多数 ESP32 板子，但由于每个板子的引脚布局与预定义功能(直接连到板子的传感器上)不尽相同，所以刷入固件后的下一件事，是弄清楚板子的引脚布局情况， 以 BPI-Bit 来说，可参考: [bpi:bit 开源硬件 📓](https://github.com/BPI-STEAM/BPI-BIT-Hardware/blob/master/readme.md), 大多数开源硬件，都会给出类似的说明。弄懂了引脚，我们就可以在 microblocks 里对其编程了。

如果你按照 [microblocks wiki ESP32](https://wiki.microblocks.fun/boards/ESP32) 操作，在使用一些 ESP32 板子时，可能会遇到无法烧录或无法连接的情况(可能是出于保护目的做的限制)，我的经验是，连接或刷入时，按住板子上的某个按键，然后松开, 或者拔下板子，刷新页面再试试。

这是我在 microblocks 对 ESP32 编程的初体验:

<video width="80%" src="https://adapter.codelab.club/video/9cbd58237c1443290a2fe955090e81.mp4" controls="controls"></video>

以上视频对应 microblocks 项目: [bpi-bit-demo.ubp](https://wwj718.github.io/post/img/bpi-bit-demo.ubp)

值得注意的是，在 microblocks 编程时感受到的灵活性，这灵活性在于，即使是接入新的 esp32 板子(看起来是硬件层的工作，每种板子使用引脚的方式不尽相同)，也与普通用户工作在同一层，完全可以在图形化环境里做到，这种工作在目前主流的任何图形化环境里都是无法做到的，John Maloney 确实做到了他所宣称的”物理计算新起点”。

虽然有煽情的嫌疑，我还是想象许多 smalltalker 那样感慨: 在 smalltalk 风格的环境里编程，真实太愉快了。

正如 Alan Kay 说的那样:

> 不需要掉头发和眼泪 –Alan Kay

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#webthings)webthings

使用 WebThings 库，可以构建兼容 [wot 协议](https://iot.mozilla.org/wot/)的 web things (wifi)

我们可以从官方内置的例子开始:

[![](https://wwj718.github.io/post/img/1641547580832.jpg)](https://wwj718.github.io/post/img/1641547580832.jpg)

这是我修改了官方例子 HelloLED.ubp 后的项目:

<video width="80%" src="https://adapter.codelab.club/video/351aadab9a00b4814546b089071b04.mp4" controls="controls"></video>

以上视频对应 microblocks 项目: [bpi-bit-demo.ubp](https://wwj718.github.io/post/img/HelloLED-esp32.ubp)

站在 webthings 的视角上，使用 microblocks WebThings 库构建出的东西充当 [webthings server](https://webthings.io/framework/), 我们既可以通过 webthings gateway(诸如英荔龙眼盒)与之交互，也可通过 [WebThings api](https://iot.mozilla.org/wot/) 与之交互。

视频中，使用 http client([httpie](https://github.com/httpie/httpie), 如果你觉得不够快可使用 [xh](https://github.com/ducaale/xh)) 与 [webThings api](https://iot.mozilla.org/wot/) 交互:

``` bash
http PUT http://10.0.0.170/properties/on on:=true # 打开
http PUT http://10.0.0.170/properties/on on:=false # 关闭
```

`10.0.0.170` 是设备的 IP 地址，可在 microblocks 交互式地读出来，开发过程中，microblocks 相当于充当了硬件的“屏幕”！ 通过这个“屏幕”我们可以在代码运行的时候，实时交互式地看到变量内部状态, 极大提高可理解性. Bret Victor 在 [Seeing Spaces](https://www.youtube.com/watch?v=klTjiXjqHrQ) 里提到 maker 们在构建复杂系统的真正挑战:

> The challenge is not building it but understanding it

我们甚至可以交互式地打断 webthings server（http server 的一层包装）, 在请求/响应之间进行实时编程！一旦尝试过这个体验，你就不愿意回到以前的编程和调试方式了，这是 smalltalk 用户熟悉的体验，John Maloney 将这种流畅的体验带入到了硬件编程领域。别再闭着眼睛排列组合代码/积木啦！

`on:=true` 看起来有点怪，这是 httpie 描述数据的方式: [Non-string JSON fields](https://httpie.io/docs/cli/non-string-json-fields)，其他 http client 不一定这样描述 json。

CodeLab 可编程空间里有许多 web things，使用这种方式编程，允许用户自下而上地自定义和扩展[可编程空间](https://codelab.club/blog/2020/04/29/%E5%8F%AF%E7%BC%96%E7%A8%8B%E7%A9%BA%E9%97%B4/)。

@hidaris 正在熟悉 microblocks，不久之后，英荔龙眼盒将支持这些 wifi things，进而使可编程空间拥有极高的可塑性。

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#%E4%B8%8E-scratch-%E4%BA%A4%E4%BA%92)与 Scratch 交互

使用 CodeLab Adapter 接管 microblocks 构建出来的 web things，使其可以与 Adapter 生态的所有异构系统交互(Scratch/Python/AI 训练平台…)，用户可以以可理解的方式，同时工作在底层和应用层。甚至徒手构建完整且独一无二的可编程空间(从硬件设备到交互界面)，大多数想法来自 smalltalk 的设计原则

<video width="80%" src="https://adapter.codelab.club/video/09e552a9ef596af9833e198997fb07.mp4" controls="controls"></video>

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#%E5%8F%AF%E7%BC%96%E7%A8%8B%E4%B9%A6%E5%8C%85)可编程书包

最近公司在设计一款可编程书包，既然老是提到 microblocks 可以让用户构建产品级的东西，于是我准备做个原型，完全可行！

<video width="80%" src="https://adapter.codelab.club/video/e364c79a8f58daee5db68ac83dcbfa.mp4" controls="controls"></video>

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#faq)FAQ

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#%E6%8E%A8%E8%8D%90%E6%9D%BF%E5%AD%90)推荐板子

我目前比较喜欢淘宝上这一款: [ESP-32开发板WIFI+蓝牙 ESP-WROOM-32 ESP-32S](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.4b2d2e8dOYRxKf&id=544831023710&_u=bmdi6ia2c56)

[![](https://wwj718.github.io/post/img/1641878883706.jpg)](https://wwj718.github.io/post/img/1641878883706.jpg)

做了简单测试，与 microblocks 兼容，John 似乎也是拿这个型号板子测试的。

刷入固件前，按住boot，然后松开。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/#%E5%8F%82%E8%80%83)参考

-   [wikipedia ESP32](https://en.wikipedia.org/wiki/ESP32)
-   [microblocks Supported Boards](https://wiki.microblocks.fun/boards/supported)
    -   [ESP32](https://wiki.microblocks.fun/boards/ESP32)
-   [Build a Private Smart Home](https://wiki.microblocks.fun/community_projects/smart_home_webthings)
-   [MicroBlocks WebThings](https://wiki.microblocks.fun/microblocks_webthings)
-   [BPI-Bit STEAM 教育开发板](https://wiki.banana-pi.org/BPI-Bit_STEAM_%E6%95%99%E8%82%B2%E5%BC%80%E5%8F%91%E6%9D%BF)
-   [bpi:bit 开源硬件 📓](https://github.com/BPI-STEAM/BPI-BIT-Hardware/blob/master/readme.md)
-   [掌控板 硬件概述](https://mpython.readthedocs.io/zh/master/board/hardware.html)
-   [Web Thing API](https://iot.mozilla.org/wot/)

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: ESP32](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/)