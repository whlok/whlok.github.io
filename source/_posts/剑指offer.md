---
title: 剑指offer
date: 2023-10-25 18:53:13
categories: 数据结构与算法
---

## [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)
请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

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

## [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数

现有矩阵 matrix 如下：
```bash
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
给定 target = 5，返回 true。
给定 target = 20，返回 false。


```cpp
//二分
class Solution {
public:
    bool findtarget(vector<int>& mat, int left, int right, int target)
    {
        while(left <= right)
        {
            int mid = (left+right)/2;
            if(mat[mid] == target)
                return true;
            else if(mat[mid] < target)
            {
                left = mid+1;
            }
            else
            {
                right = mid-1;
            }
        }
        return false;
    }
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        for(int i = 0; i < matrix.size(); ++i)
        {
            if(findtarget(matrix[i], 0, matrix[0].size()-1, target))
                return true;
        }
        return false;
    }
};
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.size() == 0)
            return false;
        int i = 0;
        int j = matrix[0].size() - 1;
        while(i < matrix.size() && j >= 0)
        {
            if(matrix[i][j] < target)
                i++;
            else if(matrix[i][j]>target)
                j--;
            else
                return true;
        }     
        return false;
    }
};
```


## [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)
给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18

> 输入: 2
> 输出: 1
> 解释: 2 = 1 + 1, 1 × 1 = 1

```cpp
class Solution {
public:
    int cuttingRope(int n) {
        vector<int> dp(n+1,0);
        if(n <= 0) return 0;
        dp[1] = 1;
        for(int i = 2; i <= n; ++i)
        {
            int res = -1;
            for(int j = 1; j < i; ++j)
            {
                res = max(res,max(dp[i-j]*j,(i-j)*j));
            }
            dp[i] = res;
        }
        return dp[n];
    }
};
```

## [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的

> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(0);
        ListNode* ret = head;
        while(l1 != NULL && l2 != NULL)
        {
            if(l1->val <= l2->val)
            {
                head->next = l1;
                l1 = l1->next;
            }
            else
            {
                head->next = l2;
                l2 = l2->next;
            }
            head = head->next;

        }
        if(l1 == NULL)
            head->next = l2;
        if(l2 == NULL)
            head->next = l1;
        return ret->next;
    }
};

```

## [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)
给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值
```bash
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

```

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) 
    {
        vector<int> ans;
        deque<int> dq;
        for(int i = 0; i < nums.size(); ++i)
        {
            while(!dq.empty() && dq.back() < nums[i])
                dq.pop_back();
            dq.push_back(nums[i]);
            if(i >= k-1)
            {
                ans.push_back(dq.front());
                if(nums[i-k+1] == dq.front())
                    dq.pop_front();
            }
        }
        return ans;
    }
};
```

## [剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)
在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

```bash
输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

```cpp
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        if(grid.size() == 0 || grid[0].size() == 0)
            return 0;
        int m = grid.size();
        int n = grid[0].size();

        for(int i = 1; i < m; ++i)
            grid[i][0] += grid[i-1][0];
        for(int j = 1; j < n; ++j)
            grid[0][j] += grid[0][j-1];
        
        for(int i = 1; i < m; ++i)
        {
            for(int j = 1; j < n; ++j)
            {
                grid[i][j] += max(grid[i-1][j],grid[i][j-1]);
            }
        }
        return grid[m-1][n-1];
    }
};
```

## [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

> 输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
> 输出：true

```cpp
class Solution 
{
public:
    bool dfs(vector<vector<char>> &board, string word, int i, int j , int w)
    {
        if(i < 0 || j < 0 || i >= board.size() || j >= board[0].size() || board[i][j] != word[w]) 
            return false;
        if(w == word.length() - 1) 
            return true;
        char tmp = board[i][j];
        board[i][j] = 0;
        if(    dfs(board,word,i-1,j,w+1)
            || dfs(board,word,i,j+1,w+1)
            || dfs(board,word,i+1,j,w+1)
            || dfs(board,word,i,j-1,w+1))
            return true;
        board[i][j] = tmp;
        return false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        for(int i = 0; i < board.size(); ++i)
        {
            for(int j = 0; j < board[0].size(); ++j)
            {
                if(dfs(board,word,i,j,0))
                {
                    return true;
                }
            }
        }
        return false;
    }
};
```

