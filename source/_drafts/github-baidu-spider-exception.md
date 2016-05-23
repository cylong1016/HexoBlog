---
title: 解决 Github Pages 禁止百度爬虫抓取的问题
date: 2016-05-22 16:24:44
categories:
    - Github
tags:
    - github
    - hexo
    - git
---
---

最近在研究网站在 Google 和百度的收录问题，Google 收录很轻松，抓取提交下链接就好了，但是百度却花了我好长时间才搞定，这也是为什么这篇博客写的这么晚 ( ╯□╰ )。有关如何能让你的网站被搜索到请参考：

> [如何在 Google 和百度里搜索到自己的网站][1]

<!-- more -->

在 [百度站长平台][2] 上进行抓取诊断的时候，发现一直抓取失败，如图：

![百度爬虫抓取错误](spider-test.png)
![百度爬虫抓取错误](exception.png)

后来才发现，原来是 Github 禁止百度爬虫抓取【我该吐槽 Github 呢？还是百度呢？】

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://www.cylong.com/blog/2016/05/22/google-baidu-search/ "如何在 Google 和百度里搜索到自己的网站"
[2]: http://zhanzhang.baidu.com/ "百度站长平台"
