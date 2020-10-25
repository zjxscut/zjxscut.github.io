# 876.链表的中间结点

#### 题目

&emsp;&emsp;给定一个带有头结点 `head` 的非空单链表，返回链表的中间结点。如果有两个中间结点，则返回第二个中间结点。

#### 思路一：转存vector类，直接获取

&emsp;&emsp;单链表具有顺序性，不能通过测量长度并且直接获取中间的节点。所以采用一个vector类将所有的节点按序存储起来，这样我们获得长度后，可以直接获得中间节点。

```java
import java.util.Vector;
class Solution {
    public ListNode middleNode(ListNode head) {
        Vector  v = new Vector ();
        while(head != null){
            v.addElement(head);
            head = head.next;
        }
        return (ListNode)v.get(v.size()/2);
    }
}
```

#### 思路二：快慢指针

&emsp;&emsp;由于中间节点的特性是整个链表长度的一半，也就是说，设定两个指针pre(指向中间节点)和p(指向尾节点)，当pre每往后移动一个节点，p就往后移动两个节点。

&emsp;&emsp;但是这样会出现p没法往后移动两个节点的情况，比如只能移动一个节点，由于题意有两个中间结点，则返回第二个中间结点。那么我们先移动pre，再移动p，一旦p的下一个指针是空指针，则返回pre，这样就可以保证pre所在的位置等于length/2.

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode pre = head, p = head;
        while(p.next != null){
            pre = pre.next;
            p = p.next;
            if(p.next == null)  return pre;
            p = p.next;
        }
        return pre;
    }
}
```

