---
title: Leetcode算法之动态规划
date: 2023-10-25 18:45:01
categories: 数据结构与算法
tags: Leetcode
---

## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

> 输入： 2
> 输出： 2
> 解释： 有两种方法可以爬到楼顶。.  1 阶 + 1 阶       ;  2 阶

思路：
1. dp[i]爬到第i阶的方法
2. dp[i] = dp[i-1] + dp[i-2]
3. dp[0] = 1;
4. dp[1] = 1;
```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n+1, 0);
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; ++i)
        {
            dp[i] = dp[i-1]+ dp[i-2];
        }
        return dp[n];
    }
};
```

## [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

> 输入：[1,2,3,1]
> 输出：4
> 解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
> 偷窃到的最高金额 = 1 + 3 = 4 。

思路：
1.  dp[i]表示偷第i个元素时可以获得的**最大**利润
2. dp[i] = max(dp[i-1],dp[i-2]+num[i]); 可以理解为偷不偷当前房屋，偷则dp[i-2]+nums[i], 不偷则dp[i-1]，比较俩种方式的最大金额。

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        //处理边界条件
        if(nums.size() == 0)
            return 0;
        if(nums.size() == 1)
            return nums[0];
        if(nums.size() == 2)
            return max(nums[0], nums[1]);

        vector<int> dp(nums.size(), 0);
        
        dp[0] = nums[0];
        dp[1] = max(nums[0],nums[1]);

        for(int i = 2; i< nums.size(); ++i)
        {
            dp[i] = max(dp[i-1], dp[i-2]+nums[i]);
        }
        return dp[nums.size()-1];
    }
};
```

## [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)
你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都围成一圈，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

> 输入: [2,3,2]
> 输出: 3
> 解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。

思路：
1. 
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        if(nums.size() == 1) return nums[0];
z        int prevMax = 0, prevvMax = 0;
        int curMax = 0, currMax = 0;
        for(int i = 0; i < nums.size() - 1; ++i)
        {
            int tmp = curMax;
            curMax = max(prevMax + nums[i], curMax);
            prevMax = tmp;
        }
        for(int i = 1; i < nums.size(); ++i)
        {
            int tmp = currMax;
            currMax = max(prevvMax + nums[i], currMax);
            prevvMax = tmp;
        }
        return max(curMax,currMax);
    }
};
```


## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票

> 输入: [7,1,5,3,6,4]
> 输出: 5
> 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
> 注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。

思路：
1. 获取最大利润，需要买卖差值最大
2. 对于当前节点，确定之前最低价值，当前值-最低值=当前最大收益，最大收益=max(当前最大收益)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxpro = 0;
        int minpri = INT_MAX;
        for(auto price: prices)
        {
            minpri = min(minpri, price);
            maxpro = max(maxpro, price - minpri);
        }
        return maxpro;
    }
};
```

## [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

> 输入: coins = [1, 2, 5], amount = 11
> 输出: 3 
> 解释: 11 = 5 + 5 + 1
> 输入: coins = [2], amount = 3
> 输出: -1
> 说明:
> 你可以认为每种硬币的数量是无限的。

思路：
1. dp[i]表示金额为i时凑硬币的最小个数
2. dp[i] = min(dp[i], (dp[i-coins[0]]...dp[i-coins[j]]) +1)
3. dp[0] = 0
注意：
1. 边界条件： i-coins[j] < 0 
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) 
    {
        int res = 0;
        vector<int> dp(amount+1, amount+1);
        sort(coins.begin(), coins.end());
        dp[0] = 0;
        for(int i = 1; i <= amount; ++i)
        {
            for(int j = 0; j < coins.size(); ++j)
            {
                if(i - coins[j] < 0)
                    break;
                dp[i] = min(dp[i], dp[i-coins[j]]+1);
            }
        }
        res = dp[amount] == amount+1 ? -1: dp[amount];
        return res;
    }
};
```

## [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少

