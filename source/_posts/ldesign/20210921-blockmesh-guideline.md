---
title: 关卡设计指导规范：白模制作
date: 2021-09-21
categories: 闸总自白
tags: 
- 关卡设计
cover: /images/20210921-blockmesh.png
toc: true
---

关卡制作管线的第二环：白模制作的指导规范。

<!--more-->

---

## Level Design Pipeline

关卡制作管线可以粗略分为三大环节：流程规划、白模制作和游玩迭代。在之前一篇文章里，总结了自己对于[流程规划的思考过程](https://uynad.github.io/2021/09/09/ldesign/20210910-how-to-ship/)。近日回顾了一个[Level Design方向的GDC视频](https://www.youtube.com/watch?v=09r1B9cVEQY)，顽皮狗的David Shaver分享了自己对于Blockmesh（白模制作）的思考角度，收益颇丰。

实际上，之前的流程规划思考，在Level Design Pipeline里就对应着Level Requirements环节。而这个GDC视频，主题是第二环节的Build Blockmesh Layout。*正巧和优秀设计师的思路接上轨，把别人的视角拿来利用，真妙。*

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/pipeline.png 700 '"Level Design Pipeline" "Level Design Pipeline"' %}

<br/>

日后会针对Playtest编写第三篇文章，就此由三部曲组成完整的关卡设计指南。希望在日后实操里不要忘本，同时要不断进化。

## Blockmesh Guidance Principles

David在开头列举了所有Blockmesh制作时的设计角度，结论放最前。

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/sum.png 700 '"Blockmesh Principles" "Blockmesh Principles"' %}

<br/>

下面的分条说明，会截取PPT原文作为留档，但文章部分是个人想法而非原文内容，请注意。

<br/>

### Affordance

Affordance是个设计领域的自创词，是Donald Norman在《设计心理学》里引入的一个心理学概念。这个词是afford（可提供）的名词变种。

> （Affordance）示能这个词，是指一个物理对象与人之间的关系。无论是动物还是人类，甚至是机器和机器人，他们之间发生的任何交互作用。
>
> 示能的体现，由物品的品质，和与之交互的主体的能力共同决定。

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/affordance.png 700 '"Affordance in Game" "Affordance in Game"' %}

<br/>

可以将示能理解为场景关卡交互，但其含义远不止“有交互功能”，还包括“统一的交互原则”。例如有裂痕的墙壁是可破坏交互物，血量归零后自我销毁；那么这种墙壁的外观、摆放逻辑等，要在整个游戏里保持一致，不能这次普攻能打碎，下次就得靠炸弹来打碎。

制作Blockmesh时，可以靠颜色和形状做出几个prototype去摆放，哪怕没写程序功能。例如我在自己的白模设计里，会用颜色深浅来区别不同高度的掩体，表示其功能的变化。

<br/>

### Denying Affordance

反示能（拒绝示能？负示能？），字面意思，是示能的反面。示能让主体意识到可交互及交互结果，反示能让主体意识到不可交互，从而主动远离。*当然，游戏界最经典的反示能就是空气墙*

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/deny.png 700 '"Denying Affordance in Game" "Affordance in Game"' %}

<br/>

contextualize是件易懂难做的事。有多种方法可以达到反示能效果，但有些方法会比其他方法分数更高。例如，不想让玩家去攀爬的缺口墙壁，会在缺口设计一些木刺和挡板，以表示“此路不通”。在一个房间存在多个入口，但设计师只希望玩家从特定口进入时，其他口应处于封闭状态。

这一节出现了个有趣的点。顽皮狗内部CE时，会**用不可见mesh圈定“不想让玩家去的反示能区”**，日后可以和游玩数据比对，以及更有效地向别人展示设计意图。

<br/>

### Visual Language - Shape

形状与颜色的使用有着相似准则，例如要在整个游戏里（或者起码本关卡）保持统一性，同时可以传达设计信息给其他设计师，作为图例来使用。

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/shape.png 700 '"Visual Language - Shape" "Visual Language - Shape"' %}

