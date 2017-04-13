---
title: 如何在 Google 和百度里搜索到自己的网站
categories:
  - Search
tags:
  - hexo
  - seo
  - search
date: 2016-05-22 17:17:49
updated: 2017-04-12 23:01:35

---
---

搭建完自己的博客或者有一个自己的网站什么的，总想着能在 Google 或者是百度里搜索到吧，这样就可以提高你的网站的知名度，说不定还会结 ♂ 交到志同道合的小伙伴呢 (●'◡'●)。下面就讲解下如何让自己的网站被搜索引擎收录~

<!-- more -->

# Google 搜索

1. 点击 [Google 网站站长][1]，然后点击首页的 `SEARCH CONSOLE`。
2. 添加你的网站，进行人机验证【就是证明这个网站是你的】。
3. 进入到 `抓取->Google 抓取方式`。
4. 点击 `抓取` 或者 `抓取并呈现`。
5. 若提示完成或者部分完成，则可以将网址 `提交至索引`，有两种提交方式：仅抓取此网址、抓取此网址及其直接链接。都有次数限制。
6. 等待一会，打开 Google，在搜索栏输入 `site:www.cylong.com`【换成你的域名】，就可以看到你网站的内容了~

![Google 抓取示例](google-search.png)

# 百度搜索

1. 点击 [百度站长平台][2]，登陆。
2. 添加你的网站，进行人机验证。
3. 点击 `抓取诊断` 判断百度是否能够抓取到你的网站【我的博客部署在 Github 上，结果 Github 禁止百度抓取 ( ╯□╰ )，如何解决请参考：[解决 Github Pages 禁止百度爬虫抓取的问题][3]】
4. 点击 `链接提交` ，这里有很多种提交方式，各有各的优点，也有详细的说明。自己选择吧【实在不会就直接手动提交】
5. 我使用的是自动提交方式，Hexo 的 Next 主题已经部署了自动推送的代码，我们只需在主题配置文件中找到 `baidu_push` 字段 , 设置其为 `true` 即可。
6. 等待一会，打开百度，在搜索栏输入 `site:www.cylong.com`【换成你的域名】，就可以看到你网站的内容了~

# 其他的搜索引擎

其他的搜索引擎我用的也比较少，不过如果你的网站不知道什么时候也会被其收录的，我试了一下，我的博客的首页就被 Bing 收录了，当然你想像上面的那些方法在各个搜索引擎中管理你的网站也是可以的，然而我并没有闲心管那些【我猜测基本都是差不多的】，有兴趣的小伙伴可以自己试一试。

# 搜索引擎优化(SEO)

{% blockquote 维基百科 https://zh.wikipedia.org/wiki/%E6%90%9C%E5%B0%8B%E5%BC%95%E6%93%8E%E6%9C%80%E4%BD%B3%E5%8C%96 搜索引擎优化 %}
搜索引擎优化（英语：search engine optimization，缩写为SEO），是一种通过了解搜索引擎的运作规则来调整网站，以及提高目的网站在有关搜索引擎内排名的方式。由于不少研究发现，搜索引擎的用户往往只会留意搜索结果最前面的几个条目，所以不少网站都希望通过各种形式来影响搜索引擎的排序，让自己的网站可以有优秀的搜索排名。当中尤以各种依靠广告维生的网站为甚。
{% endblockquote %}

**注意：** _此文章是以 Hexo 搭建的博客为基础，某些插件只支持 Hexo 搭建的博客，如果是其他博客，请寻找支持自己博客的插件或者自己实现。_

## 添加 robots.txt

robots.txt 可以告诉搜索引擎你网站的哪些页面可以被抓取，哪些页面不可以被抓取。将 robots.txt 放置在 source 根目录下。以下是我的 robots.txt：

{% code robots.txt %}
    User-agent: *
    Allow: /
    Allow: /archives/

    Disallow: /js/
    Disallow: /css/
    Disallow: /fonts/
    Disallow: /lib/
    Disallow: /fancybox/

    Sitemap: http://www.cylong.com/sitemap.xml
    Sitemap: http://www.cylong.com/baidusitemap.xml
{% endcode %}


## 添加网站地图(Sitemap)

Sitemap 上面放置了网站上需要搜索引擎抓取的所有页面的链接，有助于搜索引擎抓取你的网站，清晰了解网站的架构，益于 SEO 优化。

1. 安装 Hexo 的 sitemap 插件
{% code lang:sh %}
    npm install hexo-generator-sitemap --save       # 适用于提交给 Google
    npm install hexo-generator-baidu-sitemap --save # 适用于提交给百度
{% endcode %}

2. 在站点的 `_config.yml` 添加以下代码：
{% code lang:sh %}
    sitemap:
        path: sitemap.xml
    baidusitemap:
        path: baidusitemap.xml
{% endcode %}

3. 配置成功后，在你执行 `hexo -g` 的时候，在 public 文件夹【也就是你的站点根目录】就会出现 sitemap.xml 和 baidusitemap.xml。然后在 robots.txt 中添加如下代码：
{% code robots.txt %}
    Sitemap: http://www.cylong.com/sitemap.xml
    Sitemap: http://www.cylong.com/baidusitemap.xml
{% endcode %}

4. 第三步中搜索引擎在抓取到 robots.txt 的时候会自动抓取站点地图。你还可以手动提交给 Google 和百度，都是带有提示的傻瓜式操作，相信大家都能解决吧(●'◡'●)
* Google： [Search Console][4]
* 百度： [百度站长平台][2]

**2017-04-13 更新**

在我向百度提交站点地图 baidusitemap.xml 的时候提示 XML 解析失败，猜测可能是格式有问题，通过查看 [百度sitemap协议都支持哪些格式？][6] 发现 Hexo 生成的 baidusitemap.xml 确实有问题，不过百度支持上文中生成的 sitemap.xml 格式的 XML 文档，上传这个就正常了。

![XML 解析失败](baidusitemap-error.png)

# 一些小点子

你在你的各大社交网站的个人信息里贴上你的博客域名【比如知乎、Facebook、Twitter、Github 等等】会提高你网站的访问量哟，还有在各大社交网站上回答有关问题可以附上自己的博客地址，也会增加访问量o(^▽^)o。当然，更重要的还是你的博客要内容丰富精彩，才会吸引更多的人。如果是自己的某个网站什么的，多注意下 SEO 优化也是不错的选择。

# 参考&感谢

> [Hexo 优化：提交 sitemap 及解决百度爬虫无法抓取 GitHub Pages 链接问题][5]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://www.google.com/webmasters/ "Google 网站站长"
[2]: http://zhanzhang.baidu.com/ "百度站长平台"
[3]: /blog/2016/05/22/github-baidu-spider-exception/ "解决 Github Pages 禁止百度爬虫抓取的问题"
[4]: https://www.google.com/webmasters/tools/home?hl=zh-CN "Search Console"
[5]: http://www.yuan-ji.me/Hexo-%E4%BC%98%E5%8C%96%EF%BC%9A%E6%8F%90%E4%BA%A4sitemap%E5%8F%8A%E8%A7%A3%E5%86%B3%E7%99%BE%E5%BA%A6%E7%88%AC%E8%99%AB%E6%8A%93%E5%8F%96-GitHub-Pages-%E9%97%AE%E9%A2%98/ "Hexo 优化：提交 sitemap 及解决百度爬虫无法抓取 GitHub Pages 链接问题"
[6]: http://zhanzhang.baidu.com/college/courseinfo?id=267&page=2#h2_article_title1 "百度sitemap协议都支持哪些格式？"
