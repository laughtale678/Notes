## Day34 贪心

#### [1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

给你一个整数数组 nums 和一个整数 k ，按以下方法修改该数组：

选择某个下标 i 并将 nums[i] 替换为 -nums[i] 。
重复这个过程恰好 k 次。可以多次选择同一个下标 i 。

以这种方式修改数组后，返回数组 可能的最大和 。

```java
时间：o(nlogn)排序
空间：o(1)
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        Arrays.sort(nums);
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] < 0 && k > 0) {
                nums[i] = - nums[i];
                k--;
            }
        }
        if(k > 0 && k % 2 != 0) {
            Arrays.sort(nums);
            nums[0] = - nums[0];
        }
        return sum(nums);
    }

    public int sum(int[] nums) {
        int sum = 0;
        for(int i : nums) {
            sum += i;
        }
        return sum;
    }
}
```

#### [134. 加油站](https://leetcode.cn/problems/gas-station/)

在一条环路上有 n 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 gas 和 cost ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1 。如果存在解，则 保证 它是 唯一 的。

```java

class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int start = 0;// 记录起始位置
        int cur = 0;// 记录当前累积的燃料量
        int sum = 0;// 记录总的剩余燃料量
        for(int i = 0; i < gas.length; i++) {
            cur += gas[i] - cost[i];
            sum += gas[i] - cost[i];
            if(cur < 0) {// 如果燃料量 tank 小于 0，表示无法到达当前位置
                start = i + 1;// 更新起始位置为 i+1,因为之前任何一个点出发都不可能到达这个点，之前任何一个点都能到达，说明在这之前邮箱一直是正好用完或者有油的状态，也就是每个起始位置后每个节点都可以自带之前剩余的油这样都到不了，更不要说邮箱清空的状态从这里出发了，更加不可能到达，邮箱为负的当前节点也是，加上之前剩的都是负，更不要说从他自己开始，压根一步都走不了。
                cur = 0;//清空邮箱重新开始
            }
        }
        if(sum < 0) return -1;//如果整体加油小于油耗，那不管怎么走都完不成。
        return start;   //在整体油量够的情况下，通过排除不可能的情况找到了正确的起始点，因为这个起始点是会不断更新到直到完成遍历，因为答案保证是唯一的，由此可以推出更新完的start就是符合题意的。
    }
}
```

#### [135. 分发糖果](https://leetcode.cn/problems/candy/)

n 个孩子站成一排。给你一个整数数组 ratings 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

每个孩子至少分配到 1 个糖果。
相邻两个孩子评分更高的孩子会获得更多的糖果。
请你给每个孩子分发糖果，计算并返回需要准备的 最少糖果数目 。

```java
class Solution {
    public int candy(int[] ratings) {
        int len = ratings.length;
        int[] res = new int[len];
        res[0] = 1;
        for(int i = 1; i < len; i++) {//先跟左边元素比，第一个元素没有左边元素，省去
            if(ratings[i] > ratings[i - 1]) {
                res[i] = res[i - 1] + 1;
            }else {
                res[i] = 1;
            }
        }
        for(int i = len - 2; i >=0; i--) {//跟右边元素比，最后一个没有右边元素，省区
            if(ratings[i] > ratings[i + 1]) {
                res[i] = Math.max(res[i], res[i + 1] + 1);
            }
        }
        int sum = 0;
        for(int i : res) {
            sum += i;
        }
        return sum;
    }
}
```

