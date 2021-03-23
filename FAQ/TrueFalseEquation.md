# 方程恒成立/恒不成立

## 抽象描述

如果曾经错将表示方程的双等号 `==` 输入为单等号 `=` ，会导致 `DSolve` 等以方程为参数的函数报错。

典型报错消息：

```mathematica
DSolve::deqn
"Equation or list of equations expected instead of True in the first argument True."
"第一个参数 True 应当是方程或方程列表，而不是 True。"

DSolve::deqn
"Equation or list of equations expected instead of True in the first argument False."
"第一个参数 False 应当是方程或方程列表，而不是 False。"
```

## 具体例子

### 例1

执行错误的代码

```mathematica
DSolve[{y'[x] == 1, y[0] = 1}, y, x]
```

之后，下面的正确代码也报错：

```mathematica
DSolve[{y'[x] == 1, y[0] == 1}, y, x]
```

### 例2

执行错误的代码

```mathematica
DSolve[y'[x] = 1, y, x]
```

之后，下面的正确代码也报错：

```mathematica
DSolve[y'[x] == 1, y, x]
```

### 解决方案

#### 例1

执行

```mathematica
Clear[y]
```

即可。也可执行：

```mathematica
Clear["Global`*"]
```

#### 例2

执行

```mathematica
Clear[Derivative]
```

即可。

执行 ```Clear["Global`*"]``` 将不起作用。

### 原理解释

先前错误地执行了带 `=` 的代码会导致方程左端的表达式被赋值 `y[0]->1`，进而，之后改正了的方程被计算为恒成立的`1 == 1` ，再计算为 `True` ，不符合 `DSolve` 的用法，故报错。

清除错误的定义即可解决问题：

* 对于 `y[0] = 1` 这一次赋值，定义被绑定于头部符号 `y` ，所以清除 `y` 的定义即可。
* 对于 `y'[x] = 1` 这一次赋值，`y'[t]`的完全形式是 `Derivative[1][y][t]` ，定义被绑定于头部表达式的头部符号 `Derivative` ，所以清除 `Derivative` 的定义即可。

#### ```Clear["Global`*"]```为何对例2无效？

因为 `Derivative` 全名是 ```System`Derivative```，处于上下文 ``` System` ``` ，而不是 ``` Global` ```。

类似的疑惑也常见于 ``` System`Subscript ```。