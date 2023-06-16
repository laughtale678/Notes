## Day21 二叉树

#### [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

```java
递归：自己写的笨办法，感觉时间空间效率不高，但是没写出能在遍历二叉树同时完成运算的代码
 时间O(N)
 空间O(N)
  class Solution {
    public int getMinimumDifference(TreeNode root) {
        Deque<Integer> stack = new ArrayDeque();
        rereturn dfs(root, stack, Integer>MAX_VALUE);
    }
    public void dfs(TreeNode node, Deque<Integer> stack,int min) {
        if(node == null) return;
        dfs(node.left);
        if(!stack.isEmpty()) {
            min = Math.min(min, Math.abs(node.val - stack.peek()));
        }
        stack.push(node.val);
        dfs(node.right);
        return min;
    }
}
```

```java
递归：优化，双指针递归同步运算
 时间O(N)
 空间O(N)
  class Solution {
    int res = Integer.MAX_VALUE;
    TreeNode pre = null;
    public int getMinimumDifference(TreeNode root) {
        dfs(root);
        return res;
    }

    public void dfs(TreeNode node) {
        if(node == null) return;
        dfs(node.left);
        if(pre != null) {
            res = Math.min(res, node.val - pre.val);//递增就不用绝对值了
        }
        pre = node;
        dfs(node.right);
    }
}
```

#### [501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

给你一个含重复值的二叉搜索树（BST）的根节点 root ，找出并返回 BST 中的所有 众数（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 任意顺序 返回。

假定 BST 满足如下定义：

结点左子树中所含节点的值 小于等于 当前节点的值
结点右子树中所含节点的值 大于等于 当前节点的值
左子树和右子树都是二叉搜索树

```java
Brute Force:没有利用二叉搜索树特性，感觉很复杂，容易写错，用map存储次数后找出对大次数元素
  时间：O(n)
  空间：O(n)
class Solution {
    Map<Integer, Integer> map = new HashMap();
    public int[] findMode(TreeNode root) {
        List<Integer> list = new ArrayList();
        dfs(root);
        int max = 0;
        for(int i : map.values()) {
            max = Math.max(max, i);
        }
        for(int j : map.keySet()) {
            if(map.get(j) == max) {
                list.add(j);
            }
        }
        int[] res = new int[list.size()];
        for(int i = 0; i < res.length; i++) {
            res[i] = list.get(i);
        }
        return res;   
    }

    public void dfs(TreeNode node) {
        if(node == null) return;
        dfs(node.left);
        map.put(node.val, map.getOrDefault(node.val, 0) + 1);
        dfs(node.right);
    }

}
```

```java
优化：递归中完成。中间值的处理太难写出来了，估计下次还是写不出来。
  时间：O(n)
  空间：O(n)
  class Solution {
    int count = 1;
    int max = 0;
    TreeNode pre = null;
    List<Integer> list = new ArrayList();
    public int[] findMode(TreeNode root) {
        dfs(root);
        int[] res = new int[list.size()];
        for(int i = 0; i < res.length; i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    public void dfs(TreeNode node) {
        if(node == null) return;
        dfs(node.left);
        if (pre != null && node.val == pre.val) {//先处理计数
            count++;
        } else {
            count = 1;
        }
        if (count > max) {//再处理结果，计数大于max的时候就要更新max并且清空list重新添加，这里得添加node的值
            max = count;
            list.clear();
            list.add(node.val);
        } else if (count == max) {
            list.add(node.val);
        }
        pre = node;
        dfs(node.right);
    }
}
```

#### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

```java
递归：最原始的思路是两重递归，用辅助方法判断子树是否含有pq，然后主方法后序遍历从下至少判断每个子节点是否符合题目条件，但是写不出来。这个方法的base case跟头节点处理我不太会，而且带返回值的递归一般都不会写，还要多加练习。
时间：O(n)
空间：O(n)  
class Solution {
    int count = 0;
    TreeNode res = null;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return null;
        if(root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left != null && right != null) return root;
        if(left == null) return right;
        return left;
    }
}
```

