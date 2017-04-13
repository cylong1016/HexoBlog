---
title: 多说评论迁移至 Disqus - Java 实现
date: 2017-04-05 01:01:33
updated: 2017-04-05 01:01:33
categories:
    - Hexo
tags:
    - java
    - disqus
    - hexo
---
---

在网上找了一圈后，很多人都造过轮子，但是由于年代久远，多说和 Disqus 的评论格式可能发生变化，试了一些后并没有一个成功。无奈自己开始造轮子，不过看完两种评论文件格式后，发现其实还是瞒简单的，于是就用 Java 实现了一个。【为了节省时间就用自己最擅长的 Java 了，虽然其他语言可能会更快更方便的使用( ╯□╰ )】。下面附上工具链接和使用方法。

> [工具源码地址][3]

<!-- more -->

# 导出多说评论

1. 进入多说后台选择 `工具->导出数据`。
2. 勾上 `包含文章数据` 和 `包含评论数据` 两个选项。
3. 导出后是一个 JSON 文件，为了方便查看可以使用 [在线代码格式化][4] 工具。

# 使用工具转化

1. [点击下载][5] 转化工具，并解压。
2. 将导出的多说 JSON 文件重命名为 `duoshuo.json` 放入 `data` 文件夹下。【先删除掉存在的文件吧，其实是我的多说评论数据】
3. 双击运行 `run.bat`。将会在 `data` 文件夹下生成 `duoshuo-format.json` 【格式化后的多说评论文件，方便查看】和 `disqus.xml`【导入到 Disqus 的 XML 文件】。

**注意：如果发现并没有生成以上的两个文件，或者生成的文件数据有误，请使用以下方式运行程序。**

5. 按住 `Shift` + 鼠标右键选择 `在此处打开命令行窗口`【Windows】或者打开终端进入项目目录下【Linux】。
6. 输入以下命令并回车：
```
java -jar DuoshuoToDisqus.jar
```
7. 这种方式运行的好处是可以看到程序出错信息，同时你可以在命令最后输入你的多说评论文件路径【就不用将多说评论文件放入到 `data` 文件夹下了。】
```
java -jar DuoshuoToDisqus.jar C:\duoshuo.json
```

# 导入到 Disqus 中

1. [点击链接][6] 进入到导入页面，选择你要导入评论的站点。
2. 选择刚刚生成的 `disqus.xml` 文件，后面的选项选择 `WordPress(WXR)`，点击 `Upload`。
3. 接下来静静的等待导入完成，可以看到导入的评论和文章数量，如果出错的话可以看到错误。

# 总结

整个工具其实就是解析多说的 JSON 文件并转化成 Disqus 评论的 XML 文件。想要自己用 Java 实现的可以参考以下链接：

> [多说评论格式][1]
> [Disqus 评论格式][2]
> [Jackson - Java Object 与 JSON 之间的转化工具][7]
> [Java 通过 DOM 方式解析、创建 XML][8]
> [工具源码地址][3]

此工具需要 Java 运行环境，可以去网上搜索安装配置一下。另外此工具没有做什么非法输入的处理，所以不要尝试做一些奇怪的事情。如果运行出错请检查一下你的源多说 JSON 文件是否有错误或者使用方式是否有错，有任何问题或者想要我帮忙转化的请在下方留言或者 [联系我][9]。很高兴可以帮助到你(●'◡'●)。

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://dev.duoshuo.com/docs/500fc3cdb17b12d24b00000a "多说评论格式"
[2]: https://help.disqus.com/customer/portal/articles/472150-custom-xml-import-format "Custom XML Import Format"
[3]: https://github.com/cylong1016/DuoshuoToDisqus "工具源码地址"
[4]: http://tool.oschina.net/codeformat/json "在线代码格式化"
[5]: https://github.com/cylong1016/DuoshuoToDisqus/archive/master.zip
[6]: https://import.disqus.com/ "Import and Export - Disqus"
[7]: /blog/2017/03/31/jackson-java-json/ "Jackson - Java Object 与 JSON 之间的转化工具"
[8]: /blog/2017/04/04/java-dom-xml/ "Java 通过 DOM 方式解析、创建 XML"
[9]: /about/ "关于我"
