## Day10第五章 栈与队列part01

#### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（push、pop、peek、empty）：

实现 MyQueue 类：

void push(int x) 将元素 x 推到队列的末尾
int pop() 从队列的开头移除并返回元素
int peek() 返回队列开头的元素
boolean empty() 如果队列为空，返回 true ；否则，返回 false
说明：

你 只能 使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

```java
class MyQueue {
    Stack<Integer> stackin;
    Stack<Integer> stackout;

    public MyQueue() {
        stackin = new Stack();
        stackout = new Stack();

    }
    
    public void push(int x) {
        stackin.push(x);

    }
    
    public int pop() {
        if(stackout.isEmpty()) {
            while(!stackin.isEmpty()) {
                stackout.push(stackin.pop());
            }
        }
        return stackout.pop();

    }
    
    public int peek() {
        if(stackout.isEmpty()) {
            while(!stackin.isEmpty()) {
                stackout.push(stackin.pop());
            }
        }
        return stackout.peek();
    }
    
    public boolean empty() {
        return stackin.isEmpty() && stackout.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

#### [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（push、top、pop 和 empty）。

实现 MyStack 类：

void push(int x) 将元素 x 压入栈顶。
int pop() 移除并返回栈顶元素。
int top() 返回栈顶元素。
boolean empty() 如果栈是空的，返回 true ；否则，返回 false 。


注意：

你只能使用队列的基本操作 —— 也就是 push to back、peek/pop from front、size 和 is empty 这些操作。
你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

```java
两个queue
class MyStack {
    Queue<Integer> queue1;
    Queue<Integer> queue2;

    public MyStack() {
        queue1 = new LinkedList();
        queue2 = new LinkedList();
    }
    
    public void push(int x) {
        while(queue1.size() != 0) {
            queue2.offer(queue1.poll());
        }
        queue1.offer(x);
        while(queue2.size() != 0) {
            queue1.offer(queue2.poll());
        }
    }
    
    public int pop() {
        return queue1.poll();
    }
    
    public int top() {
        return queue1.peek();
    }
    
    public boolean empty() {
        return queue1.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

```java
一个queue
class MyStack {
    Queue<Integer> queue;

    public MyStack() {
        queue = new LinkedList();
    }
    
    public void push(int x) {
        queue.offer(x);
        int n = queue.size();
        while(n > 1) {
            queue.offer(queue.poll());
            n--;//记得把size单独存储下来进行递减，不然size一直保持不变。
        }
    }
    
    public int pop() {
        return queue.poll();
    }
    
    public int top() {
        return queue.peek();
    }
    
    public boolean empty() {
        return queue.isEmpty();
    }
}
```

