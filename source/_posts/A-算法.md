---
title: A* 算法
date: 2023-10-24 18:35:04
categories: 数据结构与算法
tags: 图论
---

## 实现原理

A*算法是一种常用于图形搜索和路径规划的启发式搜索算法。它通过综合考虑实际代价和估计代价，来寻找从起点到目标节点的最优路径。

算法步骤：

1. 创建两个空列表：Open列表和Closed列表。
2. 将起点添加到Open列表。
3. 重复以下步骤，直到找到目标节点或Open列表为空： a. 从Open列表中选择估计代价最小的节点，将其移动到Closed列表。 b. 遍历该节点的相邻节点：
   - 如果相邻节点已经在Closed列表中，跳过它。
   - 如果相邻节点不在Open列表中，计算其实际代价（从起点到当前节点的代价）和估计代价（从当前节点到目标节点的估计代价），并将其添加到Open列表。
   - 如果相邻节点已经在Open列表中，检查经过当前节点到达该相邻节点的路径是否更优，如果更优，则更新其实际代价和父节点。
4. 如果Open列表为空，表示没有找到目标节点，搜索失败；否则，从目标节点开始，沿着父节点指针一直回溯到起点，即可得到最优路径。

## 代码实现

```java
import java.util.*;

class Node {
    int x;
    int y;
    int g; // Actual cost from start node to this node
    int h; // Heuristic cost from this node to the goal node
    int f; // f = g + h
    Node parent;

    public Node(int x, int y) {
        this.x = x;
        this.y = y;
        this.g = 0;
        this.h = 0;
        this.f = 0;
        this.parent = null;
    }
}

public class AStarAlgorithm {
    private int[][] grid; // 2D array representing the map/grid
    private int rows;
    private int cols;
    private Node startNode;
    private Node goalNode;
    private List<Node> openList;
    private List<Node> closedList;

    public AStarAlgorithm(int[][] grid, int startX, int startY, int goalX, int goalY) {
        this.grid = grid;
        this.rows = grid.length;
        this.cols = grid[0].length;
        this.startNode = new Node(startX, startY);
        this.goalNode = new Node(goalX, goalY);
        this.openList = new ArrayList<>();
        this.closedList = new ArrayList<>();
    }

    public List<Node> findPath() {
        openList.add(startNode);

        while (!openList.isEmpty()) {
            Node current = findLowestFInOpenList();
            openList.remove(current);
            closedList.add(current);

            if (current.x == goalNode.x && current.y == goalNode.y) {
                return reconstructPath(current);
            }

            for (int i = -1; i <= 1; i++) {
                for (int j = -1; j <= 1; j++) {
                    if (i == 0 && j == 0) continue; // Skip current node

                    int neighborX = current.x + i;
                    int neighborY = current.y + j;

                    if (neighborX >= 0 && neighborX < rows && neighborY >= 0 && neighborY < cols) {
                        if (grid[neighborX][neighborY] == 0) continue; // Skip obstacles

                        Node neighbor = new Node(neighborX, neighborY);
                        if (closedList.contains(neighbor)) continue;

                        int tentativeG = current.g + 1; // Assuming each move has a cost of 1 (can be modified for different costs)

                        if (!openList.contains(neighbor) || tentativeG < neighbor.g) {
                            neighbor.parent = current;
                            neighbor.g = tentativeG;
                            neighbor.h = calculateHeuristic(neighbor);
                            neighbor.f = neighbor.g + neighbor.h;

                            if (!openList.contains(neighbor)) {
                                openList.add(neighbor);
                            }
                        }
                    }
                }
            }
        }

        return null; // Path not found
    }

    private Node findLowestFInOpenList() {
        return openList.stream().min(Comparator.comparingInt(node -> node.f)).orElse(null);
    }

    private int calculateHeuristic(Node node) {
        // Here, we can use any heuristic function (e.g., Manhattan distance, Euclidean distance, etc.)
        return Math.abs(node.x - goalNode.x) + Math.abs(node.y - goalNode.y);
    }

    private List<Node> reconstructPath(Node current) {
        List<Node> path = new ArrayList<>();
        while (current != null) {
            path.add(0, current);
            current = current.parent;
        }
        return path;
    }

    public static void main(String[] args) {
        int[][] grid = {
            {1, 1, 1, 1},
            {1, 0, 0, 1},
            {1, 1, 1, 1},
            {1, 1, 1, 1}
        };

        int startX = 0;
        int startY = 0;
        int goalX = 3;
        int goalY = 3;

        AStarAlgorithm aStar = new AStarAlgorithm(grid, startX, startY, goalX, goalY);
        List<Node> path = aStar.findPath();

        if (path != null) {
            for (Node node : path) {
                System.out.println("(" + node.x + ", " + node.y + ")");
            }
        } else {
            System.out.println("Path not found!");
        }
    }
}
```

