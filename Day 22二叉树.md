## Day 22二叉树

#### [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

```java
迭代：
  时间：o(longn)非平衡状态下最差情况也可能是 O(n)
  空间：o(1)
  class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode res = root;
        while(res != null) {
            if(res.val > p.val && res.val > q.val) {
                res = res.left;
            }else if(res.val < p.val && res.val < q.val) {
                res = res.right;
            } else {
                return res;
            }
        } 
        return res;  
    }
}
```

```java
递归：
  时间：o(longn)非平衡状态下最差情况也可能是 O(n)
  空间：o(longn)非平衡状态下最差情况也可能是 O(n)
  class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;
        if(root.val > p.val && root.val > q.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        if(root.val < p.val && root.val < q.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        return root;
    }
}
```

#### [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

给定二叉搜索树（BST）的根节点 root 和要插入树中的值 value ，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。

```java
迭代：比较直观一些
  时间：平衡情况O(log n)，最坏情况o(n)
  空间：o(1)
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) return new TreeNode(val);
        TreeNode cur = root;
        while(cur != null) {
            if(cur.val > val) {
                if(cur.left == null) {
                    cur.left = new TreeNode(val);
                    break;
                }else {
                    cur = cur.left;
                }
            }else {
                if(cur.right == null) {
                    cur.right = new TreeNode(val);
                    break;
                }else {
                    cur = cur.right;
                }
            }
        }
        return root;  
    }
}
```

```java
递归：有返回值的递归还是很难写对，一开始写的helper，没有返回值之后无法返回正确答案因为没有正确理解方法中参数传递机制。
  时间：平衡情况O(log n)，最坏情况o(n)
  空间：平衡情况O(log n)，最坏情况o(n) 
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) {
            return new TreeNode(val);  
        }
        if(root.val > val) {
           root.left = insertIntoBST(root.left, val);
        }else {
            root.right = insertIntoBST(root.right, val);
        }
        return root;
    }
}
```

#### [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

首先找到需要删除的节点；
如果找到了，删除它。

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null) return null;
        if(root.val > key) {
            root.left = deleteNode(root.left, key);
        }else if (root.val < key) {
            root.right = deleteNode(root.right, key);
        }else {//如果root.val== key
            if(root.left == null && root.right == null) {
                return null;
            }else if(root.left != null && root.right == null) {
                return root.left;
            }else if(root.left == null && root.right != null) {
                return root.right;
            }else {
                TreeNode leftTree = root.left;//这边的处理逻辑是删除后接上右子树，左子树变成右子树的最左子树
                TreeNode rightTree = root.right;
                while(rightTree.left != null) {
                    rightTree = rightTree.left;
                }
                rightTree.left = leftTree;
                return root.right;
            }
        }
        return root;   
    }
}
```

