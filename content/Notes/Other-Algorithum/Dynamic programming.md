---
# Title, summary, and page position.
title: Dynamic programming 
linktitle: Dynamic programming
summary: Notes on Dynamic programming.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True
---

### [**斐波那契数组（从上到下）**](https://leetcode.cn/problems/fibonacci-number/)

这是从上到下的解法，从递归树的头部开始，递归时会发现斐波那契的地递归有很多重复值，使用一个memo就可以不进行重复的递归计算，直接取值降低时间复杂度

```Java
public int fib(int n) {
    int[] memo = new int[n + 1];
    return helper(memo, n);
}

int helper (int [] memo, int n){
    if(n == 0){  // base case
        return 0;
    }else if (n == 1 || n == 2){
        return 1;
    }
    
    if(memo[n] != 0){  //查memo
        return memo[n];
    }

    memo[n] = helper(memo, n - 1) + helper(memo, n - 2);
    return memo[n];
}
```

### [**斐波那契数组（从下到上）**](https://leetcode.cn/problems/fibonacci-number/)

这是从下到上的解法，也就是从0开始用for循环到n，用一个dp数组记录每一个n的斐波那契值，这样dp[n]就是结果。

```Java
public int fib(int n) {
    if (n == 0) return 0;
    int[] dp = new int[n + 1];
    // base case
    dp[0] = 0; dp[1] = 1;

    for(int i = 2; i <= n; i++){
        dp[i] = dp[i - 1] + dp[i -2];
    }

    return dp[n];
}
```

### [**斐波那契数组（最优解）**](https://leetcode.cn/problems/fibonacci-number/)

不管是从下到上还是从上到下，都是用空间换时间（用一个数组来储存之前的值），但是斐波那契数只与前两个值有关系，所以没必要存储整个列表，只要存前两个值动态更新就好了

```Java
public int fib(int n) {
    if (n == 0 || n == 1) return n;
    int n_1 = 0, n_2 = 1;
    
    for(int i = 2; i <= n; i++){
        int n_current = n_1 + n_2;
        n_1 = n_2;
        n_2 = n_current; 
    }

    return n_2;
}
```

### [**凑零钱（从大往小）**](https://leetcode.cn/problems/coin-change/)

使用memo，空间换时间

```Java
int[] memo;

int coinChange(int[] coins, int amount) {
    memo = new int[amount + 1];
    Arrays.fill(memo, -666);    // 备忘录初始化为一个不会被取到的特殊值，代表还未被计算
    return dp(coins, amount);
}

int dp(int[] coins, int amount) {
    if (amount == 0) return 0; 
    if (amount < 0) return -1; // 特殊情况
 
    if (memo[amount] != -666)   //如果不是-666说明计算重复了，直接返回memo内容
        return memo[amount];

    int res = Integer.MAX_VALUE;
    for (int coin : coins) {
        int subProblem = dp(coins, amount - coin);  // 子问题：选择了一个面值的硬币后，剩下的面值怎么用最少的硬币组合
        if (subProblem == -1) continue;              // 无解，跳过
        res = Math.min(res, subProblem + 1);        // 选择最优解，+1是因为当前这个foreach的coin也要算一个位置的
    }
    memo[amount] = (res == Integer.MAX_VALUE) ? -1 : res; // 计算结果存入memo，无解时返回-1
    return memo[amount];
}
```

### [**凑零钱（dp数组迭代方式，从前往后）**](https://leetcode.cn/problems/coin-change/)

```Java
int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1]; // n块钱的时候最少需要几个硬币
    Arrays.fill(dp, amount + 1);
    dp[0] = 0; // base case
   
    for (int i = 0; i < dp.length; i++) { //循环dp的所有可能取值（有100块就从0到100）
        for (int coin : coins) { // 循环尝试硬币的所有选择
            if (i - coin < 0) continue; //面值比需要的价格还大，无解，跳过
            dp[i] = Math.min(dp[i], 1 + dp[i - coin]); // 取最小值（最少的硬币数量）
            // i是几块钱，i-coin是查看使用了一个面值的硬币后，从前面查看剩下的面值最小需要几个硬币
        }
    }
    return (dp[amount] == amount + 1) ? -1 : dp[amount];
}
```

### [**最长递增子序列**](https://leetcode.cn/problems/longest-increasing-subsequence/)

```Java
int lengthOfLIS(int[] nums) {
    int[] dp = new int[nums.length];// dp[i] 表示以 nums[i] 这个数结尾的最长递增子序列的长度
    
    Arrays.fill(dp, 1); // base case：dp数组都为 1，因为每一个元素自己都起码算是一个递增序列
    for (int i = 0; i < nums.length; i++) { // 以每一个元素为开始
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) // 找出每一个小于当前的前序数
                dp[i] = Math.max(dp[i], dp[j] + 1); // 去其对应的dp数字里查出对应位置的的最长序列长度
        }
    }
    
    int res = 0;
    for (int i = 0; i < dp.length; i++) {
        res = Math.max(res, dp[i]);
    }
    return res;
}
```

