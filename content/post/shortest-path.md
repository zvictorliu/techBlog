---
title: 最短路问题
description: shortest path
date: 2023-05-04
categories:
  - "coding"
tags:
  - "图"


---

此处主要主要介绍两种算法

<!--more-->

## Dijkstra 算法

贪心算法，和最小生成树的Prim算法很接近，只是是以起点为中心找总的最短的路径，会那个就会这个

## Floyd 算法

运用了动态规划的思想，状态转移方程为：
$$
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
$$
即，节点i和j之间的最短路径，应当是比较【直达】还是【换乘】

开始数组`dist`初始化为任意两个节点之间的路径，然后三重循环遍历

```c++
for (int k = 0; k < v; ++k){
    for (int i = 0; i < v; ++i){
        for (int j = 0; j < v; ++j){
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
        }
    }
}
```

这个先后顺序应该是有影响的，还是画矩阵图比较好记