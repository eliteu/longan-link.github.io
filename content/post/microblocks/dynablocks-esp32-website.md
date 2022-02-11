---
title: 'MicroBlocks 编程案例: 在 ESP32 里运行网站'
date: "2022-01-10"
tags:
  - MicroBlocks
slug: dynablocks-esp32-website
---

上周六在 CodeLab 候车群 里提到:

> 下周我打算做一个实验，在 ESP32 板子上，运行一个网站和一个聊天服务器, 然后通过端口映射，提供出公网服务。由于 ESP32 的廉价和低功耗, 意味着可以使用太阳能来驱动这个 10 多块钱的 web 后端服务器。打算在 microblocks 里开发。

心急等不到下周，周六下午去实务学堂上完课，从小洲村回到公司，取了一块 ESP32 板子回家做实验，由于 microblocks 提供了强大的沉浸式编程体验(类似 Smalltalk，放大心智的力量)，一会儿就搞出了原型。

<video width="80%" src="https://adapter.codelab.club/video/1641644601520772.mp4" controls="controls"></video>

视频的后半段，十分有趣, 当我在浏览器里请求 `/hi` 时，web 服务已经被我中止了，我临时创建了响应内容:`hello esp32`, 并立刻用它响应来自浏览器的请求, 在我点击 `响应`  积木的瞬间，浏览器才收到回复。我们在请求和响应之间进行交互式编程！这过程极大增强了 web 编程的可理解性，我们可以在 microblocks 里交互式地观察请求的细节，并随时手动给出响应，换句话说，我们的大脑和双手成了后端运行时(runtime)的一部分！

之后，我有个想法，考虑到这个太阳能驱动的网站，可能因太阳暴晒导致芯片温度升高，于是想构建一个自动降温装置: 当温度太高时，打开风扇

<video width="80%" src="https://adapter.codelab.club/video/d394a5677121477a7fe76600511f54.mp4" controls="controls"></video>

通过 microblocks 的通用实时图表功能，可以很快看出代码中的临界参数应该选什么值（当温度超过预设的临界值，就打开风扇）。大多数编译型编程环境，是”闭着眼睛排列代码/积木”。

家庭作坊，就地取材，为了压住风扇不乱跑，用的是橘子, 为了模拟阳光暴晒，用的是电吹风 ：）

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/#%E6%8A%80%E6%9C%AF%E7%BB%86%E8%8A%82)技术细节

[esp32-website.ubp](https://wwj718.github.io/post/img/esp32-website.ubp)

[![](https://wwj718.github.io/post/img/1641810100488.jpg)](https://wwj718.github.io/post/img/1641810100488.jpg)

代码简洁清晰，无需过多解释。

来看看 microblocks 的 HTTP server 库，`http server request` 积木用于获取客户端请求，`response` 积木用于给出响应。 编程模型与大多数 web 后端语言一致: 接受客户端的请求，然后给予响应，一直循环上述过程。microblocks 构建出的是动态网站！

有一处值得注意，在每次循环中，`http server request` 与 `response` 应该都只出现一次(成对出现)，如果你想多次使用`http server request`，应该把它保存到变量里，然后多次使用这个变量。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/#%E5%85%AC%E7%BD%91%E8%AE%BF%E9%97%AE)公网访问

由于 ESP32 连接到局域网 WIFI， 我打算使用 [ngrok](https://ngrok.com/)(也可使用 frp 之类的工具) 将其 web 服务端口映射到公网上。如果你直接在 ESP32 上接了网线，且有公网地址，则不需要使用 ngrok。

正好桌面上有一个树莓派，我将 `ngrok` 运行在树莓派里（树莓派和 ESP32 在同个网络下），这样就不需要关机了。

首先需要获得 ESP32 的 IP 地址，这简单，microblocks 的 WIFI 库有对应积木，交互式地查看它即可，我们[之前](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/)提到 microblocks 交互式的环境可以充当硬件的“屏幕”！ 不像许多数编译式的环境，代码一旦烧录，就不懂它在内部干嘛了，随然串口输出有用，但你需要事先知道该输出什么调试信息，这是很讽刺的，因为意外的问题，正好是你事先不知道的，好的环境应该允许你与系统进行深层次的实时交互，让你不断解决意外的问题，并加深对它的理解，所有这些都发生在行进过程中。正如 Alan Kay 喜欢说的: 在飞行中改进系统。

拿到 ESP32 的地址后（如`10.0.0.49`）, 我们就可以把它映射到公网了: `./ngrok http http://10.0.0.49`

[![](https://wwj718.github.io/post/img/1641811386329.jpg)](https://wwj718.github.io/post/img/1641811386329.jpg)

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/#%E9%A1%B5%E9%9D%A2%E4%BF%A1%E6%81%AF)页面信息

当你访问映射出的公网地址时，会看到如下页面。

[![](https://wwj718.github.io/post/img/1641811517583.jpg)](https://wwj718.github.io/post/img/1641811517583.jpg)

你可以修改前边的代码，写任何你喜欢的 html 代码，我喜欢 markdown，所以采用 markdown 来写页面，这是从 [Yoshiki Ohshima](https://tinlizzie.org/ohshima/) 那儿学来的。


``` HTML
<meta charset="UTF-8" />
## path 
* light 
    * [on](/lighton) 
    * [off](/lightoff) 
* fans 
    * [on](/fanson) 
    * [off](/fansoff) 
* [codelab](/codelab)
<style class="fallback">body {visibility: hidden;white-space: pre;font-family: monospace;}</style>
<script src="https://casual-effects.com/markdeep/latest/markdeep.min.js"></script>
<script>window.alreadyProcessedMarkdeep ||(document.body.style.visibility = "visible");</script>
```

你也可以把 html 文件和其他静态文件永久托管在 ESP32 ESP32 能存几 MB 的文件！使用 microblocks 提供的文件库即可。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/#%E5%A4%AA%E9%98%B3%E8%83%BD%E7%94%B5%E6%B1%A0)太阳能电池

我目前使用移动电源驱动网站。

[![](https://wwj718.github.io/post/img/WechatIMG5365.jpeg)](https://wwj718.github.io/post/img/WechatIMG5365.jpeg)

由于 ESP32 的低功耗, 意味着可以使用太阳能来驱动它。最近广州阴雨天，太阳能电池还没到, 以后有时间再测试。

如果你有兴趣来做，电路方面可以参考[这篇文章](https://mc.dfrobot.com.cn/thread-271789-1-1.html)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/#%E8%81%8A%E5%A4%A9%E6%9C%8D%E5%8A%A1%E5%99%A8)聊天服务器

在 ESP32 里构建聊天服务器完全没有问题，由于 ESP32 带有文件系统，可以在服务器端保留消息，为离线用户存储历史消息。

我之前想使用 websocket 来构建聊天服务器，目前 microblocks 作者没写 websocket 的用例，我还没摸索出用法。之后有空再折腾，我本来还打算使用 [htmx](https://htmx.org/) 构建客户端。

你也可以使用 HTTP server 库构建聊天服务器，应该很简单。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/#%E5%90%8E%E8%AE%B0)后记

下班之前运行的网站，12小时之后，依然稳定运行。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/#%E5%8F%82%E8%80%83)参考

-   [Microblocks 编程案例: ESP32](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32/)

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: 在 ESP32 里运行网站](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/dynablocks-esp32-website/)