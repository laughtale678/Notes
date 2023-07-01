 ## Day32 贪心算法

#### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。

返回 你能获得的 最大 利润 。

```	java
只搜集上升区间
  时间：o(n)
  空间：o(1)
class Solution {
    public int maxProfit(int[] prices) {
        int sum = 0;
        for(int i = 0; i < prices.length - 1; i++) {
            if(prices[i] <= prices[i + 1]) {
                sum += prices[i + 1] - prices[i];
            }
        }
        return sum;  
    }
}
```

#### [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

```java
从第一个元素开始，在每个元素所能到达的最远下标范围内，不断更新每个元素所能到达的最远下标，直至最远下标覆盖了最后一个元素
时间：o(n)
空间：o(1)
class Solution {
    public boolean canJump(int[] nums) {
        int maxIndex = 0;
        for(int i = 0; i <= maxIndex; i++) {//在每个元素所能到达最大index范围内遍历找到并更新最大能到达的index
            maxIndex = Math.max(i + nums[i], maxIndex);//每次更新所能到达的最大index
            if(maxIndex >= nums.length - 1) return true;
        }
        return false;  
    }
}
```

#### [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

给定一个长度为 n 的 0 索引整数数组 nums。初始位置为 nums[0]。

每个元素 nums[i] 表示从索引 i 向前跳转的最大长度。换句话说，如果你在 nums[i] 处，你可以跳转到任意 nums[i + j] 处:

0 <= j <= nums[i] 
i + j < n
返回到达 nums[n - 1] 的最小跳跃次数。生成的测试用例可以到达 nums[n - 1]。

```java
时间：o(n)
空间：o(1)
class Solution {
    public int jump(int[] nums) {
        if(nums.length == 1) return 0;//不存在跳跃问题，需要单列，因为下面的情况是必须跳跃的情况。
        int maxIndex = 0;
        int count = 0;
        int range = 0;
        for(int i = 0; i < nums.length; i++) {//在跳跃范围内遍历每个元素，直至他所能到达的最大范围覆盖了数组最后一个元素，说明下次跳跃一定能到达末尾，同时记录并更新当前跳跃范围内元素所能到达的最大跳跃范围，如果到达了边界，跳跃次数加一，说明当前跳跃范围无法满足题意。[2,3,1,1,4]举例：初始跳跃范围是0，因为要离开第一个元素必须跳跃，此时判断初始跳跃范围没有覆盖到末位，而且i已经到达边界，所以需要进行跳跃，maxIndex已经记录了下次跳跃所能到达的最大边界，所以此时跳跃次数+1，边界更新为二次跳跃最大边界。此时开始二次跳跃范围的遍历，到达3的时候，更新maxIndex发现已经覆盖了数组末位，说明3再跳跃一次即可结束循环，跳跃次数加一后即可结束循环。其他元素就不需要考虑了。因为是从跳跃次数0开始增加的，所以在这之前入过就能到达边界，一定已经break了，在这之后入过在第二次跳跃最大范围内，不管能不能到达末尾，还是跳跃一次，如果超过了第二次跳跃最大范围，那就不是最小了。首先判断maxIndex >= nums.length - 1而不是先判断i == range，个人认为如果当前已经找到了符合要求的最大区间，下次跳跃一定符合要求，直接返回正确值了。如果先判断是否在边界上，会导致count更新两次。
            maxIndex = Math.max(maxIndex, i + nums[i]);
            if(maxIndex >= nums.length - 1) {
                count++;
                break;
            }
            if(i == range) {
                range = maxIndex;
                count++;
            }
        }
        return count; 
    }
}
```

