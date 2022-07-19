---
# Title, summary, and page position.
title: LinkedList
linktitle: Array
summary: Notes on data structures.
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false
---

[**有序数组去重**](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
快慢指针
```Java
public int removeDuplicates(int[] nums) {
	int slow = 0, fast = 0;

	while(fast < nums.length){
		if(nums[slow] != nums[fast]){
			slow++;
			nums[slow] = nums[fast];
		}
		fast++;
	}
	return slow+1;
}
```


[**移除元素**](https://leetcode.cn/problems/remove-element/)
快慢指针
```Java
public int removeElement(int[] nums, int val) {
	int slow = 0, fast = 0;

	while(fast < nums.length){
		if(nums[fast] != val){
			nums[slow] = nums[fast];
			slow++;
		}
		fast++;
	}
	return slow;
}
```


[**移动0到末尾**](https://leetcode.cn/problems/move-zeroes/)
快慢指针
```Java
public void moveZeroes(int[] nums) {
	int slow = 0, fast = 0;

	while(fast < nums.length){
		if(nums[fast] != 0){
			nums[slow] = nums[fast];
			slow++;
		}
		fast++;
	}

	for(int i = slow; i < nums.length; i++){ 
		nums[i] = 0; // 把剩下的内容都变成0
	}
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


[****]()
```Java

```