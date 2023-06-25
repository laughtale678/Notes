## Day29 回溯

#### [491. 递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)

给你一个整数数组 nums ，找出并返回所有该数组中不同的递增子序列，递增子序列中 至少有两个元素 。你可以按 任意顺序 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

```java
class Solution {
    Set<List<Integer>> res = new HashSet<>();//直接用set储存结果，虽然没有剪枝效率高，但是感觉会简单很多
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        dfs(nums, 0);
        return new ArrayList(res);  
    }

    public void dfs(int[] nums, int start) {
        if(list.size() >= 2) {
            res.add(new ArrayList(list));
        }
        for(int i = start; i < nums.length; i++) {
            if(!list.isEmpty() && list.get(list.size() - 1) > nums[i]) continue;
            list.add(nums[i]);
            dfs(nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```

```java
在dfs中用set去重，但是创建set的位置其实很难写对,太抽象了
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        dfs(nums, 0);
        return res;  
    }

    public void dfs(int[] nums, int start) {
        if(list.size() >= 2) {
            res.add(new ArrayList(list));
        }
        Set<Integer> set = new HashSet<>();
        for(int i = start; i < nums.length; i++) {
            if(!list.isEmpty() && list.get(list.size() - 1) > nums[i] || set.contains(nums[i])) continue;
            set.add(nums[i]);
            list.add(nums[i]);
            dfs(nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```

#### [46. 全排列](https://leetcode.cn/problems/permutations/)

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        dfs(nums);
        return res;
        
    }
    public void dfs(int[] nums) {
        if(list.size() == nums.length) {
            res.add(new ArrayList(list));//这边往来加return也过了，因为if(list.contains(nums[i]))剪枝逐步缩小了搜索范围
        }
        for(int i = 0; i < nums.length; i++) {
            if(list.contains(nums[i])) continue;
            list.add(nums[i]);
            dfs(nums);
            list.remove(list.size() - 1);
        }
    }
}
```

#### [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/)

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列

```java
比较直观的：结果集用set去重，used仅用于跳过已经使用过的元素
class Solution {
    Set<List<Integer>> res = new HashSet<>();
    List<Integer> list = new ArrayList<>();
    boolean[] used = new boolean[8];
    public List<List<Integer>> permuteUnique(int[] nums) {
        dfs(nums);
        return new ArrayList(res); 
    }
    public void dfs(int[] nums) {
        if(list.size() == nums.length) {
            res.add(new ArrayList(list));
            return;
        }
        for(int i = 0; i < nums.length; i++) {
            if(used[i]) continue;
            list.add(nums[i]);
            used[i] = true;
            dfs(nums);
            list.remove(list.size() - 1);
            used[i] = false;
        }
    }
}
```

```java
优化：在递归回溯中去重，进一步剪枝
  class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    boolean[] used = new boolean[8];
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);//必须排序，才能确保横向正确去重
        dfs(nums);
        return new ArrayList(res); 
    }
    public void dfs(int[] nums) {
        if(list.size() == nums.length) {
            res.add(new ArrayList(list));
            return;
        }
        for(int i = 0; i < nums.length; i++) {
            if(used[i]) continue;//已经用过的元素跳过，可以理解为纵向缩小搜索集
            if(i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;//i > 0 && nums[i] == nums[i - 1]表明当前元素之前已经处理过了!used[i - 1]表明这个元素没有使用过，确保是横向的剪枝，因为纵向遍历的时候确实存在重复元素而且可以使用，所以一定要加这个条件
            list.add(nums[i]);
            used[i] = true;
            dfs(nums);
            list.remove(list.size() - 1);
            used[i] = false;
        }
    }
}
```

