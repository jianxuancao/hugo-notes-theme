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


[****]()
```Java

```


[****]()
```Java

```


[****]()
```Java

```


[****]()
```Java

```


[****]()
```Java

```