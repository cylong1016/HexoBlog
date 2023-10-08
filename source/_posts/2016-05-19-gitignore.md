---
title: .gitignore 的使用
categories:
  - Git
tags:
  - Git
  - gitignore
date: 2016-05-19 18:16:49
---
---

当我们在项目开发的时候，常常使用 Git 做版本控制工具。有时候，我们不希望某些文件或者文件夹被 Git 追踪，比如项目产生的中间文件【.class 、 .o】 ，比如某些编辑器的副本文件【一般文件名带有 ~ 符号】，还有一些 log 文件等等。使用命令行的小伙伴可以不使用 `git add` 把他们加到索引中，不过随即而来的就是一些麻烦事，在你使用 `git add .` 的时候还是会追踪这些文件。使用 `git status` 也会显示 `Untracked files……`。我一直在使用 Github For Windows ，如果没有 .gitignore 文件，每次提交之前也要取消勾选那些不想上传的文件，非常的麻烦。

<!-- more -->

# 创建 .gitignore

以上说的那些问题都可以通过一个或者更多的 .gitignore 文件搞定，你可以在项目的任意目录下创建 .gitignore 文件，此文件的影响范围是当前目录和其所有子目录，可以在不同目录创建多个 .gitignore 文件【不过一般都是在项目根目录创建一个就行了】。

需要注意的是 Windows 下直接`右键->新建文本文档`，然后键入文件名是没法创建的。。。详情请参考我的另一篇博客【这两篇博客应该是同一天写完的啊！拖延症得治呀( ╯□╰ )】：

> [Windows 使用 cmd(命令提示符)创建文件][1]

你还可以在 Github 上新建一个仓库的同时指定生成特定的 .gitignore 文件，如下图：

{% asset_img create-gitignore.png Github 自动创建指定的 .gitignore %}

# 编辑 .gitignore

创建好后，我们就可以用任何编辑器打开了，在其中填写你想要忽略的文件的文件名，Git 就会忽略这些文件。例子请往下看：

{% code lang:sh %}
    # 以'#' 开始的行，被视为注释。
    # 忽略掉所有文件名是 cylong 的文件和文件夹。
    cylong
    # 如果只想忽略 cylong 文件夹【不能有上面那段代码】。
    cylong/
    # 如果只想忽略 doc/ 下面的 cylong 文件【同样不能有上面的 cylong 和 cylong/ 】
    doc/cylong
    # 忽略所有后缀为 html 文件。
    *.html
    # about.html 不想被忽略。
    !about.html
    # 忽略所有后缀为 .o 和 .a文件。
    *.[oa]
{% endcode %}

除上面的一些例子以外，[abc]、[0-9]、[a-z]、? 这些正则表达式也是支持的。

# 使用 Github 的配置文件

如果只是我们自己写的话，可能并不会写的很全【重要的是比较麻烦是吧！】。还好 Github 已经为我们准备了各种配置文件，我们想要用哪个，就直接拿来用就好了。详情请戳：

> <https://github.com/github/gitignore>

# 删除已经跟踪的文件

如果文件已经被 Git 跟踪了， 那么这些文件即使被写在了 .gitignore 中，Git 也不会忽略他，还是会继续跟踪，解决办法是输入以下命令：

{% code lang:sh %}
    git rm --cached test.txt # 如果是文件夹，后面再加上 -r
    git commit -m 'delete test.txt'
{% endcode %}

# 参考资料&感谢

> [忽略特殊文件 - 廖雪峰的官方网站][2]
> [.gitignore 文件使用说明 - change2hao][3]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://www.cylong.com/blog/2016/05/09/windows-linux-new-file/ "Windows 使用 cmd(命令提示符)创建文件"
[2]: http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758404317281e54b6f5375640abbb11e67be4cd49e0000 "忽略特殊文件 - 廖雪峰的官方网站"
[3]: https://segmentfault.com/a/1190000000522997 ".gitignore 文件使用说明 - change2hao"
