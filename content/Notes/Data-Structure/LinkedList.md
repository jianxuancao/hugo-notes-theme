---
# Title, summary, and page position.
title: LinkedList
linktitle: LinkedList
summary: Notes on data structures.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True
---

### [206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/)

<img src="https://labuladong.github.io/algo/images/%e5%8f%8d%e8%bd%ac%e9%93%be%e8%a1%a8/2.jpg" style="zoom:33%;" />

```Java
public ListNode reverseList(ListNode head) {
    if(head == null || head.next == null){
        return head;
    }
    
    ListNode last = reverseList(head.next); 
    // 此时head的next是逆链表的尾 (2)，但尾(2)的next是null，所以把head.next.next(tail) = head, 再把head.next换成null，反转就完成了
    head.next.next = head;
    head.next = null;

    return last;
}
```

### [**反转前N节点**]()

```Java
ListNode postHead = null;
public ListNode reverseN(ListNode head, int count) {
    if(count = 1){
    	postHead = head.next;
        return head;
    }
    
    ListNode last = reverseList(head.next, count - 1); 
    // 此时head的next是逆链表的尾，但尾的next是null，所以把head.next.next(tail) = head, 再把head.next换成null，反转就完成了
    head.next.next = head;
    // postHead是需反转之后的正序链表的头
    head.next = postHead;

    return last;
}
```

### [**k个一组反转**](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

```Java
public ListNode reverseKGroup(ListNode head, int k) {
    // 先分割出需要反转的一组
    ListNode start = head, tail = head;
   	for(int i = 0; i < k; i++){
   		if(tail == null){
   			return head;
   		}
   		tail = tail.next;
   	}

   	start.next = reverse(start, tail);
   	ListNode newHead = reverseKGroup(tail, k);

   	return newHead;
}

ListNode reverse(ListNode a, ListNode b) {
    ListNode pre, cur, nxt;
    // pre是反转好的头, cur起指针作用, nxt就是下一个节点
    pre = null; cur = a; nxt = a;
    while(cur != b){ // 到b就结束
        nxt = cur.next; // 跟cur = nxt一起，交换当前节点和下一个节点的位置
        cur.next = pre; //头尾互换，（三个为一组）
        pre = cur; // 让中间的值变为头
        cur = nxt; // 前进
    }
    // 返回反转后的头结点
    return pre;
}
```

### [**merge-two-sorted-lists**](https://leetcode.cn/problems/merge-two-sorted-lists/)

<img src="https://labuladong.github.io/algo/images/%e9%93%be%e8%a1%a8%e6%8a%80%e5%b7%a7/1.gif" style="zoom:50%;" />

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

### [**基于一个节点按大小分割**](https://leetcode.cn/problems/partition-list/)

一个链表中储存的元素大小都小于 `x`，另一个链表中的元素都大于等于 `x`，最后再把这两条链表接到一起

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

### [**merge n个 sorted-lists**](https://leetcode.cn/problems/merge-k-sorted-lists/)

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

### [**返回链表的倒数第 k 个节点**]()

```Java
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

### [**remove-nth-node-from-end**](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

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

### [**链表的中间点**](https://leetcode.cn/problems/middle-of-the-linked-list/)

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

### [**两个链表是否相交**](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

```Java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // 计算两条链表的长度
    int lenA = 0, lenB = 0;
    for (ListNode p1 = headA; p1 != null; p1 = p1.next) { lenA++; }
    for (ListNode p2 = headB; p2 != null; p2 = p2.next) { lenB++; }

    // 让 p1 和 p2 到达尾部的距离相同, (比如p1长就先在p1走n步)
    ListNode p1 = headA, p2 = headB;
    if (lenA > lenB) {
        for (int i = 0; i < lenA - lenB; i++) {
            p1 = p1.next;
        }
    } else {
        for (int i = 0; i < lenB - lenA; i++) {
            p2 = p2.next;
        }
    }
    // 1、不相交，他俩同时走到尾部空指针
    // 2、相交，他俩走到两条链表的相交点，那么while结束，返回p1或者p2其实都一样
    while (p1 != p2) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p1;
}
```
第二解法
```Java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // p1 指向 A 链表头结点，p2 指向 B 链表头结点
    ListNode p1 = headA, p2 = headB;
    while (p1 != p2) {
        // p1 走一步，如果走到 A 链表末尾，转到 B 链表
        if (p1 == null) p1 = headB;
        else            p1 = p1.next;
        // p2 走一步，如果走到 B 链表末尾，转到 A 链表
        if (p2 == null) p2 = headA;
        else            p2 = p2.next;
    }
    return p1;
}
```

### [**链表是否包含环**]()

```Java
boolean hasCycle(ListNode head) { 
	// 也是用快慢指针，如果慢指针被快指针追上了，说明有个环，快指针绕了一圈
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) { // 快慢指针碰面，说明含有环
            return true;
        }
    }
    return false;
}
```

### [**找到环的起点**]()

```Java
ListNode detectCycle(ListNode head) {
    ListNode fast = head, slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) break; // 如果相遇，break
    }
    // 这里跟检测有没有环一样
    if (fast == null || fast.next == null) {
        return null; // fast 遇到空指针说明没有环
    }

    slow = head; // 慢节点重新指向头结点
    // 快慢指针同步前进，相交点就是环起点
    while (slow != fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
}
```

### [**回文**](https://leetcode.cn/problems/palindrome-linked-list/)

还有一种方法是用快慢指针找到中点，然后反转左边的链表，再同步.next比较是否一致

```Java
ListNode left;
public boolean isPalindrome(ListNode head) {
	left = head;
	return traverse(head);
}
boolean traverse(ListNode right){
	if(right == null){
		return true;
	}

	boolean result = traverse(right.next); //递归

	result = result && (right.val == left.val); //目前最右跟最左是不是一样，

	left = left.next; // 比较完成，左边进一，比较倒数第二个以此类推
	return result;
}
```