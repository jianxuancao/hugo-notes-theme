---
# Title, summary, and page position.
title: Graph
linktitle: Graph
summary: Notes on Graph.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---
图就像一个多叉树
邻接表，好处是占用的空间少,但无法快速判断两个节点是否相邻。对于邻接矩阵，我只要看[x][y]是多少就行了

> **一般图**
> 邻接表
> graph[x] 存储 x 的所有邻居节点
> List<Integer>[] graph;
> 
> 邻接矩阵
> boolean[][] matrix, [x][y]记录 x 是否有一条指向 y 的边

> **加权图**
> 邻接表
> graph[x] 存储 x 的所有邻居节点以及对应的权重
> 
> 邻接矩阵
> int[][] matrix, [x][y] 记录 x 指向 y 的边的权重，0 表示不相邻

> **无向图**
> 邻接表
> graph[x] 存储 x 的所有邻居节点以及对应的权重，x 的邻居列表里添加 y，同时在 y 的邻居列表里添加 x。
> 
> 邻接矩阵
> int[][] matrix, [x][y] 记录 x 指向 y 的边的权重，0 表示不相邻
> 对于一个link，[x][y]，[y][x]需要同时赋值，写成双向接通

[**图的遍历**]()
```Java
List<List<Integer>> res = new LinkedList<>();// 记录所有路径
    
public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    LinkedList<Integer> path = new LinkedList<>();//记录过程中经过的路径
    traverse(graph, 0, path);
    return res;
}

void traverse(int[][] graph, int s, LinkedList<Integer> path) {/* 图的遍历框架 */
    path.addLast(s);// 添加节点 s 到路径

    int n = graph.length;
    if (s == n - 1) {// 到达终点
        res.add(new LinkedList<>(path));
        // 可以在这直接 return，但要 removeLast 正确维护 path
        // path.removeLast();
        // return;
        // 不 return 也可以，因为图中不包含环，不会出现无限递归
    }

    for (int v : graph[s]) {// 递归每个相邻节点
        traverse(graph, v, path);
    }
    
    path.removeLast(); // 从路径移出节点 s
}


```


[**无环图中所有可能的路径**](https://leetcode.cn/problems/all-paths-from-source-to-target/)
```Java
List<List<Integer>> result = new LinkedList<>();// 记录所有路径
public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    LinkedList<Integer> path = new LinkedList<>();// 维护递归过程中经过的路径
    traverse(graph, 0, path);
    return result;
}

void traverse(int[][] graph, int s, LinkedList<Integer> path) {
    path.addLast(s);
    if (s == graph.length - 1) {// 到达终点
        result.add(new LinkedList<>(path));
        path.removeLast();
        return;
    }
    for (int v : graph[s]) {// 递归每个相邻节点
        traverse(graph, v, path);
    }
    path.removeLast();
}
```