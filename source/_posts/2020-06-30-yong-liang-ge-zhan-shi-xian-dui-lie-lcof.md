---
title: 用两个栈实现队列
date: 2020-06-30 20:08:36
updated: 2020-06-30 20:08:36
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 剑指Offer
    - 栈
    - 队列
---
---

# 题解

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

**示例 1：**
> 输入：
> ["CQueue", "appendTail", "deleteHead", "deleteHead"]
> [[], [3], [], []]
> 输出：[null, null, 3, -1]

**示例 2：**
> 输入：
> ["CQueue", "deleteHead", "appendTail", "appendTail", "deleteHead", "deleteHead"]
> [[], [], [5], [2], [], []]
> 输出：[null, -1, null, null, 5, 2]

**提示：**
* 1 <= values <= 10000
* 最多会对 appendTail、deleteHead 进行 10000 次调用

<!-- more -->

# 题解

队列的特性是先进先出，栈的特性是后进先出。维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，我们每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第二个栈，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。如果为空，我们将第一个栈里的元素一个个弹出插入到第二个栈里，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。

{% asset_img 两个栈实现队列.gif 两个栈实现队列 %}

```java
Stack<Integer> st1;
Stack<Integer> st2;
public CQueue() {
    st1 = new Stack<>();
    st2 = new Stack<>();
}

public void appendTail(int value) {
    st1.push(value);
}

public int deleteHead() {
    if (st2.isEmpty()) {
        while (!st1.isEmpty()) {
            st2.push(st1.pop());
        }
    }
    return st2.isEmpty() ? -1 : st2.pop();
}
```

## 复杂度分析

* 时间复杂度：对于插入和删除操作，时间复杂度均为 O(1)。插入不多说，对于删除操作，虽然看起来是 O(n) 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 st2 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 O(1)。
* 空间复杂度：O(n)。需要使用两个栈存储已有的元素。

# 来源

> [用两个栈实现队列 | 力扣（LeetCode）][1]
> [用两个栈实现队列 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/ "用两个栈实现队列 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/solution/mian-shi-ti-09-yong-liang-ge-zhan-shi-xian-dui-l-3/ "用两个栈实现队列 | 题解（LeetCode）"
