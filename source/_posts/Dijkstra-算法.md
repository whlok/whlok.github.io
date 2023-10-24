---
title: Dijkstra 算法
date: 2023-10-24 18:42:54
categories: 数据结构与算法
tags: 图论
---

## 思想

**贪心:找到该点到所有点的最短路径**

俩个集合, 一个(dis)是源点到当前该点的最短路径,一个(visited)是源点到该点是否已经找到最短路径; 初始时设定dis[] = INT_MAX, dis[start] = 0,后续需要比较是否可以找到比当前dis[i]小的值 每轮循环分别找到当前未被访问的点中到源点最短距离的点,加入visited集合中 每轮循环更新dis集合,根据确定的minpos,找到未被访问的点,且小于dis[minpos]+adj[minpos][i]时更新;

## 代码实现

```c++
#include <iostream>
#include <vector>
#include <limits.h>
#include <algorithm>
using namespace std;

// 邻接矩阵-无向图
// adj[i][j] == 0 表示 i j 不相邻
vector<vector<int>> adj = {
    {0,1,6,3,0}, 
    {1,0,7,1,2},
    {6,7,0,2,0},
    {3,1,2,0,7},
    {0,2,0,7,0}
};

// 基于邻接矩阵dijkstra求解最短路
int dijkstra(vector<vector<int>> adj, int& start, int& end, vector<int>&path)
{
    int n = adj.size();
    if(start >= n || start < 0 || end >= n || end < 0)
        return -1;
    vector<int> dis(n, INT_MAX);    // 当前点到源点的最短距离
    vector<bool> visited(n, false); // 访问集合
    vector<int> pre(n, -1);       // 当前点最短路径的前一节点

    dis[start] = 0;                 // 保证初始查找点为源点start
    pre[start] = start;          // 设置源点的prepos为start

    for(;;)
    {
        int mindis = INT_MAX;
        int minpos = -1;

        // 找到本轮查找的初始点
        for(int i = 0; i < n; ++i)
        {
            if(!visited[i] && dis[i] < mindis)
            {
                mindis = dis[i];
                minpos = i;
            }
        }
        // 不存在邻接路径
        if(minpos == -1)
            break;
        visited[minpos] = true;
        // 如果终点在确认集合中，则退出查找,保存路径
        if(visited[end])
        {
            int tmp = end;
            path.push_back(end);
            while(pre[tmp] != start && pre[tmp] != -1)
            {
                path.push_back(pre[tmp]);
                tmp = pre[tmp];
            }
            path.push_back(start);
            reverse(path.begin(), path.end());
            break;
        }
        for(int j = 0; j < n; ++j)
        {
            if(!visited[j] && adj[minpos][j] > 0)
            {
                if(dis[j] > dis[minpos] + adj[minpos][j])
                {
                    dis[j] = dis[minpos] + adj[minpos][j];
                    pre[j] = minpos;
                }
            }
        }
    }

    return dis[end];
}

void printpath(vector<int>&path)
{
        // 打印路径
    for(int i = 0; i < path.size(); ++i)
    {
        if(i != path.size()-1)
        {
            cout << path[i]+1 <<"->";
        }
        else
        {
            cout << path[i]+1;
        }
    }      
    cout << endl;
}
int main()
{
    int start, end;
    cin >> start >> end;
    vector<int> path;
    int mindis = dijkstra(adj, start, end, path);
    cout << mindis << endl;
    printpath(path);
    return 0;
}

```

