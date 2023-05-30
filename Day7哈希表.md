## Day7哈希表

#### [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

```java
HashMap
  时间：o(n^2)
  空间：o(n^2)
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer,Integer> map = new HashMap<>();
        //一二两组数组元素总和出现次数统计出来放入map中，对应元素和跟出现次数
        for(int i : nums1) {
            for(int k : nums2) {
                int sum = i + k;
                map.put(sum, map.getOrDefault(sum,0) + 1);
            }
        }
        int res = 0;
        //三四两组元素求和并用0减去和即位要在一二数组中需要寻找的目标值，只要统计目标出现次数，就是符合要求的组合个数
        for(int i : nums3) {
            for(int j : nums4) {
                int target = 0 - (i + j);
                if(map.containsKey(target)) {
                    res += map.get(target);
                }
            }
        }
        return res;
    }
}
```

#### [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

```java
数组
时间：o(n)
空间：o(1)
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] count = new int[26];
        for(int i = 0; i < ransomNote.length(); i++) {
            count[ransomNote.charAt(i) - 'a']++;
        }
        for(int i = 0; i < magazine.length(); i++) {
            count[magazine.charAt(i) - 'a']--;
        }
        for(int i : count) {
            if(i > 0) return false;
        }
        return true;
    }
}
```

#### [15. 三数之和](https://leetcode.cn/problems/3sum/)

```java
个人感觉所有题解中去重逻辑比较复杂且容易出错，我认为用HashSet来去重更加直观简便一些。
时间：O(n^2)
空间：o(1)
class Solution {//去重过程很繁琐容易错，有时候会写不出来，我更倾向用set处理此问题
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> res = new HashSet<>();
        Arrays.sort(nums);
        for(int i = 0; i < nums.length - 2; i++) {//边界容易错
            if(nums[i] > 0) break; // 如果当前数字大于0，则三数之和一定大于0，基本想不到
            int left = i + 1;
            int right = nums.length - 1;
            while(left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if(sum < 0) {
                    left++;
                }else if(sum > 0) {
                    right--;
                }else {
                    res.add(Arrays.asList(nums[i],nums[left],nums[right]));
                    left++;
                    right--;
                }
            }
        }
        return new LinkedList(res);
    }
}
```

```java
不用Set的话
时间：O(n^2)
空间：
class Solution {//去重过程很繁琐容易错，有时候会写不出来，我更倾向用set处理此问题
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        for(int i = 0; i < nums.length - 2; i++) {//边界容易错
            if(i > 0 && nums[i] == nums[i-1]) continue;//去重容易写错i > 0
            if(nums[i] > 0) break; // 如果当前数字大于0，则三数之和一定大于0，基本想不到
            int left = i + 1;
            int right = nums.length - 1;
            while(left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if(sum < 0) {
                    left++;
                }else if(sum > 0) {
                    right--;
                }else {
                    res.add(Arrays.asList(nums[i],nums[left],nums[right]));
                    left++;
                    right--;
                    while(left < right && nums[left] == nums[left - 1]) {//去重难写对left < right
                        left++;
                    }
                    while(left < right && nums[right] == nums[right + 1]) {//去重条件太难写对
                        right--;
                    }
                }
            }
        }
        return res;
    }
}
```

#### [18. 四数之和](https://leetcode.cn/problems/4sum/)

给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

0 <= a, b, c, d < n
a、b、c 和 d 互不相同
nums[a] + nums[b] + nums[c] + nums[d] == target
你可以按 任意顺序 返回答案 。

```java
用HashSet去重会简单一些
  时间：o(n^3)
  空间：o(1)
class Solution {//用set处理避免了复杂的去重问题，虽然有一些重复运算，但是更容易写对，
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Set<List<Integer>> res = new HashSet<>();
        Arrays.sort(nums);
        for(int i = 0; i < nums.length - 3; i++) {
            if (nums[i] > 0 && nums[i] > target) {
                break;//排序后第一个正数就已经大于目标，就不用做了，想不到这个
            }
            for(int j = i + 1; j < nums.length - 2; j++) {
                int left = j + 1;
                int right = nums.length - 1;
                while(left < right) {
                    long sum = (long)nums[i] + nums[j] + nums[left] + nums[right];//超出int范围只能debug的时候再想到改了，一次性写不对
                    if(sum < target) {
                        left++;
                    }else if(sum > target) {
                        right--;
                    }else {
                        res.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        left++;
                        right--;

                    }
                }
            }
        }
        return new LinkedList(res);
    }
}
```

```java
不用set
  时间：o(n^3)
  空间：o(1)
  class Solution {//用set处理避免了复杂的去重问题，虽然有一些重复运算，但是更容易写对，以下是不用set的方法
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new LinkedList<>();
        Arrays.sort(nums);
        for(int i = 0; i < nums.length - 3; i++) {
            if (nums[i] > 0 && nums[i] > target) {
                break;//排序后第一个正数就已经大于目标，就不用做了，想不到这个
            }
            if(i > 0 && nums[i] == nums[i - 1]) continue;//第一个条件容易漏
            for(int j = i + 1; j < nums.length - 2; j++) {
                if(j > i + 1 && nums[j] == nums[j - 1]) continue;//第一个条件容易漏
                int left = j + 1;
                int right = nums.length - 1;
                while(left < right) {
                    long sum = (long)nums[i] + nums[j] + nums[left] + nums[right];//超出int范围只能debug的时候再想到改了，一次性写不对
                    if(sum < target) {
                        left++;
                    }else if(sum > target) {
                        right--;
                    }else {
                        res.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        left++;
                        right--;
                        while(left < right && nums[left] == nums[left - 1]) {//第一个条件容易漏，如果不加这个限制，left更新后可能会超出边界，下面同理，所以这种条件性限制太多，不如用set
                            left++;
                        }
                        while(left < right && nums[right] == nums[right + 1]) {//第一个条件容易漏
                            right--;
                        }
                    }
                }
            }
        }
        return res;
    }
}
  
```

