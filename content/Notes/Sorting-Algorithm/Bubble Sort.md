---
# Title, summary, and page position.
title: Bubble sort
linktitle: Bubble sort
summary: Notes on Bubble sort
weight: 2

# Page metadata.
date: '2018-09-09T00:00:00Z'
type: book # Do not modify.
toc: True

---

如果相邻的两个单位，第一个（左）比第二个（右）大，则交换位置

每次一级循环控制需要进行排序的数的范围，（从0 到 len-已经排好的数量）

时间复杂度：O( n2 )

```java
static int[] bubble(int[] input) {
    for (int i = 0; i < input.length - 1; i++) {
        for (int j = 0; j < input.length - i - 1; j++) {
            if (input[j] > input[j + 1]) {
                int temp = input[j];
                input[j] = input[j + 1];
                input[j + 1] = temp;
            }
        }
    }
    return input;
}
```