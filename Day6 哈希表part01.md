## Day6 **哈希表****part01**

### [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

```java
哈希表：用int数组代表26字母出现次数
时间：O(n)
空间：O(1)
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] count = new int[26];
        if(s.length() != t.length()) return false;
        for(int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }
        for(int n : count) {
            if(n != 0) return false;
        }
        return true;
    }
}
```

```java
排序：更简单
  时间：O(nlogn)排序的时间复杂度为 O(n log n)，比较两个排序后的字符串是否相等的时间复杂度为 O(n)，所以还是O(n log n)
  空间：O(n)排序法需要额外的空间来存储排序后的字符串。这个空间取决于字符串的长度，因此空间复杂度为 O(n)，其中 n 是字符串的长度
  class Solution {
    public boolean isAnagram(String s, String t) {
        char[] ss = s.toCharArray();
        Arrays.sort(ss);
        char[] tt = t.toCharArray();
        Arrays.sort(tt);
        return Arrays.equals(ss,tt);
    }
}
```

```java
进阶：如果输入字符串包含 unicode 字符怎么办？
  把数组改为HashMap;
class Solution {
    public boolean isAnagram(String s, String t) {
        Map<Character,Integer> map = new HashMap<>();
        if(s.length() != t.length()) return false;
        for(int i = 0; i < s.length(); i++) {
            map.put(s.charAt(i),map.getOrDefault(s.charAt(i), 0) + 1);//方法不熟悉,get的内容写在前面，默认写在后面
            map.put(t.charAt(i),map.getOrDefault(t.charAt(i), 0) - 1);
        }
        for (int count : map.values()) {//遍历value的方法不熟，map.values()返回Collection 对象
            if (count != 0) {
                return false;
            }
        }
        return true;
    }
}

```

### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

```java
用Set
时间：O(n)
空间：O(n)   
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        for(int i : nums1) {
            set.add(i);
        }
        Set<Integer> set1 = new HashSet<>();
        for(int j : nums2) {
            if(set.contains(j)) {
                set1.add(j);
            }
        }
        int[] res = new int[set1.size()];
        int index = 0;
        for(int n : set1) {
            res[index++] = n;
        }
        return res;
    }
}
```

### [202. 快乐数](https://leetcode.cn/problems/happy-number/)

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果这个过程 结果为 1，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

```java
时间：O(logn)
空间：O(logn)
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();//判断是否有重复数字出现是本题解题关键，很难看出来
        while(n != 1) {
            if(!set.add(n)) {
                return false;
            }
            n = getSum(n);
        }
        return true;
    }
取数值各个位上的单数是难点
    public int getSum(int n) {//给定数字每个位置上的数字的平方和，这块代码只能硬记了。想不出来
        int sum = 0;
        while(n > 0) {
            int temp = n % 10;// Get the last digit
            sum += temp * temp;// Add the square
            n = n / 10;// Remove the last digit
        }
        return sum;
    }
}
```

### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

```java
Brute Force:
时间：O(n^2)
空间：O(1)
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for(int i = 0; i < nums.length - 1; i++) {
            for(int j = i+ 1; j < nums.length; j++) {
                int sum = nums[i] + nums[j];
                if(sum == target) return new int[] {i,j};
            }
        }
        return null;
    }
}
```

```java
HashMap:
时间：O(n)
空间：O(n)
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {//遍历的时候把每个元素跟对应的下标储存进map，在这之前判断是否存在target - nums[i]这个key，如果存在说明之前有一个元素跟当前元素的和为target，返回当前下标跟之前那个元素的下标即可
            if(map.containsKey(target - nums[i])) {
                return new int[] {map.get(target - nums[i]), i};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```

