---
title: 反转链表
date: 2020-07-16 00:29:26
updated: 2020-07-16 00:29:26
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 链表
    - 递归
---
---

# 题目描述

反转一个单链表。

**示例:**
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL

**进阶:**
> 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

<!-- more -->

# 迭代

假设存在链表 1->2->3->4->5->NULL，我们想要把它改成 5->4->3->2->1->NULL。在遍历列表时，将当前节点的 next 指针改为指向前一个元素。由于节点没有引用其上一个节点，因此必须事先存储其前一个元素。在更改引用之前，还需要另一个指针来存储下一个节点。不要忘记在最后返回新的头引用！

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

public class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }
}
```

## 复杂度分析

* 时间复杂度：Ο(N)，其中 N 是列表的长度。
* 空间复杂度：Ο(1)。

# 递归

假设节点n后面的链表均被反转，那么我需要将n的下一个节点指向n，于是有 `n.next.next = n` ，然后将n的下一个节点指向null，既 `n.next = null`。

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode res = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return res;
}

public class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }
}
```

## 复杂度分析

* 时间复杂度：Ο(N)，其中 N 是列表的长度。
* 空间复杂度：Ο(N)。由于使用递归，将会使用隐式栈空间。递归深度可能会达到 N 层。

# 来源

> [反转链表 | 力扣（LeetCode）][1]
> [反转链表 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/reverse-linked-list/ "反转链表 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-by-leetcode/ "反转链表 | 题解（LeetCode）"
