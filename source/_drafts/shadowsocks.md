---
title: 站在 Shadowsocks 的肩膀上发现精彩的世界
date: 2016-05-26 13:51:29
categories:
    - Shadowsocks
tags:
    - shadowsocks
    - windows
    - android
    - linux
    - 翻墙
---
---

国内的网络环境我不说相信大家都懂。虽然墙内的世界很丰富，但是墙外的世界还有着更加精彩的内容。这不得不让我想起了曾经看过的一部漫画——进击的巨人。大部分人都在墙内过着安逸的生活，但是总有那么一帮人，想要去墙外探索未知的世界。我就是这样的人。之前因为想要体验 Google 搜索、体验 Youtube、查阅学习资料，还有玩的部分游戏需要翻墙，找了很多免费的代理和 VPN，效果都不好，断断续续的。后来经过舍友的推荐，入了 Shadowsocks 的坑。体验了有两个月左右，效果很棒，访问速度也很快，强烈推荐给大家使用！！！(●'◡'●)

<!-- more -->

# 购买服务

1. 点击进入 [Shadowsocks][1]，进入首页后选择 `订购服务`。
2. 之后选择你想要购买的服务，点击现在订购。我选择的是 `Shadowsocks.com 普通版`。需要注意的是，虽然显示的价格是美元，但是在后面支付的时候会自动转化为人民币。
3. 界面上选择你的付款年限，然后点击继续。
4. 在结账页面，你需要填写各种信息，需要认真填写，这也是在创建账号。
5. Shadowsocks 支持 Alipay 支付宝国际版。之后付款就可以了，现在大约是 104 块钱一年。我买的时候，订购服务时显示的还是人民币，99块钱一年。虽然贵了一点点，不过还是可以接受的。
6. 之后进入 [客户中心][2]，用第4步创建的账号登陆。
7. 点击产品服务，可以看到你刚刚购买的服务，状态为有效。
8. 点击刚刚购买的服务，会看到产品详情。下面有配置文件下载，选择 Windows 版下载。下载下来是 `gui-config.json`。

# 客户端安装使用

支持的客户端：OS X， Windows， Linux， iOS， Android， OpenWRT 路由器等。
详情请参考：[客户端 - Shadowsocks][3]，客户端都在 Github 上。

## Windows 客户端

1. 点击下载 [Shadowsocks-3.0.zip][4]【写这篇博客时候的最新版本】，或者去 [Github - Shadowsocks Windows][5] 上寻找其他版本。
2. 解压后有一个 `Shadowsocks.exe` 文件。最好把这个文件放到一个目录下，比如新建一个 Shadowsocks 文件夹。
3. 把刚刚下载的 `gui-config.json` 文件放到与 `Shadowsocks.exe` 相同的目录下。
4. 双击 `Shadowsocks.exe`，会出现一个 GUI 界面，自动读取了 `gui-config.json` 文件中的内容。
5. 在右下角托盘图标上会有一个好像纸飞机的 Shadowsocks 图标，`右键->启动系统代理`，就可以越过墙壁，浏览更多丰富多彩的内容啦~
6. 另外建议设置成 `右键->开机启动`，这样不用每次开机手动启动了。还可以在 `右键->服务器` 中选择不同的服务器。

## Android 客户端

1. 点击下载 [shadowsocks-nightly-2.10.3.apk][6]【写这篇博客时候的最新版本】，或者去 [Github - Shadowsocks Android][7] 上寻找其他版本。
2. 把这个 apk 安装到手机上【可以传到手机里，打开这个 apk 就能安装了】。
3. 安装完成后仍然需要配置，支持扫描二维码配置，但是需要下载一个客户端。
4. 我不喜欢下太多的客户端，所以就手动配置，具体的值可以用记事本打开 `gui-config.json` 查看。
5. 翻墙后就可以去 Google Play 商店更新 Shadowsocks 了~(●'◡'●)

## Linux 客户端

## 其他客户端

其他的客户端基本上就剩下苹果设备了，由于本渣没有任何的苹果设备。。。所以自己研究吧。去 [客户端 - Shadowsocks][3] 可以找到相关的客户端。

# 注意

* 要是偶尔发现网络不稳定，切换线路试一下。
* 不要同时在多台设备上使用，也不要把配置文件分享给其他人，可能会遭到封号行为。不过手机和 PC 同时使用时没事的。
* 我遇到一个问题就是启动某些服务器无法下载 Google Play 里的应用，而且有些应用也提示无法在你所在的国家或者地区购买，这个时候就切换下线路，并且清除 Google Play 数据重启就行了。

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://shadowsocks.com/ "Shadowsocks"
[2]: https://portal.shadowsocks.com/clientarea.php "客户中心 - Shadowsocks"
[3]: https://shadowsocks.com/client.html "客户端 - Shadowsocks"
[4]: https://github.com/shadowsocks/shadowsocks-windows/releases/download/3.0/Shadowsocks-3.0.zip "Shadowsocks-3.0.zip"
[5]: https://github.com/shadowsocks/shadowsocks-windows/releases "Github - Shadowsocks Windows"
[6]: https://github.com/shadowsocks/shadowsocks-android/releases/download/v2.10.3/shadowsocks-nightly-2.10.3.apk "shadowsocks-nightly-2.10.3.apk"
[7]: https://github.com/shadowsocks/shadowsocks-android/releases "Github - Shadowsocks Android"