## [剑指 Offer 63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)
假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

> 输入: [7,1,5,3,6,4]
> 输出: 5
> 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
> 注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int inf = 1e9;
        int minprice = inf, maxprofit = 0;
        for(int price: prices)
        {
            maxprofit = max(maxprofit, price - minprice);
            minprice = min(price, minprice);
        }
        return maxprofit;       
    }
};
```

## [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分

> 输入：nums = [1,2,3,4]
> 输出：[1,3,2,4] 
> 注：[3,1,2,4] 也是正确的答案之一。

```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int low = 0;
        int high = nums.size()-1;
        while(low < high)
        {
            while(low < high && nums[low] % 2 == 1)
                low++;
            swap(nums[low],nums[high]);
            while(low < high && nums[high] % 2 == 0)
                high--;
            swap(nums[low],nums[high]);
        }
        return nums;
    }
};
```

## [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

若队列为空，pop_front 和 max_value 需要返回 -1

> 输入: 
> ["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
> [[],[1],[2],[],[],[]]
> 输出: [null,null,null,2,1,2]

```cpp
class MaxQueue {
public:
    MaxQueue() {
    }
    
    int max_value() {
        if(maxqt.empty())
            return -1;
        return maxqt.front();
    }
    
    void push_back(int value) {
        qt.push(value);
        while(!maxqt.empty() && maxqt.back() < value)
        {
            maxqt.pop_back();
        }
        maxqt.push_back(value);
        
    }
    
    int pop_front() {
        if(qt.empty())
            return -1;
        int res = qt.front();
        qt.pop();
        if(res == maxqt.front())
            maxqt.pop_front();
        
        return res;
    }
private:
    queue<int> qt;
    deque<int> maxqt;
};
```

## [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)
一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1

> 输入：n = 2
> 输出：2
> 输入：n = 7
> 输出：21

```cpp
class Solution {
public:
    int numWays(int n) {
        int dp[101];
        int dq[101];
        dp[0] = 1;
        dp[1] = 1; 
        for(int i = 2; i < 101; ++i)
        {
            dp[i] = (dp[i-1] + dp[i-2])%1000000007;
            
        }
        return dp[n];
    }
};
```

## [剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
> 输出：[1,2,3,6,9,8,7,4,5]

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
            vector<int> res;

            if(matrix.size() == 0|| matrix[0].size() == 0) return res;
            int t = 0; int b = matrix.size() - 1;
            int l = 0; int r = matrix[0].size() - 1;

            for(;;)
            {
                //左至右
                for(int i = l; i <= r; ++i)
                    res.push_back(matrix[t][i]);                
                if(++t > b) break;
                for(int i = t; i <= b; ++i)
                    res.push_back(matrix[i][r]);                
                if(--r < l) break;
                for(int i = r; i >= l; --i)
                    res.push_back(matrix[b][i]);                
                if(--b < t) break;    
                for(int i = b; i >= t; --i)
                    res.push_back(matrix[i][l]);                
                if(++l > r) break;  
            }
            
            return res;
    }
};
```

## [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)
给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

```bash
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
思路：
维护一个单调递减的双端队列
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        deque<int> dq;
        for(int i = 0; i < nums.size(); ++i)
        {
            while(!dq.empty() && dq.back() < nums[i])
                dq.pop_back();
            dq.push_back(nums[i]);
            if(i >= k-1)
            {
                ans.push_back(dq.front());
                if(nums[i-k+1] == dq.front())
                    dq.pop_front();
            }
        }
        return ans;
    }
};
```

## [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点

> 给定一个链表: 1->2->3->4->5, 和 k = 2.
> 返回链表 4->5.

思路：
利用一个节点先走k步，然后另外一个节点开始走，走到末尾就可以找到倒数第k个节点了
```cpp
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* p1 = head;
        ListNode* p2 = p1;
        while(k--)
        {
            p2 = p2->next;
        }
        while(p2 != NULL)
        {
            p1 = p1->next;
            p2 = p2->next;
        }
        return p1;
    }
};
```
## [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

