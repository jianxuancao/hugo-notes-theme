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


[****]()
```Java
public void moveZeroes(int[] nums) {

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