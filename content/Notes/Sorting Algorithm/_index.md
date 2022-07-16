---
# Title, summary, and page position.
title: Sorting Algorithm
linktitle: Sorting Algorithm
summary: Notes on Sorting Algorithm.
weight: 3

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---

所谓前序位置和后序位置，就是在递归传入之前，和传回来的值
```Java
void traverse(Listnode head){
	//前序，如果想正着打印一个链表，可以在正序位置print
	traverse(head.next);
	//后序，如果想倒着打印一个链表，可以在后序位置print
}
```

{{< list_children >}}
