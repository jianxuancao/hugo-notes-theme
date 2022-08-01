---
# Title, summary, and page position.
title: Brute force search
linktitle: Brute force search
summary: Notes on Brute force search.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True
---

## 排列、组合、子集问题

**生成子集是所有的问题的基本框架**

```Java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>(); // 记录回溯算法的递归路径

public List<List<Integer>> subsets(int[] nums) {
    backtrack(nums, 0);
    return res;
}

void backtrack(int[] nums, int start) {
    res.add(new LinkedList<>(track)); // 前序位置，每个节点的值都是一个子集
    for (int i = start; i < nums.length; i++) { // 从start位置行进到nums数组的末尾
        track.addLast(nums[i]); // 做选择
        backtrack(nums, i + 1);  // 通过为i+1的start参数控制树枝的遍历，避免产生重复的子集, 如果是i，那就允许重复使用
        track.removeLast(); // 撤销选择
    }
}
```

### **形式一、元素无重不可复选，即 `nums` 中的元素都是唯一的，每个元素最多只能被使用一次**

#### **框架**

```Java
 /* 组合/子集 框架 */
void backtrack(int[] nums, int start) {
    for (int i = start; i < nums.length; i++) {
        track.addLast(nums[i]);  // 做选择
        backtrack(nums, i + 1); 
        track.removeLast(); // 撤销选择
    }
}

/* 排列框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) { // 剪枝逻辑
            continue;
        }
        // 做选择
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        
        // 撤销选择
        track.removeLast();
        used[i] = false;
    }
}

```

#### [**组合**](https://leetcode.cn/problems/combinations/)

```Java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>();// 记录回溯算法的递归路径

public List<List<Integer>> combine(int n, int k) {
    backtrack(1, n, k);
    return res;
}

void backtrack(int start, int n, int k) {
    if (k == track.size()) { // base case, 遍历到了第 k 层，收集当前节点的值
        res.add(new LinkedList<>(track)); 
        return;
    }
    for (int i = start; i <= n; i++) {
        track.addLast(i);  // 选择
        backtrack(i + 1, n, k); // 通过 start 参数控制树枝的遍历，避免产生重复的子集
        track.removeLast(); // 撤销选择
    }
}
```

#### [**排列**](https://leetcode.cn/problems/permutations/)

```Java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>();// 记录回溯算法的递归路径
boolean[] used; // track 中的元素会被标记为 true

public List<List<Integer>> permute(int[] nums) {
    used = new boolean[nums.length];
    backtrack(nums);
    return res;
}

void backtrack(int[] nums) {
    if (track.size() == nums.length) { // base case，到达叶子节点，叶子保存着最后的排序结果
        res.add(new LinkedList(track));
        return;
    }
    for (int i = 0; i < nums.length; i++) { // 遍历的是可能进行排序的数
        if (used[i]) { // 已经存在 track 中的元素，不能重复使用，跳过
            continue;
        }

        used[i] = true; // 做选择
        track.addLast(nums[i]); // 把选中的要排列的数放入轨迹内
        
        backtrack(nums); // 进入下一层回溯树
        
        track.removeLast(); // 取消选择
        used[i] = false; 
    }
}
```

### **形式二、元素可重不可复选，即 `nums` 中的元素可以存在重复，每个元素最多只能被使用一次**。

以组合为例，如果输入 `nums = [2,5,2,1,2]`，和为 7 的组合应该有两种 `[2,2,2,1]` 和 `[5,2]`。

#### **框架**

```Java
Arrays.sort(nums);
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    for (int i = start; i < nums.length; i++) {
        if (i > start && nums[i] == nums[i - 1]) { // 剪枝逻辑，跳过值相同的相邻树枝
            continue;
        }
        
        track.addLast(nums[i]); // 做选择
        backtrack(nums, i + 1);
        track.removeLast();// 撤销选择
    }
}

Arrays.sort(nums);
/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) { // 剪枝逻辑
            continue;
        }
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) { // 剪枝逻辑，固定相同的元素在排列中的相对位置
            continue;
        }
        // 做选择
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        
        // 撤销选择
        track.removeLast();
        used[i] = false;
    }
}
```

#### [**子集**](https://leetcode.cn/problems/subsets-ii/)

