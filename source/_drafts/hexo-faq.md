---
title: Hexo 使用总结 & 常见问题
date: 2016-04-25 20:02:13
categories:
    - Hexo
tags:
    - hexo
---
---

# 前言

Hexo 是一个非常简洁的静态博客框架，可以快速的搭建生成自己的博客，但是在使用中总会遇到各种各样奇怪的错误，在这里我整理一下我所遇到的错误和一些使用技巧等等，也欢迎各位读者对使用技巧和使用问题进行补充。如果有什么问题也可以随时在下方提问。

<!-- more -->

# Hexo 不渲染 .md 或者 .html

我们知道，在 source 文件夹下的所有 md 文件或者 html 文件都会被渲染，有时候我们不想这些文件被渲染怎么办？比如很多时候我们想要写一个 README.md 或者一些自定义的页面。比如百度或者谷歌在验证站长权限的时候，通常都会要求在主目录下添加一个 html 文件。

## 不渲染 html 文件

在不想被渲染的 html 文件最上面添加如下代码

{% code %}
    ---
    layout: false
    ---
{% endcode %}

## 不渲染 md 文件

使用上面的办法虽然不会渲染 md 文件，但是还是将 md 文件转化成了 html 文件，如果想保留原 md 文件后缀要怎么做呢？这就需要在 `_config.yml` 中配置，找到 `skip_render` 参数，开始匹配的位置是基于你的 `source_dir` 的，一般来说，是你的 `source` 文件夹下。下面我分别列举几种常见的情况进行说明：
1. `skip_render: test/*`    单个文件夹下全部文件
2. `skip_render: test/*.md` 单个文件夹下指定类型文件
3. `skip_render: test/**`   单个文件夹下全部文件以及子目录
4. 多个文件夹以及各种复杂情况：
{% code %}
    skip_render:
      - `test1/*.html`
      - `test2/**`
{% endcode %}

# 感谢

> [Hexo常见问题解决方案 // Xuanwo's Blog][1]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.cc/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)


[1]: https://xuanwo.org/2014/08/14/hexo-usual-problem/ "Hexo常见问题解决方案 // Xuanwo's Blog"
