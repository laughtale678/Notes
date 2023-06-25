## Day28回溯

#### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在 s 中插入 '.' 来形成。你 不能 重新排序或删除 s 中的任何数字。你可以按 任何 顺序返回答案。

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> restoreIpAddresses(String s) {
        dfs(s, 0, "", 0);
        return res;
    }

    public void dfs(String s, int start, String str, int count) {//count用来计算拼接次数
        if(count == 4) {
            if(str.length() == s.length() + 4) {
            res.add(str.substring(0, str.length() - 1));//最后多加了一个.需要删除
            }
            return;
        }
        
        for(int i = start; i < s.length(); i++) {
            String tmp = s.substring(start, i + 1);
            if(tmp.length() > 1 && tmp.charAt(0) == '0' || tmp.length() > 3) break;//不符合条件：一位以上数字第一个是0或者大于三位数。因为如果不提前把三位数去掉的话，后面string转int会超出范围，而且本身剪枝也能提高效率。如果不加这个条件下面就要用long
            int tmpInt = Integer.parseInt(tmp);
            if(tmpInt >= 0 && tmpInt <= 255) {
                dfs(s, i + 1, str + tmp + '.', count + 1);
            }
        }
    }
}
```

#### [78. 子集](https://leetcode.cn/problems/subsets/)

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        dfs(nums, 0);
        return res;
    }
    public void dfs(int[] nums, int start) {
        res.add(new ArrayList(list));//一开始搜集结果的地方写错了。漏掉很多答案。因为进入当前递归即可搜集答案，否则最后一层的时候刚进入就终止了，无法搜集到该层的结果。
        if(start == nums.length) return;//终止条件可不写，因为从终止条件可以看出已不满足for循环条件
        for(int i = start; i < nums.length; i++) {
            list.add(nums[i]);
            dfs(nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```

#### [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);//排序了才能去重
        dfs(nums, 0);
        return res;  
    }

    private void dfs(int[] nums, int start) {
        res.add(new ArrayList(list));//进入时搜集结果，这个结果不能直接用list，因为引用类型存进去会一直随着递归改变
        for(int i = start; i < nums.length; i++) {
            if(i > start && nums[i] == nums[i - 1]) continue;//去重，参考40.组合总和II，横向去重，纵向不去重.要用continue不能用break，因为这个不是剪枝，只是去掉重复选项，重复元素后面仍有结果集
            list.add(nums[i]);
            dfs(nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```

