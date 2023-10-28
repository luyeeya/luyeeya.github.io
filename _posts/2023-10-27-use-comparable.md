---
layout: post
title: 使用 Comparable 接口
---

在 Java 中，实现自定义排序一般有两种方法：
1. 实现 `Comparable 接口`
2. 使用 `Comparator 比较器`

下面介绍下如何使用 Comparable 接口实现自定义排序：

以一个学生类 Student 为例，假设我们需要按照学生的成绩 score 由低到高排序：

**Student 类实现 Comparable 接口，并重写 compareTo() 方法**
```java
static class Student implements Comparable<Student> {
    private final String name;
    private final int score;

    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }

    /**
     * 按照成绩由低到高排序
     *
     * @param another 要比较的学生
     * @return 比较结果, 负值表示小于, 正值表示大于
     */
    @Override
    public int compareTo(Student another) {
        return this.score - another.score;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "', score=" + score + '}';
    }
}
```

**测试**
```java
Student[] studentArr = {
        new Student("zhangsan", 100),
        new Student("lisi", 59),
        new Student("wangwu", 88),
        new Student("zhaoliu", 60),
        new Student("tianqi", 79),
};
Arrays.sort(studentArr);
System.out.println(Arrays.toString(studentArr));
// 执行结果：[Student{name='lisi', score=59}, Student{name='zhaoliu', score=60}, Student{name='tianqi', score=79}, Student{name='wangwu', score=88}, Student{name='zhangsan', score=100}]
```