---
# Title, summary, and page position.
title: BFS, DFS
linktitle: BFS, DFS
summary: Notes on BFS and DFS.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---
[DFS其实就是前中后序递归]({{<relref "../Data-Struture/Tree.md">}})
BFS空间复杂度高，DFS空间复杂度较低。

[**BFS(计算最小深度)**](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)
```Java
public int minDepth(TreeNode root) {
    if (root == null) return 0;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    int depth = 1;
    while (!q.isEmpty()){
        int size = q.size();
        for (int i = 0; i < size; i++) {
            TreeNode cur = q.poll();
            if(cur.left == null && cur.right == null){
                return depth;
            }
            if(cur.left != null){
                q.offer(cur.left);
            }
            if(cur.right != null){
                q.offer(cur.right);
            }
        }
        depth++;
    }
    return depth;
}
```

[**带禁止组合的开锁问题（BFS）**](https://leetcode.cn/problems/open-the-lock/)
```Java
String plusOne(String s, int j) {
    char[] ch = s.toCharArray();
    if (ch[j] == '9')
        ch[j] = '0';
    else
        ch[j] += 1;
    return new String(ch);
}
// 将 s[i] 向下拨动一次
String minusOne(String s, int j) {
    char[] ch = s.toCharArray();
    if (ch[j] == '0')
        ch[j] = '9';
    else
        ch[j] -= 1;
    return new String(ch);
}


int openLock(String[] deadends, String target) {
    Set<String> deads = new HashSet<>();// 记录需要跳过的死亡密码
    for (String s : deadends) deads.add(s);
    Set<String> visited = new HashSet<>();// 记录已经穷举过的密码，防止走回头路
    Queue<String> q = new LinkedList<>();//BFS
    int step = 0;
    q.offer("0000");
    visited.add("0000");
    
    while (!q.isEmpty()) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            String cur = q.poll();
            
            if (deads.contains(cur)) continue;/* 判断是否死亡密码 */
            if (cur.equals(target)) return step;/* 判断是否到达终点 */
            
            for (int j = 0; j < 4; j++) {/* 每一次都可能是在一位数上+或者-，所以每一个情况都有8个子情况 */
                String up = plusOne(cur, j);
                if (!visited.contains(up)) {
                    q.offer(up);
                    visited.add(up);
                }
                String down = minusOne(cur, j);
                if (!visited.contains(down)) {
                    q.offer(down);
                    visited.add(down);
                }
            }
        }
        step++;
    }
    return -1;
}
```


[**带禁止组合的开锁问题（双向BFS）**](https://leetcode.cn/problems/open-the-lock/)
从底部和root同时扩散，找出相遇时间，最坏的情况下与正常BFS一样，但是大多数时候更快
```Java
int openLock(String[] deadends, String target) {
    Set<String> deads = new HashSet<>();
    for (String s : deadends) deads.add(s);
    // 用集合不用队列，可以快速判断元素是否存在
    Set<String> q1 = new HashSet<>(), q2 = new HashSet<>(), visited = new HashSet<>();
    
    int step = 0;
    q1.add("0000");
    q2.add(target);
    
    while (!q1.isEmpty() && !q2.isEmpty()) {
        Set<String> temp = new HashSet<>(); // 哈希集合在遍历的过程中不能修改，用 temp 存储扩散结果

        for (String cur : q1) { /* 将 q1 中的所有节点向周围扩散 */
            
            if (deads.contains(cur)) continue;/* 判断是否死亡密码 */
            if (q2.contains(cur)) return step;/* 判断是否到达终点 */

            visited.add(cur);

            for (int j = 0; j < 4; j++) {/* 每一次都可能是在一位数上+或者-，所以每一个情况都有8个子情况 */
                String up = plusOne(cur, j);
                if (!visited.contains(up))
                    temp.add(up);
                String down = minusOne(cur, j);
                if (!visited.contains(down))
                    temp.add(down);
            }
        }

        step++;
        // temp 相当于 q1
        q1 = q2; // 这里交换 q1 q2，下一轮 while 就是扩散 q2
        q2 = temp;
    }
    return -1;
}
```

