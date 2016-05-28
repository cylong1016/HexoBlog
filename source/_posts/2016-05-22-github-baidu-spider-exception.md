---
title: 解决 Github Pages 禁止百度爬虫抓取的问题
categories:
  - Github
tags:
  - github
  - git
  - hexo
  - 爬虫
date: 2016-05-22 16:24:44
---
---

最近在研究网站在 Google 和百度的收录问题，Google 收录很轻松，抓取提交下链接就好了，但是百度却花了我好长时间才搞定，这也是为什么这篇博客写的这么晚 ( ╯□╰ )。有关如何能让你的网站被搜索到请参考：

> [如何在 Google 和百度里搜索到自己的网站][1]

<!-- more -->

# 原因

在 [百度站长平台][2] 上进行抓取诊断的时候，发现一直抓取失败，如图：

![百度爬虫抓取错误](spider-test.png)
![百度爬虫抓取错误](exception.png)


# 解决

我想了想，既然 Github 不让百度抓取，那么我干脆把博客部署到其他地方吧。于是找到了国内的 [CODING][4] 【之前可能有很多小伙伴使用的是 [Gitcafe][5]，不过已经被 CODING 收购了，2016-05-31 号就会停止服务】。CODING 中也提供和 Github 相同的 Pages 服务。下面是具体步骤：

1. 注册登录 [CODING][4]。
2. 创建新项目，项目的后缀必须是和你的个性后缀一样。
3. 创建项目的时候你可以选择从 Github 上导入你的博客仓库或者之后自己部署到 CODING 上，如下图：
![从 Github 上导入仓库](import-from-github.png)
4. 进入你的项目，点击左侧的 `代码`，在选择 `Pages 服务`，选择 `部署分支`，默认是 `coding-pages`，然后点击 `立即开启`。
5. 绑定自己的域名，如下图：
![Coding Pages](coding-pages.png)
6. 到你的 DNS 服务商修改你的域名解析记录，这里不需要删除解析到 Github 的记录，像我下面这样配置就可以，这样正常访问还是访问到 Github 上，百度抓取的时候是抓取的 CODING 上的项目。
万网 DNS 设置：
![DNS 解析记录 - 万网](dns-parse.png)
DNSPod 设置：
![DNS 解析记录 - DNSPod](dns-parse-dnspod.png)
注意：我的域名在万网购买的，默认使用的是万网的 DNS，设置成百度后开始是好用的，后来就又抓取不到了。于是我就换成了 [DNSPod][6] 的服务，把线路类型设置成搜索引擎或者国内都可以。

# 感谢

> [解决 Github Pages 禁止百度爬虫的方法与可行性分析 - 咀嚼之味][3]
> [解决百度爬虫无法抓取github pages - Lippi-浮生志][7]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://www.cylong.com/blog/2016/05/22/google-baidu-search/ "如何在 Google 和百度里搜索到自己的网站"
[2]: http://zhanzhang.baidu.com/ "百度站长平台"
[3]: http://jerryzou.com/posts/feasibility-of-allowing-baiduSpider-for-Github-Pages/ "解决 Github Pages 禁止百度爬虫的方法与可行性分析 - 咀嚼之味"
[4]: https://coding.net "CODING"
[5]: https://gitcafe.com/ "Gitcafe"
[6]: https://www.dnspod.cn/ "DNSPod-免费智能DNS解析服务商"
[7]: http://www.ezlippi.com/blog/2016/02/baidu-spider-forbidden.html "解决百度爬虫无法抓取github pages - Lippi-浮生志"
