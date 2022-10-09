---
title: SketchUp上手时的注意事项
date: 2021-09-07
categories: 闸总自白
tags: 
- 关卡设计
cover: /images/20210907-sktup-tips.png
toc: true
---

踩完坑要长记性，下次一定。

<!--more-->

---

   <br/>

## Use Keyboard shortcuts

SU的操作逻辑和UE4有诸多不同，学习使用快捷键，效率起码提升30%。

   <br/>

## Move objects along axes

沿着红绿蓝三条轴线移动物体，移动时轨道会变成对应的颜色。如果不在轴线上移动，SU只能猜测你想移动到哪里，大概率不符合预期。

<br/>

## Use inferencing to place objects

> **The inference system is basically a system that locks your cursor in \*reference\* to any point, edge, axis, face, guide or imaginary line.**

一言以蔽之，就是自动连线。当尝试连接一条线到另一条线的端点时，SU会自动辨认并提示一个绿圈，如果按下则两线连接。如果不依靠自动连线，直接画立方体会非常困难。

<br/>

## Use 3D warehouse

SU内自带一套类似于UE4的marketPlace的资源商城，可以从里面下载各种现成建模资源。

从个人出发，关卡设计更关注空间结构和流程设计，不该花费过多时间在具体物件的设计上。

**Focus on modeling space.**

<br/>

## Model using groups

group就是UE4里的打组，可以把多个东东打成一包，下次选择时会默认全选。

如果不打组，模型的面和面又贴在一起，SU会默认为merge，想分离时会相当困难。*狠狠地犯了错，下次请别。*

值得注意的是：UE4打组后会在场景里再生成一个group actor，下次选择时只能选择一个组；SU打组后可以通过双击或三击选择里面的模型，非常感人。

<br/>

## Model using components

component是个类似于Unity里prefab的概念。一个component有多个copy时，修改其中任意一个，其他所有copy都会跟着一起变化。

非常适合模块化关卡设计，下次请记得务必使用components。

<br/>

## Copy Mode

宁知道吗？move mode和rotate mode是可以直接复制模型的，此功能类似于UE4。

ctrl + move/rotate操作，可以直接复制出一份选中模型的copy。*UE4里是alt。*

<br/>

## Create multiple copies at once

承接上面的copy mode话题，如果在copy操作时输入了times（要复制多少次），就会一次性复制出多个。

<br/>

## Create equally space copies

再次承接上面的copy mode话题，选定两个模型后可以在中间等距离复制出多个，/+times等于在中间生成的数量。

<br/>

## Use extension

字面意义，使用插件。

*当然，直接去玩玩新的软件比如Blender也是极好的。*

<br/>