> 输入: n = 12
> 输出: 3 
> 解释: 12 = 4 + 4 + 4.

思路：
1. 类似与凑零钱，只是零钱的金额为完全平方数
```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <= n; ++i)
        {
            for(int j = 1; j*j <= i; ++j)
            {
                dp[i] = min(dp[i], dp[i-j*j]+1);
            }
        }
        return dp[n];
    }
};
```

## [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)
给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
判断你是否能够到达最后一个位置

> 输入: [2,3,1,1,4]
> 输出: true
> 解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。


```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) 
    {
        int maxn = 0;
        for(int i = 0, j = 0; i <= j; ++i)
        {
            j = max(j,i+nums[i]);
            if(j >= nums.size() - 1)
                return true;
        }
        return false;
    }
};
```

## [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)
给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
你的目标是使用最少的跳跃次数到达数组的最后一个位置
> 输入: [2,3,1,1,4]
> 输出: 2
> 解释: 跳到最后一个位置的最小跳跃数是 2。
> 从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。

思路：
1. dp[i]表示跳到第i个元素时需要的最少步数
2. dp[i] = min(dp[i], dp[i-num[j]+1)
3. dp[0] = 0

```cpp
class Solution {
public:

    int jump(vector<int>& nums) 
    {
        int maxPos = 0, n = nums.size();
        int end = 0, step = 0;
        for (int i = 0; i < n - 1; ++i) 
        {
            maxPos = max(maxPos, i + nums[i]);
            if (i == end) 
            {
                end = maxPos;
                ++step;
            }       
        }
        return step;
    }
};
```

## [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)
给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？

```cpp
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

思路：
1. dp[i]表示1...i为节点组成的二叉搜索树的个数
2. dp[i] += dp[j-1]*dp[i-j]
3. dp[0] = 1;
    dp[1] = 1;
    以i根节点的二叉搜索树的个数为sum(以i-j为根节点的个数*以i+j为根节点的个数) (j= 0...i)
```cpp
class Solution {
public:
    int numTrees(int n) {
       vector<int> dp(n+1);
       dp[0] = 1;
       dp[1] = 1;
       for(int i = 2; i <= n; ++i)
       {
           //以j为根节点的不同情况
           for(int j = 1; j <= i; ++j)
           {
               dp[i] += dp[j-1]*dp[i-j];
           }
       } 
       return dp[n];
    }
};
```
## [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

若这两个字符串没有公共子序列，则返回 0

> 输入：text1 = "abcde", text2 = "ace" 
> 输出：3  
> 解释：最长公共子序列是 "ace"，它的长度为 3。

思路：
1. dp[i][j]表示text1到第i个元素时text2到第j个元素最长公共子序列的值
2. text1[i] == text2[j] : dp[i][j] = dp[i-1][j-1]+1;
3. text1[i] != text2[j] : dp[i][j] = max(dp[i][j-1],dp[i-1][j])
4. res = dp[m][n](m = text1.size(), n = text2.size())
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size()+1, vector<int>(text2.size()+1, 0));
        for(int i = 0; i < text1.size(); ++i)
            dp[i][0] = 0;
        for(int j = 0; j < text2.size(); ++j)
            dp[0][j] = 0;
        for(int i = 0; i < text1.size(); ++i)
        {
            for(int j = 0; j < text2.size(); ++j)
            {
                if(text1[i] == text2[j])
                    dp[i+1][j+1] = dp[i][j]+1;
                else
                    dp[i+1][j+1] = max(dp[i+1][j], dp[i][j+1]);
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```

## [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
给定一个无序的整数数组，找到其中最长上升子序列的长度

> 输入: [10,9,2,5,3,7,101,18]
> 输出: 4 
> 解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
> 说明:
> 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
> 你算法的时间复杂度应该为 O(n2) 。
> 进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?

思路：
1. dp[i]表示第i个元素时的最长上升子序列长度
2. dp[i] = max(dp[i],dp[j]+1)(j:0-i if(nums[i] > nums[j])
3. 由于不是找连续子序列，所以用单调栈去解决实现会出现问题

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size(), 1);
        for(int i = 0; i < nums.size(); ++i)
        {
            for(int j = 0; j < i; ++j)
            {
                if(nums[i] > nums[j])
                    dp[i] = max(dp[i], dp[j]+1);
            }
        }
        int res = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            res = max(res, dp[i]);
        }
        return res;
     
    }
};
```
## [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)
给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
注意:
每个数组中的元素不会超过 100
数组的大小不会超过 200

> 输入: [1, 5, 11, 5]
> 输出: true
> 解释: 数组可以分割成 [1, 5, 5] 和 [11].
> 输入: [1, 2, 3, 5]
> 输出: false
> 解释: 数组不能分割成两个元素和相等的子集.

思路：
1. 划分俩个数组，使各自和相等。转化成求目标值和sum/2的子数组
2. 对于每一个元素，存在俩种选择，选和不选
3. dp[i] : 到第i个元素时，能否找到目标和sum/2的数组
4. dp[i] = dp[i] || dp[j-nums[i]]

```cpp
//递归实现
class Solution {
public:
    bool res = false;
    void find(vector<int>& nums, int index, int target)
    {
        if(res || target < 0 || index >= nums.size())
            return;
        if(target == 0)
        {
            res = true; 
            return;
        }
        find(nums, index+1, target-nums[index]);
        find(nums, index+1, target);
    }
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for(int i = 0; i < nums.size(); ++i)
            sum += nums[i];
        if(sum % 2 == 1)
            return false;
        int target = sum/2;
        find(nums, 0, target);
        return res;
    }
};//备忘录实现
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n=nums.size();
        int sum=0;
        for(int i=0;i<n;i++) sum+=nums[i];
        if(sum%2 == 1) return false;
        sum /= 2;
        vector<bool> dp(sum+1,false);
        dp[0]=true;
        for(int i=0; i < n; i++)
        {
            for(int j = sum;j >= nums[i]; j--)
            {
                dp[j]= dp[j]||dp[j-nums[i]];
            }
        }
        return dp[sum];
    }
};
```

## [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)
给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。

> 输入: "(()"
> 输出: 2
> 解释: 最长有效括号子串为 "()"
> 输入: ")()())"
> 输出: 4
> 解释: 最长有效括号子串为 "()()"
> 利用栈进行括号匹配，弹出有效括号对，剩下的即是无效括号元素

思路：
1. 用栈存储元素下标，如果栈不为空，且将要插入的元素为‘）’，栈顶元素为‘（’，则将栈顶元素弹出，否则压入栈中；
2. 如果此时栈为空，直接返回总长度，代表所有括号都匹配；
3. 如果栈不为空，则依次弹出栈顶元素，作插值（注意栈顶元素可能不为元素总长度，栈底元素可能不为0，需要考虑到），取最大差值，就是我们要求的答案。

```cpp
class Solution {
public:
    int longestValidParentheses(string s) 
    {
        int maxans = 0;
        stack<int> stk;
        for (int i = 0; i < s.length(); i++) 
        {
            if(!stk.empty() && s[stk.top()] == '(' && s[i] == ')')
                stk.pop();
            else
                stk.push(i);
        }

        int tmp = s.size();
        while(!stk.empty())
        {
            maxans = max(maxans, tmp - stk.top() - 1);
            tmp = stk.top();
            stk.pop();
        }
        maxans = max(maxans, tmp);
        return maxans;
    }
};
```
## [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)
给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。
具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

> 输入："abc"
> 输出：3
> 解释：三个回文子串: "a", "b", "c"
> 输入："aaa"
> 输出：6
> 解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"

思路：
1. dp[i][j]表示s[i]-s[j]是否为回文子串
2. dp[i][j] = s[i] == s[j] && dp[i+1][j-1]
3. j == i  dp[i][j]  = true
4. j - i <= 2 若 s[i] == s[j] ,dp[i][j]  = true

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int res = 0;
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size()));
        for(int j = 0; j < s.size(); ++j)
        {
            for(int i = j; i >= 0; --i)
            {
                if(s[i] == s[j] && (j-i <= 2 || dp[i+1][j-1]))
                {
                    dp[i][j] = true;
                    res++;
                }
            }
        }
        return res;
    }
};
```

