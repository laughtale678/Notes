## Day17二叉树part4

#### [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1 。

```java
递归：自己写的，方法不是很好，属于自上而下的遍历，我感觉虽然height是自下而上，但是height本身不具备判断平衡能力，所以判断平衡还是自上而下重新判断，造成了很多重复
  时间：o(n^2),其中n 是二叉树中的节点个数。最坏情况下，二叉树是满二叉树，需要遍历二叉树中的所有节点，时间复杂度是 O(n)对于节点 p，如果它的高度是 d，则 height(p) 最多会被调用 d 次（即遍历到它的每一个祖先节点时）。对于平均的情况，一棵树的高度 h 满足 O(h)=O(logn)，因为 d≤h，所以总时间复杂度为 O(nlogn)。对于最坏的情况，二叉树形成链式结构，高度为 O(n)，此时总时间复杂度为 O(n 2 )。(官方答案分析)
  空间：o(n),其中n 是二叉树中的节点个数。空间复杂度主要取决于递归调用的层数，递归调用的层数不会超过(官方答案分析)
  class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        int left = height(root.left);
        int right = height(root.right);
        int balance = left - right;
        if(balance != 0 && balance != 1 && balance != -1) return false;
        return isBalanced(root.left) && isBalanced(root.right);
    }
    public int height(TreeNode node) {
        if(node == null) return 0;
        int left = height(node.left);
        int right = height(node.right);
        return Math.max(left,right) + 1;
    }
}
```

```java
时间：o(n)
空间：o(n)
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) == -1 ? false:true;  
    }

    public int height(TreeNode root) {//用-1做标记，后面即可直接判断出是否平衡
        if(root == null) return 0;
        int left = height(root.left);//从下往上遍历，遇到-1就说明已经不平衡了，所以碰到负一时候完成就会直接返回，表示不平衡。这个还挺难想到的
        if(left == -1) return -1;
        int right = height(root.right);
        if(right == -1) return -1;
        if(Math.abs(left - right) > 1) return -1;
        return Math.max(left, right) + 1;//
    }
```

#### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

```java
递归：要变通，base case不一定要写在最前面，base case一般作为方法出口，跟遍历的逻辑要清晰分开，不然容易出错
  时间：o(n)
  空间：o(n)
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        if(root == null) return res;
        dfs(root, res, "");
        return res;   
    }

    public void dfs(TreeNode node, List<String> list, String s) {
        s += node.val;//preorder
        if(node.left == null && node.right == null) {
            list.add(s);
            return;
        }
        if(node.left != null) dfs(node.left, list, s + "->");
        if(node.right != null) dfs(node.right, list, s + "->"); 
    }
}
```

#### [404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

给定二叉树的根节点 `root` ，返回所有左叶子之和。

```java
递归：代码中对左叶子节点的处理是在递归调用之前进行的，这使得它只会累加左子树中的左叶子节点的值，而不会累加右子树中的左叶子节点的值
  时间：o(n)
  空间：o(n)
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        int sum = 0;
        if(root == null) return sum;
        if(root.left != null) {
            if(root.left.left == null && root.left.right == null) {
                sum += root.left.val;
            } else {
                sum += sumOfLeftLeaves(root.left);
            }  
        }
        if(root.right != null) {
            sum += sumOfLeftLeaves(root.right);
        }
        return sum;
    }
}
```

