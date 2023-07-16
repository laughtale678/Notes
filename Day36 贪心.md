## Day36 贪心

#### [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

给定一个区间的集合 intervals ，其中 intervals[i] = [starti, endi] 。返回 需要移除区间的最小数量，使剩余区间互不重叠 。

```java
时间：o(nlongn)排序
空间：o(logn)快速排序算法使用递归进行分割和排序，其中涉及到创建临时变量和将数组分割成较小的子数组。递归过程中，调用栈的深度与输入规模成对数关系，因此空间复杂度为O(log n)。
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (o1, o2) -> Integer.compare(o1[0], o2[0]));//排序
        int res = 0;
        for(int i = 0; i < intervals.length - 1; i++) {//比较当前终止与下个开头，满足这个条件说明重合
            if(intervals[i][1] > intervals[i + 1][0]) {
                res++;//重合说明需要去重，次数增加
                intervals[i + 1][1] = Math.min(intervals[i + 1][1], intervals[i][1]);//这个更新边界的操作等于是去重，将下次比较的边界更新为两个之间较小的终止点，等于是把较长的那个删除了，因为题目要求最小值，那就把结束点长的移除，因为结束点长的必然更容易重合，那结束点越短的区间越不容易有重合。
            }
        }
        return res;
    }
}
```

#### [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/)

给你一个字符串 s 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 s 。

返回一个表示每个字符串片段的长度的列表。

```java
时间：o(n)
空间：o(n)每个元素都不一样，最多26个，我也不知道是o(n)还是o(1)
class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] position = new int[26];
        List<Integer> result = new LinkedList<>();
        for(int i = 0; i < s.length(); i++) {
            position[s.charAt(i) - 'a'] = i;//记录s中每个元素出现的最远index，比如第一个元素最远是8，那么切割点至少要到8才能保证第一个元素只出现在第一次切割的字符串中，同时，在这之间有其他元素可能会出现在更远的下标位置，那么光到8仍然不够，需要满足区间内所有元素的最远下标都能覆盖才行。
        }
        int left = 0;
        int right = 0;
        for(int i = 0; i < s.length(); i++) {
            right = Math.max(right, position[s.charAt(i) - 'a']);//right依次与每个元素的最大index比较并更新为最大的，直至i的位置跟right一致，那么说明i所处的位置能包含之前所有的字母，此时就是分割点
            if(i == right) {
                result.add(right - left + 1);
                left = right + 1;
            }
        }
        return result;    
    }
}
```

#### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。

```java
时间：o(nlongn)排序
空间：o(n)
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (o1, o2) -> Integer.compare(o1[0], o2[0]));//按照起点升序排列，尽可能重合
        List<int[]> list = new ArrayList<>();
        int left = intervals[0][0];
        int right = intervals[0][1];//定义左右坐标，默认就是第一个元素的坐标
        for(int i = 1; i < intervals.length; i++) {
            if(intervals[i][0] <= right) {//有重叠区间，更新右边界，此时取两者之间较大的数值
                right = Math.max(intervals[i][1], right);
            } else {
                list.add(new int[] {left, right});//没有重叠区间的时候，就把之前搜集到的左右区间加入结果集，然后更新左右区间重新开始下一轮更新
                left = intervals[i][0];
                right = intervals[i][1];
            }
        }
        list.add(new int[] {left, right});//容易漏掉最后一个元素，因为最后一个元素不管合并进去还是没合并进去，都不会触发上面加入结果集的代码了，这时候左右坐标可能是更新过的也可能是没更新过的，直接加入结果集就可以了。
        return list.toArray(new int[0][]);  //这个toArray的方法需要学习  
    }
}
```