## [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
> 输入: "babad"
> 输出: "bab"
> 注意: "aba" 也是一个有效答案。
> 输入: "cbbd"
> 输出: "bb"

思路：
1. 找出所有字串，判断是否是回文串
2. 去最大值
```cpp
//暴力解法
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for(int i = 0; i < s.size(); ++i)
        {
            for(int j = i; j < s.size(); ++j)
            {
                string str = s.substr(i,j-i+1);
                if(isvalid(str))
                    res = res.size() > str.size() ? res : str;
            }
        }
        return res;
    }
    bool isvalid(string s)
    {
        int i = 0, j = s.size()-1;
        while(i < j)
        {
            if(s[i] != s[j])
                return false;
            i++;
            j--;
        }
        return true;
    }
};
```
1. dp[i][j] 表示以i为开头，j为结尾的子串是否为回文子串
2. dp[i][j] = (s[i]  == s[j] && dp[i+1][j-1])
3. j == i -> dp[i][j] = true
4.  j - i == 1 -> s[i] == s[j] 则 dp[i][j] = true
5.  j - i == 2 -> s[i] == s[j] dp[i][j] = true

```cpp
class Solution {
public:
    string longestPalindrome(string s)
    {
        if(s.size() < 2) 
        	return s;
        string res;
        int len = 0;
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size()));
        res = s[0];
        for(int j = 0; j < s.size(); j++)
        {
            for(int i = j; i >= 0; i--)
            {
                if(s[i] == s[j] && (j - i <= 2 || dp[i+1][j-1]))
                {
                    dp[i][j] = true;
                    if(j - i > len)
                    {
                        res = s.substr(i, j - i + 1);
                        len = j - i;
                    }
                }
            }
        }
        return res;
    }
};

```

