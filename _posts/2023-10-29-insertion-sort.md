---
layout: post
title: 插入排序
---

插入排序（Insertion Sort）是一种简单直观的排序算法，其最好时间复杂度为O(n)，最坏时间复杂度为O(n<sup>2</sup>)。

它的原理是从前往后构建有序序列，对于未排序的数据，从后往前扫描已排序的序列，找到相应位置插入。

代码如下：

```java
/**
 * 插入排序
 *
 * @param arr 待排序数组
 * @param <E> 数组元素类型
 */
public static <E extends Comparable<E>> void sort(E[] arr) {
    if(arr == null || arr.length < 2) {
        return;
    }
    // 循环不变量：arr[0, i)有序，arr[i, arr.length)无序
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
