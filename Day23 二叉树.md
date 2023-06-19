## Day23 二叉树

#### [669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)

给你二叉搜索树的根节点 root ，同时给定最小边界low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在[low, high]中。修剪树 不应该 改变保留在树中的元素的相对结构 (即，如果没有被移除，原有的父代子代关系都应当保留)。 可以证明，存在 唯一的答案 。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

```java
递归：
  时间：o(n)
  空间：o(n)
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if(root == null) return null;
        if(root.val < low) {//说明该结点及它的左子树都不符合要求，但是当前节点的右节点可能存在符合条件的节点，所以要对右节点剪枝。
            return trimBST(root.right, low, high);//删除当前节点并返回其剪枝后的右节点。
        }
        if(root.val > high) {
            return trimBST(root.left, low, high);//删除当前节点并返回其剪枝后的左节点。
        }
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```

#### [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。

高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

```java
递归：
  时间：o(n)
  空间：O(log n)平衡的
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }
    
    public TreeNode build(int[] nums, int left, int right) {
        if(left > right) return null;
        int mid = left + (right - left) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = build(nums, left, mid - 1);
        root.right = build(nums, mid + 1, right);
        return root;
    }
}
```

#### [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。

```java
递归：自己写的笨方法，需要遍历两次
  
class Solution {
    List<Integer> list = new ArrayList<>();
    public TreeNode convertBST(TreeNode root) {
        dfs(root);
        convert(root);
        return root;   
    }
    public void dfs(TreeNode node) {
        if(node == null) return;
        list.add(node.val);
        dfs(node.left);
        dfs(node.right);
    }

    public int sum(int i, List<Integer> list) {
        int sum = 0;
        for(int number : list) {
            if(number >= i) {
                sum += number;
            }
        }
        return sum;
    }
    public void convert(TreeNode node) {
        if(node == null) return;
        node.val = sum(node.val, list);
        convert(node.left);
        convert(node.right);    
    }
}
```

```
递归优化：右中左遍历并依次更新sum
时间：o(n)
空间：O(n)最差情况
class Solution {
    int sum;
    public TreeNode convertBST(TreeNode root) {
        dfs(root);
        return root;
    }
    public void dfs(TreeNode node) {
        if(node == null) return;
        dfs(node.right);
        sum += node.val;
        node.val = sum;
        dfs(node.left);
    }
}
```

