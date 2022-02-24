---
title: '可编程书包'
date: "2021-10-15"
tags:
  - python
slug: ledbag
---

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#what)what

[![](https://adapter.codelab.club/img/4a2c14f97a6a25c20a2e401305a0fb77.png)](https://adapter.codelab.club/img/4a2c14f97a6a25c20a2e401305a0fb77.png)

我目前背的这个书包是 @zooming 送我的, 最近拿它来做 《Python 编程基础》课的教具, 效果好得出奇。

事情的开头是这样的。 @leeyve 前些时候买了个可编程挂饰: [imagiCharm](https://imagilabs.com/), @zooming 觉得他女儿会喜欢在这种闪闪发光的好看设备上编程。 于是在网上找到一个类似的东西，同样带有可编程led矩阵，但性价比更高: 可编程书包。

@zooming 的女儿在看到可编程背包之后，大感兴趣，于是 @zooming 来找我看看能不能对背包的全彩点阵屏进行编程，我们用了半个傍晚的功夫，就在[公共树莓派](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/public-computer-rpi/)上完成了背包的 Python 驱动。

<video width="80%" src="https://adapter.codelab.club/video/1631788391562342.mp4" autoplay="" controls="controls"></video>

过了几天，@zooming 送给我一个更大的可编程背包。就是现在我背的这个。

[![](https://wwj718.github.io/post/img/07da324ee3fa5b382c2ca3d93651ea27.png)](https://wwj718.github.io/post/img/07da324ee3fa5b382c2ca3d93651ea27.png)

拿到书包后，我用了一晚上将驱动从树莓派移植到了 macOS 和 windows, 这样 @zooming 的女儿就不需要额外添置一个树莓派了。

<video width="80%" src="https://adapter.codelab.club/video/9563d376e264a35d051f2d1ae6ca03.MP4" autoplay="" controls="controls"></video>

周末又用了一个晚上，在 JupyterLab 完成书包模拟器，如此一来还没有书包的人可以使用模拟器来编程。

我们来看看 @zooming 写的一个程序: 在 Adapter JupyterLab 里同时驱动可编程像素艺术全家桶（可编程书包Python驱动也可以驱动可编程相框、音响…）

<video width="80%" src="https://adapter.codelab.club/video/f4d76c09adbf9175727b943fc38a6b.MP4" autoplay="" controls="controls"></video>

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#why)why

最初为书包写 Python驱动 的动机是供 @zooming 教他女儿编程用。

但后来

> 最近拿它来做 《Python基础入门》课的教具, 效果好得出奇。

把可编程书包用在Python入门课上的想法，是 @leeyve 想到的，英荔教育关注编程教学内容的 **可理解性** (我目前在英荔创造乐园讲授《Python 编程基础》)，@leeyve 寻找到很多有助于实现这个目标的教学设施和内容。 @leeyve 觉得可编程书包是很好的载体，编程知识围绕它可以很容易展开，而且容易呈现得很有意思，[imagiCharm](https://imagilabs.com/) 给出了很好的示范。@leeyve 在体验[imagiCharm](https://imagilabs.com/) 的Python课程后，感受非常良好，多次给我推荐。

> 甚至可以更大胆些，将书包作为主体线索，来展开课程教学。

我试着模仿 [imagiCharm](https://imagilabs.com/) 的课程风格上了一节课:[week3-控制流与像素艺术](https://docs.google.com/presentation/d/1VB73agqVT0C_0MO2j5Ogzo5OjIRel4pwm0uELyLtXY4/edit?usp=sharing)，效果超出预期。学习者表现出很大的热情、表达欲和探索欲。这和上节课形成鲜明对比。

我在上节课里试着采用比较“干货”的[《A Byte of Python》](https://learnku.com/docs/byte-of-python/2018)开展教学时, 学习者表现出的抗拒，和 Seymour Papert 在《Mindstorms》提到的几乎一模一样, 大家不愿意参与课堂中的编程练习([基本语法与数学运算](https://learnku.com/docs/byte-of-python/2018/op_exp/3342))， “老师，我不想使用编程，用笔计算更快”。 学生们不明白这些“干货”和练习的意义何在，我想大多数教育者都曾面对过这个问题(关于why的问题)，Seymour讽刺说教育者们一种可笑的回答是，学会这些你就可以解课后习题了(学校的考核体制决定了这种笑话一直不会结束)！确实，除此之外他们在“干货”课堂上几乎没有表现出任何意义。 学生们觉得计算机是一种笨重的东西，把事情复杂化，进而抗拒使用它。

可编程书包(led矩阵屏)带来了完全不同的学习体验:

1.  知识以可见的方式呈现出来，这极大提升了可理解性。 编程语言中的许多东西是一种逻辑结构，诸如 for/while/if 控制结构，当学习者使用这些抽象的语法结构时，它在矩阵屏幕上立刻变得清晰可见！于是你就可以对自己的想法进行debug！
2.  可编程书包(led矩阵)是一种鲜活的媒介，它呈现出图形，因而可以承载和表达个性化的想法，而像素风格使这事儿非常简单。这方面的特质有点像 Scratch 舞台。非常容易激发学习者的个性化表达。
3.  完成的作品可以背在肩上作为个性化饰品。

关于这种不同的学习体验，举几个课堂上的真实例子，以下代码是可编程书包的 hello world:

[![](https://adapter.codelab.club/img/07609aadfd5b5e04a8b3a78e2fd9c9b6.png)](https://adapter.codelab.club/img/07609aadfd5b5e04a8b3a78e2fd9c9b6.png)

当你让学生尝试经典的 `print('hello world')`, 学生很快会觉得乏味，不愿意尝试更多，但当使用以上代码作为 hello world 时，几乎不需要说更多，他们就会探索不同的位置参数对结果的影响，试图按照自己的意图点亮屏幕的不同区域，以构造出某种好玩的效果(诸如竖起中指， 有个学生硬是使用这个单一的函数逐像素绘制出了中指图标，然后刷到书包上，他自称硬核玩家)。

等到他们学到更复杂的控制结构(for/while/if), 他们基本不会觉得这是一种知识负担 , 感受到的是一种力量, 一种可以帮助自己更快/简单地实现自己想法的结构，尤其是那些“硬核玩家“，有一种解脱感，不再需要逐像素控制了，“可以一次操控一大批” ！

[![](https://wwj718.github.io/post/img/ab5e5b2d81b08eda4a7feea88a2ba420.png)](https://wwj718.github.io/post/img/ab5e5b2d81b08eda4a7feea88a2ba420.png)

当学习者使用这段代码一次点亮一排 led:

```python
from codelab_adapter.led_bag import *
for i in range(16):
    set_pixel(0, i, 'red')
```

Ta 以为掌握了for循环的使用方法。你给个小挑战，说试试点亮两行，学习者很容易写下以下代码:

```python
from codelab_adapter.led_bag import *
for i in range(16):
    set_pixel(0, i, 'red')
    set_pixel(1, i, 'blue')
```

[![](https://wwj718.github.io/post/img/for-2-lines.gif)](https://wwj718.github.io/post/img/for-2-lines.gif)

等到 Ta 看到代码运行的过程，可能会傻眼，惊讶地盯着屏幕，反复运行，因为这跟 Ta 脑子里预期的并不一样。

Ta 前边看到 for 控制的 `set_pixel(0, i, 'red')`: 一次刷刷刷地点亮了一排；于是 Ta 想到，再加一个 `set_pixel(1, i, 'blue')`， 预期着第一排led先被刷刷刷点亮，然后是第二排刷刷刷点亮。 结果却发现上下两排交替运行，愣了半天 Ta 才真正理解for循环控制结构的运行过程: **语句块** 作为整个单位，每次循环中，都自上而下执行一遍整个语句块。如果没有这种可视化的编程环境，很难有如此深刻的印象。 整个逻辑结构几乎是直观地显现在眼前！ 甚至意外和错误，也是如此，可以直接与大脑里的预期图景对比。

说起来好笑，我自己甚至在第一次运行上边代码时也感到惊讶。「惊讶」意味着它和我预期不同。

我对「我对此感到惊讶」这事感到更惊讶， 我几乎每天都会用到for循环。当然也很清楚for循环的控制范围: 每次循环，完整运行一次整个语句块。但理智上虽清楚，直觉上却还是有不同的预期，这事儿非常有趣。

但自从在书包上可视化地看到这个运行过程，我好像就可以**直观**感受到for结构，几乎是一种无需思考的直觉（可能是强烈的视觉反馈造成的），在此之前错误直觉可是伴随了我好多年啊！

这让我更加理解 Alan Kay 和 Bret Victor 工作的重要性: 从动态媒介(Dynamic Media)的视角看待计算机，将它视为理解其他事物，尤其是不可见的事物的媒介，进而增强人类，拓宽他们可感知的事物的边界，使得之前无法被思考的东西，变得有形状，可感知，进而可被思考。「个人计算社区」 为这个想法奋斗了几十年，他们工作的副产品是塑造了今天的计算机和计算形态，但依然远远没有抵达目标。 我目前也追随「个人计算社区」的理想。

[![](https://www.codelab.club/img/8e7dc0768797cfa0fa044a407cbc0bd1.png)](https://www.codelab.club/img/8e7dc0768797cfa0fa044a407cbc0bd1.png)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#how)how

CodeLab Adapter 最新版本内置了可编程书包的驱动，也提供了模拟器。

关于如何对书包编程，可参考 《Python 编程基础》 课堂PPT 和 [LedBag 文档](https://adapter.codelab.club/extension_guide/ledbag/), 也推荐在 Adapter notebooks 的 `hello_LedBag.ipynb` 做实验。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#hello-world)hello world

```python
from codelab_adapter.led_bag import *
set_pixel(0, 0, 'red')
```
[![](https://adapter.codelab.club/img/07609aadfd5b5e04a8b3a78e2fd9c9b6.png)](https://adapter.codelab.club/img/07609aadfd5b5e04a8b3a78e2fd9c9b6.png)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E6%9B%B4%E5%A4%9A%E5%AE%9E%E9%AA%8C)更多实验

目前围绕可编程书包，还做了其他的一些实验。

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E7%94%9F%E6%B4%BB%E5%9C%BA%E6%99%AF)生活场景

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E4%B8%96%E7%95%8C%E6%9D%AF%E7%9B%B4%E6%92%AD)世界杯直播

想象你正在下班路上，你最喜欢的球队进入了世界杯决赛，你要向路人们实时直播比赛状况！

使用树莓派驱动书包，将树莓派连到网络上(手机热点)，实时获取决赛比分实况，将结果显示在书包上！

[![](https://wwj718.github.io/post/img/a537607af19e19ff41cd5cfd1df39667.png)](https://wwj718.github.io/post/img/a537607af19e19ff41cd5cfd1df39667.png)

#### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E8%87%AA%E8%A1%8C%E8%BD%A6%E8%BD%AC%E5%90%91%E7%81%AF)自行车转向灯

夜间骑车的时候，得注意安全 ⚠️ ，背个发光的书包是个不错的想法。

我们可以更进一步，为自行车添加转向灯，整个书包就是一个大型转向灯！

[![](https://adapter.codelab.club/img/4a2c14f97a6a25c20a2e401305a0fb77.png)](https://adapter.codelab.club/img/4a2c14f97a6a25c20a2e401305a0fb77.png)

原理是这样的，在自行车头绑一个树莓派(pi zero足够小)，将陀螺仪绑到树莓派上就能够感知车头左右转向(也可使用micro:bit)，然后把转向的结果反馈在书包上！

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E5%A4%9A%E7%BB%B4%E6%95%B0%E7%BB%84-%E7%9F%A9%E9%98%B5-%E5%BC%A0%E9%87%8F-%E6%95%99%E5%AD%A6)多维数组(矩阵/张量)教学

我写了一些使用 NumPy 操控可编程书包的的例子，对于以可视化的方式学习多维数组可能有帮助。

将图片转为形状为 `(16, 16, 3)` (x, y , color) 的多维数组。 每一个维度都可见（像素的位置、颜色）！

这对于 NumPy 的学习可能有帮助. NumPy 几乎是 Python 生态里所有深度学习的基础。

```python
from PIL import Image
img = bag.load(filename)
bag.show(bag._img)

## PIL image to Numpy
imgarr = np.array(bag._img)

# imgarr.shape (16, 16, 3)
# 多维数组运算
imgarr_1_2 = imgarr // 2  # 所有元素都 //2

## Numpy to PIL image
img = Image.fromarray(imgarr_1_2)
bag.show(img)
```

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E5%83%8F%E7%B4%A0%E6%B8%B8%E6%88%8F)像素游戏

我做了一个原型，在 JupyterLab 里连接gamepad，将可编程书包编为一个移动游戏平台！

<video width="80%" src="https://adapter.codelab.club/video/04142c1c7f22ab2b4d0820eef22c93.MP4" autoplay="" controls="controls"></video>

### [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E5%8A%A8%E7%94%BB)动画

[animation-api](https://adapter.codelab.club/extension_guide/ledbag/#animation-api)

## [](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/#%E5%8F%82%E8%80%83)参考

-   [LedBag docs](https://adapter.codelab.club/extension_guide/ledbag/)
-   [codelab projects](https://www.codelab.club/projects)
-   [week5-函数](https://docs.google.com/presentation/d/1hbEP77xLO4Qwlb3KaJl60kIuzuny5NgK8U--cvqiwDo/edit?usp=sharing)
-   [NumPy quickstart](https://numpy.org/doc/stable/user/quickstart.html)

---
_本文转载自 夜行人的博客_ - [可编程书包](https://wwj718.github.io/post/%E7%BC%96%E7%A8%8B/ledbag/)