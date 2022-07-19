---
# Title, summary, and page position.
title: LinkedList
linktitle: Binary Search Tree
summary: Notes on data structures.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---
BST 中序遍历的时候，如果先左后右就是升序排序结果，先右后左就是降序
template for all BST situations
```Java
void BST(TreeNode root, int target) {
    if (root.val == target)
        // 找到目标，做点什么
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
```


[**遍历BST**]
```Java
void traverse(TreeNode root) {
    if (root == null) return;
    traverse(root.left); // 左小
    print(root.val);// 中序遍历
    traverse(root.right); // 右大
}
```


[**第k小的元素**](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)
```Java
int counter = 0; // 记录一个遍历counter
int result; // 记录结果
public int kthSmallest(TreeNode root, int k) {
    traverse(root, k);
    return result;
}

void traverse(TreeNode root, int k) {
    if (root == null) return;
    traverse(root.left, k); // 左小
    counter++;
    if(counter == k){
        result = root.val;
    }
    traverse(root.right, k); // 右大
}
```


[**累加树**](https://leetcode.cn/problems/convert-bst-to-greater-tree/)
```Java
TreeNode convertBST(TreeNode root) {
    traverse(root);
    return root;
}

int sum = 0;
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    traverse(root.right);
    sum += root.val;
    root.val = sum;
    traverse(root.left);
}
```


[**是不是BST**](https://leetcode.cn/problems/validate-binary-search-tree/)
```Java
boolean isValidBST(TreeNode root){
	return isValidBST(root, null, null);
}
boolean isValidBST(TreeNode root, TreeNode min, TreeNode max) {
	if(root == null){
		return true;
	}
	if(min >= root.val && min != null){
		return false;
	}
	if(max <= root.val && max != null){
		return false;
	}

	return isValidBST(root.left, min, root) && isValidBST(root.right, root.val, max); 
	// 所有的左节点必须小于root，所有的右节点必须大于root, 等于也不行
}
```


[**搜索二叉树**](https://leetcode.cn/problems/search-in-a-binary-search-tree/)
```Java
//用到了二叉树的二分查找
TreeNode searchBST(TreeNode root, int target) {
	if(root == null){
		return null;
	}
	if(target > root.val && min != null){
		return searchBST(root.right);
	}
	if(target < root.val && max != null){
		return searchBST(root.left);
	}
	return root;
}
```


[**插入**]()
```Java
TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) return new TreeNode(val); // 找到空位，插入新节点

    if (root.val < val) 
        root.right = insertIntoBST(root.right, val);
    if (root.val > val) 
        root.left = insertIntoBST(root.left, val);
    return root;
}
```


[**删除**]()
```Java
TreeNode deleteNode(TreeNode root, int key) {
    if (root.val == key) { // found，删除
        if (root.left == null && root.right == null) //is leaf node
    		return null;
    	if (root.left == null) // left is empty, let right branch take it's place
    		return root.right;
		if (root.right == null) // right is empty, let left bracnh take it's place
			return root.left;
		if (root.left != null && root.right != null) { // both branch occupied
		    TreeNode minNode = getMin(root.right); // 找到右branch的最小节点, 也可以换成左的最大节点
		    root.val = minNode.val; // 把 root 改成 minNode
		    root.right = deleteNode(root.right, minNode.val); // 因为位置移动了，去右branch删除minNode
		}
    } else if (root.val > key) { // Search left
        root.left = deleteNode(root.left, key);
    } else if (root.val < key) { // Search right
        root.right = deleteNode(root.right, key);
    }
    return root;
}

TreeNode getMin(TreeNode node) {
    while (node.left != null) { // BST 最左边的就是最小的
    	node = node.left; 
    }
    return node;
}
```