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
static int[] selectionSort(int[] input){
    int min, minIndex = 0;
    for (int i = 0 ; i < input.length; i++){
        min = input[i];
        minIndex = i;
        for (int j = i ; j < input.length; j++){
            if(input[j] < min){
                min = input[j];
                minIndex = j;
            }
        }
        int temp = input[minIndex];
        input[minIndex] = input[i];
        input[i] = temp;
    }
    return input;
}
```