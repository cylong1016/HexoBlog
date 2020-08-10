---
title: Hexo 博客和 NexT 主题版本升级
date: 2020-08-10 23:11:59
updated: 2020-08-10 23:11:59
categories:
    - Hexo
tags:
    - hexo
    - next
---
---

# 前言

使用 [Hexo][] 搭建博客也有4年之久，也是一直使用 [NexT][] 主题，NexT 的简洁大方美观，特别符合我的审美。最近 Hexo 更新到 5.0.0 版本了，也是临时起意，想要更新下 Hexo 的版本，同时也一起更新下 NexT 主题，毕竟从4年前开始使用后，就再也没有升级过这两个的版本了。经常看到很多同学都升级到新版本的 NexT 主题，界面展示和功能都有较大提升。NexT 主题不仅由之前的 5.1.x 更新至 7.x，主仓库也从 [iissnan][] 名下迁移至 [theme-next] 组织。

<!-- more -->

# Hexo 搭建个人博客

在这里可能有些小伙伴是第一次接触 Hexo 和 NexT，先给大家一些参考文档，助力大家搭建一个属于自己的博客。

> [Hexo + Git 搭建免费的个人博客 | 笑话人生][1]
> [分类：Hexo | 笑话人生][2]

# 升级 Hexo

Hexo 版本升级可以通过 npm 实现，相关命令如下：

1. 全局升级 hexo-cli
```
npm install hexo-cli -g
```

2. 检查系统中的插件是否有升级的，可以看到自己前面都安装了那些插件
```
npm install -g npm-check
npm-check
```

3. 升级系统中的插件
```
npm install -g npm-upgrade
npm-upgrade
```

4. 更新全局包
```
npm update -g
```

5. 更新生产环境依赖包
```
npm update --save
```

6. 查看 Hexo 版本
```
$ hexo v
hexo: 5.0.2 # 升级到 5.0.2版本
hexo-cli: 4.2.0
os: Windows_NT 10.0.18362 win32 x64
node: 14.7.0
v8: 8.4.371.19-node.12
uv: 1.38.1
zlib: 1.2.11
brotli: 1.0.7
ares: 1.16.0
modules: 83
nghttp2: 1.41.0
napi: 6
llhttp: 2.0.4
openssl: 1.1.1g
cldr: 37.0
icu: 67.1
tz: 2020a
unicode: 13.0
```

7. 查看 package.json
```
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": "5.0.2"
  },
  "dependencies": {
    "hexo": "^5.0.2",
    "hexo-admin": "^2.3.0",
    "hexo-deployer-git": "^2.1.0",
    "hexo-generator-archive": "^1.0.0",
    "hexo-generator-baidu-sitemap": "^0.1.9",
    "hexo-generator-category": "^1.0.0",
    "hexo-generator-feed": "^3.0.0",
    "hexo-generator-index": "^2.0.0",
    "hexo-generator-searchdb": "^1.3.2",
    "hexo-generator-sitemap": "^2.1.0",
    "hexo-generator-tag": "^1.0.0",
    "hexo-renderer-ejs": "^1.0.0",
    "hexo-renderer-marked": "^3.0.0",
    "hexo-renderer-stylus": "^1.1.0",
    "hexo-server": "^2.0.0",
    "hexo-wordcount": "^6.0.1",
    "particles.js": "^2.0.0"
  }
}
```

# 升级 NexT

NexT 主题升级从 v5 升级到 v7，跨度很大，但是官方提供了升级指导：[从 NexT v5.1.x 更新][3]，这里我把我的升级过程分享给大家，也是自己摸索的一种比较方便的升级方式，同时也方便后面继续进行升级。

1. 克隆新的仓库到任一异于 next 的目录（如 next-reloaded）：
```
$ git clone https://github.com/cylong1016/hexo-theme-next themes/next-reloaded
```
如此，你可以在不修改原有的 NexT v5.1.x 目录的同时使用 next-reloaded 目录中的新版本主题。这里，我是 Fork 了主仓库 theme-next/hexo-theme-next ，方便自己后续进行定制化修改，需要更新的时候，直接从主仓库拉取最新代码即可。

2. 在 Hexo 的主配置文件中设置主题：
```
theme: next-reloaded
```
如此，你的 next-reloaded 主题将在生成站点时被加载。如果你遇到了任何错误、或只是不喜欢这一新版本，你可以随时切换回旧的 v5.1.x 版本。

3. 更新语言配置
从 v6.0.3 版本起，zh-Hans 改名为 zh-CN：https://github.com/theme-next/hexo-theme-next/releases/tag/v6.0.3
升级到 v6.0.3 及以后版本的用户，需要显式修改 Hexo 主配置文件 _config.yml 里的 language 配置，否则语言显示不正确。

4. 修改主题的 _config.yml 文件
这里，我们不直接修改主题的_config.yml 文件，因为这样操作，后续 `git pull` 更新的时候，需要解决冲突问题，即使是手动下载 release 版本，也要手动合并 _config.yml 文件。所以我们选择 NexT 提供的方式2，创建自己单独的 next.yml 进行配置：
> [数据文件][]

5. 我大概花了几个小时时间将之前全部的配置搞定了，大家可以参考：
> https://github.com/cylong1016/HexoBlog/blob/master/source/_data/next.yml

# 最后

至此花了半天时间，把 Hexo 和 NexT 主题全部升级完成，主要还是刚开始用的时候不太熟悉，后续也过了4年都没更新，所以这次花了比较多的时间，相信后面熟悉后，紧随版本，更新就会很快了。

# 感谢

[升级博客Hexo版本和Next主题版本踩坑记录 | 叹逍遥的博客][4]
[Hexo版本升级和Next主题升级之坑 | HJ_彼岸][5]
[升级Hexo及NexT主题及添加评论和阅读数 | tangbao's Blog][6]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[Hexo]: https://hexo.io/zh-cn/ "Hexo"
[NexT]: http://theme-next.iissnan.com/ "Next"
[iissnan]: https://github.com/iissnan/hexo-theme-next "iissnan"
[theme-next]: https://github.com/theme-next "theme-next"
[数据文件]: https://github.com/theme-next/hexo-theme-next/blob/master/docs/zh-CN/DATA-FILES.md "数据文件"
[1]: /blog/2016/04/19/hexo-git/ "Hexo + Git 搭建免费的个人博客 | 笑话人生"
[2]: /categories/Hexo/ "分类：Hexo | 笑话人生"
[3]: https://github.com/theme-next/hexo-theme-next/blob/master/docs/zh-CN/UPDATE-FROM-5.1.X.md "从 NexT v5.1.x 更新"
[4]: https://www.tanxiaoyao.com/post/59005 "升级博客Hexo版本和Next主题版本踩坑记录 | 叹逍遥的博客"
[5]: https://blog.csdn.net/whjkm/article/details/81088518 "Hexo版本升级和Next主题升级之坑 | HJ_彼岸"
[6]: https://blog.tangbao.me/2019/08/update-hexo-next-and-add-comment-and-view-number/ "升级Hexo及NexT主题及添加评论和阅读数 | tangbao's Blog"