---
# Title, summary, and page position.
title: Back Track
linktitle: Back Track
summary: Notes on Back Track.
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

List<List<Integer>> permute(int[] nums) { /* 主函数，输入一组不重复的数字，返回它们的全排列 */
    LinkedList<Integer> track = new LinkedList<>(); // 记录「路径」
    boolean[] used = new boolean[nums.length]; // 「路径」中的元素会被标记为true,避免重复使用
    
    backtrack(nums, track, used);
    return res;
}

// 路径：记录在 track 中
// 选择列表：nums 中不存在于 track 的那些元素（used[i] 为 false）
// 结束条件：nums 中的元素全都在 track 中出现
void backtrack(int[] nums, LinkedList<Integer> track, boolean[] used) {    
    if (track.size() == nums.length) { // 触发结束条件
        res.add(new LinkedList(track));
        return;
    }

    for (int i = 0; i < nums.length; i++) { // 
        if (used[i]) { // nums[i] 已经在 track 中，跳过（比如1，2，下一个就不能是1或者2了）
            continue;
        }
        track.add(nums[i]);// 做选择, 前序
        used[i] = true;
        backtrack(nums, track, used);// 进入下一层决策树
        track.removeLast();// 当前节点的子节点遍历完成，返回到上一层，取消选择，后序
        used[i] = false;
    }
}

```


[**n皇后**](https://leetcode.cn/problems/n-queens/)
```Java
List<List<String>> res = new ArrayList<>();

public List<List<String>> solveNQueens(int n) {
    List<String> originalboard = new ArrayList<>();
    for(int i = 0; i < n; i++){
        originalboard.add(".".repeat(n));
    }
    backtrack(originalboard, 0);
    return res;
}


// 路径：board 中小于 row 的那些行都已经成功放置了皇后
// 选择列表：第 row 行的所有列都是放置皇后的选择
// 结束条件：row 超过 board 的最后一行
void backtrack(List<String> board, int row) {
    if (row == board.size()) {// 触发结束条件
        res.add(new ArrayList<>(board));
        return;
    }

    int n = board.get(row).length();
    for (int col = 0; col < n; col++) {
        if (!isValid(board, row, col)) {// 排除不合法选择
            continue;
        }

        // 做选择
        char[] rowChar = board.get(row).toCharArray();
        rowChar[col] = 'Q';
        board.set(row, String.valueOf(rowChar));
        
        backtrack(board, row + 1);// 进入下一行决策

        // 撤销选择
        rowChar = board.get(row).toCharArray();
        rowChar[col] = '.';
        board.set(row, String.valueOf(rowChar));
    }
}

boolean isValid(List<String> board, int row, int col) {/* 是否可以在 board[row][col] 放置皇后？*/
    int n = board.size();
    for (int i = 0; i <= row; i++) {// 检查列是否有皇后互相冲突
        if (board.get(i).charAt(col) == 'Q')
            return false;
    }
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {// 检查右上方是否有皇后互相冲突
        if (board.get(i).charAt(j) == 'Q')
            return false;
    }
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {// 检查左上方是否有皇后互相冲突
        if (board.get(i).charAt(j) == 'Q')
            return false;
    }
    return true;
}
```