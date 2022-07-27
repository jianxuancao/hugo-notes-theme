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

[**DFS遍历图**]()
```Java
boolean[] visited;// 记录被遍历过的节点
boolean[] onPath;// 记录从起点到当前节点的路径

void traverse(Graph graph, int s) { //s初始为0，代表起始节点
    if (visited[s]) return;
    
    visited[s] = true;// 经过节点s，标记为已遍历
    onPath[s] = true;// 做选择：标记节点 s 在路径上
    for (int neighbor : graph.neighbors(s)) { //遍历每一个邻居
        traverse(graph, neighbor);
    }
    onPath[s] = false;// 撤销选择：节点 s 离开路径
}

```


[**节点0到节点n-1的所有可能路径**](https://leetcode.cn/problems/all-paths-from-source-to-target/)
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
        res.add(new LinkedList<>(path)); //一条路径完成，添加至result
        path.removeLast();//由于一条路径完成，path需要清除这最后一步，为上一个节点的别的可能出路做好准备
        return;
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


[**环检测算法(课程表)(DFS)**](https://leetcode.cn/problems/course-schedule/)
```Java
class Solution {
    boolean[] onPath;// 记录一次递归堆栈中的节点
    boolean[] visited;// 记录遍历过的节点，防止走回头路
    boolean hasCycle = false;// 记录图中是否有环

    boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new LinkedList[numCourses];  //建立图的邻接表（一维：每一科）（二维：每一科的前置要求）
        //如果有环证明出现了鸡生蛋，蛋生鸡的环
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new LinkedList<>();
        }
        for (int[] edge : prerequisites) {
            graph[edge[1]].add(edge[0]); // 修完课程 from 才能修课程 to
        }
        
        visited = new boolean[numCourses];
        onPath = new boolean[numCourses];
        for (int i = 0; i < numCourses; i++) {
            traverse(graph, i);
        }
        return !hasCycle;
    }

    void traverse(List<Integer>[] graph, int s) {
        if (onPath[s]) { // 出现环
            hasCycle = true;
        }
        if (visited[s] || hasCycle) {// 如果已经找到了环，也不用再遍历了    
            return;
        }
        // 把当前判断的科目设为已经遍历的课
        visited[s] = true;
        onPath[s] = true;
        for (int t : graph[s]) {
            traverse(graph, t);
        }
        onPath[s] = false;
    }
}
```


[**输出依赖图（拓扑排序）**](https://leetcode.cn/problems/course-schedule-ii/)
```Java
List<Integer> postorder = new ArrayList<>();// 记录后序遍历结果
boolean hasCycle = false;// 记录是否存在环
boolean[] visited, onPath;

public int[] findOrder(int numCourses, int[][] prerequisites) {
    List<Integer>[] graph = new LinkedList[numCourses];  //建立图的邻接表（一维：每一科）（二维：每一科的前置要求）
    //如果有环证明出现了鸡生蛋，蛋生鸡的环
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new LinkedList<>();
    }
    for (int[] edge : prerequisites) {
        graph[edge[1]].add(edge[0]); // 修完课程 from 才能修课程 to
    }

    visited = new boolean[numCourses];
    onPath = new boolean[numCourses];
    for (int i = 0; i < numCourses; i++) {
        traverse(graph, i);
    }
    if (hasCycle) {// 有环图无法进行拓扑排序
        return new int[]{};
    }
    
    Collections.reverse(postorder);// 反转后序遍历结果即为拓扑排序结果
    int[] res = new int[numCourses];
    for (int i = 0; i < numCourses; i++) {
        res[i] = postorder.get(i);
    }
    return res;
}

void traverse(List<Integer>[] graph, int s) {
    if (onPath[s]) { // 出现环
        hasCycle = true;
    }
    if (visited[s] || hasCycle) {// 如果已经找到了环，也不用再遍历了    
        return;
    }
    // 把当前判断的科目设为已经遍历的课
    visited[s] = true;
    onPath[s] = true;
    for (int t : graph[s]) {
        traverse(graph, t);
    }
    postorder.add(s); //后续遍历是从终点开始返回的（left是头）
    onPath[s] = false;
}
```


[**环检测算法（BFS 版本）**]()
1. 队列进行初始化后，从入度为 0 的节点开始加入队列
2. 开始BFS，从队列中弹出一个节点，减少相邻节点的入度，同时将新产生的入度为 0 的节点加入队列
3. 继续弹出节点，直到队列为空
4. 如果队列中没有0度的节点说明出现了环
```Java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    List<Integer>[] graph = new LinkedList[numCourses];  //建立图的邻接表（一维：每一科）（二维：每一科的前置要求）
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new LinkedList<>();
    }
    for (int[] edge : prerequisites) {
        graph[edge[1]].add(edge[0]); // 修完课程 from 才能修课程 to
    }
    int[] indegree = new int[numCourses];// 构建入度数组

    for (int[] edge : prerequisites) { //给所有节点计算有几个入度
        int from = edge[1], to = edge[0];
        indegree[to]++;// 节点 to 的入度加一
    }

    Queue<Integer> q = new LinkedList<>(); // 根据入度初始化队列中的节点
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) {
            q.offer(i);// 节点 i 没有入度，即没有依赖的节点root,作为拓扑排序的起点，加入队列
        }
    }

    int count = 0; // 记录遍历的节点个数
    while (!q.isEmpty()) {// BFS 循环
        int cur = q.poll();
        count++;
        for (int next : graph[cur]) { //当前节点的每一个可能的出路
            indegree[next]--; // 进入一个节点，把他的入度减一
            if (indegree[next] == 0) {
                // 如果入度变为 0，说明 next 依赖的节点都已被遍历
                // 代表可以进入下一个深度了
                q.offer(next);
            }
        }
    }
    return count == numCourses;// 如果所有节点都被遍历过，说明不成环
}
```