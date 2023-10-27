---

layout: post

title: 线性查找法

---

线性查找（`Linear Search`，也叫顺序查找）是一种寻找某一特定值的搜索算法，它按照一定的顺序检查数组中的每一个元素，直到找到所寻找的元素为止，其时间复杂度为`O(n)`，代码如下：
```java
/**
 * 线性查找
 *
 * @param arr    待查找数组
 * @param target 目标元素
 * @param <E>    数组元素类型
 * @return 目标元素在数组中的索引，没找到返回-1
 */
public static <T> int search(T[] data, T target) {
    if (arr == null || arr.length == 0) {
        return -1;
    }
    for (int i = 0; i < data.length; i++) {
        if (Objects.equals(data[i], target)) {
            return i;
        }
    }
    return -1;
}
```
