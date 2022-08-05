---
# Title, summary, and page position.
title: Quick sort
linktitle: Quick sort
summary: Notes on Quick sort
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True

---

1. 从数列中挑出一个元素做为pivot
2. 比pivot小的元素放在其前面，比pivot大的放后面（相同可以是任何一边
3. recursively sort smaller sub array and larger sub array



```java
public static int partition(int[] array, int low, int high) {
    int pivot = array[high]; // 取最后一个元素作为中心元素
    int pointer = low;  // 定义指向比中心元素大的指针，首先指向第一个元素

    for (int i = low; i < high; i++) {// 遍历数组中的所有元素，将比中心元素大的放在右边，比中心元素小的放在左边
        if (array[i] <= pivot) {
            // 比pivot小的元素和指针指向的元素交换位置
            // 如果比pivot大，索引向下移动，指针指向这个较大的元素，直到找到比中心元素小的元素，交换位置，指针向下移动
            int temp = array[i];
            array[i] = array[pointer];
            array[pointer] = temp;
            pointer++;
        }
    }
    
    // 中心元素和指针指向的元素交换位置
    int temp = array[pointer ];
    array[pointer] = array[high];
    array[high] = temp;
    return pointer;
}

public static void quickSort(int[] array, int low, int high) {
    if (low < high) {
        int position = partition(array, low, high);// 获取划分子数组的位置
        quickSort(array, low, position -1);// 左子数组递归调用
        quickSort(array, position + 1, high);// 右子数组递归调用
    }
}

public static void main(String[] args) {
    int[] array = {6,72,113,11,23};
    quickSort(array, 0, array.length -1);
    System.out.println("排序后的结果");
    System.out.println(Arrays.toString(array));
}

```