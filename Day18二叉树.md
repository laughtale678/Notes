## Day18二叉树

#### [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

给定一个二叉树的 **根节点** `root`，请找出该二叉树的 **最底层 最左边** 节点的值。

假设二叉树中至少有一个节点。

```java
迭代：自己的方法太崩了，还用list储存下来，看题解发现只要每次更新当前level的第一个值就行了
  
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        TreeNode res = new TreeNode();
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                TreeNode temp = queue.poll();
                if(i == 0) {
                    res = temp;//每次更新为最左侧第一个值，最后一次循环结束就是最后一层最左侧
                }
                if(temp.left != null) queue.offer(temp.left);
                if(temp.right != null) queue.offer(temp.right);
            }
        }
        return res.val;   
    }
}
```

```java
递归：写不出来，要注意为什么有些变量定义为成员变量，如果将这两个变量都作为成员变量定义在类中，那么在递归函数中可以直接访问和更新它们的值，无需传递参数。每次递归调用时，可以比较当前节点的深度与最大深度，如果更深则更新最大深度和最左节点的值。不然在递归传递中，值的传递不正确会导致结果错误。
  时间：o(n)
  空间：o(n)
  class Solution {
    int res;
    int depthPre = -1;
    public int findBottomLeftValue(TreeNode root) {
        dfs(root, 0);
        return res;    
    }

    public void dfs(TreeNode node, int depthCur) {
        if(node == null) return;
        if(node.left == null && node.right == null) {
            if(depthCur > depthPre) {
                depthPre = depthCur;
                res = node.val; 
            }  
        }
        dfs(node.left, depthCur + 1);
        dfs(node.right, depthCur + 1);
    }
}
```

#### [112. 路径总和](https://leetcode.cn/problems/path-sum/)

给你二叉树的根节点 root 和一个表示目标和的整数 targetSum 。判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。如果存在，返回 true ；否则，返回 false 。

叶子节点 是指没有子节点的节点。



```java
递归：自己的方法，第二个if的位置容易放错，一开始放在sum更新上面就错了，要等sum更新了再判断
 时间：o(n)
 空间：o(n) 
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        return dfs(root, 0, targetSum); 
    }

    public boolean dfs(TreeNode node, int sum, int targetSum) {
        if(node == null) return false;
        sum += node.val;
        if(node.left == null && node.right == null && sum == targetSum) return true;
        boolean left = dfs(node.left, sum, targetSum);
        boolean right = dfs(node.right, sum, targetSum);
        return left || right;
    }
}
```

```java
递归：题解，转化：是否存在从当前节点的子节点到叶子的路径，满足其路径和为 sum - val。
 时间：o(n)
 空间：o(n) 
  class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        if(root.left == null && root.right == null) {
            return targetSum == root.val;
        }
        boolean left = hasPathSum(root.left, targetSum - root.val);
        boolean right = hasPathSum(root.right, targetSum - root.val);
        return left || right;
    }
}
```

```java
迭代:用一个Stack遍历的同时，用另外一个Stack储存遍历到当前节点时候的sum，然后每次出栈检查是否符合题目要求即可。
 时间：o(n)
 空间：o(n)
  class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        Deque<TreeNode> stack1 = new ArrayDeque<>();
        Deque<Integer> stack2 = new ArrayDeque<>();
        stack1.push(root);
        stack2.push(root.val);
        while(!stack1.isEmpty()) {
            TreeNode temp = stack1.pop();
            int sum = stack2.pop();
            if(temp.left == null && temp.right == null && sum == targetSum) {
                return true;
            }
            if(temp.left != null) {
                stack1.push(temp.left);
                stack2.push(sum + temp.left.val);
            }
            if(temp.right != null) {
                stack1.push(temp.right);
                stack2.push(sum + temp.right.val);
            }
        }
        return false;
    }
}
```

#### [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

```java
递归：
时间：o(n)
空间：o(n)  
class Solution {
    List<List<Integer>> res = new LinkedList();
    List<Integer> list = new LinkedList();
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        dfs(root, targetSum);
        return res; 
    }

    public void dfs(TreeNode node, int sum) {
        if(node == null) return;
        list.add(node.val);
        if(node.left == null && node.right == null) {
            if(sum == node.val) {
                res.add(new ArrayList(list));
            }
        }
        dfs(node.left, sum - node.val);
        dfs(node.right, sum - node.val);
        list.remove(list.size() - 1);
    }
}
```

#### [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

给定两个整数数组 inorder 和 postorder ，其中 inorder 是二叉树的中序遍历， postorder 是同一棵树的后序遍历，请你构造并返回这颗 二叉树 。

```java
class Solution { 
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder.length != postorder.length) return null;
        Map<Integer, Integer> map = new HashMap();
        for(int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        return build(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1, map); 
    }

    public TreeNode build(int[] inorder, int is, int ie, int[] postorder, int ps, int pe, Map<Integer, Integer> map) {
        if(is > ie || ps > pe) return null;
        TreeNode node = new TreeNode(postorder[pe]);
        int root = map.get(postorder[pe]);//find index
        int leftSize = root - is;
        node.left = build(inorder, is, root - 1, postorder, ps, ps + leftSize - 1, map);
        node.right = build(inorder,root + 1, ie, postorder, ps + leftSize, pe - 1, map);
        return node;
    }

}
```

#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length != inorder.length) return null;
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        return build(preorder, 0 ,preorder.length - 1, inorder, 0, inorder.length - 1, map);    
    }

    public TreeNode build(int[] preorder,int ps, int pe, int[] inorder, int is, int ie, Map<Integer, Integer> map) {
        if(ps > pe || is > ie) return null;
        TreeNode node = new TreeNode(preorder[ps]);
        int root = map.get(preorder[ps]);
        int leftLength = root - is;
        node.left = build(preorder, ps+1, ps + leftLength, inorder, is, root -1, map);
        node.right = build(preorder,ps + leftLength + 1, pe, inorder, root + 1, ie,  map);
        return node;
    }
}
```

