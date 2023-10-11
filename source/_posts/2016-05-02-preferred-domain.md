---
title: 网站首选域
categories:
  - Web
tags:
  - Apache
  - SEO
  - Hexo
  - 301重定向
  - htaccess
date: 2016-05-02 00:06:37
updated: 2016-05-02 00:06:37
---
---

# 什么是首选域

当我们在访问一个网站的时候，使用 <http://www.cylong.com> 和 <http://cylong.com> 访问网站，获得的内容并没有区别。对于用户来说，带 www 和不带 www 访问是一样的，但是对于搜索引擎就不一样了，两种域会被视为不同的网页，使得权重分散。Google 解释如下：

{% blockquote Search Console帮助 https://support.google.com/webmasters/answer/44231 设置您的首选网域（www 版网址或非 www 版网址） %}
首选网域是您希望 Google 用来将您的网页编入索引的网域（有时也称为规范网域）。指向您网站的链接可能会同时使用 www 版和非 www 版网址（例如，<http://www.example.com> 和 <http://example.com>）。首选域是您希望 Google 用来在搜索结果中显示您网站的版本。
{% endblockquote %}

<!-- more -->

# 设置首选域

## 使用 Google Search Console 设置

<b>注意：</b> *此方式仅适合 Google*

1. 点击 [Google 网站站长][1]，然后点击首页的 `SEARCH CONSOLE`。
2. 登陆后，点击想设置首选域的网站（没有网站就添加下）。
3. 点击右上角的齿轮设置图标 <span class="fa fa-cog" aria-hidden="true"></span>，然后点击 `网站设置`。
4. 选择对应的首选域。

## 使用 301 重定向

为了使其他搜索引擎和访问者都采用你的首选版本，建议你使用 301 重定向对你的非首选网域的访问重定向到首选网域。在 `.htaccess` 文件里添加如下代码（要针对托管在运行 Apache 的服务器上的网站实施 301 重定向，你需要有访问权限，日后可能会写什么是 `.htaccess` 文件，先挖个坑）：

{% code %}
    RewriteEngine On
    RewriteCond %{http_host} ^cylong.com [NC]
    RewriteRule ^(.*)$ http://www.cylong.com/$1 [L,R=301]
{% endcode %}

使访问 <http://cylong.com> 的时候就会自动转到 <http://www.cylong.com>。

## Hexo 博客框架设置首选域

我的博客是由 [Hexo][] 搭建的。没有部署到一个 Apache 服务器上，所以上面的设置方法就没用了。博客源码由 Hexo 生成，部署在 [Github][3] 上。如何搭建 Hexo 博客请参考：

> [Hexo + Git 搭建免费的个人博客][2] 。

如果你使用的是 GitHub 提供的域名，比如我的是 <http://cylong1016.github.com>。就不需要往下看了，如果你使用的是自己购买的域名，则只需要在 CNAME 中填写你所希望的首选域就可以了，比如我的是:

{% code CNAME %}
    www.cylong.com
{% endcode %}

# 注意

当你确定你的首选域之后，在以后的 SEO 工作中，比如友情链接、链接推广时，都要采用你设置的首选域。

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[Hexo]: https://hexo.io/zh-cn/ "Hexo"
[1]: https://www.google.com/webmasters/ "Google 网站站长"
[2]: http://www.cylong.com/blog/2016/04/19/hexo-git/ "Hexo + Git 搭建免费的个人博客"
[3]: https://github.com/cylong1016/cylong1016.github.io
