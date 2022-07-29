---
# Title, summary, and page position.
title: Backpack problems
linktitle: Backpack problems
summary: Notes on Dynamic programming - Backpack problems.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---
背包问题其实本质上也是动态规划的一个子问题

[**基础背包**]()
```Java
int knapsack(int W, int N, int[] wt, int[] val) {
    int[][] dp = new int[N + 1][W + 1]; //dp[n][w]： 对前n个物品，剩余w空间的时候，可能的最大价值
    for (int i = 1; i <= N; i++) { //遍历所有的物品
        for (int w = 1; w <= W; w++) { //遍历所有的剩余容量
            if (w < wt[i - 1]) { //剩余容量不够装了，这种情况下只能选择不装入背包
                dp[i][w] = dp[i - 1][w];
            } else { // 装入或者不装入背包，择优
                dp[i][w] = Math.max(
                    dp[i - 1][w - wt[i-1]] + val[i-1], 
                    dp[i - 1][w]
                );
            }
        }
    }
    return dp[N][W];
}
```


[]()
```Java

```


[]()
```Java

```


[]()
```Java

```