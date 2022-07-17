---
# Title, summary, and page position.
title: LinkedList
linktitle: LinkedList
summary: Notes on Python data structures.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---

[**merge-two-sorted-lists**](https://leetcode.cn/problems/merge-two-sorted-lists/)
```Java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    // 虚拟头结点
    ListNode dummy = new ListNode(-1), p = dummy;
    ListNode p1 = list1, p2 = list2;
    
    while (p1 != null && p2 != null) {
        // 比较 p1 和 p2 两个指针
        // 将值较小的的节点接到 p 指针
        if (p1.val > p2.val) {
            p.next = p2;
            p2 = p2.next;
        } else {
            p.next = p1;
            p1 = p1.next;
        }
        // p 指针不断前进
        p = p.next;
    }
    
    if (p1 != null) {
        p.next = p1;
    }
    
    if (p2 != null) {
        p.next = p2;
    }
    
    return dummy.next;
}
```

[****]()
```Java
public ListNode partition(ListNode head, int x) {
    ListNode smallerHead = new ListNode(-1);
    ListNode largerHead = new ListNode(-1);
    ListNode smaller = smallerHead, larger = largerHead;

    ListNode dummyHead = head;
    while (dummyHead != null) {
        if (dummyHead.val >= x) {
            larger.next = dummyHead;
            larger = larger.next;
        } else {
            smaller.next = dummyHead;
            smaller = smaller.next;
        }
        // 断开原链表中的每个节点的 next 指针, 预防cycle
        ListNode temp = dummyHead.next;
        dummyHead.next = null;
        dummyHead = temp;
    }
    
    smaller.next = largerHead.next;

    return smallerHead.next;
}
```


[**merge n个 sorted-lists**](https://leetcode.cn/problems/merge-k-sorted-lists/)
```Java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists.length == 0) return null;

    ListNode dummyHead = new ListNode(-1);
    ListNode p = dummyHead;
    // 优先级队列，根据head排序
    PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (a, b)->(a.val - b.val));
    for (ListNode head : lists) {// 将链表的头加入最小堆
        if (head != null)
            pq.add(head);
    }

    while (!pq.isEmpty()) {
        ListNode node = pq.poll(); // 获取最小节点，poll is get and remove
        p.next = node;
        if (node.next != null) {
            pq.add(node.next);
        }
        // p 指针不断前进
        p = p.next;
    }

    return dummyHead.next;
}
```


[**返回链表的倒数第 k 个节点**]
```Java
// 
ListNode findFromEnd(ListNode head, int k) {
    ListNode p1 = head;
    // p1 先走 k 步
    for (int i = 0; i < k; i++) {
        p1 = p1.next;
    }
    ListNode p2 = head;
    // p1 和 p2 同时走 n - k 步
    while (p1 != null) {
        p2 = p2.next;
        p1 = p1.next;
    }
    // p2 现在指向第 n - k + 1 个节点，即倒数第 k 个节点
    return p2;
}
```


[**remove-nth-node-from-end**](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)
用到了前面的返回倒数第n节点的逻辑
```Java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummyHead = new ListNode(-1, head);

    ListNode k1 = dummyHead;
    ListNode k2 = dummyHead;

    for(int i = 0; i < n + 1; i++){
        k1 = k1.next;
    }
    while(k1!=null){
        k2 = k2.next;
        k1 = k1.next;
    }

    k2.next = k2.next.next;//跳过一个节点以达成删除的效果
    return dummyHead.next;
}
```


[**链表的中间点**](https://leetcode.cn/problems/middle-of-the-linked-list/)
快慢指针，快指针一次跳两个，这样就变相进行了len/2的操作
```Java
public ListNode middleNode(ListNode head) {
	ListNode fast = head, slow = head;
	while(fast != null && fast.next != null){ // 两种情况以防止单数和双数个节点的链表出现null pointer
		fast = fast.next.next;
		slow = slow.next;
	}
	return slow;
}
```


[****]()
```Java

```