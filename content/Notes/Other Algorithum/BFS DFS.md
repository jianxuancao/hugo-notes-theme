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