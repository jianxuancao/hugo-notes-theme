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

* 一棵二叉树的前序遍历结果 = 根节点 + 左子树的前序遍历结果 + 右子树的前序遍历结果

* 前序位置的代码执行自上向下，后序位置的代码执行自下向上的

方法：

1. 遍历一遍二叉树: 如果可以，用一个 traverse 函数配合外部变量来实现

2. 递归： 利用这个函数的返回值，分解问题

[**Max Depth of Tree**](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

```Java
int depth = 0;
int maxD = 0;
int maxDepth(TreeNode root) {
	traverse(root);
	return maxD;
}

void traverse(TreeNode input){
	if(input == null){
		return;
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


[**invert binary tree**](https://leetcode.cn/problems/invert-binary-tree/)

```Java
public TreeNode invertTree(TreeNode root) {
    if (root == null){
        return root;
    }
    // 这里使用递归来处理每一次的交换所用到的值，默认其已经完成了反转
    TreeNode right = invertTree(root.right); 
    TreeNode left = invertTree(root.left);

    // 这里就不需要考虑逻辑，只需要写出来我希望把左右交换就行
    root.right = left;
    root.left = right;

    return root;
}
```


[**flatten tree**](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

```Java
public void flatten(TreeNode root) {
    if(root == null){ // base case
        return;
    }

    // flatten left and right
    flatten(root.left);  
    flatten(root.right);
    TreeNode right = root.right;
    TreeNode left = root.left;

    // use right as next, hence left is now null
    root.left = null;
    root.right = left;

    // create a dummy head to preserve the root
    TreeNode dummy = root;
    while(dummy.right != null){ // traverse to the end of the head+left list
        dummy = dummy.right;
    }
    dummy.right = right; // add right to the end of list
}
```