<br/>

不同形状存在着不同的心理暗示，例如圆体是安全，方体是稳定，尖刺是危险。利用不同形状，可以有效示能。

<br/>

### Visual Language - Color

除去上述的形状功能，颜色还拥有自己的用法。

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/color.png 700 '"Visual Language - Shape" "Visual Language - Shape"' %}

<br/>

稍微可变性：颜色要适应环境美术，突出即可，不必强求同一颜色。例如可攀爬的黄色，在部分场景里可能会变为浅绿色。

场景叙事性：在白模阶段，颜色比形状的叙事功能更突出。想要设定白模舞台为学园、绿林等，可以通过简单上色来展现效果。

<br/>

### Landmarks

地标，顾名思义的存在。在这里发现了一个差异，个人会把所有用来指引路线的特殊景色都称为地标，但演讲特意指明了是“多个角度都可见的遥远对象”。鉴于演讲者的细分程度比我的要丰富得多，之后也打算划清概念，只用来指代远处对象。

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/landmark.png 700 '"Landmarks" "Landmarks"' %}

<br/>

要设计地标是所有关卡的共识，实际设计效果却大相径庭。近年来最喜欢的地标设计风格是《对马岛之鬼》，制作组曾在GDC视频中表示，他们的地标设计期望是“玩家每十秒就要看到一样新东西”。*显然，对马岛对地标的定义又宽泛了起来。*

在系统玩法相对空荡的情况下，靠地标景观去打造一个世界，不论这一设计目标本身如何，Sucker Punch算是做透彻了。

<br/>

### Openings Attract

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/opening.png 700 '"Openings Attract" "Openings Attract"' %}

<br/>

Openings在这里指开口，而非宝箱等交互物。此类开口一般会引向一个安全区，*恶意点做个开门杀也不是不行*，因为让人感到神秘，“有什么事情会发生”，因而才有动力去探索，由此被自然引导。

<br/>

### Gates & Valves

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/gate.png 700 '"Gates & Valves" "Gates & Valves"' %}

<br/>

做过线性游戏关卡的同志，应该对这一设计再熟悉不过。我们来到一个战点，为了强迫玩家面对战斗谜题，需要在入口做一道适时打开的大门（Gates），然后再在玩家进入后将其关在里面（Valves）。

将Gate与Valve分开，是因为部分情况下它们不是一样物品。例如TLOU里，流程中有一个Valve是1.5人高的下落，一旦下去后无法返回。这种Valve会比自动关门要合理得多。

Gate和Valve应该是具有极高复用价值的关卡功能，在开发关卡流程时，请记得接入并不断丰富，不要单独造轮子。

*反思了下自己的设计，大量用到自动开关门。虽说室内关卡无可奈何，但也不至于如此单一。*

<br/>

### Leading Lines

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/line.png 700 '"Leading Lines" "Leading Lines"' %}

<br/>

引导线是纯景观系，不具备玩法功能。在室内场景，这种素材相当常见，比如黄色电线、天花板水管和暴露在外的电路等。这里又引到上面的话题，黄色与圆柱体是稳定、引导的象征，因而最常见是黄色电线；如果改用红色，起码在个人想象里会很突兀。

<br/>

### Pinching

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/pinch.png 700 '"Pinching" "Pinching"' %}

<br/>

*pinch，指拿捏的意思。这里的pinching含义见第一行，通过形状摆放使玩家要穿过特定角度。*

TLOU里有两个例子。其一是公交车被摆放为集中一点型，玩家在步行时自动被引向该目标点。其二是前往医院的路上，在下地下通道时，有一辆大货车阻挡了路线。顺着大货车车身走，会自然在末尾看到远处的医院地标。

<br/>

### Framing & Composition

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/composition.png 700 '"Framing & Composition" "Framing & Composition"' %}

<br/>

