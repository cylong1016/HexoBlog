---
title: 单向链表重排：交替连接首尾节点（面试题）
date: 2025-09-02 00:30:31
updated: 2025-09-02 00:30:31
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 链表
    - 递归
    - 回溯
    - 迭代
    - 双指针
---
---

# 题目描述

假设你有一个单向链表 `L`，其首节点被标为 `head`，这个链表代表了小美的工作任务流程：
> L0 → L1 → … → Ln-1 → Ln

你需要对其进行重新组织，以达到以下新的工作任务流程：
> L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

请注意，这里不能只修改节点任务的内容，而是需要实际地进行节点任务的交换。

**示例 1:**
> **输入：**`L = 1 → 2 → 3 → 4 → 5`
> **输出：**`1 → 5 → 2 → 4 → 3`

<!-- more -->

# 核心思路

为了解决这个问题，我们需要重新组织单向链表，使其满足特定的顺序：第一个节点后跟最后一个节点，然后第二个节点，接着倒数第二个节点，以此类推。关键在于实际交换节点，而不仅仅是修改节点值。

1. **寻找链表中点：**使用快慢指针技术找到链表的中间节点。慢指针每次移动一步，快指针每次移动两步。当快指针到达末尾时，慢指针指向链表的中间节点。
2. **分割链表：**将链表从中间节点分割成两个部分。前半部分从头部到中间节点，后半部分从中间节点的下一个节点开始。
3. **反转后半部分：**将后半部分链表反转，以便能够从末尾开始遍历。
4. **合并链表：**将前半部分链表和反转后的后半部分链表交替合并，形成所需的顺序。

## 代码实现

```java
/**
 * 单向链表节点定义
 */
class ListNode {
    int val;
    ListNode next;

    ListNode() {

    }

    ListNode(int val) {
        this.val = val;
    }

    ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}

class Solution {
    /**
     * 重新组织链表
     * @param head 链表头节点
     */
    public void reorderList(ListNode head) {
        // 处理空链表或只有一个节点的情况
        if (head == null || head.next == null) {
            return;
        }
        
        // 步骤1: 使用快慢指针找到链表中点
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // 步骤2: 分割链表为两部分
        ListNode second = slow.next;
        // 断开链表
        slow.next = null;
        
        // 步骤3: 反转后半部分链表
        second = reverseList(second);
        
        // 步骤4: 合并两个链表
        ListNode first = head;
        while (first != null && second != null) {
            ListNode firstNext = first.next;
            ListNode secondNext = second.next;
            
            // 将后半部分的节点插入到前半部分的节点之间
            first.next = second;
            second.next = firstNext;
            
            // 移动指针到下一位置
            first = firstNext;
            second = secondNext;
        }
    }
    
    /**
     * 反转链表
     * @param head 要反转的链表头节点
     * @return 反转后的链表头节点
     */
    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
}
```

## 算法示例

1. 假设原始链表为：`1 → 2 → 3 → 4 → 5`
    * 找到中点：节点 `3`
    * 分割链表：前半部分 `1 → 2 → 3`，后半部分 `4 → 5`
    * 反转后半部分：`5 → 4`
    * 合并链表：`1 → 5 → 2 → 4 → 3`
2. 最终结果为：`1 → 5 → 2 → 4 → 3`

## 复杂度分析

* **时间复杂度：**`O(n)`。寻找链表中点：`O(n/2) ≈ O(n)`，反转后半部分链表：`O(n/2) ≈ O(n)`，合并两个链表：`O(n/2) ≈ O(n)`，总体时间复杂度为线性 `O(n)`。
* **空间复杂度：**`O(1)`。算法只使用了常数级别的额外空间（几个指针变量）。

---
