---
# Title, summary, and page position.
title: Island Search
linktitle: Island Search
summary: Notes on Island Search.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True
---

岛屿问题其实本质上就是当站在陆地上时，淹掉岛，直到整个岛都变成海，然后去寻找下一个岛，直到全部变成海。

[200. 岛屿数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/)

```Java
public int numIslands(char[][] grid) {
    int count = 0;
    for (int i = 0; i < grid.length; i++) { // 遍历 grid
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') {
                count++; // 每发现一个岛屿，岛屿数量加一
                dfs(grid, i, j); // 然后使用 DFS 将岛屿淹了
            }
        }
    }
    return count;
}

void dfs(char[][]grid, int i, int j){
    if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length){ // 超范围了
        return;
    }
    if(grid[i][j] == '0'){ // 下水了，这条路就没必要再继续了
		return;
    }
    grid[i][j] = '0'; //淹掉
    dfs(grid, i - 1, j);
    dfs(grid, i + 1, j);
    dfs(grid, i, j + 1);
    dfs(grid, i, j - 1);
}
```

