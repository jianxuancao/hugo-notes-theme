---
# Title, summary, and page position.
title: Insertion Sort
linktitle: Insertion Sort
summary:
weight: 2
icon: python
icon_pack: fab

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: false

---
将当前的数进行比较，找到对应位置，数组后移，插入

复杂度O( n2 )

```java
public int[] insertionSort(int[] arr){
	for (int i = 0; i < arr.length; i++) {
		int val = arr[i];
		int pos = i;
		while(pos > 0 && val < arr[pos-1]){
			arr[pos] = arr[pos-1];
			pos--;
		}
		arr[pos] = val;
	}
}
```