## Day24回溯算法

#### [77. 组合](https://leetcode.cn/problems/combinations/)

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

```java
回溯
  时间：o(n*k) for循环跟递归
  空间：O(n+k)=O(n)，即递归使用栈空间的空间代价和临时数组 
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k , 1);
        return res;
    }

    public void backtracking(int n, int k, int start) {//递归函数的返回值以及参数
        if(list.size() == k) {//回溯函数终止条件
            res.add(new ArrayList(list));
            return;
        }
        for(int i = start; i <= n; i++) {//单层搜索的过程
            list.add(i);
            backtracking(n, k, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```

```java
剪枝
  class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k , 1);
        return res;
    }

    public void backtracking(int n, int k, int start) {//递归函数的返回值以及参数
        if(list.size() == k) {//回溯函数终止条件
            res.add(new ArrayList(list));
            return;
        }
        for(int i = start; i <= n - (k - list.size()) + 1; i++) {//单层搜索的过程,k - list.size()表示还差几个元素，最多找到这个位置即可，因为大于这个位置后面不可能有正确答案
            list.add(i);
            backtracking(n, k, i + 1);
            list.remove(list.size() - 1);
        }
    }
```

