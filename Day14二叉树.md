## Day14二叉树

#### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

```java
递归：
时间：O(n),其中 n 是二叉树的节点数。每一个节点恰好被遍历一次。
空间：O(n),为递归过程中栈的开销，平均情况下为O(logn)，最坏情况下树呈现链状，为O(n)。
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        traverse(root, res);
        return res;
    }
    public void traverse(TreeNode root, List<Integer> list) {
        if(root == null) return;
        list.add(root.val);
        traverse(root.left, list);
        traverse(root.right, list);
    }
}
```

```java
迭代：相对其他两个容易理解一些，每访问一个节点先处理，再把右节点入栈左节点入栈，下次循环，弹出左节点，指针指向它，重复之前操作
时间：O(n)
空间：O(n)
  方法一：等于用栈排好遍历顺序，弹出一个处理一个
  class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new ArrayDeque();
        List<Integer> res = new LinkedList();
        if(root == null) return res;
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode temp = stack.pop();
            res.add(temp.val);
            if(temp.right != null) {
                stack.push(temp.right);//入栈顺序要跟遍历顺序相反
            }
            if(temp.left != null) {
                stack.push(temp.left);
            }   
        }
        return res;
    }
}
  
```

#### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

给你一棵二叉树的根节点 `root` ，返回其节点值的 **后序遍历** 。

```java
递归：
  时间：O(n)
  空间：O(n)
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        traversal(root, res);
        return res;
    }
    private void traversal(TreeNode root, List<Integer> list) {
        if(root == null) return;
        traversal(root.left, list);
        traversal(root.right, list);
        list.add(root.val);
    }
}
```

```java
迭代：
  时间：O(n)
  空间：O(n)
class Solution {//中左右调整顺序-》中右左 结果翻转-〉左右中
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList();
        if(root == null) return res;
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode temp = root;
        stack.push(root);
        while(!stack.isEmpty()) {
            temp = stack.pop();
            res.add(temp.val);
            if(temp.left != null ) {
                stack.push(temp.left);
            }
            if(temp.right != null) {
                stack.push(temp.right);
            }   
        }
        Collections.reverse(res);
        return res;
    }
}
```

#### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

```java
递归：
  时间：O(n)
  空间：O(n)
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        traversal(root, res);
        return res;
    }

    private void traversal(TreeNode root, List<Integer> list) {
        if(root == null) return;
        traversal(root.left, list);
        list.add(root.val);
        traversal(root.right, list);
    }
}
```

```java
迭代：Stack不空，指针不空就进入循环，循环内容：当前指针入栈并更新为他的左节点直至全部入栈，因为要从左节点开始处理（内部循环）。如果内部循环没有执行说明指针已经移动到当前节点最左侧的null节点，null不用入栈。此时弹出节点并把指针指向它，处理当前节点（就是最后一个tree的中间值），然后再讲指针指向它右侧，进行下一轮循环。符合左-中-右
  时间：O(n)
  空间：O(n)
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) return res;
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode temp = root;
        while(!stack.isEmpty() || temp != null) {//循环条件我觉得不容易写出来
            while(temp != null) {
                stack.push(temp);
                temp = temp.left;
            }
            temp = stack.pop();
            res.add(temp.val);
            temp = temp.right;
        }
        return res;
    }
}
```



