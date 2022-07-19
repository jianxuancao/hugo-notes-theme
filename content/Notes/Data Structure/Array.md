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


[**移动0到末尾**](https://leetcode.cn/problems/move-zeroes/)
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


[**找出两数之和的位置**](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)
```Java
public int[] twoSum(int[] numbers, int target) {// 快慢指针
	int slow = 0, fast = 1;

    while(slow < numbers.length - 1){
        if(numbers[slow] + numbers[fast] == target){
            numbers[slow] = numbers[fast];
            return new int[]{slow + 1, fast + 1};
        }
        if(fast == numbers.length - 1){
            slow++;
            fast = slow + 1;
            continue;
        }
        fast++;
    }
    return new int[]{-1, -1};
}

int[] twoSum(int[] nums, int target) {  //左右指针
	//这个方法会快很多，因为快慢指针本质上是穷举直到找到答案
	//左右是根据sum的偏大还是偏小动态调整
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) {
            return new int[]{left + 1, right + 1};
        } else if (sum < target) {  //偏小了就让左边进一位
            left++; // 让 sum 大一点
        } else if (sum > target) {  //偏大了就让右边退一位
            right--; // 让 sum 小一点
        }
    }
    return new int[]{-1, -1};
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