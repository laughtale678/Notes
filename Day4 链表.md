

## Day4 链表

### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

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
    public ListNode swapPairs(ListNode head) {
        ListNode sentinel = new ListNode();
        sentinel.next = head;
        ListNode pointer = sentinel;//对于单链表，要操作的两个节点，有指针在他们前一个节点才能实现
        while(pointer.next != null && pointer.next.next != null) {//指针后面如果没有或只有一个元素结束
            ListNode temp = pointer.next;  // 保存temp node即1节点
            pointer.next = pointer.next.next;//虚拟头指向2节点
            temp.next = temp.next.next;//第1节点指向3节点
            pointer.next.next = temp;//原12节点换位
            pointer = pointer.next.next;//指针跳转到更新后的2节点位置即可继续操作后面两位
        }
        return sentinel.next;
    }
}
```

```java
recursion
  class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode sentinel = new ListNode();
        sentinel.next = head;
        swap(sentinel);
        return sentinel.next;
    }
    private void swap(ListNode node) {
        if(node.next == null || node.next.next == null) {
            return;
        }
        ListNode temp = node.next;
        node.next = node.next.next;
        temp.next = temp.next.next;
        node.next.next = temp;
        swap(node.next.next);
    }
}
  
```

### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

```java
Two Pointer:快指针移动到最后一个节点时，慢指针跟随移动到最后第n+1个位置
时间复杂度: O(n)
空间复杂度: O(1)
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode sentinel = new ListNode();
        sentinel.next = head;
        ListNode fast = sentinel;
        ListNode slow = sentinel;
        int count = 0;
        while(fast.next != null) {
            if(count == n) {//保持fast跟slow之前距离是n，这样fast移动到最后一个node时，slow就在最后第n+1个node，正好操作最后第n个nodenode，有点抽象，得画画
                slow = slow.next;
                count--;//很容易遗漏
            }
            fast = fast.next;
            count++;
        }
        slow.next = slow.next.next;
        return sentinel.next;
    }
}

代码随想录中是把fast先移动到指定位置，然后两者一起移动，这样可能更加清晰一些，不用灵活调整两者之间距离。
  class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode sentinel = new ListNode();
        sentinel.next = head;
        ListNode fast = sentinel;
        ListNode slow = sentinel;
        for(int i = 0; i < n; i++) {
            fast = fast.next;
        }
        while(fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return sentinel.next;
    }
}
```

```java
Brute Force先算出size之后，然后移动指针到最后第n+1个位置进行操作即可，倒数第n个就是移除正数size - n + 1个，就只要移动到size - n的位置直接删除下一个就可以了
时间复杂度: O(n)
空间复杂度: O(1)
  class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode sentinel = new ListNode();
        sentinel.next = head;
        ListNode pointer = sentinel;
        int size = size(head);
        for(int i = 0; i < size - n; i++) {
            pointer = pointer.next;
        }
        pointer.next = pointer.next.next;
        return sentinel.next;
     
    }
    private int size(ListNode node) {
        int res = 0;
        ListNode dummy = new ListNode();
        dummy.next = node;
        ListNode p = dummy;
        while(p.next != null) {
            p = p.next;
            res++;
        }
        return res;
    }
}
```

```java
官方题解stack也很巧妙，就是要创建stack消耗空间
时间复杂度: O(n)
空间复杂度: O(n)
  class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        Stack<ListNode> stack = new Stack();
        ListNode sentinel = new ListNode();
        sentinel.next = head;
        ListNode p = sentinel;
        while(p != null) {//要把虚拟头加入，这样才能方便操作删除第一个node,这里不用p.next -- null,因为最后一个p也要入栈
            stack.push(p);
            p = p.next;
        }
        for(int i = 0; i < n; i++) {
            stack.pop();
        }
        ListNode pre = stack.peek();
        pre.next = pre.next.next;
        return sentinel.next;
    }
}
```

### [面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 `null` 。

```java
Brute Force:分别遍历两个链表并用set储存，因为给定两链表无循环，所以链表中不会有相同node，遍历到第一个相交节点时set无法添加该节点，所以返回
时间：O(n)
空间：O(n)
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode pointerA = headA;
        ListNode pointerB = headB;
        Set<ListNode> set = new HashSet();
        while(pointerA != null) {
            set.add(pointerA);
            pointerA = pointerA.next;
        }
        while(pointerB != null) {
            if(!set.add(pointerB)) {
                return pointerB;
            }
            pointerB = pointerB.next;
        }
        return null;
    }
}
```

```java
Two Pointer:
时间：O(n)
空间：O(1)
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
       ListNode pointerA = headA;
       ListNode pointerB = headB;
       while(pointerA != pointerB) {//两个节点一个A开始一个B开始遍历两条链表，因为遍历长度均为A+BB，因此，一定会相等，没有交叉点的话就是都等于null
           if(pointerA == null) {
               pointerA = headB;
           }else {
               pointerA = pointerA.next;//注意：指针移动如果不写在else里面，会导致两者移动速度不一致，结果出错，写在else里面就等于两个指针每次都移动一步，如果写在外面的话，pointerA = headB，pointerA = pointerA.next等于移动了两次。
           }
           if(pointerB == null) {
               pointerB = headA;
           }else {
                pointerB = pointerB.next;

           }
       }
       if(pointerA != null) {
           return pointerA;
       }
       return null;
    }
}


```

### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

```java
用HashSet遍历
时间复杂度:O(n)
空间复杂度:O(n)
public class Solution {
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> set = new HashSet();
        ListNode p = head;
        while(p != null) {
            if(!set.add(p)){
                return p;
            }
            p = p.next;
        }
        return null;  
    }
}
```

```java
Two Pointer:快慢指针法
时间复杂度:O(n)
空间复杂度:O(1)
  public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow) {//如果有环一定相遇
                ListNode p1 = head;
                ListNode p2 = slow;
                while(p1 != p2) {//如果相遇了，从相遇点到入环点一等等于从头出发到入环点
                    p1 = p1.next;
                    p2 = p2.next;
                }
            return p1; 
            }
        }
        return null;   
    }
}
 
```

