---

layout: post

title: 线性查找法

---

线性查找（`Linear Search`，也叫顺序查找）是一种寻找某一特定值的搜索算法，它按照一定的顺序检查数组中的每一个元素，直到找到所寻找的元素为止，其时间复杂度为`O(n)`，代码如下：



```java
/**
 * 线性查找法
 */
public class LinearSearch {
    public static <T> int search(T[] data, T target) {
        for (int i = 0; i < data.length; i++) {
            if (Objects.equals(data[i], target)) {
                return i;
            }
        }
        return -1;
    }
}
```
