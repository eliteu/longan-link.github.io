---
title: '公共树莓派'
date: "2021-09-01"
tags:
  - python
slug: public-computer-rpi
---

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E5%89%8D%E8%A8%80)前言

[![](https://wwj718.github.io/post/img/094ae88092e44b9dd08e5af38f3b2033.png)](https://wwj718.github.io/post/img/094ae88092e44b9dd08e5af38f3b2033.png)

> 他(Alan Kay)意识到显然只有那些沉浸在电脑技术和文化之中的人，就像他自己, 才会有满脑子的想法 – 《时间机器:施乐帕克与计算机时代的黎明》

CodeLab 成员之前在每周五下午的讨论会里，多次被 **公共计算空间** 的概念吸引。

这是一种混合了 [club(计算机俱乐部)](https://theclubhousenetwork.org/) 和 Dynamicland/Neverland 的想法。

想法的大致轮廓是在一个线下公共空间里，放置一些计算设备，参与者在这个环境里，用编程表达自己的想法，构建自己感兴趣的项目。大家的探索过程和成果都置于公共视野，想法和经验持续流动，彼此启发，就像 Scratch 线上社区。

我们曾以多种方式探索过这个想法:

-   [接下来我们准备做的一些事](https://www.codelab.club/blog/2018/11/08/about-codelab-club/#%E6%8E%A5%E4%B8%8B%E6%9D%A5%E6%88%91%E4%BB%AC%E5%87%86%E5%A4%87%E5%81%9A%E7%9A%84%E4%B8%80%E4%BA%9B%E4%BA%8B)
-   [Scratch 编程工作坊](https://www.codelab.club/blog/2021/07/16/scratch_workshop/)
-   可编程舞台
-   Neverland 2.0

这些探索都不够深入，并未形成 club。

___

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E5%86%85%E9%83%A8%E7%9A%84%E5%85%AC%E5%85%B1%E6%A0%91%E8%8E%93%E6%B4%BE)内部的公共树莓派

近期 @ACE 和 @尚老师 重新提起这个话题。

我想不如从内部开始。既然我们鼓励学习者在**公共计算空间**中学习，我们自己也应该在那里协作。

最简单的公共计算空间仅使用一台树莓派即可构建（配置上屏幕、鼠标、键盘）。

[![](https://wwj718.github.io/post/img/desk-dual-monitor-eab29316d622dcb49815c9f3190f7683.webp)](https://wwj718.github.io/post/img/desk-dual-monitor-eab29316d622dcb49815c9f3190f7683.webp)

我们希望在这样的环境里，学习可以更加自发地发生，知识处在有意义的情境(context)里。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E6%B4%BB%E7%9A%84%E5%AD%A6%E4%B9%A0%E7%8E%AF%E5%A2%83)活的学习环境

我们希望团队成员的经验能流动起来，彼此启发，每个人的知识和经验能成为其他人起步的基石，而不是各自平行探索。

知识和想法存在于公共计算空间里，而不是文档上。这是一个活的真实环境，不是死的文档（谁都不爱读）。

大家将自己感兴趣的项目放在计算环境里，为了他人能理解并使用，需要一开始就**写清楚使用文档**，这一般是交付项目都需要做的，由于现在处在公共视野里，别人可能随时感兴趣，所以需要提前写清楚，如此一来，团队内部能更充分被检验这些东西是否清晰。如果这些项目成熟，则鼓励大家分享到Github上，那儿也是一个“公共空间”。

以学习树莓派（Linux环境）来说， 光是掌握 Linux 日常使用，似乎就已经是令人生畏的，我该学习多少命令行指令合适呢？ 我需要记住这些命令行参数吗？

有无穷无尽的知识需要学习，令人恐惧得无法前进。即使投入了大量时间，真正用到的时候又都记不得了。

更好的方式是看看团队里有经验的人如何使用它（通常是非常真实的用例），在这样的上下文（context）里学习。遇到困难，能得到同伴的及时帮助。

而“教学是最好的学习”，提供指导的同伴，也能更深入理解自己所掌握得东西，或者因此察觉到自己并为真正掌握。

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E4%BB%A5%E5%AD%A6%E4%B9%A0-linux-%E4%B8%BA%E4%BE%8B)以学习 Linux 为例

在一个公共树莓派里，对 Linux 感兴趣的人（我自己就是)，可能会动手把这台设备折腾得适合自己的习惯，他可能会:

-   安装[配置zsh](https://github.com/ohmyzsh/ohmyzsh)，使得命令行好用
-   开启 SSH/VNC，支持远程控制
-   安装 frp/ngrok, 用于内网穿透
-   安装 tmux, 用于结对编程和后台任务
-   安装 [fabric](https://github.com/fabric/fabric)，放置一些常用自动化脚本
-   …

以上的这些配置对于实施真实的项目通常都是极有用的, 如果PBL课程仅仅关注编程技巧，而不顾及这些情境，课程的末尾可能会发现难以实施真实的项目。

设想我构建了一个自动灌溉系统(智慧农业)，农田离我家可能是很远的，如果系统出了故障，当我需要进入系统进行调试时，我需要能够远程连入树莓派，此时 ngrok 和 SSH/VNC 是非常有用的。ngrok 能够穿透网络，允许我能在世界上的任何位置与目标计算机通话（即使它没有公网IP，设想它使用手机卡联网）。SSH/VNC则允许我登陆到机器内部。仿佛我就在现场。 这个场景（**你不在现场，但需要访问它**），在大多数真实项目中都有用，无论你制作的是自动化工厂还是智能家居项目。

这些经验通过阅读 SSH/VNC/ngrok 文档，是极为无聊且难以掌握的，因为你不知道哪些参数有用，所以只好尽可能多学多记，但是很快你就都记不得了，最好的学习方式是看看熟悉它的同伴是怎么做的，去学习那些能够及时投入使用的技能，这是在复杂系统里存活下来的有用技巧。否则很容易从入门到放弃。

当有人向我请教前头系统环境里的这些东西时，我是洋洋得意的，就像小时候有人向我请教玩具箱里的东西。不信，你试试向有经验的 Linux 爱好者请教他的系统配置就知道了！ 他们会跟你讲上一个下午，热情洋溢滔滔不绝，直到你妈妈喊你吃晚饭。

如果担心自己的代码和配置被弄乱，可以通过 git 来管理。这又提供了学习git的真实动机！你不需要学习所有git技巧，学习你在当前项目里用得着的。

在这样的环境里，学习有非常真实的情境和动机。这正式大多数围绕知识的学习所缺失的东西。

团队成员在这样的公共计算空间里，分享自己折腾的东西，知识以鲜活的方式呈现出来，当他们被请求提供帮助的时候，也是充满热情的，因为这些是他们的作品！

学习者在这样的环境里，很容易上手掌握如何使用 Linux，不知容易而且有趣，成员的经验很容易彼此共享，看看那些一起玩游戏的孩子就知道了。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E5%AD%A6%E4%B9%A0-python)学习 Python

树莓派(Raspberry Pi) 的 派(Pi) 最初含义是 Python。 我们接下来聊聊这样的**公共计算环境**如何有助于 Python 的学习。

拥有共同的上下文(context)，意味着大家有同样的视野(看到一样的东西)，无论这东西是知识还是错误。 这样我们就不需要分心在环境的差异上，入门时这种分心可能导致很大的注意力浪费。

以我刚构建的这台**公共树莓派**为例， 我在里头做了以下工作。

-   安装 jupyterlab，用于结对编程，解释项目代码
-   安装 supervisor，用于开机自启和进程管理
-   安装 flask，用于搭建网站以及对外提供开放API
-   安装 requests，用于从网络上获取信息
-   安装 gpiozero，以优雅的方式控制树莓派引脚，对海量硬件编程！
-   安装 paho-mqtt, 用于物联网项目
-   安装 codelab-adapter-client，用于与 CodeLab Adapter 通信
-   安装 schedule，用于定时任务
-   安装 ntfy/apprise，用于发送通知
-   安装 sentry, 用于错误发现
-   …

这些都是我自己特别喜欢的 Python 第三方库，每一个都极为强大和通用。

随便举一个例子，使用 flask 和 gpiozero ，你可以快速构建一个可以在网络上访问的智能农业装置（诸如使用gpiozero驱动一个水泵），如果你想让全球网络都能访问它，那么使用前边提到的的 ngrok 即可。如果你想使得这个装置更加稳定可靠，加上 sentry，你就能及时收到错误信息，诸如有黑客攻击你的设备！

我使用他们做了大量真实项目，我将我之前折腾的案例，放在树莓派里，大家通过浏览和修改这些项目，很容易获得构建一个Python项目的真实技能。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E5%85%B1%E4%BA%AB%E6%A0%91%E8%8E%93%E6%B4%BE%E4%B8%8A%E7%9A%84%E5%85%B6%E4%BB%96%E4%B8%9C%E8%A5%BF)共享树莓派上的其他东西

我还在这个共享树莓派上放了许多其他东西，诸如为它配置了一个 microbit v2，新手可以从这儿入门，既可以在 Scratch 对它进行编程(microbit more), 也可以在 mu-editor 上使用它。

同时我还很愿意介绍桌面上内置 pygame zero 的游戏（源码都可阅读！）:

[![](https://wwj718.github.io/post/img/WechatIMG23.jpeg)](https://wwj718.github.io/post/img/WechatIMG23.jpeg)

此外，我在树莓派里使用 flask 运行了一个网站，介绍我们在树莓派里做的一些项目，鼓励大家通过修改这个网站，介绍自己的新项目。

当然团队成员，也可以在网站里构建自己的个人主页，如果你想这样做，你就需要理解 flask 的路由(router)功能，需要构建自己的 html 页面，如果你掌握 CSS，页面会整得更加好看，要是你连 JavaScript 都学会了，你甚至可以在出页面上构建出弹来弹去的小框框来惹恼你的访客。 这些都提供了与自身相关、有意义的学习情境。

如果有新手想加入自己的个人主页，一开始不知道在哪儿配置 flask router，我会让他与我结对编程，手把手引导他，我们远程连入jupyterlab，就可以结对编程， 一起来讨论每一行代码的用处。

使用 ngrok，这个网站可以被全球访问！

我们也鼓励团队里的黑客精神的小伙伴去攻击它！

试试 Hydra 来暴力破解密码？来一波DDoS洪水攻击？

此时，有些人可能开始构筑防御设施了:

我们构建一个蜜罐(honeypot)来捕捉黑客如何？屏蔽掉这些危险的IP，你去加固一下防火墙吧； 构建软件的那谁，这份[安全清单](https://github.com/FallibleInc/security-guide-for-developers)发你, 此外，[常见的入侵策略](https://github.com/Hack-with-Github/Awesome-Hacking)也了解一下，知己知彼百战不殆。

单是一个项目介绍网站，我们可以延伸出几乎无穷无尽的项目，而且其中的每一个都是真实有意义的。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E7%9C%8B%E5%BE%85-%E5%85%AC%E5%85%B1%E6%A0%91%E8%8E%93%E6%B4%BE-%E7%9A%84%E8%A7%86%E8%A7%92)看待 公共树莓派 的视角

一种视角是回顾计算机的早期历史，黑客们在共享计算机上工作，从彼此代码中学习(开放源代码的源头)，并改进它， 《黑客: 计算机革命的英雄》对此有许多讨论。

另一种强有力的视角是把它看做土壤，想法的种子在里头孵化、生长。

多元的生物圈更加健壮。

[![](https://wwj718.github.io/post/img/12ffa6511531df87bb03458ed1ec83f2.png)](https://wwj718.github.io/post/img/12ffa6511531df87bb03458ed1ec83f2.png)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E9%99%84%E5%BD%95)附录

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#python-%E7%8E%AF%E5%A2%83)Python 环境

所有Python有关的环境，都使用虚拟环境:

```bash
sudo apt install python3-pip
sudo pip3 install virtualenv
virtualenv -p /usr/bin/python3 --system-site-packages ~/pyenv3
```

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#jupyterlab-%E5%8D%8F%E4%BD%9C%E7%8E%AF%E5%A2%83)jupyterlab 协作环境

```bash
source ~/pyenv3/bin/activate
# 安装 jupyterlab
pip install jupyterlab
# 在虚拟环境中安装最新 jupyterlab
jupyter lab --collaborative --ip="*" --ServerApp.token=raspberry
```

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E5%AE%89%E8%A3%85%E6%9C%80%E6%96%B0%E7%9A%84-mu-editor)安装最新的 mu-editor

[Running Development Mu#raspberry-pi](https://mu.readthedocs.io/en/latest/setup.html#raspberry-pi)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/#%E5%8F%82%E8%80%83)参考

-   Dynamicland
-   OLPC
-   the clubhouse network
-   [the-art-of-command-line](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)
-   [Python教育资源大全中文版](https://github.com/wwj718/awesome-python-in-education-zh)
-   [树莓派(Raspberry Pi)资源大全中文版](https://github.com/wwj718/awesome-raspberry-pi-zh)


---
_本文转载自 夜行人的博客_ - [公共树莓派](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/)