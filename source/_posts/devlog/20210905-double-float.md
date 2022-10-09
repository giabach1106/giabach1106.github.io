---
title: -1.5为什么是double而不是float？
date: 2021-09-05
categories: Devlog
tags: 
- 游戏开发
cover: /images/20210904-float.jpg
toc: true
---

“问题很好，但为什么不问问Google呢？”

<!--more-->

---

   <br/>

这是开发Music99时遇到的问题。

### **笨鸟飞时的迷惑**

为了自动化生成Note，需要提前传好变量的值，例如transform.postion.x。1号轨道是-1.5，2号轨道是0.5，etc。

在此遇到了个迷惑问题。直接定义float key1_x = -1.5，会报错如下。

![](/images/20210905-double/error.png)

double是比float精度更高的类型，但是-1.5*从人类认知上*是很小的数字，为什么就自动变成了double呢？

为了去除报错，把 = -1.5删掉，在外面配置了-1.5，这样就能正常运行了。

在Google上随便查了下“为什么负数是float”，出来的大部分都是“如何检测负数是不是float”，答非所问。继续搜索应该是可以找到答案的，但毕竟身边有大佬@jskyzero，把问题打包了下，写了篇小论文发送过去。

然后大佬果然给出了详细解答。

   <br/>

### **大佬的耐心回复**

开幕雷击：“你知道打开浏览器 0.1+0.2不等于0.3吗？”

![](/images/20210905-double/unequal.png)

计算机里没有小数，只有浮点数，浮点数也只是近似值，无法达到准确值。

> 在[计算机科学](https://zh.wikipedia.org/wiki/計算機科學)中，**浮点**（英语：floating point，缩写为FP）是一种对于[实数](https://zh.wikipedia.org/wiki/實數)的近似值数值表现法，由一个[有效数字](https://zh.wikipedia.org/wiki/有效数字)（即**[尾数](https://zh.wikipedia.org/w/index.php?title=尾数&action=edit&redlink=1)**）加上[幂数](https://zh.wikipedia.org/wiki/冪數)来表示，通常是乘以某个[基数](https://zh.wikipedia.org/wiki/基数)的整数次[指数](https://zh.wikipedia.org/wiki/指数)得到。以这种表示法表示的数值，称为**浮点数**（floating-point number）。利用浮点进行运算，称为**浮点计算**，这种运算通常伴随着因为无法精确表示而进行的近似或舍入。

因为无法保证准确性，大部分游戏尤其是需要做数值校验的游戏，都会实现自己的无误差浮点数计算，以保证浮点数计算在各个平台架构下的结果一致性。

然后回到上面的问题：由于有误差，自动推导会自动使用精度更好的double类型。如果想使用float类型，需要显式声明为-1.5f，类似数值类型0XAA = 10，0X16进制。

   <br/>