## [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

> 输入: [-2,1,-3,4,-1,2,1,-5,4]
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

思路：
总是希望能加到能使连续数组的综合变大的数；
如果加了之后，比当前值还小，则选择以这个元素为开头重新再来。
1. 利用dp[i] 表征 以nums[i]元素结尾的连续数组的最大和
2. dp[i] = max( nums[i], dp[i-1] + nums[i])
3. res = max(res, dp[i])
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        if(n == 0) return 0;
        vector<int> dp(n);
        dp[0] = nums[0];
        int res = nums[0];
        for(int i = 1; i < n; ++i)
        {
            dp[i] = max(nums[i], nums[i] + dp[i-1]);
            res = max(res, dp[i]);
        }       
        return res;
    }
};
```
## [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)
给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
说明：每次只能向下或者向右移动一步。

> 输入:
> [
> [1,3,1],
> [1,5,1],
> [4,2,1]
> ]
> 输出: 7
> 解释: 因为路径 1→3→1→1→1 的总和最小。

思路：
1. dp[i][j]表示从左上角元素到对应i行j列的元素的路径的最小值
2. dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
3. dp[0][i] = dp[0][i-1]+grid[0][i]
4. dp[i][0] = dp[i-1][0]+grid[i][0];
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n));
        dp[0][0] = grid[0][0];

        for(int i = 1; i < n; ++i)
            dp[0][i] = dp[0][i-1]+grid[0][i];

        for(int i = 1; i < m; ++i)
            dp[i][0] = dp[i-1][0]+grid[i][0];

        for(int i = 1; i < m; ++i)
        {
            for(int j = 1; j < n; ++j)
            {
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }

        return dp[m-1][n-1];
    }
};
```
## [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

