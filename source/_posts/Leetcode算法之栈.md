---
title: Leetcode算法之栈
date: 2023-10-25 18:47:52
categories: 数据结构与算法
tags: Leetcode
---

 [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

> 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
> 有效字符串需满足：
> 左括号必须用相同类型的右括号闭合。
> 左括号必须以正确的顺序闭合。

分析
1. 判断字符串是否有效的规则是括号能匹配上且正确顺序闭合；
2. 括号匹配有先来后匹配的顺序要求，类似于栈的存储方式，考虑用栈实现
3. 字符串中除了**左**括号就是**右**括号，遍历时遇见左括号就压入栈中对应的右括号，当出现右括号时去对比栈顶元素，相等则弹出后继续，否则无法匹配。
4. 如果遍历完成后栈为空则完全匹配，如果需要匹配的时候栈已经为空则不匹配

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> st = new Stack<Character>();
        for(char c : s.toCharArray()){
            if(c == '(')
                st.push(')');
            else if(c == '[')
                st.push(']');
            else if(c == '{')
                st.push('}');
            else if(st.isEmpty() || c != st.pop()){
                return false;
            }
        }
        return st.isEmpty();
    }
}
```

[155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

> 设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
> push(x) —— 将元素 x 推入栈中。
> pop() —— 删除栈顶的元素。
> top() —— 获取栈顶元素。
> getMin() —— 检索栈中的最小元素。

示例:
> 输入：
> ["MinStack","push","push","push","getMin","pop","top","getMin"]
> [[],[-2],[0],[-3],[],[],[],[]]
> 输出：
> [null,null,null,null,-3,null,0,-2]
> 解释：
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.getMin();   --> 返回 -3.
> minStack.pop();
> minStack.top();      --> 返回 0.
> minStack.getMin();   --> 返回 -2.

思考：
1.  求最小栈，常数时间内检索到
2.  需要一个栈用于存储数据，再加一个栈来存储当前位置栈的最小值，其实一个单调栈的概念

```java
```java
public class MinStack {

    private Stack<Integer> st;
    private Stack<Integer> minSt;

    public MinStack() {
        st = new Stack<>();
        minSt = new Stack<>();
    }

    public void push(int val) {
        st.push(val);
        if (minSt.isEmpty() || val <= minSt.peek()) {
            minSt.push(val);
        } else {
            minSt.push(minSt.peek());
        }
    }

    public void pop() {
        if(!st.empty()) {
            st.pop();
            minSt.pop();
        }
    }

    public int top() {
        return st.peek();
    }

    public int getMin() {
        return minSt.peek();
    }
}

```



