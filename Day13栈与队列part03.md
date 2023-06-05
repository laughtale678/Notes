## Day13**栈与队列**part03

#### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。

```java
时间：O(n)
空间：O(k)
整体思想：用辅助单调队列记录最大值变化，插入元素时，弹出比自己小的元素，保证单调性，这样当前最大元素若被移除，第二大元素则会替代他成为当前最大元素。移除元素时，因为当前最大值在头部，只有可能发生在移除元素值与当前最大值相等的情况。
class Solution {
    class MaxDeque{//自己创建一个单调队列，头部到尾部降序排列，头部就可以记录当前区间最大值
        Deque<Integer> deque;

        public MaxDeque() {
            deque = new ArrayDeque<>();
        }
        public void add(int x) {//尾部添加
            while(!deque.isEmpty() && deque.getLast() < x) {
                deque.removeLast();//保证队列为降序，只需要插入在当前区间有可能成为最大值的元素
            }
            deque.add(x);
        }

        public void poll(int x) {//头部移除
            if(!deque.isEmpty() && x == deque.peek()){
                deque.poll();//因为按照降序排列，因此，当前目标元素最多与队列中头元素相等
            }
        }

        public int peek() {//头部元素
           return deque.peek();
        }
    }
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] res = new int[nums.length - k + 1];
        MaxDeque myqeuqe = new MaxDeque();
        for(int i = 0; i < k; i++) {
            myqeuqe.add(nums[i]);
        }
        res[0] = myqeuqe.peek();
        int index = 1;
        int left = 1;
        int right = k;
        while(right < nums.length) {
            myqeuqe.poll(nums[left - 1]);//一直犯错的地方，移除的应该是left指针前一个值，而不是当前
            myqeuqe.add(nums[right]);
            res[index++] = myqeuqe.peek();
            right++;
            left++;
        }
        return res; 
    }
}
```

#### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

```java
不了解相关知识，来不及学
```

