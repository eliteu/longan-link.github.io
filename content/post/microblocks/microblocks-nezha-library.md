---
title: 'MicroBlocks 编程案例: 创建哪吒扩展板库'
date: "2022-01-25"
tags:
  - MicroBlocks
slug: microblocks-nezha-library
---

[![](https://www.elecfreaks.com/learn-cn/_images/03444_01.png)](https://www.elecfreaks.com/learn-cn/_images/03444_01.png)

> 你无法用制造问题的思路解决问题

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/#%E5%89%8D%E8%A8%80)前言

英荔和恩孚联合举办的火星资源挑战赛，去年(月球资源挑战赛)全国有数百支队伍参加，赛事使用了[**哪吒扩展板**](https://www.elecfreaks.com/learn-cn/microbitExtensionModule/Nezha.html)来驱动小车， @Leeyve 和 @Jackson 希望基于 MicroBlocks 平台来开展赛事(替代 MakeCode 平台)。于是我这两天试着在 MicroBlocks 里接管哪吒扩展板以及小车使用到的其他传感器/执行器。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/#%E6%80%9D%E8%B7%AF)思路

哪吒扩展板来自恩孚公司, 恩孚的产品在海外很受欢迎(MicroBlocks 官方的例子有些就用了恩孚的扩展板)。我喜欢恩孚这家公司的开放作风，他们的[产品文档](http://www.elecfreaks.com/learn-cn/)开放又出色。之前我们在 CodeLab 有一些新的脑洞，恩孚的 CPO @song兄 都非常乐意提供支持。

哪吒扩展板相关的驱动代码，都在 Github 上: [PlanetX\_MicroPython/nezha.py](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/nezha.py)

我打算将这些代码翻译到 MicroBlcoks，和[MicroBlocks 编程案例: 创建 Sphero RVR 代码库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-sphero-rvr-library/)的思路一样。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/#%E5%9B%B0%E5%A2%83)困境

结果遇到了困难。

比赛小车驱动的设备，基本都采用 I2C 通信。当前工作的实质是变着花样摆弄 I2C。将电机和舵机接入 MicroBlcoks 相当简单，翻译[这部分 MicroPython 代码](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/nezha.py)即可，只用了一会儿功夫, 第一次运行就成功了！

[![](https://wwj718.github.io/post/img/1643103244669.jpg)](https://wwj718.github.io/post/img/1643103244669.jpg)

<video width="80%" src="https://adapter.codelab.club/video/1643013747408674.mp4" controls="controls"></video>
<video width="80%" src="https://adapter.codelab.club/video/1643014813058195.mp4" controls="controls"></video>

但接入颜色传感器花了我好几个小时！

[颜色传感器 MicroPython 驱动](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/color.py) 比较复杂，但也只是摆弄 I2C 而已，但与电机/舵机这类执行器不同，颜色传感器需要从 I2C 地址获取数据（不只是写入数据）。

驱动的代码量很大:

[![](https://wwj718.github.io/post/img/1643103996047.jpg)](https://wwj718.github.io/post/img/1643103996047.jpg)

代码始终没有按照预期运行。我猜测错误的最大可能是大量 16 进制数造成的（诸如粗心导致的输入错误之类的），反复检查了 5-10 遍代码，始终没找到这方面的任何错误。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/#microblocks-%E5%B9%B3%E5%8F%B0%E6%8F%90%E4%BE%9B%E7%9A%84%E5%BC%BA%E5%A4%A7%E6%94%AF%E6%8C%81)MicroBlocks 平台提供的强大支持

一筹莫展之下，我想了个主意，先缩小错误的范围，不想每次都检查一大堆的代码。 策略是对比 [颜色传感器 MicroPython 驱动](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/color.py) 和 MicroBlocks 的代码中间结果的差别，定位出最早的异常。

很快我将异常定位到了 `_i2cread_color`:

[![](https://wwj718.github.io/post/img/1643104919512.jpg)](https://wwj718.github.io/post/img/1643104919512.jpg)

虽然定位到了错误区域，始终看不出哪儿出了问题。

> 你无法用于制造问题的思路解决问题。

我试着直接操控 register， 也没有搞定(我是 I2C 新手)。

无奈之下，我放弃了自己折腾。 准备看看 MicroBlocks 其他使用 I2C 的驱动是怎么写的。

我找到了一个其他型号的颜色传感器: `TCS34725`

[![](https://wwj718.github.io/post/img/1643105327627.jpg)](https://wwj718.github.io/post/img/1643105327627.jpg)

MicroBlocks 平台强大的功能之一是允许用户深入到积木底下: 展开定义！通过不停地展开定义，我不断进入到更深层的地方。我终于弄懂了原因！

[![](https://wwj718.github.io/post/img/1643106027617.jpg)](https://wwj718.github.io/post/img/1643106027617.jpg)

于是我修复了代码。

[![](https://wwj718.github.io/post/img/1643104802020.jpg)](https://wwj718.github.io/post/img/1643104802020.jpg)

若不是 MicroBlocks 平台提供的强大支持，我不知要多久才能解决这个问题，或者根本解决不了。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/#%E6%BC%94%E7%A4%BA)演示

我们来看看使用 nezha 代码库驱动的 demo:

<video width="80%" src="https://adapter.codelab.club/video/1643103240854776.mp4" controls="controls"></video>

nezha 代码库还未完成，完成后我会提交到官方。有一些比赛会使用的设备没有接入，诸如 AI 摄像头和 4 路灰度传感器，但它们都采用 I2C 通信，复杂度也不如颜色传感器，问题应该不大。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/#%E5%B0%9D%E9%B2%9C)尝鲜

愿意尝鲜的同学可以下载使用它: [nezha.ubl](https://wwj718.github.io/post/img/nezha.ubl)

可以从这里加载自定义的代码库:

## [![](https://wwj718.github.io/post/img/1643008474077.jpg)](https://wwj718.github.io/post/img/1643008474077.jpg) 参考

-   [How to create a library](http://wiki.microblocks.fun/create_a_library)
    -   [如何在 MicroBlocks 中创建代码库](https://microblocks.zhubai.love/posts/2088166608488710144)
-   [My Blocks](https://wiki.microblocks.fun/reference_manual/myblocks)
    -   [MicroBlocks 积木块使用说明 – 自定义积木](https://microblocks.zhubai.love/posts/2094173059597713408)
-   [哪吒扩展板](https://www.elecfreaks.com/learn-cn/microbitExtensionModule/Nezha.html)
-   [哪吒扩展板](https://www.elecfreaks.com/learn-cn/microbitExtensionModule/Nezha.html)
-   [PlanetX\_MicroPython/nezha.py](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/nezha.py)
-   [PlanetX\_MicroPython/color.py](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/color.py)

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: 创建哪吒扩展板库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/)