---
title: 两数相加
date: 2019-11-25 15:17:27
updated: 2019-11-25 15:17:27
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 链表
    - 单向链表
---
---

# 题目描述

给出两个非空的链表用来表示两个非负的整数。其中，它们各自的位数是按照逆序的方式存储的，并且它们的每个节点只能存储一位数字。如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**
> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807

<!-- more -->

# 题解

由于各自的位数是按照逆序的方式存储的，所以我们只要一位一位进行加法就可以了，同时考虑溢出的情况，使用进位 carry 来表示。

{% asset_img add_two_numbers.svg 两数相加图解 %}

对于两个相加的链表，可能出现长度不一样的情况，这个时候，我们只要处理长的链表，单独加此链表的值即可。

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    int carry;
    ListNode l3 = new ListNode(0);
    ListNode l3_res = l3;
    while (l1 != null || l2 != null) {
        if (l1 == null) {
            l3.val += l2.val;
            l2 = l2.next;
        } else if (l2 == null) {
            l3.val += l1.val;
            l1 = l1.next;
        } else {
            l3.val += (l1.val + l2.val);
            l1 = l1.next;
            l2 = l2.next;
        }
        carry = l3.val / 10;
        l3.val = l3.val % 10;
        if (l1 != null || l2 != null || carry == 1) {
            l3.next = new ListNode(carry);
            l3 = l3.next;
        }
    }
    return l3_res;
}
```

另外一种处理两个链表长度不一样的方法就是，我们就将短的链表补0，继续与长的链表相加即可。

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode p = l1;
    ListNode q = l2;
    int carry = 0;
    while (q != null) {
        int sum = carry + p.val + q.val;
        p.val = sum % 10;
        carry = sum / 10;
        if (p.next == null && q.next != null) {
            p.next = new ListNode(0);
        }
        if (p.next != null && q.next == null) {
            q.next = new ListNode(0);
        }
        if (p.next == null && q.next == null && carry != 0) {
            p.next = new ListNode(carry);
        }
        p = p.next;
        q = q.next;
    }
    return l1;
}
```

## 复杂度分析

* 时间复杂度：Ο(max(m,n))，假设 m 和 n 分别表示 l1 和 l2 的长度，上面的算法最多重复 max(m,n) 次。
* 空间复杂度：Ο(max(m,n))，新列表的长度最多为 max(m,n) + 1。

# 来源

> [两数相加 | 力扣（LeetCode）][1]
> [两数相加 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/add-two-numbers/ "两数相加 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/add-two-numbers/solution/ "两数相加 | 题解（LeetCode）"
