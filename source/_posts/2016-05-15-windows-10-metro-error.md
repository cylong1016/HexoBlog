---
title: Windows 10 Metro 应用闪退、无法打开修复方法
categories:
  - Windows
tags:
  - windows
date: 2016-05-15 00:19:26
---
---

最近在使用 WindowsModernAppsTools 研究 [Windows Metro 应用翻墙][2] 问题，由于我的手贱删除了些不该删除的东西，导致我的系统中很多 Win 10 应用没法打开了，比如日历、资讯、天气、必应词典等等都打不开了。虽然平时不常用，但是强迫症的我表示没法忍受啊！于是找了很多资料，终于搞定了！

<!-- more -->

**注意：** 我说的应用都是 Windows 应用商店里的应用。

# 解决方法

1. 键盘组合键 <span class="fa fa-windows"></span> + S，呼出 Cortana，然后输入 PowerShell。在结果中的 PowerShell 上 `右键->以管理员身份运行`。
2. 输入以下命令：
{% code lang:sh %}
    Get-AppXPackage -AllUsers | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
{% endcode %}
3. 回车后等待命令执行完毕即可。
4. 以上其实是对应用进行了重置

# 参考资料&感谢

> [Win10应用商店、应用打不开或闪退的解决方法 - IT之家][1]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)


[1]: http://www.ithome.com/html/win10/166832.htm "Win10应用商店、应用打不开或闪退的解决方法 - IT之家"
[2]: http://www.cylong.com/blog/2016/06/07/windows-10-metro-shadowsocks/ "Windows 10 Metro 应用使用本地 Shadowsocks 代理"
