## Day35 贪心

#### [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

在柠檬水摊上，每一杯柠檬水的售价为 5 美元。顾客排队购买你的产品，（按账单 bills 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 5 美元、10 美元或 20 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 5 美元。

注意，一开始你手头没有任何零钱。

给你一个整数数组 bills ，其中 bills[i] 是第 i 位顾客付的账。如果你能给每位顾客正确找零，返回 true ，否则返回 false 。

```java
贪心策略就是优先消耗大面额的货币，因为小面额的使用场景更广
时间：O(n)
空间：o(1)
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int countF = 0;
        int countT = 0;
        for(int i = 0; i < bills.length; i++) {
            if(bills[i] == 5) {
                countF++;
            }else if(bills[i] == 10) {
                countF--;
                countT++;

            }else {
                if(countT > 0) {
                    countT--;
                    countF--;
                }else {
                    countF -= 3;
                }
            }
            if(countT < 0 || countF < 0) return false;
        }
        return true;    
    }
}
```

#### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

```java
思路：先按身高h从高到低排列，若身高相等则按k从低到高排列。然后遍历的时候按照k来插入list，因为从高到矮，所以k的值就是应当插入的index，因为插入位置之前的肯定是比你大的或者至少是相等的（相等的情况本身已经满足k从低到高了，后插入的相同身高元素那k一定大，一定在先插入的元素后面），同时，插入的都是矮的元素，对已经在队列中的元素k无法造成影响。
  
  时间：O(nlogn)排序
  空间：O(n)
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (o1, o2) -> 
            o1[0] == o2[0]? o1[1] - o2[1] : o2[0] - o1[0]
        );
        LinkedList<int[]> list = new LinkedList<>();
        for(int i = 0; i < people.length; i++) {
            list.add(people[i][1], people[i]);
        }
        int[][] result = new int[list.size()][];
        for (int i = 0; i < list.size(); i++) {
           result[i] = list.get(i);
        }
        return result;
    }
}
```

#### [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

有一些球形气球贴在一堵用 XY 平面表示的墙面上。墙面上的气球记录在整数数组 points ，其中points[i] = [xstart, xend] 表示水平直径在 xstart 和 xend之间的气球。你不知道气球的确切 y 坐标。

一支弓箭可以沿着 x 轴从不同点 完全垂直 地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被 引爆 。可以射出的弓箭的数量 没有限制 。 弓箭一旦被射出之后，可以无限地前进。

给你一个数组 points ，返回引爆所有气球所必须射出的 最小 弓箭数 。

```java
 时间：O(nlogn)排序
  空间：o(logn)快速排序算法使用递归进行分割和排序，其中涉及到创建临时变量和将数组分割成较小的子数组。递归过程中，调用栈的深度与输入规模成对数关系，因此空间复杂度为O(log n)。
   思路：result至少需要射一箭。按起始坐标升序排列，这样就只需要比较第一个元素结束坐标跟第二个元素的起始坐标就可以知道两者是否重叠了，如果不重叠，说明result需要多一箭。那如果是重叠的，只需要更新第二个元素结束坐标为两者之间较小的坐标（因为射箭肯定在这个坐标之内，后面的长度也没有意义可以舍弃不予考虑），然后再把第二个元素的结束坐标跟下面一个元素的起始坐标比较也就是进入循环。
class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (o1, o2) -> Integer.compare(o1[0], o2[0]));//用这种比较可以防止integer溢出边界
        int result = 1;
        for(int i = 0; i < points.length - 1; i++) {
            if(points[i][1] < points[i + 1][0]) {
                result++;
            }else {
                points[i + 1][1] = Math.min(points[i + 1][1], points[i][1]);
            }
        }
        return result;
    }
}
```

