---
title: Leetcode算法之回溯
date: 2023-10-25 18:41:52
categories: 数据结构与算法
tags: Leetcode
---

## [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)
给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

> board =
> [
> ['A','B','C','E'],
> ['S','F','C','S'],
> ['A','D','E','E']
> ]
> 给定 word = "ABCCED", 返回 true
> 给定 word = "SEE", 返回 true
> 给定 word = "ABCB", 返回 false
> board 和 word 中只包含大写和小写英文字母。
> 1 <= board.length <= 200
> 1 <= board[i].length <= 200
> 1 <= word.length <= 10^3

思路：
回溯的思想其实也是暴力的一种
1. 找到第一个与word[0]字母相同的字母，然后从她的四个方向查找，找之前将该节点元素保存，源位置置换为‘\0'，保证下一次不会查找到这个元素，等回退到这一层时在还原该元素。
2. 只有当index和word.size()-1相等时，代表所有元素查找完成并存在，返回真
3. 当查找范围超出界限，或者与当前word[index]不相等，则返回假，回退。
```cpp
class Solution {
public:
    bool dfs(vector<vector<char>>& board, string& word, int x, int y, int index)
    {
        if(x < 0 || x >= board.size() || y < 0 || y >= board[0].size() || board[x][y] != word[index])
            return false;
        if(index == word.size()-1)
            return true;
        char tmp = board[x][y];
        board[x][y] = '0';
        if(    dfs(board, word, x-1, y, index+1)
            || dfs(board, word, x+1, y, index+1)
            || dfs(board, word, x, y-1, index+1)
            || dfs(board, word, x, y+1, index+1))
            return true;
        board[x][y] = tmp;
        return false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        for(int i = 0; i < board.size(); ++i)
        {
            for(int j = 0; j < board[0].size(); ++j)
            {
                if(dfs(board, word, i, j, 0))
                    return true;
            }
        }
        return false;
    }
};
```

## [46. 全排列](https://leetcode-cn.com/problems/permutations/)
给定一个 没有重复 数字的序列，返回其所有可能的全排列。

> 输入: [1,2,3]
> 输出:
> [
> [1,2,3],
> [1,3,2],
> [2,1,3],
> [2,3,1],
> [3,1,2],
> [3,2,1]
> ]

思路：
1. 结束条件：当每轮路径存储的元素与nums元素的个数一致时，返回
2. 选择列表：visited[i] == false， 回溯的时候如果需要避免一些未被访问的元素，我们添加visited数组表示
3. 回退操作：路径数组中弹出这轮循环中的元素，visited重新置未false
```cpp
class Solution {
public:
    vector<vector<int>> result;
    void dfs(vector<int>& nums, vector<int>& vc, vector<bool>&visited)
    {
        if(vc.size() == nums.size())
        {
            result.push_back(vc);
            return;
        }
        for(int i = 0; i < nums.size(); ++i)
        {
            if(visited[i])
                continue;
            visited[i] = true;
            vc.push_back(nums[i]);
            dfs(nums, vc, visited);
            vc.pop_back();
            visited[i] = false;            
        }

    }
    vector<vector<int>> permute(vector<int>& nums) 
    {
        vector<int> vc;
        vector<bool> visited(nums.size(), false);
        dfs(nums, vc, visited);
        return result;
    }
};
```
## [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)
给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的数字可以无限制重复被选取。

说明：
所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 

> 输入：candidates = [2,3,6,7], target = 7,
> 所求解集为：
> [
> [7],
> [2,2,3]
> ]

思路：
1. 终止条件：组合和为大于等于target,等于时保存，大于时回退
2. 选择列表：由于元素可以被无限次数使用，所以不需要visited数组
3. 回退列表：将当前元素弹出，目标和对应减去
4. 去重：每次查找只需要以当前元素为起点查找即可

```cpp
class Solution {
public:
    vector<vector<int>> res;
    void dfs(vector<int>& candidates, int target, vector<int>& vc, int index)
    {
        if(target == 0)
        {
            res.push_back(vc);
            return;
        }
        else if(target < 0)
            return;

        for(int i = index; i < candidates.size(); ++i)
        {
            vc.push_back(candidates[i]);
            dfs(candidates, target-candidates[i], vc, i);
            vc.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> vc;
        dfs(candidates,target,vc,0);
        return res;
    }
};
```
## [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

> 输入：n = 3
> 输出：[
>   "((()))",
>   "(()())",
>   "(())()",
>   "()(())",
>   "()()()"
> ]

思路：
1. 终止条件：
```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        func(res, "", 0, 0, n);
        return res;
    }
    
    void func(vector<string> &res, string str, int l, int r, int n){
        if(l > n || r > n || r > l) return ;
        if(l == n && r == n) {res.push_back(str); return;}
        func(res, str + '(', l+1, r, n);
        func(res, str + ')', l, r+1, n);
        return;
    }
};
```
## [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围

```cpp
输入:
[
['1','1','1','1','0'],
['1','1','0','1','0'],
['1','1','0','0','0'],
['0','0','0','0','0']
]
输出: 1

```

```cpp
class Solution {
public:
    int res = 0;
    void dfs(vector<vector<char>>& grid, int i, int j)
    {
        if(i < 0 || j < 0 || i >= grid.size() || j >= grid[0].size() || grid[i][j] != '1')
        {
            return;
        }
        grid[i][j] = '0';
        dfs(grid, i-1, j);
        dfs(grid, i+1, j);
        dfs(grid, i, j-1);
        dfs(grid, i, j+1);
        
        return;
    }
    int numIslands(vector<vector<char>>& grid) {
        for(int i = 0; i < grid.size(); ++i)
        {
            for(int j = 0; j < grid[0].size(); ++j)
            {
                if(grid[i][j] == '1')
                {
                    res++;
                    dfs(grid, i , j);
                }
            }
        }
        return res;
    }
};
```
## [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)
给定一个包含了一些 0 和 1 的非空二维数组 grid 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

```cpp
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
 对于上面这个给定矩阵应返回 6。注意答案不应该是 11 ，因为岛屿只能包含水平或垂直的四个方向的 1 。
```

```cpp
class Solution {
public:
    int res = 0;
    void dfs(vector<vector<int>>& grid, int x, int y, int& tmp)
    {
        if(x < 0 || y < 0 || x >= grid.size() || y >= grid[0].size() || grid[x][y] == 0)
            return;
        tmp++;
        grid[x][y] = 0;
        dfs(grid, x+1, y, tmp);
        dfs(grid, x-1, y, tmp);
        dfs(grid, x, y-1, tmp);
        dfs(grid, x, y+1, tmp);
        return;
    }
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int tmp;
        for(int i = 0; i < grid.size(); ++i)
        {
            for(int j = 0; j < grid[0].size(); ++j)
            {
                if(grid[i][j] == 1)
                {
                    tmp = 0;
                    dfs(grid, i, j, tmp);
                    res = max(res, tmp);
                }
            }
        }
        return res;
    }
};
```
