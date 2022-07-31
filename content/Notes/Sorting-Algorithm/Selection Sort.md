---
# Title, summary, and page position.
title: Selection Sort
linktitle: Selection Sort
summary: Notes on Selection Sort
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True

---
找出当前的最小值，放在起始位置
随着循环继续，新的最小值放在排好的末尾

```java
public int[] selectionSort(int[] arr){
	int temp, minPointer = 0;
	for(int i = 0; i < int.length - 1; i++){ //-1 是为了给下一个for循环留出最后一位的比较空间
		minPointer = i;
		for (int j = i + 1; j < arr.length; j++){
			if(arr[minPointer]>arr[j]){
				min = j;
			}
		}
		if(min != i){
			temp = arr[i];
			arr[i] = arr[minPointer];
			arr[minPointer] = temp;
		}
	}
}
```