> 输入: m = 3, n = 2
> 输出: 3
> 解释:
> 从左上角开始，总共有 3 条路径可以到达右下角。1. 向右 -> 向右 -> 向下;2. 向右 -> 向下 -> 向右;3. 向下 -> 向右 -> 向右

思路：
1. dp[i][j]表示从左上角元素到对应i行j列的元素的路径数
2. dp[i][j] = dp[i-1][j]+ dp[i][j-1]（只能往下走或者往右走）
3. dp[i][0] = 1（往下走的只有一条路径）
4. dp[0][i] = 1（往右走的只有一条路径）
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n));
        for(int i = 0; i < m; ++i)
            dp[i][0] = 1;
        for(int i = 1; i < n; ++i)
            dp[0][i] = 1;   
        for(int i = 1; i < m; ++i)
        {
            for(int j = 1; j < n; ++j)
            {
                dp[i][j] = dp[i-1][j]+ dp[i][j-1];
            }
        }         
        return dp[m-1][n-1];
    }
};
```
## [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

> 输入: s = "leetcode", wordDict = ["leet", "code"]
> 输出: true
> 解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
> 输入: s = "applepenapple", wordDict = ["apple", "pen"]
> 输出: true
> 解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
>     注意你可以重复使用字典中的单词。
> 输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
> 输出: false

思路：
1. dp[i] 表示以是以[0,i-1]长度的字符串是否可以被拆分成wordDict其中的元素
2. dp[i] = (dp[j] && check(s[j..i−1])) , check(s[j,i-1])代表在字典中查询是否是wordDict中的元素
3. dp[0] = true;设定为初始条件，因为题目说明不存在空串，方便初始查找。

说明：
动态规划的题目总是希望能够由已知推出未知，这里dp[i]表示s[0:i-1]个元素是否可以被拆分成wordDict中的元素，其前一状态为dp[i-1],如果dp[i-1] = true ,只要s[i-1]也存在wordDict中，那么dp[i] = true;  对于

> 输入: s = "leetcode", wordDict = ["leet", "leetcode"]
> 输出: true
> 解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。

判断完”leet“存在在wordDict中后，wordDict中”leetcode”便无法再判断出结果，dp[i]不只与前一状态有关，与之前的所有状态可能都有关
因此，每次需要重新判断s.substr(j, i - j)是否存在wordDict中


```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int pos = 0;
        vector<bool> dp(s.size()+1,false);
        unordered_set<string> mp(wordDict.begin(), wordDict.end());
        dp[0] = true;
        for(int i = 1; i <= s.size(); ++i)
        {
            for(int j = 0; j < i; ++j)
            {
                if(dp[j] && mp.find(s.substr(j, i - j))!= mp.end())
                {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.size()];
    }
};
```
## [494. 目标和](https://leetcode-cn.com/problems/target-sum/)
给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

> 输入：nums: [1, 1, 1, 1, 1], S: 3
> 输出：5
> 解释：
> -1+1+1+1+1 = 3
> +1-1+1+1+1 = 3
> +1+1-1+1+1 = 3
> +1+1+1-1+1 = 3
> +1+1+1+1-1 = 3
> 一共有5种方法让最终目标和为3。
> 提示：
> 数组非空，且长度不会超过 20 。
> 初始的数组的和不会超过 1000 。
> 保证返回的最终结果能被 32 位整数存下。

思路：
1. 这里提示32位整数, long long 
2. 递归终点
（1）到达数组的末端
（2）如果到末端，此时S == 0， res++
3. 根据当前nums[index]的符号变化（+，-），改变下一次的目标值

```cpp
class Solution {
public:
    int res = 0;
    int findTargetSumWays(vector<int>& nums, int S)
    {
        find(nums, 0, S);
        return res;
    }
    void find(vector<int>& nums, int index, long long S)
    {
        if(index == nums.size())
        {
            if(S == 0) res++;
            return;
        }       
        find(nums, index+1, S+nums[index]);
        find(nums, index+1, S-nums[index]);
    }
};
```

## [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)
给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。
你可以对一个单词进行如下三种操作：
1. 插入一个字符
2. 删除一个字符
3. 替换一个字符

> 输入：word1 = "horse", word2 = "ros"
> 输出：3
> 解释：
> horse -> rorse (将 'h' 替换为 'r')
> rorse -> rose (删除 'r')
> rose -> ros (删除 'e')
> 输入：word1 = "intention", word2 = "execution"
> 输出：5
> 解释：
> intention -> inention (删除 't')
> inention -> enention (将 'i' 替换为 'e')
> enention -> exention (将 'n' 替换为 'x')
> exention -> exection (将 'n' 替换为 'c')
> exection -> execution (插入 'u')
1. dp[i][j]表示word1[0-i]到word2[0-j]的编辑距离
2. word1和word2可为空，初始化dp[0-i] = i, dp[0-j] = j
3. 这里我们只对一个单词操作，共计三种操作，插入删除替换，等价于对另一个单词删除插入替换
（1）从word1[0-i]编辑到word2[0-j]只需要删除word1第i个元素即可: dp[i][j] = dp[i-1][j] + 1,;
（2）从word1[0-i]编辑到word2[0-j]只需要替换word1第i个元素即可: dp[i][j] = dp[i-1][j-1] + 1,;
（3）从word1[0-i]编辑到word2[0-j]只需要插入word1第i个元素即可: dp[i][j] = dp[i][j-1] + 1,;
每次操作，三者取最小的；当然如果word1[i] == word2[j]则直接跳过，不用加1.

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int len1 = word1.size(), len2 = word2.size();
        vector<vector<int>> dp(len1+1, vector<int>(len2+1, 0));
        
        for(int i = 0; i <= len1; ++i)
            dp[i][0] = i;
        for(int i = 0; i <= len2; ++i)
            dp[0][i] = i;

        for(int i = 1; i <= len1; ++i)
        {
            for(int j = 1; j <= len2; ++j)
            {
                if(word1[i-1] == word2[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1])) + 1;
            }
        }
        
        return dp[len1][len2];
    }
};
```
## [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)
在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

