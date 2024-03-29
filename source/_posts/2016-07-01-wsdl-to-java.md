---
title: 使用 Eclipse 根据 WSDL 生成 Java 代码
date: 2016-07-01 17:39:06
updated: 2016-07-01 17:39:06
categories:
    - Java
tags:
    - Java
    - WSDL
    - Web Service Client
    - Eclipse
---
---

这学期的 SOA 课程已经结束了，但是我为什么感觉什么都没有学到呢！！反正学到什么就整理下好了！这次是使用 Eclipse 根据 WSDL 生成 Java 代码。整个过程都是自动的，完全是傻瓜式的！

<!-- more -->

# 准备

* WSDL 地址【自己可以发布，如何发布我会在日后写，先挖个坑<del>一定要填上</del>】
* [Eclipse IDE for Java EE Developers][1]

# 步骤

1. 打开 Eclipse，`File -> New -> Dynamic Web Project`
2. 右键刚刚新建的项目，`New -> Other -> Web Services -> Web Service Client`
3. 在 Service definition 输入 WSDL 的地址，之后点 `Next` 或者 `Finish` 即可
4. 会生成类似下面这样的代码结构
{% asset_img wsdl-to-java.png WSDL to Java %}
5. 其中 xxxProxy.java 中就有服务提供的接口方法，直接调用就行了。

# 参考&感谢

> [使用Eclipse生成Web Service Client - andybang1981的日志][2]

---

[1]: http://www.eclipse.org/downloads/eclipse-packages/ "Eclipse IDE for Java EE Developers"
[2]: http://andybang1981.blog.163.com/blog/static/177927368201426001589/ "使用Eclipse生成Web Service Client - andybang1981的日志"
