---
title: Hexo 集成多说评论 + 多说分享 + 美化多说
categories:
    - Hexo
tags:
    - Hexo
    - 插件
    - Next
date: 2016-04-25 19:27:11
---
---

# 多说简介

多说是一款追求极致体验的社会化评论框，可以用微博、QQ、人人、豆瓣等帐号登录并评论。功能强大且永久免费。Hexo 默认使用的评论插件是国外的 Disqus，。对于国内来说，使用多说无非是最好的。这篇文章就介绍下如何在 Hexo 中添加多说评论插件和多说 CSS 的美化，顺便说一下多说分享插件。至于如何搭建 Hexo 博客请参考：

> [Hexo + Git 搭建免费的个人博客][1] 

<!-- more -->

# 集成多说

<b>注意:</b> *我使用的是 NexT 主题，集成多说插件非常简单，这也是我喜欢这个主题的原因，简单、高效。其他主题的配置需要你们自己研究了，不过都差不多的。NexT 主题更多第三方服务请参考：*

> [第三方服务集成 - NexT 使用文档][2]

## NexT 主题

下面说一下 NexT 主题如何集成多说。首先要在 [多说][] 创建一个站点，具体步骤如下：

1. 登录后在首页选择 "我要安装"。
2. 创建站点，填写表单。如图：
{% asset_img duoshuo_create.png 多说创建站点 %}
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

{% code lang:html %}
    <% if (!index && post.comments && config.disqus_shortname){ %>
    <section id="comments">
        <div id="disqus_thread">
            <noscript>Please enable JavaScript to view the <a href="//disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
        </div>
    </section>
    <% } %>
{% endcode %}

改为：

{% code lang:html %}
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

{% asset_img duoshuo_text.png 多说自定义文本 %}

## 自定义多说分享图标

多说提供很多平台的分享服务，有时候我们肯能并不需要这么多，要如何修改呢？
首先，进入到 `themes\next\layout\_partials\share\duoshuo_share.swig` ，在这里就可以修改图标，至于都有什么图标，请参考：

> [多说分享组件自定义图标 - 多说开发者中心][4]

## 多说评论显示 UA

在每一条多说评论后显示评论者所使用的代理信息（如 操作系统、浏览器），效果如下：

{% asset_img duoshuo_ua.png 多说评论显示 UA 示例 %}

启用此功能，需要编辑`主题配置文件` `_congig.yml` 如下：

{% code %}
    # Make duoshuo show UA
    # user_id must NOT be null when admin_enable is true!
    # you can visit http://dev.duoshuo.com get duoshuo user id.
    duoshuo_info:
      ua_enable: true
      admin_enable: true
      user_id: xxxxxx
      admin_nickname: 权限汪
{% endcode %}

只要设置 `ua_enable` 为 true 即可显示 UA 信息。 `admin_enable` 是用于显示 `博主` 文字，表明评论者是博主【默认显示的是博主，我给改成权限汪了】，此字段需要同时配置 user_id。 请访问 [多说开发者中心][7]，登录并访问 `我的主页` 获取 user_id ， 此 ID 是网址最后那串数字。

## 自定义 CSS

登陆 [多说][]，点击后台管理，在 `设置/基本设置/自定义CSS` 中可以修改多说的 CSS 样式，如图：

{% asset_img duoshuo_css.png 多说 - 自定义 CSS %}

我的修改如下（可以使用键盘上的上下左右查看看不到的代码）：