> 输入: "the sky is blue"
> 输出: "blue is sky the"

思路: 
1. 去掉头尾的空格
2. 从尾部开始遍历，遍历到空格的地方，利用substr() 截取对应长度的字符串，加入结果结合中
```cpp
class Solution {
public:
    string reverseWords(string s) {
        if(s.empty()) return s;
        int len  = 0;
        string res;
        for(int i = s.size() - 1; i >= 0; --i)
        {
            if(s[i] == ' '&& len != 0)
            {
                res+=s.substr(i+1,len)+" ";
                len = 0;
                continue;
            }
            if(s[i] != ' ') len++;
        }
        if(len != 0) res+=s.substr(0,len) +" ";
        if(res.size() > 0) res.erase(res.size() - 1,1);
        return res;
    }
};
```
## [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)
请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。
若队列为空，pop_front 和 max_value 需要返回 -1

>输入: 
>["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
>[[],[1],[2],[],[],[]]
>输出: [null,null,null,2,1,2]
>输入: 
>["MaxQueue","pop_front","max_value"]
>[[],[],[]]
>输出: [null,-1,-1]

```cpp
class MaxQueue {
public:
    MaxQueue() {
    }
    
    int max_value() {
        return maxqt.empty() ? -1 : maxqt.front();
    }
    
    void push_back(int value) {
        qt.push(value);
        while(!maxqt.empty() && maxqt.back() < value)
            maxqt.pop_back();
        maxqt.push_back(value);
    }
    
    int pop_front() {
        if(qt.empty())
            return -1;
        int front = qt.front();
        qt.pop();
        if(front == maxqt.front())
            maxqt.pop_front();
        return front;
    }
private:
    queue<int> qt;
    deque<int> maxqt;
};
```

## [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)
给定一棵二叉搜索树，请找出其中第k大的节点

```bash
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```
思路：
对于二叉搜索树来说，中序遍历可以得到有序数组，第k大即为倒数第k个元素
```cpp
class Solution {
public:
    int kthLargest(TreeNode* root, int k) {
        vector<int> res;
        traverse(root,res); 
        // for(int i = 0; i < res.size(); ++i)  
        //     std::cout << res[i]<<" ";
        return res[res.size() - k];
    }
    void traverse(TreeNode* root, vector<int>& res)
    {
        if(root == NULL)
            return;
        traverse(root->left,res);     
        res.push_back(root->val);
        traverse(root->right,res);
    }
};
```

## [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)
定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* p = head;
        ListNode* q = NULL;
        while(p != NULL)
        {
            ListNode* tmp = p->next;
            p->next = q;
            q = p;
            p = tmp;
        }
        return q;
    }
};
```
## [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。
示例:
给定如下二叉树，以及目标和 sum = 22
思路：
1.  递归终止条件：当前目标和为0，且为叶子节点
2. 
```bash
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
[
   [5,4,11,2],
   [5,8,4,5]
]
```

```cpp
class Solution {
public:
    vector<vector<int>> vs;
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<int> vc;
        path(root, sum, vc);
        return vs;
    }
    void path(TreeNode* root, int sum, vector<int>& vc)
    {
        if(root == NULL) 
            return;
        int tmp = sum - root->val;
        vc.push_back(root->val);
        if(tmp == 0 && !root->left && !root->right)
        {
            vs.push_back(vc);
        }
        path(root->left, tmp, vc);
        path(root->right, tmp, vc);
        vc.pop_back();
    }
};
```

## [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
例如，
[2,3,4] 的中位数是 3
[2,3] 的中位数是 (2 + 3) / 2 = 2.5
设计一个支持以下两种操作的数据结构：
void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数

> 输入：
> ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
> [[],[1],[2],[],[3],[]]
> 输出：[null,null,null,1.50000,null,2.00000]
> 输入：
> ["MedianFinder","addNum","findMedian","addNum","findMedian"]
> [[],[2],[],[3],[]]
> 输出：[null,null,2.00000,null,2.50000]



```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }
    
    void addNum(int num) {
        lt.push(num);
        ht.push(lt.top());
        lt.pop();

        if(lt.size() < ht.size())
        {
            lt.push(ht.top());
            ht.pop();
        }
    }
    
    double findMedian() {
        return lt.size() > ht.size() ? (double) lt.top() : (lt.top() + ht.top()) * 0.5;
    }
