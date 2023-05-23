# Day1 数组

## [704. 二分查找](https://leetcode.cn/problems/binary-search/)

### 左闭右闭Binary Search

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;//左闭右闭，left == right需要执行循环
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] < target) {
                left = mid + 1;   
            }else if(target < nums[mid]) {
                right = mid - 1;
            }else {
                return mid;
            }
        }
        return -1;
    }
}
```

### 左闭右开Binary Search

```java
	class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length;//左闭右开，left == right不需要执行循环
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] < target) {
                left = mid + 1;   
            }else if(target < nums[mid]) {
                right = mid;  //不同于右闭，更新right==mid后的搜索区间不包含mid在内，区别于右闭
            }else {
                return mid;
            }
        }
        return -1;
    }
}
```

## [27. 移除元素](https://leetcode.cn/problems/remove-element/)

###  Brute Force，没想出来

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int size = nums.length;
        for(int i = 0; i < size; i++) {
            if(nums[i] == val) {
                for(int j = i + 1; j < size; j++) {
                    nums[j - 1] = nums[j];
                }
                i--;//当前i所在位置的数字已经更新了，所以要先把i--，然后本次循环结束后i++，还是回到当前
                size--;//相当于每次移除一个元素
            }
        }
        return size;
    }
}
```

### Two Pointer

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int fast = 0;
        int slow = 0;
        while(fast < nums.length) {
            if(nums[fast] != val) {
                nums[slow] = nums[fast];
                slow++;//找到val的情况下更新慢指针
            }
            fast++;//每次更新快指针
        }
        return slow;
    }
}
```



