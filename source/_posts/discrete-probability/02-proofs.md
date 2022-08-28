---
title: 【离散数学与概率论-第2讲】证明
abbrlink: 63912
keywards: discrete-mathematics-and-probability-theory
tag: 离散数学与概率论
mathjax: true
date: 2022-08-28 13:36:22
description: 一个数学证明提供了一种确保一个命题为真的手段。证明是非常强大的，并且从某种程度上证明其实很像计算机程序。
cover: https://res.cloudinary.com/cuijiacai/image/upload/v1661665935/discrete-mathematics/proofs/proofs_ajvmwv.png
---

# 证明

在科学研究中，我们常常是通过实验的方式来积累证据，从而判断一个命题的有效性。但是在数学中，我们会追求一种更加绝对的确定性。一个数学证明提供了一种*确保(guaranteeing)*一个命题为真的手段。

证明是非常强大的，并且从某种程度上证明其实很像计算机程序。其实，在数学证明与计算机程序这两个我们将在这个系列中触及的概念之间有着很深的历史渊源——在大约一个世纪以前，计算机的发明与数学证明思想的探索之间是紧密绑定的。

## 证明的应用

所以我们可能会想要证明怎样的一些“和计算机科学相关的”命题呢？这里有两个例子：

1. 程序P对于每一个输入都会终止吗？
2. 程序P是否正确地计算了函数f(x)，也就是说对于每个输入x，它是否输出了f(x)？

注意到这些命题指的是一个程序在*无穷(infinitely)*多输入下的行为。对于这样的一种命题，我们可以通过大量输入测试的方式提供*证据(evidence)*，说明它对于很多的x的值来说是正确的。

但不幸的是，这并不能保证这个命题对于我们还未测试的无穷多个x来说也是正确的！为了确保这个命题的正确性，我们必须提供一个*严格的证明(rigorous proof)*。

## 证明的概念

那么，什么是**证明(proof)**呢？证明是一个有穷的步骤序列，其中每个步骤称为*逻辑推导(logical deduction)*，这些一步一步的逻辑推演保证了我们想要证明的那个命题是真的。特别地，一个证明的力量在于，通过*有限(finite)*的方法和步骤，我们能够确保一个关于*无穷多个案例(infinitely many cases)*的命题为真。

## 证明的典型结构

更特别的，证明通常具有如下的典型结构。

1. 回忆我们已经有的一些命题，在不需要证明的情况下我们承认它们是对的（我们总得从某个地方开始），这些命题称之为**公理(axiom)**或者**假设(postulate)**。
    - 我们认为公理与假设是不证自明的，有时候还会用到一些已经被证明正确的命题，我们称之为**定理(theorem)**或者**引理(lemma)**。
    - 一般定理是我们的最终结论，而引理是为了证明结论而引入的辅助结论。
2. 从这些公理与假设开始，一个证明由一系列的逻辑推导（单纯应用逻辑规则的步骤）组成。这最终会形成一系列的语句，如果上一句话是真的，那么下一句话就必然是真的（否则这就是一个伪证）。
    - 这个性质是由逻辑规则保证的：每个语句都是遵照前面的语句推导而来的。
    - 这些逻辑规则被认为是深藏在人类思考中的精华。它们在计算机的设计中起到了中心的作用，从数字逻辑或者电路设计背后的基本原则开始，到整个计算机大厦的构建。
    - 从一个更高层的视角来看，这些逻辑规则在人工智能的发展中起到了不可替代的作用，人工智能的一个终极目标就是在电脑上模拟出人脑。

## 本讲结构

在这一讲中，我们会先规定一些数学符号以及引入一些后续会使用到的基本数学事实（第2节）。

之后，我们会介绍4种不同的证明技巧：
- **直接法(direct proof)**-第3节
- **逆否法(proof by contraposition)**-第4节
- **反证法(proof by contradiction)**-第5节
- **穷举法(proof by cases)**-第6节

再然后，我们会简单地讨论一些常见的陷阱（第7节）以及证明风格（第8节）上的建议。

最后，我们会有一些证明的练习题（第9节）和作业题（第10节）。

