## Day25回溯

#### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：

只使用数字1到9
每个数字 最多使用一次 
返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

```java
	回溯：
    时间：O(9^k)
    空间：O(C(9, k))
class Solution {
    List<List<Integer>> res = new ArrayList();
    List<Integer> list = new ArrayList();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k, n, 1, 0);
        return res; 
    }
    public void backtracking(int k, int n, int start, int sum) {
        if(sum > n) return;
        if(list.size() == k) {
            if(sum == n) {
                res.add(new ArrayList(list));
            }
            return;
        }
       
        for(int i = start; i <= 9 - (k - list.size()) + 1; i ++) {
            list.add(i);
            backtracking(k, n, i + 1, sum + i);
            list.remove(list.size() - 1);
        }
    }
}
```

#### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

```java
class Solution {
    List<String> res = new ArrayList();
    public List<String> letterCombinations(String digits) {
        Map<Character, String> map = new HashMap<>();
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");
        if(digits.length() == 0) return res;
        backtracking(digits, 0, new StringBuilder(), map);
        return res;      
    }
    public void backtracking(String digits, int index, StringBuilder sb, Map<Character, String> map) {
        if(index == digits.length()) {
            res.add(sb.toString());
            return;
        }
        String tmp = map.get(digits.charAt(index));
        for(int i = 0; i < tmp.length(); i++) {
            sb.append(tmp.charAt(i));
            backtracking(digits, index + 1, sb, map);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```

