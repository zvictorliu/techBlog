---
title: 拓扑排序
description: topological sort
date: 2023-05-04
categories:
  - "coding"
tags:
  - "图"

---

拓扑排序简单来说就是剥皮

<!--more-->

## 依赖关系和环

- 课程表先修问题
- 华为机考的题

## 最小高度

从最简单的情况开始，逐步加上去，就不难发现这个规律了

- leetcode t310

```c++
class Solution { // 拓扑排序
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        // 要想到拓扑排序，首先得意识到，这个最短树它其实是可以一层一层给剖出来的
        // 没有认识到这个规律就想不到拓扑排序
        vector<int> res;
        //! 总有比较奇葩的特殊情况
        if (n == 1) {
            res.push_back(0);
            return res;
        }
        // todo 邻接表（比邻接矩阵好一些）
        vector<vector<int>> adjMap(n);
        vector<int> degree(n,0); // 记录度的表，拓扑排序需要这个
        for (auto edge: edges){
            adjMap[edge[0]].push_back(edge[1]);
            adjMap[edge[1]].push_back(edge[0]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }

        queue<int> leaves;
        // todo 加入叶子节点（度为1的）
        for (int i = 0; i < n; i++){
            if (degree[i]==1) leaves.push(i);
        }

        // todo BFS遍历，一直到最后一层就是最短树的节点了，这是建立在认识到的规律得出的结论上的
        while (!leaves.empty())
        {
            // todo 叶子节点取出来，要放进res里面，后面会被更新，最后在其中的就是目标
            res.clear(); //* 需要更新
            // todo 取出这一层叶子节点的方法
            //? 我之前想的是队列放入的是一个个向量，但其实可以通过个数来做到
            int sz = leaves.size();
            for (int i = 0; i < sz; i++){ // 出队多少次
                int leaf = leaves.front();
                leaves.pop();
                res.push_back(leaf); // 放入res
                // todo 出队后，更新度和相邻节点的度
                degree[leaf]--; //? 这还是必要的
                for (auto neibor: adjMap[leaf]){
                    degree[neibor]--;
                    if (degree[neibor] == 1) leaves.push(neibor); //? 在这里就顺便一起判断了
                }
            }
        }
        return res;
        
    }
};
```

## 定义的安全节点

其实感觉和最小高度类似，想必也是有类似的规律，只是这次不见得是度为1的

- leetcode t802

```c++
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        // 出度为0是终端节点
        // graph[i]中都是终端节点的是安全节点
        // 终端节点一定是安全节点
        // 如果去掉第一层终端节点，新产生的终端节点在原来的图上一定是都连向终端节点的，因此也一定是安全节点
        // 如果去掉第二层终端节点，新产生的终端节点在原来的图上，一定有连向第二层的，那么就不是安全节点
        // 所以应该就只是前两层的终端节点？还是都是？好吧，这也算
        vector<int> res;
        int N = graph.size();
        vector<int> degree(N);
        queue<int> terminals;
        // todo 还需要一个入的graph
        vector<vector<int>> parents(N);
        for (int i = 0; i < N; i++){
            degree[i] = graph[i].size();
            if (degree[i] == 0) terminals.push(i);
            for (auto cd : graph[i]){
                parents[cd].push_back(i);
            }
        }
        

        int cnt = 0;
        while (cnt !=2 && !terminals.empty()){ //只看前两层
            int sz = terminals.size();
            for (int i = 0; i < sz; i++){
                int tml = terminals.front();
                terminals.pop();
                res.push_back(tml);
                degree[tml] --;
                for (auto neibor : parents[tml]){
                    degree[neibor]--;
                    if (degree[neibor] == 0) terminals.push(neibor);
                }
            }
            cnt ++;
        }
        sort(res.begin(), res.end());
        return res;

    }
};
```

