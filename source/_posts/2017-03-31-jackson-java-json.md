---
title: Jackson - Java Object 与 JSON 之间的转化工具
date: 2017-03-31 21:55:36
categories:
  - Java
tags:
  - java
  - jackson
  - json
---
---

一直在找一个 Java Object 与 JSON 之间方便快捷的转化工具，在舍友的推荐下了解到了 Jackson，使用之后对其爱不释手，现在推荐给大家。

# JSON 简介

{% blockquote JSON 中文 http://www.json.org/json-zh.html 介绍 JSON %}
JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。 易于人阅读和编写。同时也易于机器解析和生成。 它基于JavaScript Programming Language, Standard ECMA-262 3rd Edition - December 1999的一个子集。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。 这些特性使JSON成为理想的数据交换语言。
{% endblockquote %}

以上链接中包含了 JSON 的详细介绍，其实 JSON 对象【"名称/值"对的集合】和 Java 对象是对应的，JSON 数组【值的有序列表】和 Java 的数组是对应的。下面就用一些具体的实例來说明。

<!-- more -->

# JSON 和 Java 的映射

1. JSON 示例
```
{
  "name" : "cylong",
  "age" : 33,
  "position" : "Developer",
  "salary" : 7500,
  "skills" : [ "java", "python" ],
  "date" : "2017-03-31 12:29:42"
}
```
2. 对应的 Java 类
``` java
public class Staff {

  private String name;
  private int age;
  private String position;
  private BigDecimal salary;
  private List<String> skills;
  // Jackson 语法
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
  private Date date;

  // getter and setter
}
```

相信以上的例子可以让你很好的理解两者之间的对应关系。其实不仅 Java，其他语言也类似。

# Jackson 使用

1. 在 `pom.xml` 中添加依赖项
{% codeblock lang:xml pom.xml %}
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.6.3</version>
</dependency>
{% endcodeblock %}
2. Java 对象转化成 JSON
```java
ObjectMapper mapper = new ObjectMapper();
Staff obj = new Staff();

// 将 Java 对象转化成 JSON 并写入文件中
mapper.writeValue(new File("c:\\file.json"), obj);

// 将 Java 对象转化成 JSON 字符串
String jsonInString = mapper.writeValueAsString(obj);
```
3. JSON 转化成 Java 对象
```java
ObjectMapper mapper = new ObjectMapper();
String jsonInString = "{'name' : 'mkyong'}";

// 从文件中读取 JSON 并转化成 Java 对象
Staff obj = mapper.readValue(new File("c:\\file.json"), Staff.class);

// 从 URL 中读取 JSON 并转化成 Java 对象
Staff obj = mapper.readValue(new URL("http://mkyong.com/api/staff.json"), Staff.class);

// 将 JSON 字符串转化成 Java 对象
Staff obj = mapper.readValue(jsonInString, Staff.class);
```

# 参考&感谢

其实以上的内容在下面的链接中均有详细的介绍，我只是一个代码的搬运工( ╯□╰ )，就当是自己的笔记好了。

> [Jackson 2 – Convert Java Object to / from JSON][1] 【Jackson 使用详细教程】
> [Jackson Date][4] 【有关 Jackson 对日期的处理】
> [JSON 中文文档][2] 【概念性的东西，最下面也有不同语言的支持】
> [JSON 教程 - 极客学院][3] 【包含 JSON 基础介绍和在其他语言中使用 JSON】

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/ "Jackson 2 – Convert Java Object to / from JSON"
[2]: http://www.json.org/json-zh.html "JSON 中文文档"
[3]: http://wiki.jikexueyuan.com/project/json/ "JSON 教程 - 极客学院"
[4]: http://www.baeldung.com/jackson-serialize-dates "Jackson Date"