{% code lang:css %}
    /* 设置圆形头像 */
    #ds-reset .ds-avatar img {
        width: 54px; height:54px;   /*设置图像的长和宽*/
        border-radius: 27px;        /*设置图像圆角效果,在这里我直接设置了超过 width/2 的像素，即为圆形了*/
        -webkit-border-radius: 27px;/*圆角效果：兼容webkit浏览器*/
        -moz-border-radius: 27px;
    }

    /* 所有样式 */
    * {
        -webkit-border-radius: 0px !important;
        border-radius: 0px !important;
        box-shadow: inset 0 0 0px #fff !important;
    }

    /* 喜欢按钮 */
    #ds-thread #ds-reset a.ds-like-thread-button {
        border: 1px solid #f3726d;
        background: #fff !important;
        text-shadow: 0 0px 0 #fff !important;
    }

    /* 点击喜欢后弹出的分享图标 */
    .ds-service-link {
        background: url("//static.duoshuo.com/images/service-icons-color-flat.png?v=2") no-repeat;
        _background-image: url("//static.duoshuo.com/images/service-icons-color-flat.gif?v=2");
    }

    /* 上面那个分享图标我是想改成扁平化的，但是修改了后 */
    /* 下面这几个图标的坐标位置属性（多说原生的代码）就无效了，不知道为什么，于是自己手动写上去了 */
    #ds-reset .ds-qzone {
        background-position: 0 -128px;
    }

    #ds-reset .ds-qqt {
        background-position: 0 -64px;
    }

    #ds-reset .ds-renren {
        background-position: 0 -32px;
    }

    #ds-reset .ds-kaixin {
        background-position: 0 -80px;
    }

    #ds-reset .ds-douban {
        background-position: 0 -96px;
    }

    #ds-reset .ds-baidu {
        background-position: 0 -208px;
    }

    /* 被顶起来的评论 */
    #ds-reset .ds-gradient-bg {
        background: #E9E9E9 !important;
    }

    /* 评论框默认显示文字样式 */
    #ds-thread #ds-reset .ds-textarea-wrapper textarea, #ds-thread #ds-reset .ds-textarea-wrapper .ds-hidden-text {
        font-family: "Microsoft YaHei", Verdana, sans-serif !important;
    }

    /* 发布评论按钮 */
    #ds-thread #ds-reset .ds-post-button {
        font-family: "Microsoft YaHei", Verdana, sans-serif !important;
        border: 0 none !important;
        color: #FFFFFF !important;
        background: none repeat scroll 0 0 #F3726D !important;
        cursor: pointer !important;
        display: inline-block !important;
        text-transform: none !important;
        transition: all 0.3s ease 0s !important;
        -moz-transition: all 0.3s ease 0s !important;
        -webkit-transition: all 0.3s ease 0s !important;
        text-align: center !important;
        text-shadow: 0 0px 0 #fff !important;
    }

    /* 发布评论按钮  hover */
    #ds-thread #ds-reset .ds-post-button:hover {
        background: none repeat scroll 0 0 #303030 !important;
        color: white !important;
    }

    /* 评论框边框 */
    .theme-next #ds-thread #ds-reset .ds-textarea-wrapper {
        border-color: #DEDEDE !important;
    }

    /* 评论框下面框的边框(好绕啊！) */
    #ds-thread #ds-reset .ds-post-toolbar {
        border: 1px solid #DEDEDE !important;
        background: #E9E9E9 !important;
    }

    /* 昵称 */
    #ds-reset .ds-highlight {
        color: #F3726D !important;
    }

    /* 顶 */
    #ds-thread #ds-reset .ds-post-liked a.ds-post-likes {
        color: #F3726D !important;
    }

    /* 发布旁边的“分享到”图标 */
    #ds-reset .ds-service-icon {
        background-image: url("//static.duoshuo.com/images/service-icons-color-flat.png") !important;
        _background-image: url("//static.duoshuo.com/images/service-icons-color-flat.gif") !important;
    }

    /* 隐藏多说底部的版权 */
    #ds-thread #ds-reset .ds-powered-by {
        display: none;
    }

    /* 最近访客的头像 */
    #ds-recent-visitors .ds-avatar img {
        border-radius: 27px !important;
        -webkit-border-radius: 27px !important;
        -moz-border-radius: 27px !important;
    }

    /* 最近访客的头像 */
    #ds-recent-visitors .ds-avatar {
        float: left
    }
{% endcode %}

我比较喜欢简洁扁平化的风格，所以做了上述的更改。小伙伴们具体看那些组件不顺眼，要修改掉，在网页上右键，选择 `检查元素` 就可以看到相对应的类名和 ID 等等。如果不会 CSS，建议你去 [CSS 教程 - W3School][3] 简单的学习一下 CSS 的基本知识。

另外我发现 NexT 主题也对多说的 CSS 样式做了些更改，CSS路径 `themes\next\source\css\_common\components\third-party\duoshuo.styl`，所以如果小伙伴用了其他主题，显示样式可能有点区别。 目前就先改这么多，更多的样式还在开发中，会不定期的更新。如果小伙伴有什么更好的样式，欢迎留言~

# 更新

**2016-05-28 更新**
## 添加站点最近访客功能

你只需要在想要显示的地方添加如下代码即可：

{% code lang:html %}
    <div class="ds-recent-visitors"
        data-num-items="36"
        data-avatar-size="42"
        id="ds-recent-visitors">
    </div>
{% endcode %}

data-num-items：显示的最近访客数量
data-avatar-size： 访客头像大小
CSS 设置：请参考上面的自定义CSS

当然，前提是你使用了多说评论功能，因为最近访客功能就是由多说提供的。我是直接写在了 `about/index.md` 文件中。[点此][10] 看看我的访客功能(●'◡'●)

# 参考资料

> [多说使用帮助 - 多说开发者中心][5]
> [讨论区 - 多说开发者中心][6]
> [主题配置 - NexT 使用文档][8]
> [第三方服务集成 - NexT 使用文档][9]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://www.cylong.com/blog/2016/04/19/hexo-git/ "Hexo + Git 搭建免费的个人博客"
[2]: http://theme-next.iissnan.com/third-party-services.html "第三方服务集成 - NexT 使用文档"
[3]: http://www.w3school.com.cn/css/index.asp "CSS 教程 - W3School"
[4]: http://dev.duoshuo.com/docs/5497972ba1165bfd53cf4263 "多说分享组件自定义图标 - 多说开发者中心"
[5]: http://dev.duoshuo.com/docs/ "多说使用帮助 - 多说开发者中心"
[6]: http://dev.duoshuo.com/discussion "讨论区 - 多说开发者中心"
[7]: http://dev.duoshuo.com/ "多说开发者中心"
[8]: http://theme-next.iissnan.com/theme-settings.html "主题配置 - NexT 使用文档"
[9]: http://theme-next.iissnan.com/third-party-services.html "第三方服务集成 - NexT 使用文档"
[10]: http://www.cylong.com/about/ "关于我 - 笑话人生"
[多说]: http://duoshuo.com/ "多说"
