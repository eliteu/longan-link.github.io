---
title: '我的世界(Minecraft)'
date: "2021-08-16"
tags:
  - python
slug: minecraft
---

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/#%E5%89%8D%E8%A8%80)前言

[![](https://www.minecraft.net/content/dam/minecraft/home/home-hero-1200x600.jpg)](https://www.minecraft.net/content/dam/minecraft/home/home-hero-1200x600.jpg)

[Hanson](https://codelab.club/blog/2020/05/18/%E7%BC%96%E7%A8%8B%E5%B0%91%E5%B9%B41+1%E8%AE%BF%E8%B0%88/) 初中刚毕业，这个假期时间多，他在网上租了一台 Linux 服务器，在里边架设了一个 Minecraft 服务器，并邀请20多个朋友加入 McLab 小组。周末的晚上，他邀请我加入其中。我登录的时候，游戏里是一个雨夜，我跟Hanson第一次“碰面”.

这是我第一次被 Minecraft 深深震撼，一种极为真实的与朋友共处的感受，也引起我对 [Metaverse](https://en.wikipedia.org/wiki/Metaverse) 概念的兴趣，很可惜， 今天媒体上围绕 Metaverse 的讨论大多数非常无聊。

在游戏里，夜已深，雨不停，Hanson 开车到我出生的地方接我，带我到他自己搭建的屋子里, 让我在他家过夜，避免夜里被外头怪物伤害。 Hanson给了我一些胡萝卜吃，并带我到阳台看他用lua（游戏里有真实的lua解释器）写的一个程序（[OpenComputers](https://www.curseforge.com/minecraft/mc-mods/opencomputers)），这个程序控制屋外雨夜中电线杆上的三盏灯。之后他在这台机器上写了许多程序，最近在里头捣鼓一个操作系统同时也在规划城市的扩建问题。

与 Hanson 在 Minecraft 里的这次碰面，导致我在里头留了下来，之前有过多次从入门到放弃的经历。

> 让孩子对计算机编程，而不是让计算机编程孩子 – Seymour Papert

有几种方式对 Minecraft 编程:

-   [Minecraft Pi](https://www.minecraft.net/en-us/edition/pi): 树莓派内置的 Minecraft，可以视为删减版的 Minecraft，不支持红石系统等
-   [raspberryjuice](https://github.com/zhuowei/RaspberryJuice)
-   [raspberryjammod](https://github.com/arpruss/raspberryjammod)

以上几个项目都实现了[Minecraft Pi Protocol](https://wiki.vg/Minecraft_Pi_Protocol)（作为socket server）。

如此一来，我们就可以在不同编程语言中通过 socket client 对 Minecraft 编程 ，诸如 :

-   Python client: [mcpi](https://github.com/martinohanlon/mcpi)
-   [Smalltalk Client](http://croquetweak.blogspot.co.uk/2013/02/smalltalk-bindings-for-minecraft-pi.html)。

本文使用 [raspberryjammod](https://github.com/arpruss/raspberryjammod)。没有使用[Minecraft Pi](https://www.minecraft.net/en-us/edition/pi)有两个原因:

-   raspberryjammod 允许使用 Minecraft 的所有能力（真实环境），尤其是红石系统， [Minecraft Pi](https://www.minecraft.net/en-us/edition/pi)做不到
-   Minecraft Pi依赖于树莓派，无法在普通计算机上使用

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/#%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE)环境配置

以下是我自己的配置过程，如果你不是使用 [HMCL](https://hmcl.huangyuhui.net/)运行 Minecraft， 请移步参考 [raspberryjammod 主页](https://github.com/arpruss/raspberryjammod) 。

1.  下载运行 [HMCL](https://hmcl.huangyuhui.net/download), 选择版本: `1.12.2`
2.  安装Forge: HMCL->自动安装->安装 Forge, 选择版本: `14.23.5`
3.  将[raspberryjammod](https://github.com/arpruss/raspberryjammod/releases/tag/0.94)解压后的 `1.12.2/RaspberryJamMod.jar` 移到 `.minecraft/mods` 。 `.minecraft` 目录与 HMCL 文件位于同一目录。
4.  把[mcpipy](https://github.com/arpruss/raspberryjammod/releases/download/0.94/python-scripts.zip)解压后移到minecraft文件夹里。

启动 Minecraft。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/#python-%E9%A9%B1%E5%8A%A8)Python 驱动

以下是使用 Python 对 Minecraft 编程的 2 个例子， 使我的世界（现实世界）与《我的世界》进行互动（基于[CodeLab Adapter](https://adapter.codelab.club/)）

第一个例子使用我的世界里的红石拉杆，控制现实中的灯泡:

Scratch 部分使用了 LonganHub 插件，用以驱动氛围灯。

Python部分如下:（Adapter Jupyterlab 内置了 mcpi 库）
```python
import time
from mcpi import minecraft # CodeLab Adapter 内置了这个驱动
from codelab_adapter_client.message import send_message, receive_message

mc = minecraft.Minecraft.create()

tilePos = mc.player.getTilePos() # player 站立的位置

last_power = 0
while True:
    time.sleep(0.1)
    
    block_power = mc.getBlockWithData(-7,0,0).data # 积木的位置通过 tilePos 推知，也可以使用 F3 查看位置。 getBlockWithData 可以用来获取积木的状态
    if block_power != lasest_power: # 避免重复发送
        if block_power == 0:
            send_message('close') # 发往 Scratch 的 message
        if power > 0 :
            send_message('open')
    last_power = block_power
```

第二个例子，通过按下 Microbit 引爆TNT！

在 Scratch 里 使用了 Microbit More 插件
<video width="80%" src="https://adapter.codelab.club/video/1629096775200063.mp4" controls="controls"></video>

```python
from codelab_adapter_client.message import receive_message
while True:
    message = receive_message()
    if message == 'A': # 收到 Scratch EIM 插件发来的 A 字符串
        mc.setBlock(-7,0,1, 69, 14) # 操控拉杆: 打开
        time.sleep(0.5)
    if message == 'B':
        mc.setBlock(-7,0,1, 69, 6) # 操控拉杆: 关闭
        time.sleep(0.5)
```
## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/#%E9%99%84)附

附上我最近在 Minecraft 折腾的其他项目:

1.  自动化路灯
2.  Minecraft 的 Smalltalk 驱动

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/#%E8%87%AA%E5%8A%A8%E5%8C%96%E8%B7%AF%E7%81%AF)自动化路灯

只有在夜晚才亮起， 构造了两种小夜灯:

1.  基于比较器
2.  基于机械结构

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/#minecraft-%E7%9A%84-smalltalk-%E9%A9%B1%E5%8A%A8)Minecraft 的 Smalltalk 驱动

[Dr. Bert Freudenberg（Croquet团队成员）](http://www.freudenbergs.de/bert/) 在 2013年在 Squeak 里实现了 Minecraft API: [使用文档](https://ericclack.blogspot.com/2014/03/programming-raspberry-pi-minecraft-with.html)，不过有个小bug，我做了修复，在2021年的树莓派系统里测试可用。

将 toNumbers 方法改为:

```smalltalk
Minecraft>>toNumbers: aString
	^Array streamContents: [:out | | in |
		in := aString readStream.
		[in atEnd] whileFalse: [
			out nextPut: (in upToAnyOf:  ',|'  asCharacterSet) asNumber]]

```

以下是 workspace 里的测试脚本

```
m := Minecraft connectTo: '192.168.31.59'.
p := m playerTile.

m chatPost: 'hello world'

m blockAt: p put: 1.

1 to: 100 do: [ :i | 
    m blockAt: (p + {i. i. i}) put: 1.
]

"m cameraModeFixed."
"m cameraModeFollow."
"m cameraModeNormal."
"m cameraPos: #(24 10 18)"
```
<video width="80%" src="https://adapter.codelab.club/video/1629096899852783.mp4" controls="controls"></video>

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/#%E5%8F%82%E8%80%83)参考

-   [Smalltalk Bindings for Minecraft Pi](http://croquetweak.blogspot.co.uk/2013/02/smalltalk-bindings-for-minecraft-pi.html)
    -   [Dr. Bert Freudenberg](http://www.freudenbergs.de/bert/)
-   [Programming Raspberry Pi Minecraft with Squeak Smalltalk](https://ericclack.blogspot.com/2014/03/programming-raspberry-pi-minecraft-with.html)
-   [Adapter Minecraft](https://adapter.codelab.club/extension_guide/minecraft/)
    -   [mcpi](https://github.com/martinohanlon/mcpi)
    -   [minecraft-api-reference](https://www.stuffaboutcode.com/p/minecraft-api-reference.html)
-   [Minecraft Pi Protocol](https://wiki.vg/Minecraft_Pi_Protocol)
-   [Minecraft Pi API](https://github.com/martinohanlon/Minecraft-Pi-API/blob/master/api.md)
    -   客户端连接到服务器并通过 TCP/IP 发送 API 命令
    -   mcpi-protocol 允许外部进程(程序)与 Minecraft Pi API 的运行实例交互。该协议可以从任何具有network socket支持的编程语言轻松实现和使用。
    -   端口4711
    -   Commands are clear text lines (ASCII, LF terminated)
    -   坐标系统大多数坐标都是三个整数向量(x，y，z)的形式, (0,0,0)是出生点。(x，z)是地平面，y 朝向天空。
-   [Terse Guide to Squeak](https://squeak.org/documentation/terse_guide/)


---
_本文转载自 夜行人的博客_ - [我的世界(Minecraft)](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/minecraft/)