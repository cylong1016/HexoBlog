---
title: Hexo 集成 Disqus 评论
date: 2017-03-26 22:36:12
categories:
  - Hexo
tags:
  - hexo
  - 插件
---
---

# 前言

从创建博客的时候我就纠结用国内的多说还是国外的 Disqus，鉴于多说是国内的，是中文的，而且相比国内其他的系统更加稳定，功能多样，毅然选择了多说。不过后来多说经常崩溃，总是看不到评论，一直想换成 Disqus 或者国内其他的评论系统。这次好了，不用纠结了，用了一年的多说即将在[2017年6月1日关闭服务][1]，不得不换一个。在我刚开始使用 Next 主题的时候，只支持多说和 Disqus，目前已经支持了很多，可以参考：

> [Next 第三方服务集成][2]

我后来还是选择了 Disqus 作为新的评论系统，虽然 Disqus 在国内有时候被墙掉了，英文读起来也比较费劲，但是强大的功能和用户体验让我对 Disqus 爱不释手。至于国内也有很多评论系统可以代替多说，但是根据先前的一些系统来看，最后都没有走下去呢╮(╯▽╰)╭。 我觉得 Disqus 不会半路 GG 吧 (╯‵□′)╯︵┻━┻

<!-- more -->

# Next 主题集成 Disqus

1. 登陆 [Disqus][3]，点击 `GET STARTED` 开始创建站点，之后就可以点击右上角的 `Admin` 进入后台管理。
![Disqus 创建站点](disqus_index.png)
2. 点击第二条 `I want to install Disqus on my site`。
![](disqus_intent.png)
3. 按照表单填写信息，记住 `Website Name` 这条属性。
![](disqus_create.png)
4. 接下来按照指引填写信息，完成第三步 `3.Configure Disqus` 后点击最下面 `Complete Setup` 完成创建。【中间会有一个嵌入代码的案例，不是 Next 主题的可以参考下】
![](disqus_settings.png)
5. 接下来配置主题下面的 `config.yml` 文件。
> 大于等于5.1.1版本，将 disqus 下的 enable 设定为 true，同时提供您的 shortname。 count 用于指定是否显示评论数量。
{% code %}
disqus:
  enable: true
  shortname:
  count: true
{% endcode %}
> 小于5.1.1 版本，设定 disqus_shortname 的值即可。
{% code %}
  disqus_shortname: shortname
{% endcode %}
6. 接下来就可以进入后台管理设置你的评论了。


---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://dev.duoshuo.com/threads/58d1169ae293b89a20c57241 "重要通知: 多说即将关闭"
[2]: http://theme-next.iissnan.com/third-party-services.html "Next 第三方服务集成"
[3]: https://disqus.com "Disqus"