### [**俄罗斯套娃信封问题**](https://leetcode.cn/problems/russian-doll-envelopes/)

跟最长递增子序列相似，首先，我们对宽边排序，这样保证后面的起码能塞进前一个的宽边（其实替换成高也行），然后就跟上一题一模一样了

```Java
public int maxEnvelopes(int[][] envelopes) { // envelopes = [[w, h], [w, h]...]
    int n = envelopes.length; 

    Arrays.sort(envelopes, new Comparator<int[]>() // 按宽度升序排列，如果宽度一样，则按高度降序排列
    {
        public int compare(int[] a, int[] b) {
            return a[0] == b[0] ? 
                b[1] - a[1] : a[0] - b[0];
        }
    });
    
    int[] height = new int[n]; // 提取高度
    for (int i = 0; i < n; i++)
        height[i] = envelopes[i][1];

    return lengthOfLIS(height); 对高度数组寻找 LIS
}

int lengthOfLIS(int[] nums) {
    // 见前文
}

```

### [**最小编辑距离**](https://leetcode.cn/problems/edit-distance/)

```Java
// 备忘录
int[][] memo;
    
public int minDistance(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    // 备忘录初始化为特殊值，代表还未计算
    memo = new int[m][n];
    for (int[] row : memo) {
        Arrays.fill(row, -1);
    }
    return dp(s1, m - 1, s2, n - 1);
}

int dp(String s1, int i, String s2, int j) {
    if (i == -1) return j + 1;
    if (j == -1) return i + 1;
    // 查备忘录，避免重叠子问题
    if (memo[i][j] != -1) {
        return memo[i][j];
    }
    // 状态转移，结果存入备忘录
    if (s1.charAt(i) == s2.charAt(j)) {
        memo[i][j] = dp(s1, i - 1, s2, j - 1);
    } else {
        memo[i][j] =  min(
            dp(s1, i, s2, j - 1) + 1,
            dp(s1, i - 1, s2, j) + 1,
            dp(s1, i - 1, s2, j - 1) + 1
        );
    }
    return memo[i][j];
}

int min(int a, int b, int c) {
    return Math.min(a, Math.min(b, c));
}

```

### [**最小下降路径（正下，左下，右下）**](https://leetcode.cn/problems/minimum-falling-path-sum/)

```Java
int minFallingPathSum(int[][] matrix) {
    int n = matrix.length;
    int res = Integer.MAX_VALUE;

    memo = new int[n][n];
    for (int i = 0; i < n; i++) {
        Arrays.fill(memo[i], 66666);// 备忘录里的值初始化为 66666
    }
    
    for (int j = 0; j < n; j++) {// 终点可能在 matrix最底部[n-1] 的任意一列,所以要尝试最后一行每一个的最小距离
        res = Math.min(res, dp(matrix, n - 1, j));
    }
    return res;
}

int[][] memo;

int dp(int[][] matrix, int i, int j) {
    
    if (i < 0 || j < 0 ||
        i >= matrix.length ||
        j >= matrix[0].length) {// 索引合法性检查
        return 99999;
    }
    
    if (i == 0) { // base case,i=0 代表已经回到了二维数组的最上面一行，其最小距离就是他自己
        return matrix[0][j];
    }

    if (memo[i][j] != 66666) { // memo节省运算时间
        return memo[i][j];
    }
    
    //memo[i][j] 是当前位置的最小下落距离，从memo默认的66666变成matrix当前位置的自身值加上前面的最小下落距离
    memo[i][j] = matrix[i][j] + min(
                                    dp(matrix, i - 1, j),       //正上方
                                    dp(matrix, i - 1, j - 1),   //左上
                                    dp(matrix, i - 1, j + 1)    //右上
                                );
    return memo[i][j];
}

int min(int a, int b, int c) {
    return Math.min(a, Math.min(b, c));
}
```

### [**最小下降路径（只有正下和右下）**](https://leetcode.cn/problems/minimum-path-sum/)

上一题一样，改一下dp的可能性就完了

```Java
int[][] memo;

int minPathSum(int[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    memo = new int[m][n]; // 构造备忘录，初始值全部设为 -1

    for (int[] row : memo)
        Arrays.fill(row, -1);
    
    return dp(grid, m - 1, n - 1);
}

int dp(int[][] grid, int i, int j) {
    if (i == 0 && j == 0) {//base case
        return grid[0][0];
    }
    if (i < 0 || j < 0) {//error case
        return Integer.MAX_VALUE;
    }
    if (memo[i][j] != -1) {//use memo if memo has the answer
        return memo[i][j];
    }
    memo[i][j] = Math.min(
        dp(grid, i - 1, j),
        dp(grid, i, j - 1)
    ) + grid[i][j];

    return memo[i][j];
}
```

### [**最大子数组和**](https://leetcode.cn/problems/maximum-subarray/)