> 输入: 
> 1 0 1 0 0
> 1 0 1 1 1
> 1 1 1 1 1
> 1 0 0 1 0
> 输出: 4

思路：
1. dp[i][j]表示以第i行第j列为右下角所能构成的最大正方形边长
2. dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1

```cpp
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) 
    {
        if(matrix.size() == 0 || matrix[0].size() == 0)
            return 0;
        int maxside = 0;
        int rows = matrix.size();
        int colums = matrix[0].size();
        vector<vector<int>> dp(rows,vector<int>(colums));
        for(int i = 0; i < rows; ++i)
        {
            for(int j = 0; j < colums; ++j)
            {
                if(matrix[i][j] == '1')
                {
                    if(i == 0 || j == 0)
                    {
                        dp[i][j] = 1;
                    }
                    else
                    {
                        dp[i][j] = min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1]) + 1;
                    }
                    maxside = max(maxside, dp[i][j]);
                }
            }
        }
        int maxsquare = maxside*maxside;       
        return maxsquare;
    }
};
```
## [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)
给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
求在该柱状图中，能够勾勒出来的矩形的最大面积。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200831230019151.png#pic_center)
以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200831230038327.png#pic_center)
图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。

> 输入: [2,1,5,6,2,3]
> 输出: 10

思路：
1. 

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int ans = 0;
        vector<int> st;
        heights.insert(heights.begin(),0);
        heights.push_back(0);
        for(int i = 0; i<heights.size(); ++i)
        {
            while(!st.empty() && heights[st.back()] > heights[i])
            {
                int cur = st.back();
                st.pop_back();
                int left = st.back()+1;
                int right = i - 1;
                ans = max(ans,(right - left + 1) * heights[cur]);

            }
            st.push_back(i);
        }
        return ans;
    }
};
```
## [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)
给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

> [
> ["1","0","1","0","0"],
> ["1","0","1","1","1"],
> ["1","1","1","1","1"],
> ["1","0","0","1","0"]
> ]
> 输出: 6

思路：
1. dp[i][j]
```cpp
class Solution {
public:
    int maxrecangle(vector<int> &heights)
    {
        int maxs = 0;
        stack<int> st;

        heights.push_back(-1);
        st.push(-1);
        for(int i = 0; i < heights.size(); ++i)
        {
            while(st.top()!= -1 && heights[i] < heights[st.top()])
            {
                int num = st.top();
                st.pop();
                maxs = max(maxs,heights[num] *(i - st.top() - 1));
            }
            st.push(i);
        }
        return maxs;  
    }

