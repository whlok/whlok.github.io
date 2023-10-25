---
title: Leetcode算法之字符串
date: 2023-10-25 18:49:35
categories: 数据结构与算法
tags: Leetcode
---

## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

思路：
1. 明确字串含义：连续的字符串，区别于子序列
2. 如何区别字符串重复？哈希表
3. 如何计算长度？利用下标，右下标-左下标（双指针）
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char, int> mp;
        int res = 0, l = 0, r = 0;
        while(r < s.size())
        {
            if(mp.count(s[r]))
                l = max(l, mp[s[r]]+1);
            mp[s[r]] = r;
            r++;
            res = max(res, r-l);
        }
        return res;
    }
};
```

## [394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)
给定一个经过编码的字符串，返回它解码后的字符串。
编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。
你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。
此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入

> 输入：s = "3[a]2[bc]"
> 输出："aaabcbc"
> 输入：s = "3[a2[c]]"
> 输出："accaccacc"
> 输入：s = "2[abc]3[cd]ef"
> 输出："abcabccdcdcdef"

```cpp
class Solution 
{
public:
    string decodeString(string s) 
    {
        string res;
        stack<int> nums;
        stack<string> str;
        int num = 0;
        string tmp = "";
        for(int i = 0; i < s.size(); ++i)
        {
            if(s[i] >= '0' && s[i] <= '9')
               num = num * 10 + s[i] - '0';
            else if(s[i] == '[')
            {
                nums.push(num);
                num = 0;
                str.push(tmp);
                tmp.clear();
            }
            else if((s[i] >= 'a' && s[i] <= 'z') || (s[i] >= 'A' && s[i] <= 'Z'))
                tmp += s[i];
            else if(s[i] == ']')
            {
                int k = nums.top();
                nums.pop();
                for(int j = 0; j < k; ++j)
                {
                    str.top() += tmp;
                }
                tmp = str.top();
                str.pop();
            }
        }
        res += tmp;
        return res;
    }   
};
```
