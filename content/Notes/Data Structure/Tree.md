---
# Title, summary, and page position.
title: Tree
linktitle: Tree
summary: Notes on Python data structures.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---
一棵二叉树的前序遍历结果 = 根节点 + 左子树的前序遍历结果 + 右子树的前序遍历结果
前序位置的代码执行自上向下，后序位置的代码执行自下向上的

**Max Depth of Tree**
```Java
int depth = 0;
int maxD = 0;
int maxDepth(TreeNode root) {
	traverse(root);
	return maxD;
}

void traverse(TreeNode input){
	if(input == null){
		return 
	}

	depth++; // came to the next level, accumulate depth 

	if(input.left == null && input.right == null){ // reach leaf node, this is the base case
		maxD = Math.max(maxD, depth);
	}

	traverse(input.left);
	traverse(input.right);

	depth--; // left a level and went to the other branch of input's parent, unregister depth because we came up a level
}

```


[**Diameter of tree**](https://leetcode.cn/problems/diameter-of-binary-tree/submissions/)
```Java
int max = 0;
public int diameterOfBinaryTree(TreeNode root) {
    maxDepth(root);
    return max;
}

int maxDepth(TreeNode root){
    if(root == null){
        return 0;
    }

    int leftmax = maxDepth(root.left);
    int rightmax = maxDepth(root.right);
    
    int diameter = leftmax + rightmax;
    max = Math.max(diameter, max);
    
    return 1 + Math.max(leftmax, rightmax);
}
```
