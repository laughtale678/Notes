## Day8字符串

#### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

```java
时间：O(n)
空间：O(1)
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;
        while(left < right) {
            char temp = s[right];
            s[right] = s[left];
            s[left] = temp;
            left++;//边界移动容易漏写
            right--;
        }
    }
}
```

#### [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。

如果剩余字符少于 k 个，则将剩余字符全部反转。
如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

```java
时间：o(n)
空间：o(n)
  class Solution {
    public String reverseStr(String s, int k) {
        char[] c = s.toCharArray();
        for(int i = 0; i < c.length; i += 2 * k) {
            reverse(c, i, Math.min(i + k - 1, c.length - 1));
        }
        return new String(c);
    }
    
    public void reverse(char[] c, int left, int right) {
        while(left < right) {
            char temp = c[right];
            c[right] = c[left];
            c[left] = temp;
            left++;//容易漏写
            right--;//容易漏写
        }
    }
} 
```

#### [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

```java
时间：o(n)
空间：o(n)
class Solution {
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < s.length(); i++) {
            if(s.charAt(i) == ' ') {
                sb.append("%20");
            }else {
                sb.append(s.charAt(i));
            }
        }
        return sb.toString();
    }
}
```

#### [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

给你一个字符串 s ，请你反转字符串中 单词 的顺序。

单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。

返回 单词 顺序颠倒且 单词 之间用单个空格连接的结果字符串。

注意：输入字符串 s中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

```java
自己的方法：
  
class Solution {
    //整体思想：把每个单词提取出来放入list，然后倒序遍历拼接
    public String reverseWords(String s) {
        StringBuilder sb = new StringBuilder();
        StringBuilder res = new StringBuilder();
        ArrayList<String> list = new ArrayList();
        for(int i = 0; i < s.length(); i++) {
            while(i < s.length() && s.charAt(i) != ' ') {
                sb.append(s.charAt(i));
                i++;
            }
            if(sb.length() != 0) {
                list.add(sb.toString());
                sb.setLength(0);
            }
        }
        for(int i = list.size() - 1; i >= 0; i--) {
            res.append(list.get(i));
            if(i != 0) {
                res.append(' ');
            }
        }
        return res.toString();
    }
}
```

```java
双指针方法：
时间：o(n)
空间：o(n)含结果的湖
class Solution {
    //整体思想：用双指针找出每一个单词的起点跟结束点
    public String reverseWords(String s) {
        String res = "";
        int start = 0;
        while(start < s.length()) {
            while(start < s.length() && s.charAt(start) == ' ') {
                start++;
            }
            int end = start;
            while(end < s.length() && s.charAt(end) != ' ') {
                end++;
            }
            if(start == end) break;//不加这条会导致加一个空的word进到结果中，前面会多一个空格，这个判断也可以放在判断word是否为有长度，因为substring的问题是，在两点相等时依然返回字符串。
            String word= s.substring(start, end);
            if(res.length() == 0) {
                res = word + res;
            }else {
                res = word + " " + res;
            }
            start = end;
        }
        return res;
    }
}
```

```java
```



#### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

```java
substring()没有大写字母，用的太少，总是写错
时间：o(n) s.substring(n, s.length())和s.substring(0, n)。这两个操作的时间复杂度都是O(N)
空间：o(n)  substring开辟了额外空间
class Solution {
    public String reverseLeftWords(String s, int n) {
        return s.substring(n, s.length()) + s.substring(0, n);
    }
}
```

```java
翻转：感觉两次内部翻转的边界极难一次写对
时间：o(n)
空间：o(1)  
class Solution {
    public String reverseLeftWords(String s, int n) {
        char[] ss = s.toCharArray();
        reverse(ss, 0, ss.length - 1);//整体翻转
        reverse(ss, 0, ss.length - 1 - n);//翻转前面
        reverse(ss, ss.length - n, ss.length - 1);//翻转最后n个
        return new String(ss);
    }
    public void reverse(char[] c, int left, int right) {
        while(left < right) {
            char temp = c[right];
            c[right] = c[left];
            c[left] = temp;
            left++;
            right--;
        }
    }
}
  
```