# 一些数学记号与基本事实

在这一讲当中，我们会使用到如下的一些数学记号和基本事实：

- 用 $\mathbb{Z}$ 表示整数集，也就是说 $\mathbb{Z} = \\{..., -2, -1, 0, 1, 2, ...\\}$ ；用 $\mathbb{N}$ 表示自然数集， 也就是 $\mathbb{N} = \\{0, 1, 2, ...\\}$ 。

- 两个整数的和与积是一个整数，也就是说整数集对于加法与乘法运算*封闭(closed)*；自然数集同样也对于加法和乘法运算封闭。

- 给定整数a和b，我们称a整除b（记为 $a|b$ ）当且仅当存在一个整数q使得 $b = aq$。

    - 例如 $2 | 10$ ，因为存在一个整数 $q = 5$ 使得 $10 = 5\cdot 2$。

- 如果一个自然数 $p$ 只被1和它本身整除，我们称这是一个*质数(prime number)*。

- 我们使用 $:=$ 来表示一个定义。比如说 $q := 6$ 定义了一个初值为6的变量q。

# 直接证明

## 概念

有了第1讲的命题逻辑的语言，我们现在就可以讨论证明方法了，真正有趣的部分就要开始了（说实话，命题逻辑挺无聊的），你准备好了吗？如果准备好了，下面是我们的第一个证明方法，叫做**直接证明(direct proof)**。

在这个小节里面，记住我们的目标是给出一个清晰而又简洁的证明。让我们从一个简单的例子开始吧。

{% note blue 'fas fa-ruler' flat %}
**定理2.1：**对于任意 $a,b,c \in \mathbb{Z}$ ，如果 $a | b$ 且 $a | c$ ，那么 $a | (b + c)$ 。
{% endnote %}

> 概念检查：用P(x,y)表示“x|y”，理解上述的语句等价于：
> $$ (\forall a,b,c\in\mathbb{Z})((P(a,b)\wedge P(a,c))\Longrightarrow P(a, b + c)) $$

## 基本步骤

从一个更高的层次来看，一个直接证明的进行过程如下。

对于每个x，我们尝试证明的命题具有形式 $P(x)\Longrightarrow Q(x)$ 。

它的一个直接证明会对于一个泛化的，一般的值x，从假设P(x)开始，通过一系列的蕴含最终得出Q(x)：

{% note green 'fas fa-pen' flat %}
**直接证明(direct proof)**

$\quad$ 目标：证明 $P\Longrightarrow Q$ 。
    
$\quad$ 方法：假设P

$\quad\quad$ ......

$\quad\quad$ 于是Q 

{% endnote %}

## 实例

{% hideToggle 定理2.1证明 %}

假设 $a | b$ 且 $a | c$ ，也就是说，存在 $q_1\in\mathbb{Z}, q_2\in\mathbb{Z}$ 使得 $b = q_1a, c = q_2a$。

于是， $b + c = q_1a+q_2a = (q_1+q_2)a$。

而整数集 $\mathbb{Z}$ 对于加法封闭，我们有 $(q_1 + q_2)\in\mathbb{Z}$；

因此 $a|(b+c)$ 。 $\square$

{% endhideToggle %}

是不是非常简单？但是请停下来稍微思考一下，先前我们说定理2.1等价于
$$
(\forall a,b,c\in\mathbb{Z})((P(a,b)\wedge P(a,c))\Longrightarrow P(a, b + c))$$

那么在证明的哪个地方我们遇到了量词 $\forall$ 呢？

这个地方的关键是证明并没有给a、b、c假设任何*特定的值(specific value)*，实际上，我们的证明对于任意随机的 $a, b, c\in\mathbb{Z}$ 都成立。也因此，我们才真正地证明了我们想要的结论。

> 概念检查：请给出下面语句的直接证明：
>
> 对于任意 $a,b,c\in\mathbb{Z}$ ，如果 $a|b$ 且 $a|c$ ，那么 $a | (b - c)$ 。

下面让我们来尝试一点更具有挑战性的证明吧。

