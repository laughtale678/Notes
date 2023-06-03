## Day11	栈与队列

#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
每个右括号都有一个对应的相同类型的左括号。

```java
时间：O(n)遍历一次
空间：o(n)最坏情况全部添加到栈中
class Solution {
    public boolean isValid(String s) {//思路是左括号进栈，右括号匹配上就出栈，都能出栈说明完全匹配
        char[] sc = s.toCharArray();
        Stack<Character> stack = new Stack();
        for(char c : sc) {
            if(c == '(' || c == '[' || c == '{') {
                stack.push(c);//如果是这三种类型，就加入stack
            }else {
                if(stack.isEmpty()) {//如果不是上面三种而stack是空的，说明不匹配
                    return false;
                }else if(stack.peek() == '(' && c == ')' ||//剩下三种情况如果匹配上就出栈
                         stack.peek() == '[' && c == ']' ||
                         stack.peek() == '{' && c == '}'
                ) {
                    stack.pop();
                }else {
                    return false;//一种情况都没有说明没有匹配上，就不用继续了
                }
            }
        }
        return stack.isEmpty();  
    }
}
```

#### [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

```java
class Solution {
    public String removeDuplicates(String s) {
        Stack<Character> stack = new Stack();
        for(char c : s.toCharArray()) {
            if(!stack.isEmpty() && stack.peek() == c) {//对每个字符，如果等于stack栈顶的，就消除
                stack.pop();
            }else {
                stack.push(c);
            }
        }
        StringBuilder sb = new StringBuilder();
        while(!stack.isEmpty()) {
            sb.insert(0, stack.pop());//插入在开头，调整顺序
        }
        return sb.toString();
    }
}
```

#### [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

给你一个字符串数组 tokens ，表示一个根据 逆波兰表示法 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

注意：

有效的算符为 '+'、'-'、'*' 和 '/' 。
每个操作数（运算对象）都可以是一个整数或者另一个表达式。
两个整数之间的除法总是 向零截断 。
表达式中不含除零运算。
输入是一个根据逆波兰表示法表示的算术表达式。
答案及所有中间计算结果可以用 32 位 整数表示。

```java
时间：O(n) 需要遍历数组 tokens一次
空间：O(n)使用栈存储计算过程
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stack = new ArrayDeque();
        for(String s : tokens) {
            if(!s.equals("+")  && !s.equals("-") && !s.equals("*") && !s.equals("/")) {
                stack.push(Integer.parseInt(s));//String转int
            }else {
                int i = stack.pop();
                int j = stack.pop();
                if(s.equals("+")) {
                    stack.push(j + i);//i跟j的顺序一定要弄对
                }else if(s.equals("-")) {
                    stack.push(j - i);
                }else if(s.equals("*")) {
                    stack.push(j * i);
                }else if(s.equals("/")) {
                    stack.push(j / i);
                }
            }
        }
        return stack.pop();
    }
}
```

