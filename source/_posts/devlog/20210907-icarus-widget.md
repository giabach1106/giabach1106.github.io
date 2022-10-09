---
title: icarus自定义：修改归档挂件
date: 2021-09-07
categories: Devlog
tags: 
- 网页开发
cover: /images/20210907-icarus.png
toc: true
---

修改icarus归档挂件时遇到的问题及解决过程。

<!--more-->  

 <br/>

之前的归档挂件，会显示月份+年份，在文章数量较多时，这一规则会让归档挂件非常累赘。打算修改归档挂件的布局，改成只按照年份分类。

## 去Issue里搜索方案

  在github工程里搜索相关信息，搜到了这个[讨论](https://github.com/ppoffice/hexo-theme-icarus/issues/413) ，编辑者提供的解决方案是：

>[@chenshen03](https://github.com/chenshen03) 请根据https://hexo.io/docs/helpers#list-archives 的参数描述在https://github.com/ppoffice/hexo-theme-icarus/blob/master/includes/helpers/override.js#L16 修改list_archives()的参数.

```
const $ = cheerio.load(this.list_archives({format: "YYYY MMMM"}), { decodeEntities: false });
```

但这个讨论是2019年2月发生的，如今工程已经不再存在这一路径。在这里得知的信息是，最开始icarus是继承了hexo的list.archives然后自己覆写的归档挂件，并不是独立实现。

<br/>

## 去Google里搜索方案

  扩大搜索范围，在Google里寻找了下。有其他作者也想改写归档挂件的效果，蹭了下别人的工作成果。顺藤摸瓜最后还是回到了官方文档，原来人家[在FAQ里提到了](https://ppoffice.github.io/hexo-theme-icarus/uncategorized/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98/) ，瞎子就是我自己。

> 插件和挂件的布局文件已被移至一个单独的Node.js库中——[`hexo-component-inferno`](https://github.com/ppoffice/hexo-component-inferno)。 这样，主题开发者可以更好地在不同主题之间复用这些通用组件，并且普通用户可以更简便地覆盖这些内置组件。
>
> 若要自定义这些组件，从`hexo-component-inferno`仓库中拷贝布局文件并把它们放入`<icarus_directory>/layout`下的的相应目录中。 例如，如果你想要自定义Valine评论插件，你可以从`hexo-component-inferno`仓库中拷贝 [`src/view/comment/valine.jsx`](https://github.com/ppoffice/hexo-component-inferno/blob/0.2.4/src/view/comment/valine.jsx) 到`<icarus_directory>/layout/comment/valine.jsx`。 同时像下面这样改正此文件头部的一些Node.js引用：

```
<icarus_directory>/layout/comment/valine.jsx

- const { cacheComponent } = require('../../util/cache');
+ const { cacheComponent } = require('hexo-component-inferno/lib/util/cache');
```

> 最后，用`hexo clean`清理你的站点并重新生成HTML文件。

<br/>

## 手动改写jsx脚本

根据上面的方法，在layout里新建了个archives.jsx，把`hexo-component-inferno`仓库里的代码拷贝过来，修改了开头的引用，然后开始魔改。

我的需求：1. 把月份+年份的分类规则改成年份；2. 点击对应年份，进去后是该年份的归档数据。

一共修改了三个地方：

1. 修改了type的默认类型，从monthly改成yearly。(L90)
2. 修改了分类规则type。(L119)
3. 修改了点击归档时返回地址的方法，不再在尾端加上月份，只返回archives/年份。 

    const link = (item) => {
        let url = `${config.archive_dir}/${item.year}/`;
    	//注释掉的部分，会在尾端加入月份
        //if (type === 'monthly') {
        //    if (item.month < 10) url += '0';
        //    url += `${item.month}/`;
        //}
    
        return url_for(url);
    
    };

修改完测试了下，完美无瑕，需求closed。

<br/>

