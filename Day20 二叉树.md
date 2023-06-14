Day20 二叉树

#### [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

给定一个不重复的整数数组 nums 。 最大二叉树 可以用下面的算法从 nums 递归地构建:

创建一个根节点，其值为 nums 中的最大值。
递归地在最大值 左边 的 子数组前缀上 构建左子树。
递归地在最大值 右边 的 子数组后缀上 构建右子树。
返回 nums 构建的 最大二叉树 。

```java
递归：
  时间：o(n^2)
  空间：o(n)
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return build(nums, 0, nums.length - 1);   
    }

    public TreeNode build(int[] nums,int left, int right) {
        if(left > right) return null;
        int max = left;//max的值很容易错，写成0或者负数都不对，因为left,right是边界，不然会越界
        for(int i = left; i <= right; i++) {
            if(nums[i] > nums[max]) {
                max = i;
            }
        }
        TreeNode node = new TreeNode(nums[max]);
        node.left = build(nums, left, max - 1);
        node.right = build(nums, max + 1, right);
        return node;
    }
}
```

#### [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

给你两棵二叉树： root1 和 root2 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

注意: 合并过程必须从两个树的根节点开始。

```java
递归：自己写的比较复杂，因为想不到单独拆开写base case
  时间：o(n)
  空间：o(n)
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1 == null && root2 == null) return null;
        TreeNode node = new TreeNode();
        if(root1 == null && root2 != null) {
            node.val = root2.val;
            node.left = mergeTrees(null, root2.left);
            node.right = mergeTrees(null, root2.right);
        }else if(root1 != null && root2 == null) {
            node.val = root1.val;
            node.left = mergeTrees(root1.left, null);
            node.right = mergeTrees(root1.right, null);
        }else {
            node.val = root1.val + root2.val;
            node.left = mergeTrees(root1.left, root2.left);
            node.right = mergeTrees(root1.right, root2.right);
        }
        return node;
    }
}
```

```java
递归优化：
  时间：o(n)
  空间：o(n)
 class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1 == null && root2 == null) return null;
        if(root1 == null) return root2;
        if(root2 == null) return root1;
        TreeNode node = new TreeNode(root1.val + root2.val);
        node.left = mergeTrees(root1.left, root2.left);
        node.right = mergeTrees(root1.right, root2.right);
        return node;
    }
}
```

#### [700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

给定二叉搜索树（BST）的根节点 root 和一个整数值 val。

你需要在 BST 中找到节点值等于 val 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 null 。

```java
递归：二叉搜索树左边小于根值右边大于根值的特征
  时间：o(n)
  空间：o(n)
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null) return null;
        if(root.val == val) return root;
        if(val < root.val) return searchBST(root.left, val);
        if(val > root.val) return searchBST(root.right, val);
        return null;
    }
}
```

```java
迭代：利用二叉搜索树特性
 时间：o(n)
 空间：o(n)
  class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        TreeNode node = root;
        while(node != null) {
            if(node.val == val) {
                return node;
            }else if(val < node.val) {
                node = node.left;
            }else {
                node = node.right;
            }
        }
        return null;
    }
}
```

#### [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。

有效 二叉搜索树定义如下：

节点的左子树只包含 小于 当前节点的数。
节点的右子树只包含 大于 当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

```java
递归：自己的思路，中序遍历看list是不是递增的,第一次做掉入了陷阱，只比较了每个子树的左节点值右节点值跟根值，实际上要左树，右树通盘考虑。
  时间：o(N)
  空间：O(N)
class Solution {
    public boolean isValidBST(TreeNode root) {
        List<Integer> list = new ArrayList();
        dfs(list, root);
        for(int i = 0; i < list.size() - 1; i++) {
            if(list.get(i) >= list.get(i + 1)) return false;
        }
        return true;  
    }
    public void dfs(List<Integer> list, TreeNode root) {
        if(root == null) return;
        dfs(list, root.left);
        list.add(root.val);
        dfs(list, root.right);
    }
}
优化：在递归中直接完成比较
  class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;;
        boolean left = isValidBST(root.left);
        if(root.val <= pre) return false;
        pre = root.val;
        boolean right = isValidBST(root.right);
        return left && right; 
    }
}
```

```java
迭代：
 时间：o(N)
 空间：O(N) 
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode temp = root;
        long pre = Long.MIN_VALUE;
        while(!stack.isEmpty() || temp != null) {
            while(temp != null) {
                stack.push(temp);
                temp = temp.left;
            }
            temp = stack.pop();
            if(pre >= temp.val) return false;
            pre = temp.val;//更新pre的值
            temp = temp.right;
        }
        return true;
    }
}
```

