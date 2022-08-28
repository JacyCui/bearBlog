---
title: 【离散数学与概率论-第1讲】命题逻辑
description: 在这第一讲中，我们会从各种数学定理会采用的逻辑形式开始学习，同时我们还会学习如何处理这些逻辑形式，从而使得它们更容易去证明。
abbrlink: 58002
keywards: discrete-mathematics-and-probability-theory
tag: 离散数学与概率论
mathjax: true
cover: https://res.cloudinary.com/cuijiacai/image/upload/v1661584594/discrete-mathematics/propositional-logic/proposition_fuxsnm.png
date: 2022-08-27 15:17:22
---

# 命题逻辑

为了能够流畅的处理数学语句，你需要理解数学语言的基本框架。在这第一讲中，我们会从各种数学定理会采用的逻辑形式开始学习，同时我们还会学习如何处理这些逻辑形式，从而使得它们更容易去证明。在接下来的几讲中，我们会学习几种不同的证明方法。

## 命题的概念

我们的第一个用于构建数学大厦的砖块是**命题(proposition)**的表示法，命题是一个要么为真，要么为假的语句。

比如说下面的这些语句都是命题：

1.  $\sqrt{3}$ 是无理数。

2.  $1 + 1 = 5$ 。

3. 熊桑在他10岁生日的时候早饭吃了两个鸡蛋。

下面的这些语句显然不是命题：

4.  $2 + 2$ 。

5.  $x^2 + 3x = 5$ 。（x是啥？）

下面的这些语句也不是命题（尽管有些书上说它们是命题）。在我们这个系列中，我们约定好命题不应当包含模糊不清的表述。

6. 熊桑经常吃蜂蜜。（什么叫做“经常”？）
7. 熊桑的视频不受欢迎。（什么叫做“不流行”？）

## 命题的组合

### 与或非

多个命题可以组合在一起，从而形成更复杂的命题。令P、Q和R是表示命题的变量（比如说，P可以表示“3是奇数”）。最简单的将这些命题组合在一起的方式是使用连接词“与”、“或”和“非”。

1. **合取(conjunction)**： $P\wedge Q$（“P且Q”）。上述合取式只有当P和Q都为真的时候才为真。

2. **析取(disjunction)**： $P\vee Q$（“P或Q”）。上述析取式P和Q至少有一个为真的时候就为真。

3. **否定(negation)**： $\neg P$（“非P”）。上述否定式只有在P为假的时候才为真。

像这样带有变量的语句叫做**命题式(propositional forms)**。

有一个被称为**排中律(law of the excluded middle)**的基本原则说：对于任意命题P，要么P为真，要么 $\neg P$ 为真（但是不会两个都为真）。于是，我们会发现 $P\vee\neg P$ 永远是真的，无论P本身的*真值(truth value)*是真还是假。

对于一个命题式，如果无论其中的变量的真值是什么，整个式子永远是真的话，我们称其为*重言式(tautology)*。

相对的，像 $P\wedge\neg P$ 这样的永远为假的式子，我们称之为*矛盾式(contradiction)*。

> 概念检查：令P表示“3为奇数”，Q表示“4为奇数”，R表示“4 + 5 = 49”，那么 $P\wedge R$ ， $P\vee R$ 和 $\neg Q$ 的值分别是什么？

### 真值表

**真值表(truth table)**是描述一个命题式所有可能值的有效工具。真值表和函数表是一样的：你先列举所有变量的所有可能的输入值，然后根据这些输入列举对应的输出值。（顺序不影响）

比如说下面是合取与否定的真值表。

|P|Q| $P \wedge Q$ |
|:-:|:-:|:-:|
|T|T|T|
|T|F|F|
|F|T|F|
|F|F|F|

|P| $\neg P$ |
|:-:|:-:|
|T|F|
|F|T|

> 概念检查：试着写出析取的真值表。

### 蕴含

最重要也是最微妙的命题式是蕴含：

4. **蕴含(implication)**： $P\Longrightarrow Q$ （“P蕴含于Q”）。这和“如果P，那么Q”是等价的。

这里，P称之为蕴含的*假设(hypothesis)*，Q称为蕴含的*结论(conclusion)*。

> 其实还有另一种说法，称P为*前提(antecedent)*，Q为*结果(consequent)*。

我们在日常生活中经常会遇到蕴含，比如说：

- 如果你站在雨里，那么你会被淋湿的。
- 如果你通过了这个课程，你就会得到一个证书。

一个蕴含式 $P \Longrightarrow Q$ 只有当P为真且Q为假的时候才为假。

