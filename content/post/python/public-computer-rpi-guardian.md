---
title: '公共树莓派案例: 树莓派守护者!'
date: "2021-09-02"
tags:
  - python
slug: public-computer-rpi-guardian
---


> 在一个线下公共空间里，放置一些计算设备，参与者在这个环境里，用编程表达自己的想法，构建自己感兴趣的项目。大家的探索过程和成果都置于公共视野，想法和经验持续流动，彼此启发，就像 Scratch 线上社区。 – [《公共树莓派》](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/)

本文是 [《公共树莓派》](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/) 的第一个案例。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi-guardian/#%E6%A0%91%E8%8E%93%E6%B4%BE%E5%AE%88%E6%8A%A4%E8%80%85)树莓派守护者！

[![](https://wwj718.github.io/post/img/pi_swzbe54be0b.png)](https://wwj718.github.io/post/img/pi_swzbe54be0b.png)

树莓派守护者是我在2016年做的一个项目。 那时刚玩树莓派不久。

**树莓派守护者！** 的想法是: 使用树莓派 + 超声波传感器，构建一个小偷报警器，解决我童年时候遇到的一个问题:

> 老板说自己早便发现他们入室盗窃，为了证据确凿，设计了一套精巧的陷阱：在柜台入口，近地面处系一根绳子，绳子一直连到老板睡觉的卧室，在卧室里系上易拉罐。少年们再次登门，触发开关，弄倒老板卧室的易拉罐，老板醒后，有备而来，少年们毫无知觉，来个瓮中捉鳖. – [树莓派守护者!](https://wwj718.github.io/post/iot/pi-guardian/)

我觉得这计划稍有不足:

> 有一处不够优美:报警器竟是绳子做的！如果少年们更警觉些，看到绳子，或是踩到之后便逃离，老板恐怕要竹篮打水一场空。

我想用树莓派作为解决方案:

> 将超声波传感器（或者红外线）放在柜台下边，当有人路过时，树莓派给老板手机发送一条短信（或邮件），这个隐形的卫士几乎没有破绽

后来我发现，这装置可以用来**守护私密空间**: 当树莓派检测到有人进入房间，给电脑发送信号，将屏幕切换到阅读状态。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi-guardian/#%E5%9C%A8%E5%85%AC%E5%85%B1%E6%A0%91%E8%8E%93%E6%B4%BE%E4%B8%AD%E6%9E%84%E5%BB%BA%E5%AE%83)在公共树莓派中构建它

我在**公共树莓派**中构建的这个项目，希望能通过这些项目分享我的经验，并希望团队成员能 remix 它，去构建自己的项目。

也想借此分享我喜欢怎样使用树莓派（Linux）.

整个项目在一个 notebook:[distance.ipynb](https://github.com/aimaker-space/Public_RPI_PBL/blob/main/AlanRussell_Lab/distance.ipynb)里， 在局域网里打开树莓派的 jupyterlab 地址，就可以发现并运行这个项目，当然也可以通过 SSH/VNC 进入树莓派运行它。

[![](https://wwj718.github.io/post/img/3ec09f955d4ceba7ceb7e3947cc522c4.png)](https://wwj718.github.io/post/img/3ec09f955d4ceba7ceb7e3947cc522c4.png)

项目的开头使用 markdown 对项目做了基本描述：从想法的来源到使用到的核心库。

今早，我给团队成员分享我的这个项目，演示完抓小偷的功能，我邀请大家进入 jupyterlab 里与我结对编程，通过修改 notify 函数，将通知推送给自己。

接下来演示了如何使用 git 管理这个项目，以及如何将 jupyterlab 当作主要的编程环境： 文件上传与管理以及系统管理(terminal)都可以在 jupyterlab 里完成。

分享结束之后大家各自都有想实现的新想法，并希望我给出进一步学习 Git/Linux 的新资料，我把它们放在了[这里](https://github.com/aimaker-space/Public_RPI_PBL#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi-guardian/#%E5%B0%8F%E7%BB%93)小结

项目中的好几个部分是通用的:

-   展示树莓派社区最好的[GPIO驱动](https://gpiozero.readthedocs.io/)的用法，用于与各类传感器交互
-   展示如何推送通知（展示了2种方式），这在大多数项目中都很有用
-   展示如何在 jupyterlab 中使用 terminal
    -   在terminal里使用 ngrok 将 jupyterlab 投射到公网
-   分享到 Github: [Public\_RPI\_PBL](https://github.com/aimaker-space/Public_RPI_PBL)
    -   使用 Git 做版本管理
    -   使用项目wiki写博客
-   讨论了开机自启的思路（暂未实现）

大家的热情是超出我的预期的，几乎每个人都觉着过去这些令人生畏的东西非常有意思。

我们希望这个**公共计算空间**随着时间的演进，会持续产生新项目新想法，彼此启发与混合。

就像你家阳台的花盆里所发生的事情。

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi-guardian/#%E5%8F%82%E8%80%83)参考

-   [公共树莓派](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/)
-   [树莓派守护者!](https://wwj718.github.io/post/iot/pi-guardian/)
-   [超声波传感器](https://gpiozero.readthedocs.io/en/stable/recipes.html#distance-sensor)
-   [Public\_RPI\_PBL](https://github.com/aimaker-space/Public_RPI_PBL)

---
_本文转载自 夜行人的博客_ - [公共树莓派案例: 树莓派守护者!](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi-guardian/)