    int maximalRectangle(vector<vector<char>>& matrix) {
        if(!matrix.size()) return 0;
        vector<int> dp(matrix[0].size(),0);
        int maxsquare = 0;
        for(int i = 0; i < matrix.size(); ++i)
        {
            for(int j = 0; j < matrix[0].size(); ++j)
            {
                dp[j] = (matrix[i][j] == '1') ? dp[j] + 1 : 0;
            }
            maxsquare = max(maxsquare,maxrecangle(dp));
        }
        return maxsquare;
    }
};
```


## [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)
给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积

> 输入: [2,3,-2,4]
> 输出: 6
> 解释: 子数组 [2,3] 有最大乘积 6。

思路：
1. dp[i]表示到第i+1个元素时可以得到的最大乘积
2. dp[i] = max(dp[i-1]*nums[i],nums[i])
   ===>>>>>>
            dp_max[i] = max(dp_max[i-1]*nums[i], max(dp_min[i-1]*nums[i], nums[i]));
            dp_min[i] = min(dp_min[i-1]*nums[i], min(dp_max[i-1]*nums[i], nums[i]));
            max = max(max, dp_max[i]);
4. res = max(res, dp[i]);

```cpp
//错误1：未考虑到几个负值可以得到最大乘积的结果
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if(nums.size() == 0)
            return 0;
        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        int res = nums[0];
        for(int i = 1; i < nums.size(); ++i)
        {
            dp[i] = max(dp[i-1]*nums[i], nums[i]);
            res = max(res, dp[i]);
        }
        return res;
    }
};
//添加求得当前最大乘积值和最小乘积值
class Solution
{
public:
    int maxProduct(vector<int>& nums) {
        int maxn = INT_MIN;
        int tmp_max = 1;
        int tmp_min = 1;
        for(int i = 0; i < nums.size(); ++i)
        {
            tmp_max *= nums[i];
            tmp_min *= nums[i];
            if(nums[i] < 0) 
                swap(tmp_max,tmp_min);
            tmp_max = max(tmp_max, nums[i]);
            tmp_min = min(tmp_min, nums[i]);
            maxn = max(tmp_max, maxn);
        }
        return maxn;
    }
};
```

## [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)
给定一个非空的整数数组，返回其中出现频率前 k 高的元素

> 输入: nums = [1,1,1,2,2,3], k = 2
> 输出: [1,2]
> 输入: nums = [1], k = 1
> 输出: [1]

思路：
1. 使用map存储每个元素的频次
2. 使用优先队列，容量为k,依次插入元素

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        vector<int> result;
        for(int i = 0; i < nums.size(); ++i)
        {
            mp[nums[i]]++;
        }
        //(频率，元素)
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int, int>>> p;
        for(auto iter = mp.begin(); iter != mp.end(); iter++)
        {
            if(k == p.size())
            {
                if(iter->second > p.top().first)
                {
                    p.pop();
                    p.push(make_pair(iter->second,iter->first));
                }
            }
            else{
                p.push(make_pair(iter->second,iter->first));
            }
        }
        while(!p.empty())
        {
            result.push_back(p.top().second);
            p.pop();
        }
        reverse(result.begin(),result.end());
        return result;
    }
};
```
## [692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)
给一非空的单词列表，返回前 k 个出现次数最多的单词。
返回的答案应该按单词出现频率由高到低排序。如果不同的单词有相同出现频率，按字母顺序排序