摄影的构图知识，在关卡设计里同样适用。关卡要将玩家的注意力吸引到兴趣点上，既要让目标置于正中央，又要适度遮挡不必要的物体。

TLOU里*又*有个好例子，乔尔沿着上坡前进，转角看到占据画面2/3正中央的大坝。同时，大坝在远景里也非常详尽，细节丰富的物体会自然吸引玩家眼光。

<br/>

### Breadcrumbs

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/breadcrumb.png 700 '"Breadcrumbs" "Breadcrumbs"' %}

<br/>

面包屑，最著名的例子就是马里奥系列中的金币。

可以是场景中的小摆设、敌人、拾取物、交互物或资源等，与上面的指引物的区别在于，面包屑相对较小且不影响整体layout。演讲者并不推荐过度使用面包屑功能，虽然很有效，但会让人轻视整体layout设计。

<br/>

### Textures

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/texture.png 700 '"Textures" "Textures"' %}

<br/>

存在于模型材质里的信息，尤其是文本信息，可以提供非常直观的引导作用。在现代或未来题材里，这种引导方法非常好用，且能大大丰富场景叙事功能。*在城市里设计各种玩梗招牌，居然比做关卡还有趣……但要记得和文案对，不然会被重拳出击。*

之前在[虹咲动画场景叙事分析](https://uynad.github.io/2021/09/03/article/20210904-nijigasaki/)里，提到了交通信号对于人物心理的暗示功能。下次有合适主题时，也想用下试试。

<br/>

### Movement

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/movement.png 700 '"Movement" "Movement"' %}

<br/>

移动是指场景元素的移动，而非角色本身。环境生物、动态场景、特效和敌人等，都可以成为“会动的路标”。

演讲者在这里提到，移动是关卡提高引导性的最后一个创口贴，除非到了万事休矣的地步，最好不要大量使用。

<br/>

### Light & God Rays

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/light.png 700 '"Light & God Rays" "Light & God Rays"' %}

<br/>

演讲者把打光提到了一个很高的地步，要求在白模阶段就定好灯光方案。从顽皮狗作品里也能看出，大部分关卡的灯光存在感极高，不仅仅是为了做美术氛围而存在。

灯光的**有和无、强与弱、暖与冷**是个人的三层指标，建模完成后，导入引擎内要记得多多打光。

*回顾了下之前的白模，只在出入口等重要点放了点光源，非常敷衍，汗颜。*

<br/>

### The Squinting Test

{% img "box px-0 py-0 ml-auto mr-auto" /images/20210921-blockmesh/squint.png 700 '"The Squinting Test" "The Squinting Test"' %}

<br/>

此条为演讲者新增，并非设计角度，而是审视设计的角度。演讲者举出了Titanfall2中的一幕，要求读者一起盯着画面几秒，思考几个问题：你注意到了什么？注意对象的先后顺序是？你的最终目标点是哪个？……

在审视关卡的过程里，不断调整关卡结构与灯光，让玩家去注意设计师希望他们注意的地方。

<br/>

## Epilogue：为何写，如何用

Q：你这篇文章不就是GDC视频的读后感嘛。这个视频很多人都看过，肯定也有很多人写过repo，你为什么还要写这篇文章呢？

A：确实。有很多人在我之前就学会了这个，可能还比我做得好；但这不代表我就不需要去做了嘛。

我非常认同这篇演讲的设计切入点，读后感文章既是复读，也是个人延伸。把它写进自己的blog，日后可以对照文本版去思考设计，也满足了个人的园丁鸟癖好。*园丁鸟喜欢收集亮晶晶的东西，会叼回来放进自己的巢。*

这篇演讲的15条准则，没有先后次序，甚至不完全是并列顺序。例如视觉语言形状与颜色，其实是普适性原则；而面包屑和移动，则偏向后期的修补调整。

演讲的立意，不是教人“如何去做白模”，而是“如何将白模做得更好”。想要提高白模水平，还是要自己亲自实践。

<br/>

