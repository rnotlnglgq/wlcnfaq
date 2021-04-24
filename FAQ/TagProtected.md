# 消息：标签被保护

## 抽象描述

对符号进行定义后，如果试图对以此符号为头部的表达式进行定义，则会报错

典型报错消息：

```mathematica
Set::write
"Tag someSymbol in somePattern is Protected."
"somePattern 中的标签 someSymbol 被保护。"

SetDelayed::write
"Tag someSymbol in somePattern is Protected."
"somePattern 中的标签 someSymbol 被保护。"
```

## 具体例子

对符号进行定义

```mathematica
h = a+b
```

之后，如果试图对以此符号为头部的表达式进行定义，则会报错

```mathematica
h[x_] := x^2
```

### 解决方案

#### 伪解决

```mathematica
HoldPattern[h][x_] := x^2
```

在定义时，可使用 `HoldPattern` 避免 `h` 被计算，从而这个定义将能成功。

#### 真解决

删除符号的值

```mathematica
h=.
```

### 原理解释

定义 `h[x_] := x^2` 时，`h` 会被计算为值 `a+b` ，从而定义对象变为 `Plus[a, b][x_]` ，在不特意指定绑定符号的情况下，默认会尝试将定义绑定到表达式的头部的头部 `Plus` （ **子值** 绑定方式）。但由于 `Plus` 是系统内置符号，具有 **属性** `Protected` 而不能修改定义，所以报错。

#### 为何第一种方案被称为是「伪解决」？

定义 `HoldPattern[h][x_] := x^2` 后，符号 `h` 上同时绑定有 **自身值** 和 **下值** 两种定义。在计算表达式 `h[x]` 时，总是先计算头部 `h` ，进而导致头部不再是 `h` ，从而下值定义不会起任何作用。

> 除非该自身值定义是 `h=h` 。