---
title: 愚人节快乐
date: 2019-04-01 22:35:22
updated: 2019-04-01 22:35:22
categories:
    - Java
tags:
    - Java
    - Lambda
    - 随笔
---
---

事情起因是看到一个面试题，原题大概是，生成 N 个 1 到 1000 之间的随机数(N <= 1000)，对于重复的数字，只取其中一个，并对结果进行从小到大排序。正好前几天了解了下 Java 的 Lambda 表达式和 Stream API，突然想起来，这可以一行代码搞定啊。于是就尝试的写了一下。【原题还是比较复杂的，我就提取了精华部分】

<!-- more -->

一行代码能搞定的事情从不会多写【为了美观我还是换行了】。

``` java
IntStream.range(0, 100)
    .map(x -> (int)(Math.random() * 1000 + 1))
    .distinct()
    .sorted()
    .forEach(System.out::println);
```

自从 Java 8 增加了 Lambda 表达式和 Stream API 后，很多操作都非常的方便，可以参考我上一篇博客：

> [Java 8 的 Lambda 表达式和 Stream API | 笑话人生][1]

<span style="background-color:#000000">其实这就是一篇愚人节凑数用的博客，顺便祝愿下女票可以找到心仪的实习，相信自己，努力终将不会白费 (^_^)</span>

---

[1]: http://www.cylong.com/blog/2019/03/18/lambda/ "Java 8 的 Lambda 表达式和 Stream API | 笑话人生"