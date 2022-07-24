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