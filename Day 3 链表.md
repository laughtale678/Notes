## Day 3 链表

### [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

时间复杂度：O(n)

空间复杂度：O(1)

Iteration

```java
单个指针
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode sentinel = new ListNode();//用虚拟头节点，这样的话每个节点的移除逻辑保持一致，始终保持指针节点指向目标前一个
                                           //这样便于操作删除目标节点。
        sentinel.next = head;
        ListNode p = sentinel;
        while(p.next != null) {
            if(p.next.val == val) {
                p.next = p.next.next;
            }else {
                p = p.next;//一开始写错了，没有用else，因为当p指针才做删除后面节点之后，p.next已经更新了，此时不用再次更新
                           //p = p.next,如果这样的话就跳过了一个节点，而且会导致空指针问题。因为这种情况下，最多就是把最后
                           //一位删除后p.next == null,循环就结束了，如果不加else的话，删除后p还要后移一位，可能直接p到了
                           //null的位置，此时进循环条件判断就会出现空指针异常报错。
            }
        }
        return sentinel.next;
    }
}
```

```java
双指针
  class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode sentinel = new ListNode();
        sentinel.next = head;
        ListNode pre = sentinel;
        ListNode cur = head;
        while(cur != null) {
            if(cur.val == val) {
                pre.next = pre.next.next;
            }else {
                pre = pre.next;//指针的移动时机特别容易错，每次都不记得加else，因为移除之后pre的next自动已经更新了，cur是永远                                                                                                                                                            正常移动
            }
            cur = cur.next;
        }
        return sentinel.next;
    }
}
```

### [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

你可以选择使用单链表或者双链表，设计并实现自己的链表。

单链表中的节点应该具备两个属性：val 和 next 。val 是当前节点的值，next 是指向下一个节点的指针/引用。

如果是双向链表，则还需要属性 prev 以指示链表中的上一个节点。假设链表中的所有节点下标从 0 开始。

实现 MyLinkedList 类：

MyLinkedList() 初始化 MyLinkedList 对象。
int get(int index) 获取链表中下标为 index 的节点的值。如果下标无效，则返回 -1 。
void addAtHead(int val) 将一个值为 val 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
void addAtTail(int val) 将一个值为 val 的节点追加到链表中作为链表的最后一个元素。
void addAtIndex(int index, int val) 将一个值为 val 的节点插入到链表中下标为 index 的节点之前。如果 index 等于链表的长度，那么该节点会被追加到链表的末尾。如果 index 比长度更大，该节点将 不会插入 到链表中。
void deleteAtIndex(int index) 如果下标有效，则删除链表中下标为 index 的节点。

```java
class ListNode {
        int val;
        ListNode next;
        public ListNode(){

        }
        public ListNode(int val) {
            this.val = val;
        }
        public ListNode(int val, ListNode next) {
            this.val =val;
            this.next = next;
        }
    }

class MyLinkedList {

    ListNode sentinel; //用了虚拟头节点
    int size;//不能忘记增删的时候要调整size

    public MyLinkedList() {
        sentinel = new ListNode();
        size = 0;
    }
    
    public int get(int index) {
        ListNode p = sentinel;
        if(index < 0 || index > size - 1) return -1;
        for(int i = 0; i <= index; i++) {
            p = p.next;
        }
        return p.val;
    }
    
    public void addAtHead(int val) {
        ListNode temp = new ListNode(val);
        temp.next = sentinel.next;
        sentinel.next = temp;
        size++;
    }
    
    public void addAtTail(int val) {
        ListNode p = sentinel;
        while(p.next != null) {
            p = p.next;
        }
        p.next = new ListNode(val);
        size++;
    }
    
    public void addAtIndex(int index, int val) {
        ListNode p = sentinel;
        if(index > size) return;//根据题目要求index可以等于size，插入尾部
        for(int i = 0; i < index; i++) {//index < 0时p停留在虚拟头节点不移动，添加至头部
            p = p.next;
        }
        ListNode temp = new ListNode(val);
        temp.next = p.next;
        p.next = temp;
        size++;
    }
    
    public void deleteAtIndex(int index) {
        ListNode p = sentinel;
        if(index >= size || index < 0) return;
        for(int i = 0; i < index; i++) {
            p = p.next;
        }
        p.next = p.next.next;
        size--;
    }
}
```

### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

时间复杂度：O(n)

空间复杂度：O(1)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null) {
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```

