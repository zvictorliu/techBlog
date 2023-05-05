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

## Bellman-Ford 算法

适合那种限制经过的边数、节点数的问题

核心算法：

```c++
for (auto i = 0; i < n - 1; i++) {
    for (auto j = 0; j < m; j++) {//对m条边进行循环
      auto edge = edges[j];
      // 松弛操作
      if (distance[edge.to] > distance[edge.from] + edge.weight ){ 
        distance[edge.to] = distance[edge.from] + edge.weight;
      }
    }
}
```



- 刚开始源点到其它节点的距离都是INF，那么i=0时只有从源点出发的边能够被修改，完成后代表的是只能经过1点边情况下的最短路径
- 依次类推，遍历完`i`时距离表表示的是最多能经过`i+1`条边所用的最短距离