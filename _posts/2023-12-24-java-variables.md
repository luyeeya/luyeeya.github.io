---
layout: post
title: Java 变量
mermaid: true
---

### 变量
变量相当于内存中数据存储空间的一个表示，是程序的基本组成单位

**变量声明**
```java
int sum;
```

**变量赋值**
```java
sum = 0;
```

**变量声明并赋值**
```java
int sum = 0; // 通常使用这种方式
```

**注意事项**
1. 不同类型的变量，占用的空间大小不同
   > 例如：int 变量占 4 个字节，double 变量占 8 个字节
2. 变量三要素：数据类型 + 变量名 + 值
3. 同一个作用域内变量不能重名
4. 变量必须先声明后使用


### + 号
> 运算顺序：从左到右
1. 左右两边都是数值型时，做加法运算
   ```java
   System.out.println(1 + 2); // 结果：3
   ```
2. 左右两边任一是字符时，做字符拼接
   ```java
   System.out.println(1 + "2"); // 结果：12
   ```

### 数据类型

Java 数据类型包括 **8 种基本数据类型**和 **3 种引用数据类型**：

<div class="mermaid">
graph LR
types(Java 数据类型)
types --> basic(基本数据类型（3类8种）)
--> number(数值型)
number --> integer(整数类型)
number --> ieee754(浮点类型)
basic --> character(字符型)
basic --> bool(布尔型)
integer --> byte(byte)
integer --> short(short)
integer --> int(int)
integer --> long(long)
ieee754 --> float(float)
ieee754 --> double(double)
character --> char(char)
bool --> boolean(boolean)
types --> ref(引用数据类型)
ref --> clazz(类 class)
ref --> interface(接口 interface)
ref --> array("数组 []")
</div>

#### 整数类型
整数类型就是用于存放整数值的类型

|类型|空间占用|默认值|范围|
|-|-|-|-|
|byte|1 字节|0|[-2<sup>7</sup>, 2<sup>7</sup> - 1] 即 [-128, 127]|
|short|2 字节|0|[-2<sup>15</sup>, 2<sup>15</sup> - 1] 即 [-32768, 32767]|
|int|4 字节|0|[-2<sup>31</sup>, 2<sup>31</sup> - 1] 即 [-2,147,483,648, 2,147,483,647]|
|long|8 字节|0L|[-2<sup>63</sup>, 2<sup>63</sup> - 1]|

> - 更大范围的整数用 BigInteger 表示
> - bit 是计算机最小存储单位，byte 是计算机基本存储单元，1 byte 等于 8 bit
> - Java 整型字面量默认为 int 类型，当它被赋值给某个变量时，会自动向下或向上转换到变量对应的类型

#### 浮点类型
浮点类型用来表示一个小数

|类型|空间占用|默认值|范围|
|-|-|-|-|
|float（单精度）|4 字节|0.0f|[-3.403e38, 3.403e38]|
|double（双精度）|8 字节|0.0d|[-1.798e308, 1.798e308]|

> 浮点数在计算机中的存储形式：浮点数 = 符号位 + 尾数位 + 指数位
> 尾数位可能丢失，造成精度损失（浮点数不是精确值，是近似值）
> Java 浮点字面量默认为 double 类型

**判断两个浮点数的大小**
```java
double d1 = 2.7;
double d1 = 8.1 / 3;
// 错误做法
if (d1 == d2) {}

// 正确做法（这边 0.000001 是一个足够小的值，要根据实际业务场景确定该值大小）
if (Math.abs(d1 - d2) < 0.000001) {}
```

#### 字符类型
char 表示字符类型，用来表示单个字符，多个字符用字符串表示

**本质**
字符类型的本质是一个整数（Unicode 码）
存储过程：一个字符，如'a' ➡ 找到字符对应的 unicode 码值，如97 ➡ 存储二进制形式的码值，如0110 0001

#### 布尔类型
boolean 表示布尔类型，只有两种取值：true 和 false，一般用在分支判断和循环条件中

> 不可以用 0 或 非 0 代替 false 和 true


### 类型转换

#### 自动类型转换
对于基本数据类型，进行赋值或运算时，取值范围小的数据类型会自动转换为取值范围大的数据类型

