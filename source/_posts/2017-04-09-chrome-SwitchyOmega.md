---
title: Chrome 配置 SwitchyOmega
date: 2017-04-09 02:12:39
updated: 2023-10-12 02:12:39
categories:
  - Shadowsocks
tags:
  - Shadowsocks
  - 翻墙
  - SwitchyOmega
---
---

此文章是以 Shadowsocks 代理为例，若想使用 Shadowsocks 请先安装对应系统的客户端并启动。详情请参考：

> [站在 Shadowsocks 的肩膀上发现精彩的世界 | 笑话人生][1]

# Chrome 浏览器

无论是用户体验、强大的功能还是丰富的扩展程序都完爆国内的各种浏览器好不好 (╯‵□′)╯︵┻━┻。强烈推荐啊！目前已经可以在不翻墙的情况下去下载 [Chrome（桌面版）][2] 了，账号数据同步方面也不需要翻墙了。鼓掌撒花 ★,°:.☆(￣▽￣)/$:.°★ 

# SwitchyOmega

Google Chrome 浏览器上的一个代理扩展程序，可以轻松快捷地管理和切换多个代理设置。比如我们接下来要介绍的 `自动切换模式`。

<!-- more -->

## 下载安装

点击 [SwitchyOmega][3]，下载页面有详细的安装教程，仔细看一下就好。

## 配置 Shadowsocks 情景模式

1. 打开 Chrome， 点击右上角的 <span class="fa fa-globe" aria-hidden="true"></span> 图标，再点击 `选项`。
{% asset_img Shadowsocks-icon.png Shadowsocks 图标 %}
2. 点击左侧的 `新建情景模式`，输入情景模式名称 `Shadowsocks`（自己任意设置名称），类型选择第一个 `代理服务器`。创建完成后做如下配置：
{% asset_img new.png 新建情景模式 %}
你也可以自己设置不代理的地址列表。如上图。
3. 保存后你就可以通过这个情景模式科学上网了。

## 配置自动切换模式

配置好 Shadowsocks 情景模式后虽然可以使用 Chrome 浏览器科学上网了，但是这样的话无论你访问什么网站都会走代理，有时候访问国内的一些网站反而会很慢，这时候自动切换模式就解决了这个问题。下面介绍一下如何配置自动切换模式。

1. 点击左侧的 `自动切换`，或者自己新建情景模式，类型选择第二个 `自动切换模式`。然后做如下配置：
{% asset_img auto.png 自动切换模式 %}
* `切换规则` 是在访问 `条件设置` 的域名时候使用后面设置的 `情景模式`。比如图中我设置 `*.google.com` 和 `*.github.com` 使用 `Shadowsocks` 情景模式（刚刚创建的那个情景模式）。我们可以点击 `添加条件` 来添加自己的规则。
* 将图中 `规则列表规则` 前面的框打 √，再将后面的 `情景模式` 设置为 `Shadowsocks`，意思是规则列表中的内容，我们使用 `Shadowsocks` 情景模式。然后 `规则列表设置` 中：
  - 规则列表格式： AutoProxy；
  - 规则列表网址： <https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt>
* 这样设置完成 `规则列表规则` 后就不需要在切换规则中一个一个添加条件了。
* `切换规则` 最后一行的 `默认情景模式` 代表不在规则列表中网址我们使用 `直接连接` 情景模式，也就是说不走代理。

# 参考资料

> [SwitchyOmega | GitHub][4]
> [gfwlist | GitHub][5]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: /blog/2016/05/26/shadowsocks/ "站在 Shadowsocks 的肩膀上发现精彩的世界 | 笑话人生"
[2]: http://www.google.cn/chrome/browser/desktop/index.html "Chrome（桌面版）"
[3]: https://github.com/FelisCatus/SwitchyOmega/releases "FelisCatus/SwitchyOmega | GitHub"
[4]: https://github.com/FelisCatus/SwitchyOmega "FelisCatus/SwitchyOmega | GitHub"
[5]: https://github.com/gfwlist/gfwlist "gfwlist/gfwlist | GitHub"
