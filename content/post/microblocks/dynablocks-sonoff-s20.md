---
title: 'MicroBlocks 编程案例: sonoff 智能插座'
date: "2022-01-14"
tags:
  - MicroBlocks
slug: dynablocks-sonoff-s20
---

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-sonoff-s20/#%E5%89%8D%E8%A8%80)前言

市面上的许多智能设备都搭载了 ESP32 系列微控制器，由于 microblocks 支持 ESP32(esp32 devkit-v1) 和 ESP8266(NodeMCU), 想用 microblocks 来接管真实世界的设备。

特别提醒: 千万不要在插座这类「强电设备」通着电的时候对其操作，否则可能对你造成伤害，也可能毁掉电脑。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-sonoff-s20/#%E6%80%9D%E8%B7%AF)思路

寻思着从接管哪个设备开始。

由于我对硬件拆解和分析电路板都不够熟悉，不想随便选择一个设备贸然开始，那样时间投入和结果产出都不可预期，打算在社区已有的工作上前进，这样即使遇到问题，社区里很可能已经有解决方案了。不想在硬件拆解上投入太多时间，那不是我感兴趣的。后来证明这个决策十分明智。

从[笔记系统](https://wwj718.github.io/post/%E9%9A%8F%E7%AC%94/zettelkasten-foam/)里找到 [接管 sonoff s20 智能插座的教程](https://esphome.io/devices/sonoff_s20.html)。淘宝下单买了一个 sonoff s20 插座(几十块钱)，准备按照教程里的方法，为这插座刷入新固件，然后在 microblocks 里对其编程。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-sonoff-s20/#%E4%B8%BA-sonoff-s20-%E5%88%B7%E5%85%A5%E5%9B%BA%E4%BB%B6)为 sonoff s20 刷入固件

拆开到货的 sonoff s20 智能插座，发现与教程里的电路布局并不一样。

这是教程里国外的版本:

[![](https://wwj718.github.io/post/img/sonoff_s20_pcb-esphome.jpeg)](https://wwj718.github.io/post/img/sonoff_s20_pcb-esphome.jpeg)

这是我从淘宝买到的国内版本:

[![](https://wwj718.github.io/post/img/WechatIMG5506.jpeg)](https://wwj718.github.io/post/img/WechatIMG5506.jpeg)

一番 Google，了解到不同国家卖的 sonoff 智能插座, 电路板布局有所不同。按照教程里的引脚布局，无法刷入固件。 又一番 Google，找到了我这款插座的[引脚说明](https://www.a-netz.de/blog/2019/06/tasmota-firmware-for-sonoff-s20-wifi-switches.html):

[![](https://wwj718.github.io/post/img/tasmota_socket.jpeg)](https://wwj718.github.io/post/img/tasmota_socket.jpeg)

@weibin 帮我焊了一个与上图类似的 90 度折角的针脚。

按上图的引脚布局，使用 USB-UART 桥接器(必须兼容 3.3V。否则你会毁掉你的 S20!)将插座连到电脑。

[![](https://wwj718.github.io/post/img/WechatIMG139.jpeg)](https://wwj718.github.io/post/img/WechatIMG139.jpeg)

往 usb 口插入了 USB-UART 桥接器, 电脑里就可以看到 port 了(这不意味着接线的正确性)。

我打算先 `erase_flash` 它:

`esptool.py --chip esp32s2 --port /dev/tty.usbserial-14320 erase_flash`

始终处于连接中，一开始以为是接线的问题，交换了 tx/rx，依然没解决问题。想起之前刷 esp32 板子时，也遇到类似的问题，需要按 boot 键，但我在 sonoff 智能插座电路板上没找到 boot 按键，唯一的一个功能键按了无效。

一时毫无头绪，@junnan 说如果能看到微控制器的引脚，使用万用表测试下应该能搞清楚问题，但电路板的微控制器在看不到的一面，已经被固定死了。

翻了几个文档，都没提 esptool.py 如何连接上板子。 我猜测通过观看视频应该能找到文档里缺失的操作细节，毕竟操作者的行为里， 必须包含所有信息，不然不会成功。于是继续去开源社区里答案。果然，这个问题确实被解决了，有个用户在使用开源项目 Tasmota 接管 sonoff 智能插座时，录了一个视频，通过看视频里的操作，我弄懂了操作方法:[Flashing the Sonoff S20 WiFi Smart Plug with open source Tasmota firmware](https://www.youtube.com/watch?v=5k_35ppDPho)（8 分 10 秒），使用 esptool.py 刷东西之前，需要先拔下 vcc 线(其余 3 根线连着)，然后按住电路板上的按钮，插入 vcc 线之后，再松开按钮。

一试之下果然奏效(@junnan 猜测这等效于 boot 按键):

`esptool.py --chip esp8266 --port /dev/tty.usbserial-14320 erase_flash`

按照同样的操作，往智能插座里刷入[vm\_nodemcu.bin 固件](https://wwj718.github.io/post/img/vm_nodemcu.bin)

`esptool.py --chip esp8266 --port /dev/tty.usbserial-14320 write_flash -fs 1MB -fm dout 0x00000 ~/Downloads/vm_nodemcu.bin`

esptool.py 提示 sonoff s20 使用的微控制器是 esp8266EX。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-sonoff-s20/#%E4%BD%BF%E7%94%A8-microblocks-%E7%BC%96%E7%A8%8B)使用 microblocks 编程

我没有使用 microblocks 网页刷入固件，因为也会遇到一样的连接问题（需要 boot）。为了排除问题, 选择使用 esptool.py，因为开源社区有好几个项目已经用它完成了固件刷入。

固件刷入后，microblocks 顺利连接插座!

如何弄懂不同功能对应的引脚呢？

读文档当然是很好的方法，但很多东西是没有文档的，就像这个物理世界没有使用文档一样。信奉建构主义的个人计算社区会告述你: 动手尝试它！

“要了解这个世界，你必须建造它。”——Pavese

这是 John Maloney(曾是 smalltalk 团队成员) 喜欢的编程风格: [寻找哪个引脚对应 user LED](https://wiki.microblocks.fun/boards/ESP32)

[![](https://wwj718.github.io/post/img/1642050248181.jpg)](https://wwj718.github.io/post/img/1642050248181.jpg)

<video width="80%" src="https://adapter.codelab.club/video/1641984170207628.mp4" controls="controls"></video>

现场凑近的话，可以听到继电器的滴答声。我们已经接管了智能插座的核心元件: 继电器。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-sonoff-s20/#%E5%90%8E%E8%AE%B0)后记

虽然在线编程没有问题(webthings 之类的库都能正常工作)。 但离线时，程序并未运行，猜测是持久化部分出的问题, 诸如板子上有写保护机制，又或者自动重置之类的东西。

由于整个硬件并未开放，这上头投入过多的时间并不值得，我不打算解决这个问题。

本文的兴趣更多是探索 microblocks 的可能性。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-sonoff-s20/#%E5%8F%82%E8%80%83)参考

-   [esphome sonoff\_s20](https://esphome.io/devices/sonoff_s20.html)
-   [tasmota Sonoff-S20 serial-connection](https://tasmota.github.io/docs/devices/Sonoff-S20/#serial-connection)
-   [TASMOTA FIRMWARE FOR SONOFF S20 WIFI SWITCHES](https://www.a-netz.de/blog/2019/06/tasmota-firmware-for-sonoff-s20-wifi-switches.html)
-   [Flashing the Sonoff S20 WiFi Smart Plug with open source Tasmota firmware](https://www.youtube.com/watch?v=5k_35ppDPho)

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: sonoff 智能插座](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-sonoff-s20/)