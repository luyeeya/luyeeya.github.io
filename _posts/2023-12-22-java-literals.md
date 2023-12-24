---
layout: post
title: Java 字面量
mermaid: true
---

1. Integral Literals
2. Floating-Point Literals
3. Char Literals
4. String Literals
5. Boolean Literals
6. Null Literals

整型常量默认是 int 类型，浮点常量默认是 double 类型

<div class="mermaid">
graph LR
literals(Java 字面量)

literals --> integral_literals
integral_literals(整型字面量)
integral_literals --> decimal_integer(十进制字面量)
integral_literals --> octal_integer(八进制字面量)
integral_literals --> hexa_decimal_integer(十六进制字面量)
integral_literals --> binary_integer(二进制字面量)

literals --> floating_point_literals(浮点字面量)
floating_point_literals --> floating(十进制表示)
floating_point_literals --> decimal(科学计数法表示)

literals --> char_literals(字符字面量)

literals --> string_literals(字符串字面量)

literals --> boolean_literals(布尔字面量)

literals --> null_literals(null)
</div>


|字面量|描述|举例|
|-|-|-|
|十进制字面量|每一位可取值：0 ~ 9|2023|
|八进制字面量|每一位可取值：0 ~ 7，以 `0` 开头|017|
|十六进制字面量|每一位可取值：0 ~ 9 或 A ~ F，以 `0x` 开头|0x4F|
|二进制字面量|每一位可取值：0 或 1，以 `0b` 开头|0b11000110|
|十进制表示的浮点字面量|单精度后面加 `f`，双精度后面可加 `d`（不加 d 默认为双精度）|3.14f、3.14d、3.14|
|科学计数法表示的浮点字面量|包含符号、尾数和指数（`e`）三个部分|3.141e2、-3.141e2、3.141e-2|
|字符字面量|被一对单引号 `''` 包起来，Unicode 码点范围：0 ~ 65535|'A'|
|字符串字面量|被一对双引号 `""` 包起来|"hello"、"你好"、"666"、"\n"|
|布尔字面量|取值只有 true 或 false|true、false|
|null|表示引用为空|null|



