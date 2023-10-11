---
title: Java 通过 DOM 方式解析、创建 XML
date: 2017-04-04 23:49:23
updated: 2017-04-04 23:49:23
categories:
    - Java
tags:
    - Java
    - DOM
    - XML
---
---

# DOM 简介

DOM（Document Object Model） 是 W3C 处理 XML 的标准 API，不仅 Java 其他很多语言，比如 Javascript、PHP等等语言都实现了该标准。Java 类库支持 DOM 操作【也就是说不需要下载依赖其他包】。DOM 以树状结构组织节点和信息的集合，这种结构允许开发人员对 XML 文档进行增删改查。为了分析该树状结构，我们需要加载整个 XML 文档进行构造分析，所以消耗资源比较大，建议在操作小文件的时候使用。

<!-- more -->

# 创建 XML 文档

1. 创建 XML Document 对象
```java
/**
 * 创建 XML Document 对象
 * @return XML Document 对象
 * @author cylong
 * @version 2017年4月5日 上午12:19:41
 */
private static Document createXMLDocument() {
    DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
    DocumentBuilder builder = null;
    Document document = null;
    try {
        builder = factory.newDocumentBuilder();
        document = builder.newDocument();
        Element root = document.createElement("college");
        document.appendChild(root);

        Element student = document.createElement("student");
        student.setAttribute("id", "0");
        Element name = document.createElement("name");
        name.appendChild(document.createTextNode("cylong"));
        student.appendChild(name);
        Element biography = document.createElement("biography");
        biography.appendChild(document.createCDATASection("Hello"));
        student.appendChild(biography);
        Element age = document.createElement("age");
        age.appendChild(document.createTextNode("24"));
        student.appendChild(age);

        Element student1 = document.createElement("student");
        student1.setAttribute("id", "1");
        Element name1 = document.createElement("name");
        name1.appendChild(document.createTextNode("cylong1"));
        student1.appendChild(name1);
        Element biography1 = document.createElement("biography");
        biography1.appendChild(document.createCDATASection("World"));
        student1.appendChild(biography1);
        Element age1 = document.createElement("age");
        age1.appendChild(document.createTextNode("25"));
        student1.appendChild(age1);

        root.appendChild(document.createComment("学生0"));
        root.appendChild(student);
        root.appendChild(document.createComment("学生1"));
        root.appendChild(student1);
    } catch (ParserConfigurationException e) {
        e.printStackTrace();
    }
    return document;
}
```
2. 将创建的 XML Document 写入到文件中
```java
/**
 * 将创建的 XML Document 写入到文件中
 * @param document
 * @param path 文件路径
 * @author cylong
 * @version 2017年4月3日 上午2:39:32
 */
private static void writeXML(Document document, String path) {
  try {
	TransformerFactory tf = TransformerFactory.newInstance();
	Transformer transformer = tf.newTransformer();
	DOMSource source = new DOMSource(document);
	transformer.setOutputProperty(OutputKeys.ENCODING, "utf8");
	transformer.setOutputProperty(OutputKeys.INDENT, "yes");
	PrintWriter pw = new PrintWriter(new FileOutputStream(path));
	StreamResult result = new StreamResult(pw);
    transformer.transform(source, result);
  } catch (TransformerConfigurationException e) {
	e.printStackTrace();
  } catch (IllegalArgumentException e) {
 	e.printStackTrace();
  } catch (FileNotFoundException e) {
	e.printStackTrace();
  } catch (TransformerException e) {
	e.printStackTrace();
  }
}
```

# 解析 XML 文档

1. 解析 XML 文件
```java
/**
 * 解析 XML 文档
 * @param path XML 文档路径
 * @author cylong
 * @version 2017年4月3日 上午2:48:53
 */
private static void parserXML(String path) {
    try {
        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        DocumentBuilder db = dbf.newDocumentBuilder();
        Document document = db.parse(path);

        // optional, but recommended
        // read this - http://stackoverflow.com/questions/13786607/normalization-in-dom-parsing-with-java-how-does-it-work
        document.getDocumentElement().normalize();

        Element root = document.getDocumentElement();
        System.out.println("Root element : " + root.getNodeName());
        NodeList students = root.getElementsByTagName("student");
        for(int i = 0; i < students.getLength(); i++) {
            Node student = students.item(i);
            System.out.println(student.getNodeName());

            NodeList info = student.getChildNodes();
            for(int j = 0; j < info.getLength(); j++) {
                Node meta = info.item(j);
                if (meta.getNodeType() == Node.ELEMENT_NODE) {
                    System.out.println(meta.getNodeName() + ":" + meta.getTextContent());
                }
            }
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (ParserConfigurationException e) {
        e.printStackTrace();
    } catch (SAXException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

2. 注意
可以 [点此链接][1] 了解其用处。
```java
// optional, but recommended
// read this - http://stackoverflow.com/questions/13786607/normalization-in-dom-parsing-with-java-how-does-it-work
document.getDocumentElement().normalize();
```


# 参考&感谢

> [Github 完整的代码案例][2]
> [How to read XML file in Java – (DOM Parser)][3]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: http://stackoverflow.com/questions/13786607/normalization-in-dom-parsing-with-java-how-does-it-work
[2]: https://github.com/cylong1016/CodeJava/blob/master/src/cylong/xml/ParserXML.java
[3]: https://www.mkyong.com/java/how-to-read-xml-file-in-java-dom-parser/
