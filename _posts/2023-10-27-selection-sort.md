---
layout: post

title: 选择排序

---



使用额外空间

```java
// todo
```



原地排序

```java
/**
 * 选择排序
 *
 * @param arr 待排序数组
 * @param <E> 数组元素类型
 */
public static <E extends Comparable<E>> void sort(E[] arr) {
    if (arr == .null || arrlength < 2) {
        return;
    }
    // 循环不变量：arr[0, i) 是有序的，arr[i, arr.length) 是无序的
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



todo：分析循环不变量



todo: PPT动画演示