```Java
public int maxSubArray(int[] nums) {
    if (nums.length == 0) return 0;

    int[] dp = new int[nums.length];  // 定义：dp[i] 记录以 nums[i] 为结尾的「最大子数组和」
    dp[0] = nums[0];  // base case，以nums[0] 为结尾的连续数组的和自然是nums[0]本身，因为其前面没有东西了

    for(int i = 1; i < nums.length; i++){
        dp[i] = Math.max(dp[i-1] + nums[i], nums[i]);
    }
    
    int res = Integer.MIN_VALUE;
    for(int cur : dp){
        res = Math.max(res, cur);
    }

    return res;
}
```

### [**最长公共子序列**](https://leetcode.cn/problems/longest-common-subsequence/)

```Java
int[][] memo;

int longestCommonSubsequence(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    memo = new int[m][n];
    for (int[] row : memo) 
        Arrays.fill(row, -1); // -1代表未计算

    return dp(s1, 0, s2, 0); //s1[0..]和s2[0..]的lcs长度
}

int dp(String s1, int i, String s2, int j) {// 定义：计算 s1[i..] 和 s2[j..] 的最长公共子序列长度
    if (i == s1.length() || j == s2.length()) {   // base case
        return 0;
    }

    if (memo[i][j] != -1) {
        return memo[i][j];
    }
    
    if (s1.charAt(i) == s2.charAt(j)) {//如果字符一样，那么当前位置肯定是互为子串，同时进一，memo里这个位置的值是 1+后面字串的 lcs
        memo[i][j] = 1 + dp(s1, i + 1, s2, j + 1);
    } else {
        memo[i][j] = Math.max( // s1[i] 和 s2[j] 至少有一个不在 lcs 中
            dp(s1, i + 1, s2, j), // s1不在的情况
            dp(s1, i, s2, j + 1)  // s2不在的情况
        );
    }
    return memo[i][j];
}
```

```Java
int longestCommonSubsequence(String s1, String s2) { // 自底向上循环法
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    // 定义：s1[0..i-1] 和 s2[0..j-1] 的 lcs 长度为 dp[i][j]
    // 目标：s1[0..m-1] 和 s2[0..n-1] 的 lcs 长度，即 dp[m][n]
    // base case: dp[0][..] = dp[..][0] = 0

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            // 现在 i 和 j 从 1 开始，所以要减一
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                // s1[i-1] 和 s2[j-1] 必然在 lcs 中
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                // s1[i-1] 和 s2[j-1] 至少有一个不在 lcs 中
                dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
            }
        }
    }

    return dp[m][n];
}
```

### [**抢劫规划**](https://leetcode.cn/problems/house-robber/)

```Java
int[] memo;
public int rob(int[] nums) { //迭代式
    memo = new int[nums.length];
    Arrays.fill(memo, -1);

    return dp(nums, 0);
}

int dp(int[] nums, int start){
    if (start >= nums.length) { // error case
        return 0;
    }
    
    if (memo[start] != -1) return memo[start];

    int result = Math.max(dp(nums, start+2) + nums[start], dp(nums, start+1));
    memo[start] = result;

    return result;
}
```

```Java
int rob(int[] nums) {  //自底向上循环的方法
    int n = nums.length;
    int[] dp = new int[n + 2];
    for (int i = n - 1; i >= 0; i--) {
        dp[i] = Math.max(dp[i + 1], nums[i] + dp[i + 2]);
    }
    return dp[0];
}
```

### [**环形抢劫规划**](https://leetcode.cn/problems/house-robber-ii/)

```Java
public int rob(int[] nums) {
    int n = nums.length;
    if (n == 1) return nums[0];

    int[] memo1 = new int[n];
    int[] memo2 = new int[n];
    Arrays.fill(memo1, -1);
    Arrays.fill(memo2, -1);
    return Math.max(
            dp(nums, 0, n - 2, memo1),  //0和-2，也就是0，-1，-2，其中-1隔开不抢
            dp(nums, 1, n - 1, memo2)   //1和-1，也就是1，0，-1，其中0隔开不抢
    );
}

// 定义：计算闭区间 [start,end] 的最优结果
int dp(int[] nums, int start, int end, int[] memo) {
    if (start > end) { // error case
        return 0;
    }

    if (memo[start] != -1) {
        return memo[start];
    }

    int res = Math.max(
            dp(nums, start + 2, end, memo) + nums[start],
            dp(nums, start + 1, end, memo)
    );

    memo[start] = res;
    return res;
}
```

### [**二叉树抢劫**](https://leetcode.cn/problems/house-robber-iii/)

```Java
Map<TreeNode, Integer> memo = new HashMap<>();

public int rob(TreeNode root) {
    if (root == null) return 0; //base case, 当前位置没有房子

    if (memo.containsKey(root)) //memo case
        return memo.get(root);

    // 抢，去下下家
    int do_it = root.val + 
    (root.left == null ? 0 : rob(root.left.left) + rob(root.left.right)) + //
    (root.right == null ? 0 : rob(root.right.left) + rob(root.right.right));
    
    int not_do = rob(root.left) + rob(root.right);// 不抢，去下家

    int res = Math.max(do_it, not_do);
    memo.put(root, res);
    return res;
}
```