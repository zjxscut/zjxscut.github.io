---
title: leetcode_138_复制带随机指针的链表
date: 2020-09-13 18:58:50
tags: leetcode
---

[题目原文链接](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

　　深拷贝即需要复制一个一模一样的链表出来，而非做链接指向。

​		思路：复制一样数量的节点出来，然后对节点进行链接处理，使用map的键值对<原本节点，新生节点>

```java
class Solution {
    public Node copyRandomList(Node head) {
        Node p = head;
        Map<Node, Node> m = new HashMap();
        // 将原链表装载入HashMap中
        while (p != null) {
            Node n = new Node(p.val);
            m.put(p, n);
            p = p.next;
        }

        p = head;
        while (p != null) {
            Node loc = m.get(p);
            // 复制next链接
            loc.next = p.next == null ? null : m.get(p.next);
            // 复制random链接
            loc.random = p.random == null ? null : m.get(p.random);
            p = p.next;
        }
        return m.get(head);
    }
}
```