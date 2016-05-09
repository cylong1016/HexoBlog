---
title: Windows 使用 cmd(命令提示符)创建文件
categories:
  - Windows
tags:
  - windows
  - cmd
  - linux
  - shell
date: 2016-05-09 18:48:52
---
---

最近在研究 [.gitignore 的使用][1]，于是想创建一个 `.gitignore` 文件做测试，`右键->新建->文本文档` 再输入文件名后，惊讶的发现，Windows 竟然提示： 请键入文件名！【不得不去吐槽！】。看样子 Windows 是把 gitignore 当成文件名的后缀了。没办法，只好打开 cmd(命令提示符)去创建文件了。

<!-- more -->

# 打开 cmd

一般有以下方式打开 cmd【*我使用的是 Windows 10 系统，某些方法可能没用*】：
*   在某个文件夹下按住 `Shift` 键，然后点击 `鼠标右键`，选择 `在此处打开命令窗口`（建议使用此方式，这样你就不用 cd 到指定的文件夹了）。
*   键盘组合键 <span class="fa fa-windows"></span> + R，然后输入 `cmd` 并确定。
*   键盘组合键 <span class="fa fa-windows"></span> + X，然后选择 `命令提示符`。
*   键盘组合键 <span class="fa fa-windows"></span> + S，呼出 Cortana，然后输入 cmd 并确认（Windows 10才支持）。

# 创建文件

```
    echo hello > .abc
```

解释：

1. "echo hello" ：输出 "hello" 到控制台。
2. "\> .abc" ：输出重定向到 .abc 文件中【没有此文件就会创建】。
3. 当然你也可以使用其他的命令，比如 `type`，来输出重定向。核心思想就是把某些字符串输出重定向到指定文件里就可以了。

# 创建文件夹

```
    mkdir .abc
```

# Linux 使用 shell 创建文件

顺便说一下使用 shell 创建文件吧。上面的那些命令在 shell 中都可以使用，另外说一个在 shell 中更方便的命令。

```sh
    touch .abc
```

# 注意

写本文的目的是在 Windows 下创建类似 .abc 这样的文件没法使用 `右键->新建` 来创建文件。如果你想创建一个 abc.txt 就不要这么麻烦啦【也没人会这么做吧(●'◡'●)】

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)


[1]: /blog/2016/05/09/gitignore/ ".gitignore 的使用"
