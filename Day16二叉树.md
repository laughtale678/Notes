## Day16二叉树

#### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

```java
递归：受到了昨天最小深度的影响，自己写成这样了
时间：O(N)
空间：O(N)
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        if(root.left == null && root.right == null) return 1;
        if(root.left == null) return maxDepth(root.right) + 1;
        if(root.right == null) return maxDepth(root.left) + 1;
        return Math.max(maxDepth(root.right) + 1, maxDepth(root.left) + 1);
    }
}
优化：
  class solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

```java
时间：O(N)
空间：O(N)
class Solution {
    public int maxDepth(TreeNode root) {
        int res = 0;
        if(root == null) return res;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                TreeNode temp = queue.poll();
                if(temp.left != null) queue.offer(temp.left);
                if(temp.right != null) queue.offer(temp.right);
            }
            res++;
        }
        return res;
    }
}
```

#### [559. N 叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)

给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

N 叉树输入按层序遍历序列化表示，每组子节点由空值分隔（请参见示例）。

```java
迭代：
  时间：O(n)
  空间：O(n)
  class Solution {
    public int maxDepth(Node root) {
        int res = 0;
        if(root == null) return res;
        Queue<Node> queue = new ArrayDeque<>();
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                Node temp = queue.poll();
                if(temp.children != null) {
                    for(Node n : temp.children) {
                        queue.offer(n);
                    }
                }
            }
            res++;
        }
        return res;
    }
}
  
```

```java
递归：
  时间：O(n)
  空间：o(n)
  class Solution {
    public int maxDepth(Node root) {
        int max = 0;
        if(root == null) return max;
        for(Node n : root.children) {
            max = Math.max(max, maxDepth(n));
        }
        return max + 1;
    }
}
```

#### [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明：**叶子节点是指没有子节点的节点

```java
递归：
  时间：O(n)
  空间：o(n)
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        if(root.left == null && root.right == null) return 1;
        if(root.left == null) return minDepth(root.right) + 1;;
        if(root.right == null) return minDepth(root.left) + 1;;
        return Math.min(minDepth(root.right),minDepth(root.left)) + 1;
    }
}
```

```
迭代：第一次碰到叶子节点就返回所在深度
  时间：O(n)
  空间：o(n)
class Solution {
    public int minDepth(TreeNode root) {
       int depth = 0;
       if(root == null) return depth;
       Queue<TreeNode> queue = new ArrayDeque<>();
       queue.offer(root);
       while(!queue.isEmpty()) {
           depth++;
           int size = queue.size();
           for(int i = 0; i < size; i++) {
               TreeNode temp = queue.poll();
               if(temp.right == null && temp.left == null) return depth;
               if(temp.left != null) queue.offer(temp.left);
               if(temp.right != null) queue.offer(temp.right);
           }
       }
       return depth;
    }
}

```

#### [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

给你一棵 完全二叉树 的根节点 root ，求出该树的节点个数。

完全二叉树 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

```java
迭代：比较直观
时间：O(n)
空间：o(n)
class Solution {
    public int countNodes(TreeNode root) {
        int count = 0;
        if(root == null) return count;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                TreeNode temp = queue.poll();
                count++;//出来一个算一个
                if(temp.left != null) queue.offer(temp.left);
                if(temp.right != null) queue.offer(temp.right);
            }
        }
        return count;
    }
}
```

```java
```

