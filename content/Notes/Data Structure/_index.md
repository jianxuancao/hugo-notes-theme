---
# Title, summary, and page position.
title: Data Structure
linktitle: Data Structure
weight: 3

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---

所谓前序位置和后序位置，就是刚刚进入一个位置的时候，和离开（有返回值）的时候
```Java
void traverse(Listnode head){
	//前序，如果想正着打印一个链表，可以在正序位置print
	traverse(head.next);
	//后序，如果想倒着打印一个链表，可以在后序位置print
}
```
中序位置则是二叉树上的左child都遍历完了，即将开始便利右child时

所有递归都需要一个base case


{{< list_children >}}