比如说，上面的第一句话只有当你站在雨里，但是却没有被淋湿的时候才为假；第二句话只有当你通过了这个课程，但是却没有获得证书的时候才为假。

这是 $P \Longrightarrow Q$ 的真值表（其中有多余的一列我们之后会解释）：

|P|Q| $P\Longrightarrow Q$ | $\neg P\vee Q$ |
|:-:|:-:|:-:|:-:|
|T|T|T|T|
|T|F|F|F|
|F|T|T|T|
|F|F|T|T|

注意到 $P\Longrightarrow Q$ 在P为假的时候总是真的。这意味着有许多语句从字面上听起来可能不是很合理，但是从数学的角度来说是真的。

> 因为在假设条件不成立的情况下，这个语句不对结论负责，结论不管怎么样都不影响语句的真假性。

比如说：“如果猪会飞，那么马就会读书了”或者“如果14是奇数，那么1 + 2 = 18”。这些语句我们在日常生活中并不会说，但是在数学中这些是很正常的。

当一个蕴含在假设是假的时候结论却愚蠢地真了，我们称这是一种*空真(vacuously true)*。

### 逻辑等价式

通过比较上面的真值表的最后两列，我们发现 $P \Longrightarrow Q$ 和 $\neg P\vee Q$ 是**逻辑等价(logically equivalent)**的。

也就是说，对于P和Q所有可能的真值， $P \Longrightarrow Q$ 和 $\neg P\vee Q$ 同真同假。我们写作：

$$
(P \Longrightarrow Q) \equiv (\neg P\vee Q)
$$

$P\Longrightarrow Q$ 是*数学定理(mathematical theorems)*采用的最常见的形式。下面是表述该蕴含的一些不同方式。

1. 如果P，那么Q(if P, then Q)；

2. Q如果P(Q if P)；

3. P仅当Q(P only if Q)；

4. P对于Q是充分的(P is sufficient for Q)；

5. Q对于P是必要的(Q is necessary for P)；

6. Q除非非P(Q unless not P)；

> 上面的种种表述其实看英文会更清晰一些。

如果 $P\Longrightarrow Q$ 和 $Q\Longrightarrow P$ 都是真的，我们称“P当且仅当Q”（缩写成“P iff Q”，其中iff = if and only if）。正式地，我们写作 $P \Longleftrightarrow Q$。注意 $P \Longleftrightarrow Q$ 只有在P与Q同真同假的时候才为真。

例如，令P为“3是奇数”，Q为“4是奇数”，R为“6是偶数”，则下面的这些命题式都是真的：

- $P \Longrightarrow R$

- $Q \Longrightarrow P$（空真）

因为 $P \Longrightarrow R$ 和 $R \Longrightarrow P$ ，我们也可以看到 $P \Longleftrightarrow Q$ 是真的。

## 逆与逆否

给定一个蕴含式 $P \Longrightarrow Q$，我们可以定义它的

1. **逆否(contrapositive)**： $\neg Q \Longrightarrow \neg P$

2. **逆(converse)**： $Q \Longrightarrow P$

比如说“如果你通过了这个课程，你就会获得一张证书”的逆否是“如果你没有获得证书，你一定没有通过这个课程”，逆是“如果你得到了一张证书，你通过了这个课程”。

那么逆否命题和原命题说的是同一件事情吗？逆命题和原命题说的又是同一件事情吗？

让我们来看一下真值表：

|P|Q| $\neg P$ | $\neg Q$ | $P \Longrightarrow Q$ | $Q \Longrightarrow P$ | $\neg Q \Longrightarrow \neg P$ | $P \Longleftrightarrow Q$|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|T|T|F|F|T|T|T|T|
|T|F|F|T|F|T|F|F|
|F|T|T|F|T|F|T|F|
|F|F|T|T|T|T|T|T|

注意到 $P \Longrightarrow Q$ 与它的逆否命题在真值表中是同真同假的，所以它们是逻辑等价的：

$$
(P \Longrightarrow Q) \equiv \neg Q \Longrightarrow \neg P
$$

有许多同学可能会混淆逆命题与逆否命题，这里我们需要记住，原命题与其逆否命题是等价的，而与逆命题并不是等价的。

当两个命题式式逻辑等价式，我们可以认为它们说的是同一件事情。在下一讲中，我们会看到这个性质在证明数学定理的时候有多实用。

# 量词

## 基本概念

你在实践中遇到的数学语句可能不只是由像“3是奇数”这样的简单命题组成的。相反，你会看到许多像这样的语句：

