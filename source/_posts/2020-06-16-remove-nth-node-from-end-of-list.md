---
title: 删除链表的倒数第N个节点
date: 2020-06-16 02:35:53
updated: 2020-06-16 02:35:53
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 链表
    - 指针
    - 双指针
    - 滑动窗口
---
---

# 题目描述

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

**示例：**
> 给定一个链表: 1->2->3->4->5, 和 n = 2.
> 当删除了倒数第二个节点后，链表变为 1->2->3->5.

**说明：**
> 给定的 n 保证是有效的。

**进阶：**
> 你能尝试使用一趟扫描实现吗？

<!-- more -->

# 两次遍历

最简单的思路，我们发现其实是删除从列表开头数起的第 `(L - n + 1)` 个结点，其中 L 是列表的长度。只要我们找到列表的长度 L，这个问题就很容易解决。我们先遍历一次链表，求出链表的长度 L，然后再遍历一次链表，删除倒数第 n 个节点即可。需要注意的是如果 `L = n`，那么要直接返回 `head.next`。

{% asset_img 两次遍历.png 两次遍历 %}

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    int len = 0;
    ListNode node = head;
    do {
        len++;
        node = node.next;
    } while (node != null);
    if (len == n) {
        return head.next;
    }
    int index = 0;
    node = head;
    do {
        if (++index == len - n) {
            node.next = node.next.next;
            return head;
        }
        node = node.next;
    } while (node != null);
    return head;
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

* 时间复杂度：O(L)，该算法对列表进行了两次遍历，首先计算了列表的长度 L 其次找到第 `L - n` 个结点。 操作执行了 `2L - n` 步，时间复杂度为 O(L)。
* 空间复杂度：O(1)，只需要额外的常数级别的空间。

# 一次遍历

上述算法可以优化为只使用一次遍历。我们可以使用两个指针而不是一个指针。第一个指针从列表的开头向前移动 `n + 1` 步，而第二个指针将从列表的开头出发。现在，这两个指针被 n 个结点分开。我们通过同时移动两个指针向前来保持这个恒定的间隔，直到第一个指针到达最后一个结点。此时第二个指针将指向从最后一个结点数起的第 n 个结点。我们重新链接第二个指针所引用的结点的 next 指针指向该结点的下下个结点。

{% asset_img 一次遍历.png 一次遍历 %}

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode first = dummy;
    ListNode second = dummy;
    for (int i = 1; i <= n + 1; i++) {
        first = first.next;
    }
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    second.next = second.next.next;
    return dummy.next;
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

* 时间复杂度：O(L)，该算法对含有 L 个结点的列表进行了一次遍历。因此时间复杂度为 O(L)。
* 空间复杂度：O(1)，只需要额外的常数级别的空间。

# 来源

> [删除链表的倒数第N个节点 | 力扣（LeetCode）][1]
> [删除链表的倒数第N个节点 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/ "删除链表的倒数第N个节点 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/solution/shan-chu-lian-biao-de-dao-shu-di-nge-jie-dian-by-l/ "删除链表的倒数第N个节点 | 题解（LeetCode）"
