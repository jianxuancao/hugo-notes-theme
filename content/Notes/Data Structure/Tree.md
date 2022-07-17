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

[**maximum tree**](https://leetcode.cn/problems/maximum-binary-tree/)
```Java
public TreeNode constructMaximumBinaryTree(int[] nums) {
    return build(nums, 0, nums.length-1);
}

TreeNode build(int[]nums, int left, int right){
    if(right < left){ // base case
        return null;
    }

    // find max and it's pointer
    int max = Integer.MIN_VALUE;
    int pointer = -1;
    for(int i = left; i <= right; i++){
        if(nums[i] > max){
            max = nums[i];
            pointer = i;
        }
    }    

    TreeNode root = new TreeNode(max);
    root.left = build(nums, left, pointer - 1);  // parse the left 
    root.right = build(nums, pointer + 1, right);  // parse the right 
    
    return root;
}
```

[**tree from preorder/inorder**](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
```Java
HashMap<Integer, Integer> valToIndex = new HashMap<>();

public TreeNode buildTree(int[] preorder, int[] inorder) {
    for (int i = 0; i < inorder.length; i++) { // 放入hashmap方便读取
        valToIndex.put(inorder[i], i);
    }
    return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
}

TreeNode build(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
    
    if (preStart > preEnd) {  // basecase
        return null;
    }

    int rootVal = preorder[preStart]; // root 节点对应的值就是前序遍历数组的第一个元素
    int rootindex = valToIndex.get(rootVal); // rootVal 在中序遍历数组中的索引, 以此来分割inorder序列

    int leftSize = rootindex - inStart;
    TreeNode root = new TreeNode(rootVal);

    root.left = build(preorder, preStart + 1, preStart + leftSize, inorder, inStart, rootindex - 1);
    root.right = build(preorder, preStart + leftSize + 1, preEnd, inorder, rootindex + 1, inEnd);
    return root;
}
```

[**tree from inorder and postorder**](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
```Java
HashMap<Integer, Integer> valToIndex = new HashMap<>();
public TreeNode buildTree(int[] inorder, int[] postorder) {
    for (int i = 0; i < inorder.length; i++) { // 放入hashmap方便读取
        valToIndex.put(inorder[i], i);
    }
    return build(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
}

TreeNode build (int[] inorder, int inStart, int inEnd, int[] postorder, int postStart, int postEnd){
    if (inStart > inEnd) { // base case
        return null;
    }

    int rootVal = postorder[postEnd];
    int rootIndex = valToIndex.get(rootVal);

    TreeNode root = new TreeNode(rootVal);

    root.left = build(inorder, inStart, rootIndex-1, postorder, postStart, postStart + rootIndex - inStart - 1); 
    root.right = build(inorder, rootIndex + 1, inEnd, postorder, postStart + rootIndex - inStart, postEnd - 1);
    
    return root;
}
```