1. 对于所有的自然数n， $n^2+n+41$ 是质数。

2. 如果n是一个奇数，那么 $n^3$ 也是奇数。

3. 存在既是奇数，也是偶数的整数k。

本质上，这些语句一次性断言了许多的简单命题（甚至是无穷多的简单命题）。

比如说，第一个语句断言 $0^2 + 0 + 41$ 是质数， $1^2 + 1 + 41$ 也是质数，等等。

再比如说，最后一个语句是在说k可以取所有整数中的任意一个数，但是我们至少可以找到k的一个整数值，满足这个语句的条件。

为什么上面的3个例子是命题，而我们之前又说 $x^2 + 3x = 5$ 不是命题呢？

原因是在这几个例子中，存在一个底层的*全集(universe)*，我们是在这个全集中研究问题，并且这个语句在这个全集下被*量化(quantified)*了。（比如说上面将所有自然数代入到 $n^2 + n + 41$ 的过程。）

为了能够数学化地表示这些语句，我们需要两个**量词(quantifier)**：*全称量词(universal quantifier)* $\forall$ 和*存在量词(existential quantifier)* $\exists$ 。比如说：

1. “一些哺乳动物会下蛋。”从数学的角度来看，“一些”表示“至少有一个”，所以这个语句是在说“存在一个哺乳动物x，x会下蛋”如果我们令全集U是所有的哺乳动物组成的集合，那么我们可以这样写： 
    $$
    (\exists x \in U)(x\ lays\ eggs)
    $$
    - 有些时候，当全集比较清晰显然的时候，我们可以省略U，直接写 $\exists x (x\ lays\ eggs)$
2. “对于所有的自然数n， $n^2 + n + 41$ 是质数”，将自然数集当作全集，则可以表示为：
    $$
    (\forall n\in \mathbb{N})(n^2 + n + 41\ is\ prime)
    $$

## 有限全集下量词与析取合取的转化

在一个*有限(finite)*的全集下，我们可以将含有存在量词和全称量词的命题以一种不含量词的方式来表达，分别使用析取与合取。

比如说，如果我们的全集 $U = \\{1, 2, 3, 4\\}$，则

$$
(\exists x \in U)P(x) \equiv P(1)\vee P(2) \vee P(3) \vee P(4)
$$

$$
(\forall x \in U)P(x) \equiv P(1) \wedge P(2) \wedge P(3) \wedge P(4)
$$

不过，当全局是一个无限集的时候，比如说自然数集，这样的转化就没办法书写了。

> 概念检查：使用量词来表示下面的两个语句：
> - 对于所有的整数x， $2x+1$ 是奇数。
> 
> - 存在一个2和4之间的整数。

## 多量词

一些语句可能有多个量词。我们接下来会看到，量词之间是不能随意交换的。我们可以先从一些自然语言的例子来看待这个问题，考虑下面两个例子：

- “每次我从熊桑家门口经过的时候，总有某个人在偷蜂蜜。”
- “有某个人，每次我从熊桑家门口经过的时候，那个人都在偷蜂蜜。”

第一句话是说每次我从熊桑家门口经过的时候，总有人在偷蜂蜜，但是每次可能都是一个不同的人在偷蜂蜜。第二句话是说有一个人特别坏，每次我从熊桑家门口经过的时候，这个人都在偷蜂蜜。

从数学的角度来看，我们在量化两个全集： $T = \\{我从熊桑家门口经过的时间\\}$ 和 $P = \\{人\\}$ 。

- 第一句话是在说： $(\forall t \in T)(\exists p \in P)(p在t时偷蜂蜜)$

- 第二句话是在说： $(\exists p \in P)(\forall t\in T)(p在t时偷蜂蜜)$

让我们来看一个更加数学化的例子，考虑：

1. $(\forall x \in \mathbb{Z})(\exists y \in \mathbb{Z})(x < y)$

2. $(\exists y\in \mathbb{Z})(\forall x \in \mathbb{Z})(x < y)$

第一个语句是在说给定一个整数，我总能找到一个更大的整数。而第二个语句说了一个非常不一样的事情：存在一个最大的整数。第一个命题是真的，而第二个命题是假的。

# 否定

一个命题P是假的意味着什么？意味着它的否定 $\neg P$ 是真的。

如果有一些规则能够让我们处理否定的话，那将会很有帮助，这一点在下一讲关于证明的内容中我们会更深刻的体会到。

## 析取与合取的否定

