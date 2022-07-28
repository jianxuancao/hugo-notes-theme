---
# Title, summary, and page position.
title: Back Trace (Tree)
linktitle: Back Trace (Tree)
summary: Notes on Back Trace.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---
回溯算法的框架就是DFS遍历，同时在遍历到叶的时候，判断是否符合条件，如果符合，保存下来
```Java
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

[**全排列**](https://leetcode.cn/problems/permutations/)
这个题其实就是回溯算法的基本框架
```Java
List<List<Integer>> res = new LinkedList<>();

/* 主函数，输入一组不重复的数字，返回它们的全排列 */
List<List<Integer>> permute(int[] nums) {
    
    LinkedList<Integer> track = new LinkedList<>();// 记录「路径」
    boolean[] used = new boolean[nums.length];// 「路径」中的元素会被标记为true,避免重复使用
    
    backtrack(nums, track, used);
    return res;
}

// 路径：记录在 track 中
// 选择列表：nums 中不存在于 track 的那些元素（used[i] 为 false）
// 结束条件：nums 中的元素全都在 track 中出现
void backtrack(int[] nums, LinkedList<Integer> track, boolean[] used) {    
    if (track.size() == nums.length) {// 触发结束条件
        res.add(new LinkedList(track));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) {// nums[i] 已经在 track 中，跳过
            continue;
        }
        track.add(nums[i]);// 做选择
        used[i] = true;
        backtrack(nums, track, used);// 进入下一层决策树
        track.removeLast();// 取消选择
        used[i] = false;
    }
}

```