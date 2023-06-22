## Day27回溯

#### [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。

candidates 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 target 的不同组合数少于 150 个。

```java
未剪枝
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        backtracking(candidates, target, 0, 0);
        return res;  
    }

    public void backtracking(int[] candidates, int target, int sum, int start) {
        if(sum > target) return;
        if(sum == target) {
            res.add(new ArrayList(list));
            return;
        }
        for(int i = start; i < candidates.length; i++) {
            list.add(candidates[i]);
            backtracking(candidates, target, sum + candidates[i], i);
            list.remove(list.size() - 1);
        }
    }
}
```

```java
剪枝，排序有点离谱，根本想不到。如果不排序就直接剪枝的话，会漏掉平行循环中的正确答案，比如8743，8+8比8+7要大，8+8不符合要求不代表8+7不符合，应该继续遍历，所以如果要在for循环中剪枝，就要排序，不然就老老实实用sum的值来控制
  class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);//排序
        backtracking(candidates, target, 0, 0);
        return res;  
    }

    public void backtracking(int[] candidates, int target, int sum, int start) {
        if(sum == target) {
            res.add(new ArrayList(list));
            return;
        }
        for(int i = start; i < candidates.length; i++) {
            if(sum + candidates[i] > target) break;//剪枝，提前终止本次循环
            list.add(candidates[i]);
            backtracking(candidates, target, sum + candidates[i], i);
            list.remove(list.size() - 1);
        }
    }
}
```

#### [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)

给定一个候选人编号的集合 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用 一次 。

注意：解集不能包含重复的组合。

```java
class Solution {
    List<List<Integer>> res = new ArrayList();
    List<Integer> list = new ArrayList();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtracking(candidates, target, 0, 0);
        return res;
    }
    public void backtracking(int[] candidates, int target, int start, int sum) {
        if(sum == target) {
            res.add(new ArrayList(list));
            return;
        }
        for(int i = start; i < candidates.length; i++) {
            if(sum + candidates[i] > target) break;
            if(i > start && candidates[i] == candidates[i - 1]) continue;//新增去重操作
            list.add(candidates[i]);
            backtracking(candidates, target, i + 1, sum + candidates[i]);//递归中start总写成start+1，理解不深
            list.remove(list.size() - 1);
        }
    }
}
```

#### [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    List<String> list = new ArrayList<>();
    public List<List<String>> partition(String s) {
        backtracking(s, 0);
        return res;
    }

    public void backtracking(String s, int start) {
        if(start == s.length()) {
            res.add(new ArrayList(list));
            return;
        }
        for(int i = start; i < s.length(); i++) {
            String tmp = s.substring(start, i + 1);//右边界不包含，所以+1
            if(isPalindrome(tmp)) {//如果不满足回文串就没必要继续递归了
                list.add(tmp);
                backtracking(s, i + 1);//容易写成start+1，这样会导致横向移动时候不是从当前节点向后，而是每次都从start向后
                list.remove(list.size() - 1);
            }
        }
    }

    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        while(left < right) {
            if(s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

