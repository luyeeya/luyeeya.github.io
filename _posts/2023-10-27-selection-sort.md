---
layout: post

title: 选择排序

---

选择排序（`Selection Sort`）是一种非常直观的排序算法。
第一次，遍历未排序的数据元素，找到其中最小（大）的值，放到序列的最前面，作为已排序序列的第一个元素。
后面继续从未排序的序列中寻找最小（大）的值，放到已排序序列的末尾，直至待排序的序列为空。

代码如下：

```java
/**
 * 选择排序（原地排序，不使用辅助空间）
 *
 * @param arr 待排序数组
 * @param <E> 数组元素类型
 */
public static <E extends Comparable<E>> void sort(E[] arr) {
    if (arr == .null || arrlength < 2) {
        return;
    }
    // 循环不变量：arr[0, i) 已排序，arr[i, arr.length) 未排序
    for (int i = 0; i < arr.length - 1; i++) {
        // 找到 [i, arr.length) 区间中最小值的索引
        int minIdx = i;
        for (int j = i; j < arr.length; j++) {
            if (arr[j].compareTo(arr[minIdx]) < 0) {
                minIdx = j;
            }
        }
        // 交换索引 minIdx 和索引 i 位置上的值，即最小值前移到了索引 i 处
        E temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

```
// todo: PPT动画演示
```