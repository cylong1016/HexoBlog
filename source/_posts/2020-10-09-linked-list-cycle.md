---
title: 环形链表
date: 2020-10-09 21:45:48
updated: 2020-10-09 21:45:48
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 指针
    - 双指针
    - 链表
    - 哈希表
    - HashSet
---
---

# 题目描述

给定一个链表，判断链表中是否有环。如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

**进阶：**

你能用 O(1)（即，常量）内存解决此问题吗？

**示例 1：**
{% asset_img circularlinkedlist.png 环形链表 %}
> 输入：head = [3, 2, 0, -4], pos = 1
> 输出：true
> 解释：链表中有一个环，其尾部连接到第二个节点。

<!-- more -->

# 题解

比较简单的方法是遍历整个链表，将每个元素都加入到 HashSet 中，根据 HashSet 的没有重复元素的特性，当遇到重复的元素，说明遍历这个链表访问了重复的元素，即链表中有环。HashSet 是使用 HashMap 实现的，关于实现细节可以直接看源码。

> [浅谈 HashMap | 笑话人生][3]

```java
public boolean hasCycle(ListNode head) {
    HashSet<ListNode> visit = new HashSet<ListNode>();
    while (head != null) {
        if (!visit.add(head)) {
            return true;
        }
        head = head.next;
    }
    return false;
}

class ListNode {
   int val;
   ListNode next;
   ListNode(int x) {
       val = x;
       next = null;
 }
}
```

## 复杂度分析

* 时间复杂度：O(N)，其中 N 是链表中的节点数。最坏情况下我们需要遍历每个节点一次。
* 空间复杂度：O(N)，其中 N 是链表中的节点数。主要为哈希表的开销，最坏情况下我们需要将每个节点插入到哈希表中一次。

# 快慢指针

我们定义两个指针，一快一慢。慢指针每次只移动一步，而快指针每次移动两步。初始时，慢指针在位置 head，而快指针在位置 head.next。这样一来，如果在移动的过程中，快指针反过来追上慢指针，就说明该链表为环形链表。否则快指针将到达链表尾部，该链表不为环形链表。

```java
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    ListNode slow = head;
    ListNode fast = head.next;
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}
```

## 复杂度分析

* 时间复杂度：O(N)，其中 N 是链表中的节点数。
  * 当链表中不存在环时，快指针将先于慢指针到达链表尾部，链表中每个节点至多被访问两次。
  * 当链表中存在环时，每一轮移动后，快慢指针的距离将减小一。而初始距离为环的长度，因此至多移动 N 轮。
* 空间复杂度：O(1)。我们只使用了两个指针的额外空间。

# 来源

> [环形链表 | 力扣（LeetCode）][1]
> [环形链表 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/linked-list-cycle/ "环形链表 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/linked-list-cycle/solution/huan-xing-lian-biao-by-leetcode-solution/ "环形链表 | 题解（LeetCode）"
[3]: /blog/2019/09/10/hashmap/ "浅谈 HashMap | 笑话人生"