private:
    //将较小的部分存在大根堆，较大的部分存在小根堆
    //大根堆元素个数 - 小根堆元素个数 = 0 或者 1
    priority_queue<int, vector<int>, greater<int>> ht;
    priority_queue<int> lt;
};
```

## [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)
输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
要求时间复杂度为O(n)

> 输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

1. dp[i]:第i个数时,连续最大子数组和
2. dp[i] = max(dp[i-1]+nums[i], nums[i])
3. dp[i] = nums[0]
4. maxs = max(dp[i], maxs)
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        vector<int> dp(nums.size()+1, 0);
        dp[0] = nums[0];
        int maxs = dp[0];
        for(int i = 1; i < nums.size(); ++i)
        {
            dp[i] = max(dp[i-1]+nums[i], nums[i]);
            maxs = max(maxs, dp[i]);
        }
        return maxs;
    }
};

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum = 0;
        int maxs = INT_MIN;

        for(int num: nums)
        {
            if(sum <= 0)
                sum = num;
            else
                sum += num;
            maxs = max(maxs, sum);
        }
        
        return maxs;
    }
};


```

## [剑指 Offer 67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)
写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。
当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。
该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0。
说明：
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

> 这里是引用
> 输入: "42"
> 输出: 42
> 输入: "   -42"
> 输出: -42
> 解释: 第一个非空白字符为 '-', 它是一个负号。
> 我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
> 输入: "4193 with words"
> 输出: 4193
> 解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字
> 输入: "words and 987"
> 输出: 0
> 解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
> 因此无法执行有效的转换
> 输入: "-91283472332"
> 输出: -2147483648
> 解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
> 因此返回 INT_MIN (−231)

思路：
1. 
```cpp
class Solution {
public:
    int strToInt(string str) {
        long res = 0;
        int flag = 1, index = 0;
        while(index < str.size() && str[index] == ' ')
            index++;
        for(int i = index; i< str.size(); ++i)
        {
            if(i == index && (str[index] == '-' || str[index] == '+'))
            {
                if(str[index] == '-')
                {
                    flag = -1;
                }
                else if(str[index] == '+')
                {
                    flag = 1;
                }
            }
            else if(isdigit(str[i]))
            {
                res = (res*10+(str[i]-'0'));
                if(flag == 1 && res > INT_MAX)
                    return INT_MAX;
                else if(flag == -1 && -res < INT_MIN)
                    return INT_MIN;
            }
            else
                break;
        }
        return res*flag;
    }
};
```

## [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)
输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

> 例如:
> 给定的树 A:
> 输入：A = [1,2,3], B = [3,1]
> 输出：false

思路：
1. 二叉树相关首先想到递归解决，递归问题应当单独设计一个子函数，避免受一些特殊情况
2. 递归终止条件：
（1）B == NULL 子树查找完成: TRUE
（2）B != NULL && A == NULL: FALSE
3. 递归顺序：
（1）当前节点
（2）左子树
（3）右子树
```cpp
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(B == NULL || A == NULL) return false;
        return dfs(A,B) || isSubStructure(A->left, B) || isSubStructure(A->right, B);
    }
    bool dfs(TreeNode*A, TreeNode*B)
    {
        if(B == NULL) return true;
        if(A == NULL) return false;
        return A->val == B->val && dfs(A->left, B->left) && dfs(A->right, B->right);
    }
};
```
## [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200910141948527.png#pic_center)

> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
> 输出: 3
> 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL)
            return NULL;
        if(root == p || root == q)
            return root;
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p,q);
        if(left != NULL && right != NULL)
            return root;
        if(left != NULL)
            return left;
        if(right != NULL)
            return right;
        return NULL;
    }
};
```
