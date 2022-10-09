---
title: 如何正确地将SketchUp白模优雅导入UE4
date: 2021-09-20
categories: 闸总自白
tags: 
- 关卡设计
cover: /images/20210918-suue.png
toc: true
---

SketchUp → Datasmith → UE4 = SU白模流程验证。

<!--more-->

---

## 插件导入流程

SketchUp导入UE4需要借助导出插件Datasmith，这是官网的文档说明：[结合使用Datasmith与SketchUp Pro](https://docs.unrealengine.com/4.27/zh-CN/WorkingWithContent/Importing/Datasmith/SoftwareInteropGuides/SketchUp/)。

### 安装Datasmith

去[Datasmith网页](https://www.unrealengine.com/zh-CN/datasmith/plugins)下载SketchUp PRO导出器，安装，它会自动识别你安装的SU版本。

### SU内Datasmith导出

插件安装完毕后，就能导出datasmith文件了。可以全量导出，包括场景摄影机（scene）和光照，因此UE4内可以只准备个完全空白的level。

### UE4内Datasmith导入

在UE4内的工具栏，找到Datasmith，选择对应文件并导入。导入时可以选择1.导入位置 2.导入内容 等选项，根据需要勾选。导入成功后，会把资源放入当前关卡。

如果工具栏没有Datasmith，要去plugins里面进行启用，而不是project settings。

<br/>

## UE4的SM生成规则

自己做了个简单的对比实验，看看UE4是以什么规则去生成SM的。*上网搜应该也可以搜到，和实验结果应该是一样的。*

![](/images/20210920-suue/ex.png)

**no group**：完全没有打组。所有连在一起的面都导出成了一个SM，没有父物体。

**1 group**：全部打了一个组。所有连在一起的面都导出成了一个SM，有一个父物体。

**wall/floor group**：每个墙面/地板面打了一个组。所有组单独导出了一个父物体+一个SM。

这几个白模较为简单，实际上，如果组内有面分开的情况，会在父物体下导出成多个SM。

对于no group和1 group，把对应SM的碰撞改为 **Use Complex Collision As Simple** 就OK了。对于wall/floor group也适用，但由于墙面和地板面本来就是简单box，也可以直接添加简单碰撞。

<br/>

## SU绘制规范

因为SU绘制不规范，导入后吃了很多亏，有苦说不出。为了保证导入模型的质量，从而实现**SU修改白模→导入UE4进行验证→SU修改白模**的关卡验证流程，需要提高SU的白模质量。

1. 可复用内容打成组件，复用性较低的不要打成组件，群组即可。
2. 如果不是完全确定尺寸与结构，尽量以墙→地板→内部结构的颗粒度去打组。
3. 所有能被正反面看到的结构都需要是墙体。即，只有关卡边缘才可能用面。
4. 管理正反面，保证所有朝外的面都是正面。
5. 给每个block拍摄一个scene，方便日后修改与查看。
6. 如果存在多层，尽量给整体→每层→每block都拍摄顶视图scene。
7. 及时擦除无用的参考线，不要等线多到选都选不中了才开始擦。
8. 多往场景里拖预制件小人，进行尺寸对照。*做完后记得删*
9. （踩坑了会继续补充，虚位以待）

<br/>