首先，让我们看一下如何对合取与析取进行否定：

$$
\neg(P\wedge Q) \equiv (\neg P \vee \neg Q)
$$

$$
\neg(P\vee Q) \equiv (\neg P\wedge \neg Q)
$$

这两个等价式被称为**德·摩根律(De Morgan's Laws)**，并且它们是十分符合我们的知觉的：比如说，如果 $P \wedge Q$ 不是真的，那么P和Q里面至少有一个是假的（反之亦然）。

> 概念检查：通过写真值表的方式来证明德·摩根律。

## 含量词的否定

包含量词的命题否定也遵循着和上面类似的法则。我们从一个简单的例子开始看起。

假设全集是 $\\{1, 2, 3, 4\\}$ ，令 $P(x)$ 表示命题“ $x^2 > 10$ ”。我们可以发现：
- $\exists x P(x)$ 是真的，但是 $\forall x P(x)$ 是假的；

- $\neg(\forall x P(x))$ 和 $\exists x\neg P(x)$ 都是真的，因为 $P(1)$ 为假；

- $\forall x \neg P(x)$ 和 $\neg(\exists x P(x))$ 都是假的，因为 $P(4)$ 为真。

后面两队语句同真同假其实并不是偶然，因为我们有如下的逻辑等价式：

$$
\neg(\forall x P(x)) \equiv \exists x \neg P(x)
$$

$$
\neg(\exists x P(x)) \equiv \forall x \neg P(x)
$$

这两个逻辑等价式对于所有的全集上的命题P都是成立的（包括无限集）。

> 从有限全集的角度来看，我们将含有量词的命题转化成用析取或者合取表示的形式，那么，上述的两个等价式其实就是德·摩根律。

我们可以从语意的角度来尝试直观的理解上面的等价式。举一个例子，我们在整数集 $\mathbb{Z}$ 上研究问题， $P(x)$ 表示命题“x是奇数”。我们知道命题 $\forall x P(x)$ 是假的，因为不是每一个整数都是奇数。因此，这个命题的否定 $\neg(\forall x P(x))$ 应该是真的。

但是，我们从语意的角度应该怎么说呢？如果每个整数都是奇数这句话不是真的，那就说明，肯定存在某些整数，它们不是奇数（而是偶数）。用命题式来书写则是： $\exists x \neg P(x)$ ，因此这两个式子是等价的。

## 多重量词的否定

下面我们来看一个更复杂的例子，确定一个全集和命题式 $P(x, y)$ 。假设我们有命题 $\neg(\forall x \exists y P(x, y))$ 且我们想要将这个否定操作符放到量词里面，而不是放在最外面。根据上面的规律，我们可以这样操作：

$$
\neg(\forall x \exists y P(x, y)) \equiv \exists x \neg(\exists y P(x, y)) \equiv \exists x \forall y \neg P(x, y)
$$

注意到我们通过让否定操作符在量词之间传播的方式，将复杂的否定转化成了一个规模更小的，更简单的问题。当然，传播的时候，量词需要“翻转”。

最后我们再看一个更复杂的例子：

先使用量词将语句“至少存在3个不同的整数x，满足P(x)”写成命题的形式。一种做法是：

$$
\exists x\exists y\exists z(x\ne y\wedge y\ne z\wedge z\ne x\wedge P(x)\wedge P(y)\wedge P(z))
$$

(这里，所有的量词都以整数集 $\mathbb{z}$ 为全集)

现在我们再用量词将语句“**最多**有3个不同的整数x满足P(x)”写成命题的形式。一种做法是：

$$
\exists x\exists y\exists z\forall d (P(d) \Longrightarrow d = x \vee d = y \vee d = z)
$$

下面是一种等价形式：

$$
\forall x\forall y\forall v\forall z((x \ne y \wedge y\ne v \wedge v\ne x \wedge x\ne z \wedge y\ne z \wedge v\ne z)\Longrightarrow \neg(P(x)\wedge P(y)\wedge P(v)\wedge P(z)))
$$

> 这里请停一下，确认自己理解为什么上面两个式子说的是同一件事情。

最后，如果我们想要表达“**恰好(exactly)**有3个不同的整数x满足P(x)”应该怎么说呢？

现在，这个应该很简单了，我们可以直接将上面两个命题合取一下。因为至少有3个且最多有3个可不就是恰好有3个嘛。

> 后面是一些习题，习题中会涉及一些结论，我会帮大家点出来，希望最好能够记一下，因为还是比较常用的。

# 练习题

## 命题

将下面的文字表述转化成命题逻辑或者将命题逻辑转化成文字表述，并简要解释每个命题是真是假。

1. 存在不是有理数的实数。

2. 所有的整数都是自然数或者负数，但不会同时是。

3. 如果一个自然数被6整除，它要么被2整除，要么被3整除。

4. $(\forall x\in \mathbb{Z})(x \in \mathbb{Q})$

5. $(\forall x\in \mathbb{Z})(((2 | x) \wedge (3 | x)) \Longrightarrow (6 | x))$

6. $(\forall x\in \mathbb{N})((x > 7) \Longrightarrow ((\exists a, b\in \mathbb{N})(a + b = x)))$

{% hideToggle 解答 %}

1. $(\exists x \in \mathbb{R})(x\in Q)$ ；这是真的，比如说 $\pi$ 。

2. $(\forall x\in \mathbb{Z})(((x\in\mathbb{N})\vee(x < 0)) \wedge \neg((x\in\mathbb{N})\wedge(x < 0)))$ ；这是真的，因为自然数的定义就是非负整数。

3. $(\forall x \in \mathbb{N})((6 | x) \Longrightarrow ((2 | x) \vee (3 | x)))$ ；这是真的，因为 $6k = 2\cdot 3\cdot k$ (6的倍数一定既是2的倍数，也是3的倍数)。

4. 所有的整数都是自然数；这是真的，因为所有的整数n都可以写成 $\frac{n}1$ 的形式。

5. 所有的被2或者3整除的数一定被6整除；这是假的，反例：2被2整除，但是并不被6整除。

6. 所有比7大的自然数都可以写成另外两个自然数加和的形式；这是真的，我们可以直接写成0加上这个自然数本身。

{% endhideToggle %}

## 真值表

通过列举真值表的方式判断下列逻辑等价式是否成立。

1. $P\wedge (Q\vee P) \equiv P\wedge Q$

2. $(P\vee Q)\wedge R \equiv (P\wedge R)\vee(Q\wedge R)$

3. $(P\wedge Q)\vee R \equiv (P\vee R)\wedge (Q\vee R)$

{% hideToggle 解答 %}

1. 不等价，当P为真，Q为假的时候，等价式的左边为真，但右边为假。真值表略。

> 这个等价式是错的，但是和它类似的有两个等价式是对的：
> $$P\wedge(P\vee Q) \equiv P\vee(P\wedge Q) \equiv P$$
> 我们称这个逻辑等价式为吸收律。

2. 等价，真值表略。

3. 等价，真值表略。

> 上面两个逻辑等价式也称为分配律。

{% endhideToggle %}

## 必要条件和充分条件

1. 给定蕴含式 $A \Longrightarrow B$，A是B的 ------ 条件。

2. 给定蕴含式 $\neg A \Longrightarrow \neg B$，A是B的 ------ 条件。

3. 给定蕴含式 $\neg B \Longrightarrow \neg A$，A是B的 ------ 条件。

4. 给定蕴含式 $B \Longrightarrow A$，A是B的 ------ 条件。

{% hideToggle 解答 %}

1. 充分；
2. 必要；
3. 充分；
4. 必要。

{% endhideToggle %}
## 逻辑等价式

判断下面的逻辑等价式是否成立并简要说明你的理由。

1. $\forall x(P(x)\wedge Q(x))\equiv\forall xP(x)\wedge\forall xQ(x)$

2. $\forall x(P(x)\vee Q(x))\equiv\forall xP(x)\vee\forall xQ(x)$

3. $\exists x(P(x)\vee Q(x))\equiv\exists xP(x)\vee\exists xQ(x)$

4. $\exists x(P(x)\wedge Q(x))\equiv\exists xP(x)\wedge\exists xQ(x)$

{% hideToggle 解答 %}

1. 正确，左边为真可以推出右边为真，右边为真也可以推出左边为真。

2. 错误，右边为真可以推出左边为真，但是左边为真不能推出右边为真。
    - 因为左边可以有时候P(x)为真，有时候Q(x)为真，但是右边必须P(x)永远为真或者Q(x)永远为真。
    - 反例：假设全集为自然数集，P(x)表示“x为奇数” ，Q(x)表示“x为偶数”，则左边为真，右边为假。

3. 正确，左边为真可以推出右边为真，右边为真也可以推出左边为真。

4. 错误，左边为真可以推出右边为真，但是右边为真不能推出左边为真。
    - 因为右边存在的两个让P(x)为真的x和让Q(x)为真的x不一定是同一个x
    - 反例：P(x)表示“x = 1”，Q(x)表示“x = 2”，则左边为假，右边为真。

> 这两个正确的逻辑等价式最好记一下，合取关系可以分配全称量词，析取关系可以分配存在量词。

{% endhideToggle %}

# 作业题

## 蕴含

无论P表示什么，下面哪些蕴含式是永真的？如果是永真的，请简要解释，如果不是，请给出反例。

1. $\forall x\forall y P(x,y) \Longrightarrow \forall y\forall x P(x,y)$ 。

2. $\forall x\exists y P(x, y) \Longrightarrow \exists y\forall x P(x, y)$ 。

3. $\exists x\forall y P(x,y) \Longrightarrow \forall y\exists x P(x,y)$ 。

{% hideToggle 解答 %}

1. 永真。
    - 两个相同的量词交换位置不影响命题的含义。

2. 不永真。
    - “任意x存在y”，对于每个不同的x可能存在不同的y；但是“存在y，对于任意x”，说明对于每个不同的x存在的是相同的一个y。
    - 反例：P(x,y)表示"x < y"，左边为真，但是右边为假。

3. 永真。
    - 理由和2类似，右边的结论比左边的结论更弱，左边可以推导出右边。

{% endhideToggle %}
## 含量词的等价式

判断下表中左边的命题和右边的命题是否在逻辑上是等价的，并简要说明你的答案。

|序号|命题1|命题2|
|:-:|:-:|:-:|
|1| $\forall x((\exists y Q(x, y)) \Longrightarrow P(x))$ | $\forall x\exists y(Q(x,y) \Longrightarrow P(x))$ |
|2| $\neg\exists x\forall y(P(x,y)\Longrightarrow\neg Q(x,y))$ | $\forall x((\exists yP(x,y)) \wedge (\exists y Q(x,y)))$ |
|3| $\forall x\exists y(P(x)\Longrightarrow Q(x,y))$ | $\forall x(P(x) \Longrightarrow (\exists yQ(x,y)))$ |

{% hideToggle 解答 %}

1. 不等价。
    - 命题1 $\equiv \forall x(\neg(\exists yQ(x,y))\vee P(x)) \equiv\forall x((\forall y\neg Q(x,y))\vee P(x))$ ；

    - 命题2 $\equiv \forall x\exists y(\neg Q(x,y)\vee P(x))\equiv\forall x((\exists y\neg Q(x,y))\vee P(x))$；

    - 上面两个式子等价变形之后显然是不一样的，一个是任意y，另一个是存在y（其余部分都相同）。
    - 这里还可以举反例说明，反例略。

2. 不等价。
    - 命题1 $\equiv\neg\exists x\forall y(\neg P(x,y)\vee\neg Q(x,y))\equiv\neg\exists x\forall y\neg(P(x,y)\wedge Q(x,y))$
        
        $\equiv\forall x\exists y(P(x,y)\wedge Q(x,y))$

    - 到这里形式上和命题2的差别就是存在量词的分配了，而我们在之前的有一道题目中已经验证了只有析取关系才能分配存在量词，合取关系才能分配全称量词量词，否则就不是逻辑恒等式了。
    - 因此，这里的两个命题是不等价的。当然，也可举反例说明，反例略。

3. 等价。
    - 命题1 $\equiv\forall x\exists y(\neg P(x)\vee Q(x,y))\equiv\forall x(\neg P(x)\vee(\exists yQ(x,y)))\equiv$ 命题2
    - 两边都化成析取合取的形式即可发现左右是逻辑等价式。

> 上面的解答的代数变形过程会用到我们之前已经讲解过的几条逻辑等价式：
> - $P \Longrightarrow Q \equiv \neg P \vee Q$
>
> - $\forall x(P(x)\wedge Q(x))\equiv\forall xP(x)\wedge\forall xQ(x)$
>
> - $\exists x(P(x)\vee Q(x))\equiv\exists xP(x)\vee\exists xQ(x)$
>
> - $\neg(P\wedge Q) \equiv (\neg P \vee \neg Q)$
>
> - $\neg(P\vee Q) \equiv (\neg P\wedge \neg Q)$
>
> - $\neg(\forall x P(x)) \equiv \exists x \neg P(x)$
>
> - $\neg(\exists x P(x)) \equiv \forall x \neg P(x)$

{% endhideToggle %}

{% note green 'fas fa-check' flat %}
恭喜完成离散数学与概率论第1讲《命题逻辑》所有内容的学习！
{% endnote %}
