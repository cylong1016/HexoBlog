---
title: Hexo 集成多说评论 + 多说分享 + 美化多说
categories:
  - Hexo
tags:
  - Hexo
  - 插件
date: 2016-04-25 19:27:11
---
---

# 多说简介

多说是一款追求极致体验的社会化评论框，可以用微博、QQ、人人、豆瓣等帐号登录并评论。功能强大且永久免费。Hexo 默认使用的评论插件是国外的 Disqus，。对于国内来说，使用多说无非是最好的。这篇文章就介绍下如何在 Hexo 中添加多说评论插件和多说 CSS 的美化，顺便说一下多说分享插件。至于如何搭建 Hexo 博客请参考：

> [Hexo + Git 搭建免费的个人博客][1] 。

<!-- more -->

# 集成多说

<b>注意:</b> *我使用的是 Next 主题，集成多说插件非常简单，这也是我喜欢这个主题的原因，简单、高效。其他主题的配置需要你们自己研究了，不过都差不多的。Next 主题更多第三方服务请参考：*

> [第三方服务集成 - Next 使用文档][2]

## Next 主题

下面说一下 Next 主题如何集成多说。首先要在 [多说][] 创建一个站点，具体步骤如下：

1. 登录后在首页选择 "我要安装"。
2. 创建站点，填写表单。如图：
![多说创建站点](duoshuo_create.png)
3. 在博客站点配置文件 `_config.yml` 中添加如下代码：
{% code %}
    # 多说评论功能
    duoshuo_shortname: xxx # 你填写在多说域名中的值

    # 多说分享服务，必须与多说评论同时使用
    duoshuo_share: true
{%endcode%}
4. 启用后默认在所有页面都会显示多说评论框，比如分类页面、标签页面、自定义的关于页面，如果不想在这些页面显示评论框，找到对应的`index.md`文档，在`Front-matter`(文件最上方以 `---` 分隔的区域)中加入 `comments: false` 这一行代码就行了

## Landscape 主题
顺便说一下 Hexo 默认的 Landscape 主题如何集成多说，具体步骤如下：

1. 与上面相同
2. 修改 `themes\landscape\layout\_partial\article.ejs` 模板，把：

{% code %}
    <% if (!index && post.comments && config.disqus_shortname){ %>
    <section id="comments">
        <div id="disqus_thread">
            <noscript>Please enable JavaScript to view the <a href="//disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
        </div>
    </section>
    <% } %>
{% endcode %}

改为：

{% code %}
    <% if (!index && post.comments && config.duoshuo_shortname){ %>
    <section id="comments">
        <!-- 多说评论框 start -->
        <div class="ds-thread"
            data-thread-key="<%= post.layout %>-<%= post.slug %>"
            data-title="<%= post.title %>"
            data-url="<%= page.permalink %>">
        </div>
        <!-- 多说评论框 end -->

        <!-- 多说公共JS代码 start (一个网页只需插入一次) -->
        <script type="text/javascript">
            var duoshuoQuery = {short_name:'<%= config.duoshuo_shortname %>'};
            (function() {
                var ds = document.createElement('script');
                ds.type = 'text/javascript';ds.async = true;
                ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
                ds.charset = 'UTF-8';
                (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(ds);
            })();
        </script>
        <!-- 多说公共JS代码 end -->
    </section>
    <% } %>
{% endcode %}

# 美化多说

## 多说设置

登陆 [多说][]，点击后台管理，在`设置`中修改，包括基本设置、自定义文本、默认头像、外观主题。如图：

![多说自定义文本](duoshuo_text.png)

## 自定义多说分享图标

多说提供很多平台的分享服务，有时候我们肯能并不需要这么多，要如何修改呢？
首先，进入到 `themes\next\layout\_partials\share\duoshuo_share.swig` ，在这里就可以修改图标，至于都有什么图标，请参考：

> [多说分享组件自定义图标 - 多说开发者中心][4]

## 自定义 CSS

登陆 [多说][]，点击后台管理，在 `设置/基本设置/自定义CSS` 中可以修改多说的 CSS 样式，如图：

![多说 - 自定义 CSS](duoshuo_css.png)

我的修改如下：

{% code lang:css %}
    /* 设置圆形头像 */
    #ds-reset .ds-avatar img {
        width: 54px; height:54px;   /*设置图像的长和宽*/
        border-radius: 27px;        /*设置图像圆角效果,在这里我直接设置了超过 width/2 的像素，即为圆形了*/
        -webkit-border-radius: 27px;/*圆角效果：兼容webkit浏览器*/
        -moz-border-radius: 27px;
    }
{% endcode %}

具体看那些组件不顺眼，要修改掉，在网页上右键，选择 `检查元素` 就可以看到相对应的类名和 ID 等等。如果不会 CSS，建议你去 [CSS 教程 - W3School][3] 简单的学习一下 CSS 的基本知识。

目前就先改这么多，更多的样式还在开发中，会不定期的更新。如果小伙伴有什么更好的样式，欢迎留言~

# 参考资料

> [多说使用帮助 - 多说开发者中心][5]
> [讨论区 - 多说开发者中心][6]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://www.cylong.com/blog/2016/04/19/hexo-git/ "Hexo + Git 搭建免费的个人博客"
[2]: http://theme-next.iissnan.com/third-party-services.html "第三方服务集成 - Next 使用文档"
[3]: http://www.w3school.com.cn/css/index.asp "CSS 教程 - W3School"
[4]: http://dev.duoshuo.com/docs/5497972ba1165bfd53cf4263 "多说分享组件自定义图标 - 多说开发者中心"
[5]: http://dev.duoshuo.com/docs/ "多说使用帮助 - 多说开发者中心"
[6]: http://dev.duoshuo.com/discussion "讨论区 - 多说开发者中心"
[多说]: http://duoshuo.com/ "多说"
