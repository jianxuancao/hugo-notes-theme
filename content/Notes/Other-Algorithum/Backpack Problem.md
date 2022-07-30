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


[**完全背包**](https://leetcode.cn/problems/coin-change-2/submissions/)
```Java
public int change(int amount, int[] coins) {
    int[][] dp = new int[coins.length + 1][amount + 1]; //dp[n][w]： 对前n个物品，剩余w空间的时候，可能的最大价值

    //base case
    for(int i = 0; i <= coins.length; i++){
        coins[i][0] = 1;
    }

    for (int i = 1; i <= coins.length; i++) { //遍历所有硬币
        for (int w = 1; w <= amount; w++) { //遍历所有的剩余容量
            if (j - coins[i-1] >= 0)
                dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]];
            else 
                dp[i][j] = dp[i - 1][j];
        }
    }
    return dp[coins.length][amount];
}

```


[**分出两个一样值的子集**](https://leetcode.cn/problems/partition-equal-subset-sum/)
```Java
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int num : nums) sum += num;
    
    if (sum % 2 != 0) return false; // 和为奇数时，不可能划分成两个和相等的集合

    int n = nums.length;
    sum = sum / 2; //半个背包
    
    boolean[][] dp = new boolean[n + 1][sum + 1]; 
    // true: 对于容量为 j 的背包，若只是用前 i 个物品，可以有一种方法把背包恰好装满。

    //base case: dp[..][0] = true 和 dp[0][..] = false
    //因为背包没有空间的时候，就相当于装满了，而当没有物品可选择的时候，肯定没办法装满背包。
    for (int i = 0; i <= n; i++)
        dp[i][0] = true;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= sum; j++) {
            if (j < nums[i - 1]) {
                dp[i][j] = dp[i - 1][j]; // 背包容量不足，不能装入第 i 个物品
            } else {
                //装入或不装入背包,只要有个true就可以，所以用 or
                //如果不把 nums[i](第i个物品)装入背包，是否能够恰好装满背包，取决于上一个状态 dp[i-1][j]
                //如果把 nums[i](第i个物品)装入了背包，是否能够恰好装满背包，取决于dp[i-1][j-nums[i-1]]
                dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]];
            }
        }
    }
    return dp[n][sum];
}
```


[]()
```Java

```