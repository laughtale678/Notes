## Day2 **数组**part02

### [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

#### Brute Force: Sorting

直接遍历时用平方替换，然后用Arrays排序方法。

Time Complexity：遍历O(n),排序O(nlog n),整体O(nlog n）

Space Complexity: O(1)，官方答案是O(log⁡n) 说除了存储答案的数组以外，我们需要 O(log⁡n) 的栈空间进行排序。不太理解。

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        for(int i = 0; i < nums.length; i ++) {
            nums[i] = nums[i] * nums[i];
        }
        Arrays.sort(nums);
        return nums;
    }
}
```

#### Two Pointer:
 给定数组有序数组含负数，因此平方后最大的数不是在左边就是在后面，是一个两侧向中间递减的数组，用左右指针比较后向中间递进，将比较后较大值从后向前插入新数组后返回。即使都为正数或者都为负数，也不会有影响，这样就是单侧指针不断移动。

Time Complexity O(n)  

Space Complexity 不含答案O(1),含答案O(n)

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int maxIndex = nums.length - 1;
        int[] res = new int[nums.length];
        while(left <= right) {
            if(nums[left] * nums[left] <= nums[right] * nums[right]) {
                res[maxIndex--] = nums[right] * nums[right];
                right--;
            }else {
                res[maxIndex--] = nums[left] * nums[left];
                left++;
            }
        }
        return res;
    }
}
```

### [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

#### Brute Force: 超时了

Time Complexity：O(n^2)

Space Complexity: O(1)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int res = nums.length + 1;
        for(int i = 0; i < nums.length; i++) {
            int sum = 0;
            for(int j = i; j < nums.length; j++) {
                sum += nums[j];
                if(sum >= target) {
                    res = Math.min(res, j - i + 1);
                    break;
                }
            }
        }
        return res == nums.length + 1 ? 0 : res;   
    }
}
```

#### Sliding Window:

尝试了没写出来，自己尝试写在一个while里面，发现left指针的更新逻辑有问题。这里面需要注意的是当sum >= target 的时候需要不断更新left指针，不能用if。

Time Complexity：O(n) 代码随想录解析：每个元素在滑动窗后进来操作一次，出去操作一次，每个元素都是被操作两次，所以时间复杂度是 2 × n 也就是O(n)

Space Complexity: O(1)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int res = nums.length + 1;
        int sum = 0;
        for(int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while(sum >= target) {
                res = Math.min(res, right - left + 1);
                sum -= nums[left];
                left++;
            }
        }
        return res == nums.length + 1 ? 0 : res;  
    }
}
```

### [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

之前写过一次有点印象，但是还是没有写对。模拟填充过程，一开始每次填充相同的个数，发现无论如何写不对，后面改成每次更新一侧边界，就没问题了，可能更加清晰一些。循环条件一开始的是或，发现这题的情况下正方形，每次各边界进度相同，&& 和｜｜都可以。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        int left = 0;
        int right = n - 1;
        int up = 0;
        int down = n - 1;
        int num = 1;
        while(left <= right && up <= down) {
            for(int i = left; i <= right; i++) {
                res[up][i] = num++;
            }
            up++;
            for(int i = up; i <= down; i++) {
                res[i][right] = num++;
            }
            right--;
            for(int i = right; i >= left; i--) {
                res[down][i] = num++;
            }
            down--;
            for(int i = down; i >= up; i--) {
                res[i][left] = num++;
            }
            left++;  
        }
        return res;
    }
}
```

