## Day 30贪心算法

#### [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

```java
时间：O(nlogn)
空间：O(1)
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int count = 0;
        for(int i = 0; i < s.length; i++) {
            if(count == g.length) return count;
            if(s[i] >= g[count]) {
                count++;
            }
        }
        return count;
    }
}
```

#### [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 摆动序列 。第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。

例如， [1, 7, 4, 9, 2, 5] 是一个 摆动序列 ，因为差值 (6, -3, 5, -7, 3) 是正负交替出现的。

相反，[1, 4, 7, 2, 5] 和 [1, 7, 4, 5, 5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。
子序列 可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。

给你一个整数数组 nums ，返回 nums 中作为 摆动序列 的 最长子序列的长度 。

```java
题解对我来说可以理解，但是不是那种能记住的，因为感觉思维很不常规，情况太多。这个是其他地方的题解更容易理解一些，没有复杂的情况判断。
  时间：O(n)
  空间：O(1)
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if(nums.length < 2) return nums.length;
        int up = 1;//题目中说有一个数字也符合条件，因此，统计有效上升跟下降的起点均为1
        int down = 1;
        for(int i = 1; i < nums.length; i++) {
            if(nums[i] > nums[i - 1]) {
                up = down + 1;
            }
            if(nums[i] < nums[i - 1]) {
                down = up + 1;
            }
        }
        return Math.max(up, down);//逻辑：第一个可以当成上升下降都行，有一个up说明多了一个有效节点，有一个down说明多一个有效节点，up跟down其实都是结果，两者只是在不同情况下进行更新。然后后面只有up更新了down才会更新，只有down更新了up才会更新，这样确保每个有效节点之前跟之后一定是一正一负。遇到相等或者连续上升下降都不会影响结果，那返回值取较大的，因为最终增加的值可能是up也可能是down，最后更新的那个就是最大的
    }
}
```

#### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

```java
暴力解法双重for循环超时：
  贪心算法：尽可能保持每一次取最大的和（如果当前和是负数，就重新从下个元素开始，因为负数与其他元素相加只会让总和更小）那如果和是正数的时候，碰到负数为什么不重置。因为每次最大和都已经更新在结果中，只要和是正数，继续往后采集就有增加总和的可能性。
  时间：O(n)
  空间：O(1)
  class Solution {
    public int maxSubArray(int[] nums) {
        int res = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = 0; i < nums.length; i ++) {
            sum += nums[i];
            res = Math.max(sum, res);
            if(sum < 0) {//如果sum小于0，那么sum继续往后加总只会让后面的数字变小，不如把sum重置，从下个元素重新开始计算，这样才有可能取到最大的sum
                sum = 0;
            }
        }
        return res;   
    }
}
  
```

