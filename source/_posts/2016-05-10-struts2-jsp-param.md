---
title: Struts2 参数依赖注入失败
date: 2016-05-10 10:19:42
categories:
    - Java
tags:
    - Java
    - SSH
    - JSP
---
---

这是在 [SegmentFault][] 上看到的一个问题，正好前不久学习了 J2EE 的 SSH 框架，看到这个问题忍不住答了一下。结果问题并不像表面上那么简单。没办法，自己挖的坑，跪着也要填完( ▼-▼ )。原问题链接：

> [struct2框架jsp页面传参失败][1]

<!-- more -->

# 源码

和提问者要了源码，方便查找BUG。源代码太长，我就截取了部分代码。

{% code lang:html addFlower.jsp %}
    <s:form action="addFlower" method="post">
         <table width="380" >
              <tr>
                 <td>鲜花ID：</td>
                 <td>
                 <s:textfield id="goodsId" name="goods.goodsId"/>
                 </td>
              </tr>
              <tr>
                  <td><input type="submit" value="添加" class="btn" />
              </tr>
         </table>
    </s:form>
{% endcode %}

{% code lang:xml struts.xml %}
    <package name="flowers" namespace="/" extends="struts-default">
        <action name="addFlower" class="flowers" method="addFlowerInfo">
             <result name="index">/flowerInformation/flowerInformation.jsp</result>
        </action>
    </package>
{% endcode %}

{% code lang:xml applicationContext.xml %}
    <bean name="adminUser" class="com.xhydxs.action.AdminUserAction"
        scope="prototype">
        <property name="adminUserBiz" ref="adminUserBiz" />
    </bean>
    <bean name="flowers" class="com.xhydxs.action.FlowerAction" scope="prototype">
        <property name="flowerBiz" ref="flowerBiz" />
    </bean>
{% endcode %}

{% code lang:java FlowerAction.java %}
    public class FlowerAction extends ActionSupport
        implements RequestAware, SessionAware {

        private Goods goods;

        public Goods getGoods() {
            return goods;
        }

        public void setGoods(Goods goods) {
            this.goods = goods;
        }

        public String addFlowerInfo() {
            System.out.println("goods:" + goods);
            return "index";
        }
    }
{% endcode %}

# 运行

基本上看这些代码是完全正确的啊，并没有什么错误【后来证明确实也没有错误( ╯□╰ )】。拿到项目代码的我运行了下【运行前由于环境不同，我配置了好久啊啊啊啊！】，输出一直是 `goods:null`。这我就非常奇怪了，和之前写的 J2EE 项目对比了下，套路都是一样的啊！后来我发现了上面的一堆错误信息【为什么不早点看这些错误信息呢】，基本上都是围绕着下面这个错误：

{% code %}
    Caused by: java.lang.IllegalStateException: Cannot convert value of type [com.xhydxs.action.AdminUserAction] to required type [com.xhydxs.entity.AdminUser] for property 'adminUser': no matching editors or conversion strategy found
{% endcode %}

# 解决

可以发现是类型转化问题，为什么会让 AdminUserAction 转化成 AdminUser 呢？这两个类怎么会互相转化呢？于是我使用全局查找 AdminUserAction 和 AdminUser 这两个关键词【用 Atom 编辑器】。终于让我发现以下的代码！！！

{% code lang:xml applicationContext.xml %}
    <bean name="adminUser" class="com.xhydxs.action.AdminUserAction"
        scope="prototype">
        <property name="adminUserBiz" ref="adminUserBiz" />
    </bean>
{% endcode %}

{% code lang:xml Goods.hbm.xml %}
    <many-to-one name="adminUser" class="com.xhydxs.entity.AdminUser"
        fetch="select">
        <column name="UPD_OPR" length="20" not-null="true" />
    </many-to-one>
{% endcode %}

发现了么？name 都是 adminUser，然而 class 分别对应着 AdminUserAction 和 AdminUser。然后把 AdminUserAction 对应的 name 改掉就行了。。。。至于为什么上面这个错误会导致 goods 为 null，因为在 Goods 类里有一个 AdminUser 成员变量，导致 struts 没法构建 goods 这个实例了>3<

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[SegmentFault]: https://segmentfault.com/ "SegmentFault"
[1]: https://segmentfault.com/q/1010000005088213/a-1020000005088292 "struct2框架jsp页面传参失败"