给你一个整数数组 `nums`，其中可能包含重复元素，请你返回该数组所有可能的子集。比如输入 `nums = [1,2,2]`，你应该输出：[ [],[1],[2],[1,2],[2,2],[1,2,2] ]

```java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>();

public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums); // 先排序，让相同的元素靠在一起
    backtrack(nums, 0);
    return res;
}

void backtrack(int[] nums, int start) {
    res.add(new LinkedList<>(track));// 前序位置，每个节点的值都是一个子集
    
    for (int i = start; i < nums.length; i++) {
        if (i > start && nums[i] == nums[i - 1]){ // 剪枝，值相同的相邻树枝，只遍历第一条。比如输入数组有俩2，那有的时候相邻的会出现[1,2],[1,2']
            continue;
        }
        track.addLast(nums[i]);
        backtrack(nums, i + 1);
        track.removeLast();
    }
}
```

#### [40. 组合总和 II - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-ii/)

这题本质上跟子集是一样的，唯一不同的是只有路径和是正好等于target的时候才add到result set

```Java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>();
int trackSum = 0;

public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    Arrays.sort(candidates); // 先排序，让相同的元素靠在一起
    backtrack(candidates, 0, target);
    return res;
}

void backtrack(int[] nums, int start, int target) {
    if(trackSum == target){
        res.add(new LinkedList<>(track));// 前序位置，每个节点的值都是一个子集
    }
    
    if (trackSum > target) { // base case，超过目标和，直接结束,否则时间太长了
        return;
    }

    for (int i = start; i < nums.length; i++) {
        if (i > start && nums[i] == nums[i - 1]){ // 剪枝，值相同的相邻树枝，只遍历第一条。比如输入数组有俩2，那有的时候相邻的会出现[1,2],[1,2']
            continue;
        }
        track.addLast(nums[i]);
        trackSum += nums[i];
        backtrack(nums, i + 1, target);
        track.removeLast();
        trackSum -= nums[i];
    }
}
```

#### [排列](https://leetcode.cn/problems/permutations-ii/)

比如输入 `nums = [1,2,2]`，函数返回 `[ [1,2,2],[2,1,2],[2,2,1] ]`

本质上仍是全排列，但是因为有两个2，所以会出现12‘2和122’，但是他们其实是一样的，所以就要用一个used数组来去除已经用过的元素

```Java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>();
boolean[] used; //记录nums数组里的元素在当前路径上有没有被用过

public List<List<Integer>> permuteUnique(int[] nums) {
    Arrays.sort(nums); // 先排序，让相同的元素靠在一起
    used = new boolean[nums.length];
    backtrack(nums);
    return res;
}

void backtrack(int[] nums) {
    if (track.size() == nums.length) { // 只保留叶子
        res.add(new LinkedList(track));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) { 
            continue;
        }
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) { // 新添加的剪枝逻辑，固定相同的元素在排列中的相对位置
            // 如果前面的相邻相等（nums[i] == nums[i - 1]）元素没有用过（!used[i - 1]），则跳过
            continue;
        }
        track.add(nums[i]);
        used[i] = true;
        backtrack(nums);
        track.removeLast();
        used[i] = false;
    }
}
```

### **形式三、元素无重可复选，即 `nums` 中的元素都是唯一的，每个元素可以被使用若干次**。

```Java
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    for (int i = start; i < nums.length; i++) {       
        track.addLast(nums[i]);// 做选择
        backtrack(nums, i);
        track.removeLast(); // 撤销选择
    }
}

/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        track.addLast(nums[i]);// 做选择
        backtrack(nums);
        track.removeLast();// 撤销选择
    }
}

```

#### [39. 组合总和 - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum/)

```Java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>(); // 记录回溯算法的递归路径
int sum = 0;
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    backtrack(candidates, 0, target);
    return res;
}

void backtrack(int[] nums, int start, int target) {
    if(sum + nums[start] == target){
    	res.add(new LinkedList<>(track)); // 前序位置，每个节点的值都是一个子集
        return;
    }

    for (int i = start; i < nums.length; i++) { // 从start位置行进到nums数组的末尾
        track.addLast(nums[i]); // 做选择
        sum += nums[i];
        backtrack(nums, i, target);  // 通过 start 参数控制树枝的遍历，避免产生重复的子集
        track.removeLast(); // 撤销选择
        sum -= nums[i];
    }
}
```

