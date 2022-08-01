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
        backtrack(nums, i + 1);  // 通过 start 参数控制树枝的遍历，避免产生重复的子集
        track.removeLast(); // 撤销选择
    }
}
```

**形式一、元素无重不可复选，即 `nums` 中的元素都是唯一的，每个元素最多只能被使用一次**

[**组合**](https://leetcode.cn/problems/combinations/)

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

### [**排列**](https://leetcode.cn/problems/permutations/)

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

**形式二、元素可重不可复选，即 `nums` 中的元素可以存在重复，每个元素最多只能被使用一次**。

以组合为例，如果输入 `nums = [2,5,2,1,2]`，和为 7 的组合应该有两种 `[2,2,2,1]` 和 `[5,2]`。

**形式三、元素无重可复选，即 `nums` 中的元素都是唯一的，每个元素可以被使用若干次**。
