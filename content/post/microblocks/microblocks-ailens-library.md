---
title: 'MicroBlocks 编程案例: 创建 AI 摄像头库'
date: "2022-01-29"
tags:
  - MicroBlocks
slug: microblocks-ailens-library
---

[![](https://www.elecfreaks.com/learn-en/_images/05035_01.png)](https://www.elecfreaks.com/learn-en/_images/05035_01.png)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/#%E5%89%8D%E8%A8%80)前言

接[上文](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-nezha-library/), 火星资源挑战赛使用了 AI 摄像头: [AILens](https://www.elecfreaks.com/learn-cn/microbitplanetX/ai/Plant_X_EF05035.html)

于是我打算将其接入 MicroBlocks。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/#%E6%80%9D%E8%B7%AF)思路

AILens 相关的 MicroPython 驱动代码，都在 Github 上: [PlanetX\_MicroPython/AILens.py](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/AILens.py)

我试着将这些代码翻译到 MicroBlcoks，和之前的几篇文章思路一样。

___

在 MicroBlocks 里的编程，十分愉快。 由于已经熟悉了对 I2C 设备编程，轻车熟路 ，完成 AILens 的驱动可能不到 1 小时。

[![](https://wwj718.github.io/post/img/1643196398857.jpg)](https://wwj718.github.io/post/img/1643196398857.jpg)

我已经将驱动[提交给了官方](https://bitbucket.org/john_maloney/smallvm/pull-requests/19)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/#%E6%BC%94%E7%A4%BA)演示

<video width="80%" src="https://adapter.codelab.club/video/1643171522875763.mp4" controls="controls"></video>
<video width="80%" src="https://adapter.codelab.club/video/1643172494658270.mp4" controls="controls"></video>

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/#%E5%B0%9D%E9%B2%9C)尝鲜

愿意尝鲜的同学可以下载使用它: [AILens.ubl](https://wwj718.github.io/post/img/AILens.ubl)

可以从这里加载自定义的代码库:

[![](https://wwj718.github.io/post/img/1643008474077.jpg)](https://wwj718.github.io/post/img/1643008474077.jpg)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/#%E7%BC%96%E5%86%99%E4%BB%A3%E7%A0%81%E5%BA%93-library-%E7%9A%84%E6%8A%80%E5%B7%A7)编写代码库(library)的技巧

在 MicroBlocks 里编写代码库非常简单。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/#%E5%88%9D%E5%A7%8B%E5%8C%96)初始化

在 MicroBlocks 里，我们只能将自定义积木导出为代码库(library)， 所以「当启动时积木」不会被到导出代码库里。

这带来一个问题，一些需要初始化的代码怎么办呢？ 这类代码往往有个特征: 只需要初始化一次。类似 Python class 里的 `__init__` 函数。

初始化的技巧是通用的，技巧如下:

[![](https://wwj718.github.io/post/img/1643195686935.jpg)](https://wwj718.github.io/post/img/1643195686935.jpg)

(ps: 图中初始化代码做了简化处理)

`setup` 自定义积木，一般配合一个变量使用:`initialized`, 通过这个变量来记忆是否是第一次操作(一开始变量是 0)。 值得之一的是 setup 幂等函数。 这样你可以把`setup` 放到任何地方（确保其他代码运行之前先运行setup），不必担心多次掉用它。

这些技巧我也是通过「显示积木定义」学来的 : )

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/#%E4%B8%8B%E6%8B%89%E6%A1%86)下拉框

AILens 代码库里，`switch mode` 积木使用了下拉框

[![](https://wwj718.github.io/post/img/1643196398857-1.jpg)](https://wwj718.github.io/post/img/1643196398857-1.jpg)

目前无法在 IDE 里添加下拉框类型的输入。我是直接修改文本代码做到的（通过学习其他 library 的代码）。

但我不喜欢在文本里编程，我的做法是，在 MicroBlocks图形环境里，大体完成自定义积木后

[![](https://wwj718.github.io/post/img/1643255190150.jpg)](https://wwj718.github.io/post/img/1643255190150.jpg)

再在文本环境里微调:

[![](https://wwj718.github.io/post/img/1643255276038.jpg)](https://wwj718.github.io/post/img/1643255276038.jpg)

（与 Scratch 插件里的做法类似）

相信未来图形IDE里就会添加这个支持。

### 参考
- [PlanetX_MicroPython/AILens.py](https://github.com/lionyhw/PlanetX_MicroPython/blob/master/AILens.py)

---
_本文转载自 夜行人的博客_ - [MicroBlocks 编程案例: 创建 AI 摄像头库](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/microblocks-ailens-library/)