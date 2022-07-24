---
# Title, summary, and page position.
title: Dynamic programming 
linktitle: Dynamic programming
summary: Notes on Dynamic programming.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---

[**斐波那契数组（从上到下）**](https://leetcode.cn/problems/fibonacci-number/)
这是从上到下的解法，从递归树的头部开始，递归时会发现斐波那契的地递归有很多重复值，使用一个memo就可以不进行重复的递归计算，直接取值降低时间复杂度
```Java
public int fib(int n) {
    int[] memo = new int[n + 1];
    return helper(memo, n);
}

int helper (int [] memo, int n){
    if(n == 0){
        return 0;
    }else if (n == 1 || n == 2){
        return 1;
    }
    
    if(memo[n] != 0){
        return memo[n];
    }

    memo[n] = helper(memo, n - 1) + helper(memo, n - 2);
    return memo[n];
}
```


[**斐波那契数组（从下到上）**](https://leetcode.cn/problems/fibonacci-number/)
这是从下到上的解法，也就是从0开始用for循环到n，用一个dp数组记录每一个n的斐波那契值，这样dp[n]就是结果。
```Java
public int fib(int n) {
    if (n == 0) return 0;
    int[] dp = new int[n + 1];
    // base case
    dp[0] = 0; dp[1] = 1;

    for(int i = 2, i <= n; i++){
        dp[i] = dp[i - 1] + dp[i -2];
    }

    return dp[n];
}
```


[**斐波那契数组（最优解）**](https://leetcode.cn/problems/fibonacci-number/)
不管是从下到上还是从上到下，都是用空间换时间（用一个数组来储存之前的值），但是斐波那契数只与前两个值有关系，所以没必要存储整个列表，只要存前两个值动态更新就好了
```Java
public int fib(int n) {
    if (n == 0 || n == 1) return n;
    int n_1 = 0, n_2 = 1;
    
    for(int i = 2, i <= n; i++){
        int n_current = n_1 + n_2;
        n_1 = n_2;
        n_2 = n_current; 
    }

    return n_2;
}
```


[**凑零钱（从大往小）**](https://leetcode.cn/problems/coin-change/)
使用memo，空间换时间
```Java
int[] memo;

int coinChange(int[] coins, int amount) {
    memo = new int[amount + 1];
    Arrays.fill(memo, -666);// 备忘录初始化为一个不会被取到的特殊值，代表还未被计算
    return dp(coins, amount);
}

int dp(int[] coins, int amount) {
    if (amount == 0) return 0;
    if (amount < 0) return -1;
 
    if (memo[amount] != -666)   // 查备忘录
        return memo[amount];

    int res = Integer.MAX_VALUE;
    for (int coin : coins) {
        int subProblem = dp(coins, amount - coin); // 计算子问题
        if (subProblem == -1) continue; // 无解
        res = Math.min(res, subProblem + 1); // 选择最优解，+1是因为当前这个foreach的coin也要算一个位置的
    }
    memo[amount] = (res == Integer.MAX_VALUE) ? -1 : res; // 计算结果存入memo
    return memo[amount];
}
```


[**凑零钱（dp数组迭代方式，从前往后）**](https://leetcode.cn/problems/coin-change/)
使用memo，空间换时间
```Java
int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1]; // dp中i块钱的时候需要几个硬币
    Arrays.fill(dp, amount + 1);
    dp[0] = 0; // base case
   
    for (int i = 0; i < dp.length; i++) { //循环在遍历所有状态的所有取值
        for (int coin : coins) { //循环尝试所有选择
            if (i - coin < 0) continue; //无解，跳过
            dp[i] = Math.min(dp[i], 1 + dp[i - coin]);//取最小值（最少的硬币数量）
            //i是几块钱，i-coin是查看使用了一个面值的硬币后，从前面查看剩下的面值最小需要几个硬币
        }
    }
    return (dp[amount] == amount + 1) ? -1 : dp[amount];
}
```
