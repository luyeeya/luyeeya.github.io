---
layout: post
title: 插入排序
---

插入排序是一种简单直观的排序算法。
它的原理是从前往后构建有序序列，对于未排序的数据，从后往前扫描已排序的序列，找到相应位置插入。

```java
public static <E extends Comparable<E>> void sort(E[] arr) {
    if(arr == null || arr.length < 2) {
        return;
    }
    for(int i = 1; i < arr.length; i++) {
        for(int j = i; j > 0; j--) {
            if (arr[j - 1].compareTo(arr[j]) > 0) {
                E temp = arr[j - 1];
                arr[j - 1] = arr[j];
                arr[j] = temp;
            }
        }
    }
}
```
