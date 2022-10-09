---
title: 《怪物猎人崛起》工程解包及MOD制作指南
date: 2022-02-14
categories: Devlog
tags: 
- 游戏开发
- 战斗设计
cover: /images/20220214-mhr-export.png
toc: true
---
关于如何解包MHR工程并修改数据制作MOD的说明文档。WIP

<!--more-->

---

*以下内容为草稿，待整理成完整文章，在做了在做了*

## 解包前置条件

### 下载游戏本体

### 熟悉RE引擎

可以用REFramework在游戏里看看基本是什么东西。

### 下载必需工具

<br/>

## 简简单单开个包

PC已经有pak，不需要再进行NS镜像反拆这一步。

### 拷贝pak和list至RETools目录

分别把本体的re_chunk_000.pak和mhrisePC.list复制到RETools的目录里。前者包含了游戏的数据文件，后者是源文件对应路径列表。

### “对pak使用解包bat吧”

```
写个bat，内容如下
@setlocal enableextensions
@pushd %~dp0
.\REtool.exe -h mhrisePC.list -x -skipUnknowns %1
@popd
@pause
```

注意mhrisePC.list，需要和自己复制进来的list同名（bat会按照这个去读）

开解！把pak拖到bat上执行，最终会解出来个同名文件夹，里面就是数据文件。

<br/>

## 赏读RSZ数据结构

*（RE引擎和Unity很像…和Unreal也有点像。）*拆出来的文件都是.user .tex等格式，无法直接打开，需要用RSZ（RE的一个结构probably）+010Editor帮忙分析。

### 安装010Editor

直接谷歌就好。010Editor是一款用来编辑二进制文件的十六进制编辑器，只要将一个类型的二进制文件进行模板定义，之后同类型的文件都会调用同一个模板并自动分析。

### 安装RSZ Template

从github上把人家主干clone下来，里面有个bt文件。在101里view installed templates，安装这个bt，然后就能分析SCN, PFB, USER, RCOL, FSMV2, MOTFSM, or BHVT格式的文件并变成人类可读的十进制文件了。

<br/>

## 《转生之我是MHR设计师》

转生目标：把迅龙棍的外观更换为泡壶棍模型。

### 找到对应文件

暂时没找到文件路径说明，需要自己去翻，可以参考同类MOD的修改文件路径。最后打开了操虫棍武器数据的basedata。

### 比照并修改对应数据

比照找来的excel表格寻找数据，表格给出了对应的十六位数据但不知为啥搜不出来。看了几行，感觉解出来的数据显示不完整……幸好武器data是按照sortID来排列的，是按照sortID来排列的，是按照sortID来排列的，是按照sortID来排列的！迅龙棍是7 8 9，泡壶棍是10 11 12，轻松找到。

### 文件另存为

这次只改modID，不需要动模型，改完modelID后另存为就行。

<br/>

## 简简单单打个包

### RisePakPatch打包

下个Rise Pak Patch包，把自己要打包的文件收拾进来。

需要从natives开始的完整的路径！natives上一级需要再来个文件夹。

整个拖到build-pak-patch.bat上，打出来个re_chunk_00x.pak.patch_00y.pak，拖进游戏根目录然后改名。

### 存放到游戏路径并验收

目前re_chunk_00x.pak.patch_00y.pak只支持命名到006，即每个版本包预留了六个槽位。超出的时候需要自己merge mod，可以手动合并已知功能，或者去N网下载别人制作的merge工具。
