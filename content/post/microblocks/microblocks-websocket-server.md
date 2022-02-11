---
title: 'MicroBlocks 编程案例: WebSocket server'
date: "2022-02-08"
tags:
  - MicroBlocks
slug: microblocks-websocket-server
---

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-websocket-server/#%E5%89%8D%E8%A8%80)前言

MicroBlocks 内置了若干与网络相关的库(都在 Network 分类下):

[![](https://wwj718.github.io/post/img/1644316118417.jpg)](https://wwj718.github.io/post/img/1644316118417.jpg)

就网络通信而言，对于许多用例，HTTP 是最简易的协议。但有时，我们需要更好的实时性或想要双向通信，那么 WebSocket 和 MQTT 就是更好的选择。

什么时候使用 MQTT，什么时候使用 WebSocket 呢？

-   如果你想在在公网范围内通信， 使用 MQTT 可能更简单。
-   局域网内的应用，推荐 WebSocket，它不需要额外的 broker（实时性也更好）

（目前 MicroBlocks 只提供 WebSocket server 库，而不提供 WebSocket client 库，我问 John 是否有计划实施 WebSocket client ，他说他还想不到典型用例。如果你有典型用例欢迎在 MicroBlocks issue 里提）

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-websocket-server/#%E4%BD%BF%E7%94%A8-websocket-server-%E5%BA%93)使用 WebSocket server 库

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-websocket-server/#%E8%B0%81%E5%85%88%E6%8B%94%E6%9E%AA)谁先拔枪

我之前用 mediapipe 做了一个 AI 演示项目: 谁先拔枪。

<video width="80%" src="https://adapter.codelab.club/video/1631788391568750.mp4" controls="controls"></video>

游戏有两位玩家，倒计时 3-2-1，先拔枪且对准对方(水平指向对方)的人获胜。

游戏创意来自 Switch 上的游戏  [《1 2 Switch》](https://www.nintendo.com/games/detail/1-2-switch-switch/)，《1 2 Switch》利用 Switch 手柄里的陀螺仪来检测玩家的手势。

[![](https://wwj718.github.io/post/img/1644317102712.jpg)](https://wwj718.github.io/post/img/1644317102712.jpg)

而我的演示里，使用机器视觉来做，这样的好处除了不依赖于手柄，还有儿时过家家的童趣: 比划手指当枪使(biu~biu~biu~)

为了增加表现力，我在游戏区域周围加了一个灯带: 模拟子弹从胜利的一方射出。 有围观者说它有 90 年代舞厅的浮夸效果，夜色下尤甚。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-websocket-server/#microblocks-%E7%99%BB%E5%9C%BA)MicroBlocks 登场

那么哪儿用到 MicroBlocks 呢？

机器视觉相关的程序我们使用 Python 构建。

MicroBlocks 很适合用来控制灯带 (NeoPixel) 。为了让 Python 程序与灯带交互，一种简单的想法是让控制灯带的板子提供出网络服务。

按这个思路，我们选择 ESP32 而不是 micro:bit。

这个用例需要很高的实时性，任何的延迟都会破坏玩家的幻想/沉浸感(《游戏设计艺术》)， 于是好的想法是使用 WebSocket 而不是 HTTP。

程序相当简单，操控 NeoPixel 的部分不必赘言，因为 NeoPixel 库简洁明了。

我们来看看 WebSocket server 如何构建。 最小原型大约是这样的

[![](https://wwj718.github.io/post/img/1644318737680.jpg)](https://wwj718.github.io/post/img/1644318737680.jpg)

首先你需要先将板子连上 WiFi，然后启动 WebSocket server，之后使用一个无限循环不停监听事件。

这里需要注意的是: 每次调用`last WebSocket event`都拿到一个当前的最新事件（如果没有数据就是 false），如果你想要多次使用事件数据，得把它先存到变量里，然后反复使用变量。

由于积木设计得很直观，MicroBlocks 又是实时交互式的，我在这里就不写太多细节了。你要是对什么感兴趣，动手试试就懂啦！

> 教会破坏学的乐趣 – Seymour Papert

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-websocket-server/#%E5%AE%A2%E6%88%B7%E7%AB%AF)客户端

客户端要如何与之通信呢，这个简单, 你可以使用任何语言的 websocket 客户端与之通信，任何语言！诸如在浏览器里打开 dev console，与之通信。

唯一需要注意的是， WebSocket server 运行在 81 端口（我已经将这些信息添加到 WebSocket server 库的说明信息里了）。

在我的例子中，使用 [Python websocket client](https://github.com/websocket-client/websocket-client) 与之通信:


```
from websocket import create_connection

ws = create_connection("ws://192.168.21.108:81")
ws.send('hi from Python')
```

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-websocket-server/#%E6%9B%B4%E5%AE%8C%E6%95%B4%E7%9A%84%E4%BE%8B%E5%AD%90)更完整的例子

你可能想构建产品级的东西，放到淘宝去卖。 上边这个基于 WebSocket 的灯带控制器是不能直接交付给用户的，因为不是“开箱可用”的， 你可能会想要这样的常见功能（IoT 设备通常都有）: 配网机制。

用户拿到产品，先得为 ta 配网，这些产品通常是这么工作的: 第一次开机时，设备发出无线热点，用户连接到设备热点后，打开一个配置页面，填入房间路由器的用户名/密码，然后设备就会连上 WIFI，之后就可以用客户端控制它啦。(当然另有不少设备使用蓝牙配网)。

MicroBlocks 里已经内置了这样的例子!

[![](https://wwj718.github.io/post/img/1644319624649.jpg)](https://wwj718.github.io/post/img/1644319624649.jpg)

[![](https://wwj718.github.io/post/img/1644319683366.jpg)](https://wwj718.github.io/post/img/1644319683366.jpg)

其实，我更喜欢的方式是，直接将 MicroBlocks 看作终端用户界面，这样就不用以上这些复杂代码了！

不过每个人都有自己的喜好， MicroBlocks 当然也可以满足主流需要。放到淘宝去卖吧！

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: WebSocket server](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-websocket-server/)