{% note blue 'fas fa-ruler' flat %}
**定理2.2：** 令 $0 < n < 1000$ 是一个整数。如果n的各数位和被9整除，那么n也被9整除。
{% endnote %}

我们可以看到，这个语句等价于

$$
(\forall n\in\mathbb{Z}^{+})((n < 1000) \Longrightarrow (sum\ of\ n's\ digits\ divisible\ by\ 9 \Longrightarrow n\ divisible\ by\ 9))
$$

这里， $\mathbb{Z}^+$ 表示正整数集 $\\{1, 2, ...\\}$。

现在，证明的过程和之前类似：
- 我们从假设开始，假设一个一般的，各数位和被9整除的整数n；
- 然后我们用一系列的蕴含来导出n本身也被9整除的结论。

{% hideToggle 定理2.2证明 %}

令 n 的十进制表示可以写作 $n = \overline{abc}$ ， 也就是说 $n = 100a + 10b + c$。

假设 $9 | a + b + c$ ，即 $\exists k \in\mathbb{Z}, s.t. a + b + c = 9k$ (这里 $s.t.$ 意为使得 - such that)。

于是我们有：
$$
n = 100a+10b+c = 99a+9b+(a+b+c) = 99a+9b+9k = 9(11a+b+k)
$$

所以， $9 | n$ 。 $\square$

{% endhideToggle %}

定理2.2的逆是否也是真的呢？回忆一下 $P \Longrightarrow Q$ 的逆是 $Q \Longrightarrow P$ 。

因此定理2.2的逆定理就是在说对于任意的整数 $0 < n < 1000$ ，如果n被9整除，那么n的数位和也被9整除。

{% note blue 'fas fa-ruler' flat %}
**定理2.3：**(定理2.2的逆定理) 令 $0 < n < 1000$ 是一个整数。如果n被9整除，那么n的各数位和也被9整除。
{% endnote %}

{% hideToggle 定理2.3证明 %}

对于n的数位采用定理2.2证明当中相同的表示法 $n = \overline{abc}$，

$$9 | n \Longrightarrow n = 9l, l\in \mathbb{Z}$$

$$\Longrightarrow 100a+10b+c = 9l \Longrightarrow 99a + 9b + (a + b + c) = 9l$$

$$\Longrightarrow a + b + c = 9(l - b - 11a)$$

$$\therefore 9 | a + b + c. \square$$

{% endhideToggle %}

我们现在应该已经对于证明有所体悟了。结合定理2.2以及其逆定理2.3，我们会发现，9整除n的数位和当且仅当9整除n。换句话说，这两句话是逻辑上等价的。

在这里，我们需要学到的一个关键是：无论何时你想要证明一个等价式 $P \Longleftrightarrow Q$ ，总是通过分别证明 $P \Longrightarrow Q$ 且 $Q \Longrightarrow P$ 的方式来进行。（就像我们上面所做的那样）

> 这里的n不一定非得是3位数，无论是几位数的n，上面的两个结论都是成立的，这里是为了降低难度，所以限制了n<1000。

# 逆否法

## 概念

下面进入我们的第二个证明技巧。回忆一下之前命题逻辑的讨论中，我们学习过蕴含 $P\Longrightarrow Q$ 和 $\neg Q\Longrightarrow \neg P$ 是等价的（可以化成析取式来看）。其中，后者称为前者的**逆否命题(contrapositive)**。

然而，有的时候证明 $\neg Q\Longrightarrow \neg Q$ 要比证明 $P\Longrightarrow Q$ 简单得多。

因此逆否法通过证明逆否命题的正确性来证明原命题的正确性。

## 基本步骤

{% note green 'fas fa-pen' flat %}
**逆否法(proof by contrapositive)**

$\quad$ 目标：证明 $P\Longrightarrow Q$ 。
    
$\quad$ 方法：假设 $\neg Q$

$\quad\quad$ ......

$\quad\quad$ 于是 $\neg P$

$\quad$ 结论： $\neg Q\Longrightarrow\neg P$ ，这与 $P\Longrightarrow Q$ 是等价的。

{% endnote %}


## 实例




