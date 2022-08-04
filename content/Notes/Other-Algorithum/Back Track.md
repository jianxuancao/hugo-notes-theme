---
# Title, summary, and page position.
title: Back Track
linktitle: Back Track
summary: Notes on Back Track.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True
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

### [**全排列**](https://leetcode.cn/problems/permutations/)

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
// 选择列表：nums 中还没用过的元素（used[i] 为 false）
// 结束条件：nums 中的元素全都在 track 中出现
void backtrack(int[] nums, LinkedList<Integer> track, boolean[] used) {    
    if (track.size() == nums.length) { // 触发结束条件，就是路径已经与所有的元素一样长，完全排好了
        res.add(new LinkedList(track));
        return;
    }

    for (int i = 0; i < nums.length; i++) { 
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

### [**n皇后**](https://leetcode.cn/problems/n-queens/)

```Java
List<List<String>> res = new ArrayList<>();

public List<List<String>> solveNQueens(int n) {
    List<String> originalboard = new ArrayList<>(); //create a cheess board
    for(int i = 0; i < n; i++){
        originalboard.add(".".repeat(n)); // . means no queen 
    }
    backtrack(originalboard, 0); //从第0行开始
    return res;
}

// 路径：board 中小于 row 的那些行都已经成功放置了皇后
// 选择列表：第 row 行的所有列都是放置皇后的选择
// 结束条件：row 超过 board 的最后一行
void backtrack(List<String> board, int row) {
    if (row == board.size()) {// 触发结束条件
        res.add(new ArrayList<>(board)); //添加到结果集里
        return;
    }

    int n = board.get(row).length();
    for (int col = 0; col < n; col++) { // 当前这一行每一个位置都判断一下
        if (!isValid(board, row, col)) {// 首先排除不合法选择
            continue;
        }

        // 做选择
        char[] rowChar = board.get(row).toCharArray();
        rowChar[col] = 'Q';
        board.set(row, String.valueOf(rowChar));
        
        backtrack(board, row + 1);// 进入下一行决策， 
        // 比如：当前一行为。。q 。的时候，看看下一行能不能凑出来，一直到全都凑出来为止，相当于树结构

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

### [37. 解数独 - 力扣（LeetCode）](https://leetcode.cn/problems/sudoku-solver/)

```Java
public void solveSudoku(char[][] board) {
    backtrack(board, 0, 0);
}

boolean backtrack(char[][]board, int i, int j){
    //顺序很重要，先判断是不是出界了，不然判断有没有值会indexoutofbound
    if (j == 9) { // 穷举到最后一列的话就换到下一行重新开始。
        return backtrack(board, i + 1, 0);
    }
    if (i == 9) { // 找到一个可行解，触发 base case
        return true;
    }
    if(board[i][j] ！= '.'){
        return backtrack(board, i, j + 1); //已经有值，不必穷尽，回溯下一个
    }

    for (char ch = '1'; ch <= '9'; ch++) {
        if (!isValid(board, i, j, ch))// 如果遇到不合法的数字，就跳过
            continue;

        board[i][j] = ch; //选择

        if (backtrack(board, i, j + 1)) {// 如果找到一个可行解，立即结束
            return true;
        }

        board[i][j] = '.'; //撤销选择
    }    
    return false;// 穷举完 1~9，依然没有找到可行解，此路不通
}

boolean isValid(char[][] board, int r, int c, char n) {// 判断 board[i][j] 是否可以填入 n
    for (int i = 0; i < 9; i++) {        
        if (board[r][i] == n) return false;// 判断行是否存在重复
        if (board[i][c] == n) return false;// 判断列是否存在重复
        if (board[(r/3)*3 + i/3][(c/3)*3 + i%3] == n)// 判断 3 x 3 方框是否存在重复
            return false;
    }
    return true;
}
```

### [22. 括号生成 - 力扣（LeetCode）](https://leetcode.cn/problems/generate-parentheses/)

```Java
public List<String> generateParenthesis(int n) {
    List<String> result = new ArrayList<>();
    if (n == 0) return result;
    String track = "";
    backtrack(track, n, n, result);
    return result;
}

void backtrack(String track, int left, int right, List<String>result){
    if(right < left || right<0 || left<0){ // error case
        return;
    }
    if (left == 0 && right == 0){
        result.add(track);
        return;
    }
        
    track = track + "(";
    backtrack(track, left - 1, right, result); 
    track = track.substring(0, track.length()-1);
    track = track + ")";
    backtrack(track, left, right - 1, result); 
    track = track.substring(0, track.length()-1);
}
```

