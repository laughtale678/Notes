## Day9字符串part02

#### [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串的第一个匹配项的下标（下标从 0 开始）。如果 needle 不是 haystack 的一部分，则返回  -1 。

```java
brute force ：用substring 逐一匹配
  时间：o(n^2)循环的时间复杂度为 O(n)。每次迭代中使用的 substring 方法的时间复杂度也是 O(n)
  空间：o(1)
class Solution {
    public int strStr(String haystack, String needle) {
        int hLen = haystack.length();
        int nLen = needle.length();
        if(hLen < nLen) return -1;
        int start = 0;
        int end = nLen;
        while(end <= hLen) {
            if(haystack.substring(start, end).equals(needle)) return start;
            start++;
            end++;
        }
        return -1;
    }
}
```

KMP没学

#### [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

```java

```

