---
# Title, summary, and page position.
title: Bubble sort
linktitle: Bubble sort
summary:
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false

---

如果相邻的两个单位，第一个（左）比第二个（右）大，则交换位置

每次一级循环控制需要进行排序的数的范围，（从0 到 len-已经排好的数量）

时间复杂度：O( n2 )

```java
public int[] bubbleSort(int[] arr){
	for (int i =  arr.length; i >= 0; i--){
		for(int j = 0; j < i; j--){
			if(arr[j] > arr[j+1]){
				int temp = arr[j+1];
				arr[j+1] = arr[j];
				arr[j] = temp;
 			}
		}
	}
	return arr;
}

```