> 输入: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
> 输出: ["i", "love"]
> 解析: "i" 和 "love" 为出现次数最多的两个单词，均为2次。
> 注意，按字母顺序 "i" 在 "love" 之前。
> 输入: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
> 输出: ["the", "is", "sunny", "day"]
> 解析: "the", "is", "sunny" 和 "day" 是出现次数最多的四个单词，
> 出现次数依次为 4, 3, 2 和 1 次。

思路：
和上一题类似

```cpp
class Solution {
public:
    struct cmp{
        bool operator()(pair<string, int>&a, const pair<string, int>&b)
        {
            if(a.second != b.second)
                return a.second > b.second;
            else
                return a.first < b.first;
        }
    };
    vector<string> topKFrequent(vector<string>& words, int k) {
        map<string, int> mp;
        vector<string> vc;
        for(int i = 0; i < words.size(); ++i)
        {
            mp[words[i]]++;
        }
        priority_queue<pair<string, int>, vector<pair<string, int>>, cmp> p;
        for(auto m:mp)
        {
            p.push(m);
            if(p.size() > k)
                p.pop();
        }
        while(!p.empty())
        {
            vc.push_back(p.top().first);
            p.pop();
        }
        reverse(vc.begin(), vc.end());
        return vc;
    }
};
```
## [44. 通配符匹配](https://leetcode-cn.com/problems/wildcard-matching/)
给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '*' 的通配符匹配。

'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。

说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *

> 输入:
> s = "aa"
> p = "a"
> 输出: false
> 解释: "a" 无法匹配 "aa" 整个字符串。

思路：

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        vector<vector<bool>> dp(s.size()+1, vector<bool>(p.size()+1,false));

        dp[0][0] = true;

        for(int i = 1; i <= s.size(); ++i)
            dp[i][0] = false;

        for(int i = 1; i <= p.size(); ++i)
        {
            if(p[i-1] != '*')
                break;
            dp[0][i] =  dp[0][i-1];
        }

        for(int i = 1; i <= s.size(); ++i)
        {
            for(int j = 1; j <= p.size(); ++j)
            {
                if(s[i-1] == p[j-1] || p[j-1] == '?')
                    dp[i][j] = dp[i-1][j-1];
                else if(p[j-1] == '*')
                {
                    dp[i][j] = dp[i-1][j] || dp[i][j-1];
                }
            }
        }  
        return dp[s.size()][p.size()];      
    }
};
```

## [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)
给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

```bash
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。
说明:
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。

> 输入:
> s = "aa"
> p = "a"
> 输出: false
> 解释: "a" 无法匹配 "aa" 整个字符串。
> 输入:
> s = "aa"
> p = "a*"
> 输出: true
> 解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。


```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        if(p.empty())
            return s.empty();
        vector<vector<int>> dp(s.size()+1, vector<int>(p.size()+1, 0));
        dp[0][0] = 1;
        for(int i = 1; i <= p.size(); ++i)
        {
            if(p[i-1] != '*')
                continue;
            else
                dp[0][i] = dp[0][i-2];
        }
        for(int i = 1; i <= s.size(); ++i)
        {
            for(int j = 1; j <= p.size(); ++j)
            {
                if(s[i-1] == p[j-1] || p[j-1] == '.')
                    dp[i][j] = dp[i-1][j-1];
                else if(p[j-1] == '*')
                {
                    dp[i][j] = dp[i][j-2];
                    if(p[j-2] == '.' || p[j-2] == s[i-1])
                    {
                        dp[i][j] = dp[i][j] || dp[i-1][j];
                    }
                }
            }
        }
        return dp[s.size()][p.size()];
    }
};
```