<div class="mermaid">
graph LR
char(char)
--> int(int)
--> long(long)
--> float(float)
--> double(double)

byte(byte)
--> short(short)
--> int(int)
</div>

> 例如：`float f = 100L;` 是可以正常执行的语句，根据上面的图，我们知道 `100L` 会自动类型转换为 float 类型（由于 float 类型有指数加成，其取值范围是比 long 类型大的）

**一些规则：**
1. 当整型字面量（变量不行）在 byte 或者 short 取值范围内时，可以直接赋值给 byte 和 short 变量
   ```java
   // 整型字面量赋给 byte
   byte b1 = 127; // OK，127 在 [-128, 127] 内
   byte b2 = 128; // 报错
   
   // 整型字面量赋给 short
   short s1 = 32767; // OK，32767 在 [-32768, 32767] 内
   short s2 = 32768; // 报错
   ```
2. byte 和 char、short 和 char 不能自动转换类型
   ```java
   byte b = 1;
   char c = b; // 错误: 不兼容的类型: 从byte转换到char可能会有损失
   ```
3. byte、short 和 char 混合运算时，会先转成 int 类型再计算
   ```java
   byte b1 = 1;
   byte b2 = 2;
   byte b3 = b1; // OK
   byte b4 = b1 + b1; // 错误: 不兼容的类型: 从int转换到byte可能会有损失
   byte b5 = b1 + b2; // 错误: 不兼容的类型: 从int转换到byte可能会有损失
    
   short s1 = 1;
   short s2 = b1 + s1; // 错误: 不兼容的类型: 从int转换到short可能会有损失
   ```
4. 多种类型的数据混合运算时，会先将所有的数据都转换成取值范围最大的数据类型，再计算
5. 取值范围大的数据类型不能被赋值给取值范围小的数据类型，反之可以
6. boolean 不参与类型自动转换

#### 强制类型转换
使用强制转换符 `()`，可以强制将范围大的数据类型转换成范围小的数据类型

**注意事项:**
1. 可能造成精度降低或数据溢出
   ```java
   int i = (int)1.9;
   System.out.println(i); // 精度降低，结果为：1

   int j = 256;
   byte b = (byte)j;
   System.out.println(b); // 数据溢出，结果为：0
   ```
2. 强转符号只对最近的操作数有效
   > 本质是强转符号优先级大于加减乘除运算符，因此可用小括号提升部分表达式的优先级
   ```java
   // 优先级问题导致 x 和 y 的值不同
   int x = (int)3.14 * 10; // 结果：30
   int y = (int)(3.14 * 10); // 结果：31
   ```
3. 可以直接将 int 字面量赋给 char 类型变量，但将 int 变量值赋给它的时候，需要强转
   ```java
   // 
   char c1 = 100; // OK

   int i = 100;
   char c2 = i; // 错误: 不兼容的类型: 从int转换到char可能会有损失
   char c3 = (char)i; // OK
   ```

#### 基本数据类型和 String 类型互相转换
**基本数据类型转 String 类型**
1. 将基本数据类型加上空字符串即可：`X + ""`
2. 使用 String 的 valueOf 方法：`String.valueOf(X)`
   ```java
   byte b = 1;
   short c = 2;
   int i = 3;
   long l = 4L;
   float f = 5.0f;
   double d = 6.0;
   char ch = '7';
   boolean bool = true;
   String s1 = String.valueOf(b);
   String s2 = String.valueOf(c);
   String s3 = String.valueOf(i);
   String s4 = String.valueOf(l);
   String s5 = String.valueOf(f);
   String s6 = String.valueOf(d);
   String s7 = String.valueOf(ch);
   String s8 = String.valueOf(bool);
   ```


**String 类型解析成基本数据类型**
一般使用基本数据类型包装类的 parseXXX 方法：
```java
byte b2 = Byte.parseByte(s1);
short c2 = Short.parseShort(s2);
int i2 = Integer.parseInt(s3);
long l2 = Long.parseLong(s4);
float f2 = Float.parseFloat(s5);
double d2 = Double.parseDouble(s6);
char ch2 = s7.charAt(0); // 特殊：取字符串的第一个字符
boolean bool2 = Boolean.parseBoolean(s8);
```

> 要确保 String 类型可以转成有效的其他基本类型数据，否则程序会抛出异常