---
title: Hexo评论插件：从Disqus换成Waline
date: 2022-02-15
categories: Devlog
tags: 
- 网页开发
cover: /images/20220215-comment-plugin.png
toc: true
---
“这么简单的东西也要水一篇文章吗？”

<!--more-->

---

## 开发需求

1. Disqus功能过于强大，个人网站的使用率不高。
2. Disqus在网站有了流量后会插播广告，观感极差。*腾讯作风*
3. 希望评论功能的响应速度更快。
4. 能对评论数据进行导出、增删等操作。*控评*
5. 兼容匿名评论和登录评论两种模式。
6. 无需科学上网即可看到全部评论内容。

<br/>

## 方案调研

不满意别人的功能，无非是自己写或者换个功能用。自己写首先被PASS了，我如果有那么勤奋就不会用现成的blog框架，*况且写得也肯定没人家好。*

搜索了下信息，在Gitalk和Valine里进行了二选一。考虑到Gitalk不支持匿名评论，而且每次都要初始化很麻烦，就打算用Valine。用到一半突然悬崖勒马了，因为检索到许多关于Valine在Hexo Icarus框架下暴露IP信息的安全隐患讨论，最后决定用**基于Valine衍生的Waline。**

<br/>

## 使用步骤

本人是对着[Waline快速上手指南](https://waline.js.org/guide/get-started.html#leancloud-%E8%AE%BE%E7%BD%AE-%E6%95%B0%E6%8D%AE%E5%BA%93)和[Icarus评论配置](http://ppoffice.github.io/hexo-theme-icarus/Plugins/Comment/icarus%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97-%E7%94%A8%E6%88%B7%E8%AF%84%E8%AE%BA%E6%8F%92%E4%BB%B6/)来做的，二合一就完事儿了。这里只记录最基本的步骤和过程心得。


### 配置LeanCloud

[LeanCloud](https://console.leancloud.app/apps)是一个一站式后端云服务提供商。之所以选择它，是因为它提供了不要钱且不要服务器的数据存储服务。注册时记得选择国际版，因为国内版要域名备案。开发者版本流量小了点，但使用免费。

创建了应用后，在数据存储→结构化数据→结构化数据里新建两个class，分别是Comment和Counter，前者是评论数据，后者是点击量数据。

（可选）在设置→安全中心里，把除了数据存储之外的服务开关全部关闭，提高安全性。

跳转到设置→应用凭证，此时已经有了AppID、AppKey和MasterKey三条密钥。


### 配置Vercel

Vercel是一个站点托管平台，网上讨论盛赞曰：“类似于Github Page，但远比Github Page强大。”

*残念的是，本网站就是通过Travis CI部署在Github Page上的。*

打开[Vercel](https://vercel.com/)网址，通过Github账号进行登录，然后[从这个网址开始](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fwalinejs%2Fwaline%2Ftree%2Fmain%2Fexample)创建一个新的git repository。根据Waline的说明配置好新的repository，部署成功后点击Go to Dashboard跳转到控制台。此时需要配置对应的LeanCloud密钥。

> 点击顶部的 `Settings` - `Environment Variables` 进入环境变量配置页，并配置三个环境变量 `LEAN_ID`, `LEAN_KEY` 和 `LEAN_MASTER_KEY` 。它们的值分别对应上一步在 LeanCloud 中获得的 `APP ID`, `APP KEY`, `Master Key`。

配置完毕后，点击Deployments并进行一次Redeploy，部署成功后，刚刚的环境变量就生效了。跳转到Overview界面，等到STATUS变成Ready后，点击Visit获得一个新网址，也就是服务器网址。

### 配置Icarus主题文件

打开网站根目录的_config.icarus.yml，配置以下内容：

```
comment:
    type: waline
    server_url: https://your-domain.vercel.app
    lang: zh-CN                                   # 可选填
    visitor: false                                # 可选填
    emoji:                                        # 可选填
      - 'https://cdn.jsdelivr.net/gh/walinejs/emojis/weibo'
    dark: auto                                    # 可选填
    meta: ["nick", "mail", "link"]                # 可选填
    required_meta: []                             # 可选填
    login: enable                                 # 可选填
    avatar: mp                                    # 可选填
    word_limit: 0                                 # 可选填
    page_size: 10                                 # 可选填
    avatar_cdn: 'https://sdn.geekzu.org/avatar/'  # 可选填
    avatar_force: false                           # 可选填
    highlight: true                               # 可选填
    math_tag_support: false                       # 可选填
    copyright: true                               # 可选填
    locale:                                       # 可选填
      placeholder: 'Comment here...'

```

重点是server_url，填入刚刚获得的服务器网址。剩下配置项的对应功能，*我懒得复述了，*可以自己去查阅[Waline客户端文档](https://waline.js.org/guide/client/intro.html)。

<br/>

## 额外测试：评论数据管理

Waline提供了一个评论管理功能，可以在 `<serverURL>/ui/register`来修改、置顶或删除评论。

那如果我在这里对评论进行各种操作，LeanCloud那边会发生什么呢？

### 删除带有回复的评论

在一篇文章下，先回复了评论A，然后对评论A回复了评论B。此时在管理界面删除掉评论A，会发现评论A和评论B都消失了。但是打开LeanCloud的数据存储，会发现评论A的数据消失了，而评论B的数据还在。

### 回复本地blog

在localhost:4000本地端口进行调试时，对文章进行回复。管理平台和LeanCloud都能收到数据，*毕竟服务器url都没变，*不过评论不会出现在推送后的在线网站里。

### 在注册管理平台前评论

在注册评论管理平台前，我已经发表了一条评论。注册了平台后，这条评论就无法显示在网页上了，但还可以在管理平台和LeanCloud上查到数据。可能踩到了个无关痛痒的bug，假装无事发生。
