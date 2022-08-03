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

### [200. 岛屿数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/)

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

### [1254. 统计封闭岛屿的数目 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-closed-islands/)

```Java
public int closedIsland(int[][] grid) {
    int count = 0;
    int m = grid.length, n = grid[0].length;
    for(int j = 0; j < n; j++){
        dfs(grid, 0, j);// top
        dfs(grid, m - 1, j);// bottom
    }
    for(int i = 0; i < m; i++){  
        dfs(grid, i, 0);// left
        dfs(grid, i, n - 1);// right
    }
    
    for (int i = 0; i < m; i++) { // 遍历 grid
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 0) {
                count++; // 每发现一个岛屿，岛屿数量加一
                dfs(grid, i, j); // 然后使用 DFS 将岛屿淹了
            }
        }
    }
    return count;
}
void dfs(int[][]grid, int i, int j){
    if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length){ // 超范围了
        return;
    }
    if(grid[i][j] == 1){ // 下水了，这条路就没必要再继续了
        return;
    }
    grid[i][j] = 1; //淹掉
    dfs(grid, i - 1, j);
    dfs(grid, i + 1, j);
    dfs(grid, i, j + 1);
    dfs(grid, i, j - 1);
}
```

### [695. 岛屿的最大面积 - 力扣（LeetCode）](https://leetcode.cn/problems/max-area-of-island/submissions/)

```Java
public int maxAreaOfIsland(int[][] grid) {
    int size = 0;
    int m = grid.length, n = grid[0].length;
            
    for (int i = 0; i < m; i++) { // 遍历 grid
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) {
                size = Math.max(size, dfs(grid, i, j));
            }
        }
    }
    return size;
}
int dfs(int[][]grid, int i, int j){
    if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length){ // 超范围了
        return 0;
    }
    if(grid[i][j] == 0){ // 下水了，这条路就没必要再继续了
        return 0;
    }
    grid[i][j] = 0; //淹掉
    return dfs(grid, i - 1, j) + dfs(grid, i + 1, j) + dfs(grid, i, j + 1) + dfs(grid, i, j - 1) + 1;
}
```

### [1905. 统计子岛屿 - 力扣（LeetCode）](https://leetcode.cn/problems/count-sub-islands/)

```Java
public int countSubIslands(int[][] grid1, int[][] grid2) {
    int size = 0;
    int m = grid1.length, n = grid1[0].length;
            
    for (int i = 0; i < m; i++) { // 遍历 grid
        for (int j = 0; j < n; j++) {
            if (grid2[i][j] == 1 && grid1[i][j] == 0) {
                dfs(grid2, i, j);
            }
        }
    }
    for (int i = 0; i < m; i++) { // 遍历 grid
        for (int j = 0; j < n; j++) {
            if (grid2[i][j] == 1) {
                size++;
                dfs(grid2, i, j);
            }
        }
    }
    return size;
}
```

