---
title: 【静态分析-第8讲】指针分析-上下文敏感
abbrlink: 46841
keywards: static-analysis
tag: 静态分析
mathjax: true
date: 2022-09-08 12:02:24
description:
cover: https://s2.loli.net/2022/09/08/xtAdzujhCVGMl4P.png
---

# 引入

## 引例

首先，我们通过一个例子来看一下上下文不敏感的指针分析存在的问题。考虑如下的类结构：

```java
interface Number {
    int get();
}

class One implements Number {
    public int get() {
        return 1;
    }
}

class Two implements Number {
    public int get() {
        return 2;
    }
}
```

假设我们要对下面这段程序进行常量传播的分析：

```java
void main() {
    Number n1, n2, x, y;
    n1 = new One(); // o1
    n2 = new Two(); // o2
    x = id(n1);
    y = id(n2);
    int i = x.get();
}

Number id(Number n) {
    return n;
}
```

变量 $i$ 在常量传播分析中的结果是？

如果我们进行上下文不敏感的指针分析，最终我们得到的PFG如下：

<img src="https://s2.loli.net/2022/09/08/jFh9ymRHJCop4uA.png" alt="ci-eg" style="zoom:35%;" />

从而，分析 `x.get()` 时会对两个可能的接收对象分别调用 `get` 方法，从而得到结果为1或2，即 $i$ 的常量传播分析结果为 NAC，这是错误的。

错误的原因是，由于我们不区分上下文，导致对于 `id` 方法的所有调用会汇合到一处，从而丢失了精度，造成了后续指针的指向集合当中存在假的积极值（False Positive）。

但如果我们进行上下文敏感的分析，则最终得到的PFG如下：

<img src="https://s2.loli.net/2022/09/08/GR2VHJBoN93iZhF.png" alt="cs-intro-eg" style="zoom:30%;" />

于是，分析 `x.get()` 只会对接收对象 $o_1$ 调用目标方法，从而 $i$ 的常量传播分析结果为 $i = 1$ ，这是准确的。

## 上下文不敏感与上下文敏感

关于上下文敏感与不敏感的基本概念参见定义6.6以及6.3.2节的相关内容。

上下文不敏感的分析的精度丢失主要是由于下面一些原因：
- 在动态执行的过程中，每一个方法可能在不同的调用语境中被调用多次。
- 在不同的调用语境下面，方法中的变量可能会指向不同的对象。
- 在上下文不敏感的指针分析中，不同的上下文被混合在了一起，并传递给程序的其他部分（通过返回值或者副作用），这就导致了虚假的数据流。
    - 比如说上面的引例，其上下文不敏感的分析结果中， $pt(x)$ 中的 $o_2$ 和 $pt(y)$ 中的 $o_1$ 都是虚假的指向对象，从而给常量传播分析造成了虚假的数据流。

{% note green 'fas fa-lightbulb' flat %}
{% label 结论8.1 green %}：上下文敏感通过区分不同语境下的不同数据流来模拟调用上下文，从而提高精度。
{% endnote %}

最古老的，也是最为人熟知的上下文敏感策略是调用点敏感（也称为调用串），用一连串的调用点来表示上下文，即：本方法的调用点、调用者的调用点、调用者的调用者的调用点...... 调用点敏感其实就是对动态执行过程中调用栈的抽象，这里我们有个直觉上的印象就好，因为我们后面会以它为例去讲解规则和算法。具体的各种上下文敏感策略的形式化的定义会在8.4（这一讲的第4节）中详细讲解。

比如说之前的引例，方法 `id(Number)` 有两个上下文，`[5]` 和 `[6]`。

### 基于克隆的上下文敏感

{% note blue 'fas fa-book' flat %}
{% label 定义8.1 blue %} 定义**基于克隆的上下文敏感的指针分析（Cloning-based Context-sensitive Pointer Analysis）**为满足以下3个条件的指针分析：
- 每个方法都被一个或者多个上下文修饰；
- 变量也被上下文修饰（继承自声明该变量的方法）；
- 对于每个上下文，都会恰有一个方法及该方法中的变量的克隆。
{% endnote %}

基于克隆的上下文敏感分析是实现上下文敏感最直接的方法，就像我们在引例中所做的那样，我们将两个不同上下文中的 `id` 方法用两个结点表示，在前面分别用其上下文修饰。

## 上下文敏感的堆

在6.3.1节中，我们讨论了指针分析四大要素中的的堆抽象，当时我们提出的抽象策略是分配点抽象，将同一分配点处产的所有对象抽象成一个对象。

而现在，分配点处的变量已经上下文敏感了，那么分配点处分配的对内存也应该是上下文敏感的，这样可以细化堆内存抽象的粒度，从而提高指针分析的精度。

并且，面向对象语言（比如说Java）本身就是典型的堆密集型（heap-intensive）语言，会频繁地操作堆内存。

在实践中，为了提升精度，上下文敏感也应当被应用到堆抽象中。
- 抽象的对象也应当用上下文来修饰（称为堆上下文），最普遍的选择是继承该对象分配点所在方法的上下文。
- 上下文敏感的堆抽象在分配点抽象的基础上提供了一个粒度更精确的堆模型。
    ![cs-allocation-site.png](https://s2.loli.net/2022/09/08/hZsQtijlGbpz7ef.png)

{% note blue 'fas fa-book' flat %}
{% label 定义8.2 blue %} 称区分**堆上下文（Heap Context）**的的分配点抽象为**上下文敏感的分配点抽象（Context-sensitive Allocation-site Abstraction）**。
{% endnote %}

那么，为什么上下文敏感的堆抽象能够提升精度呢？
- 在动态执行的过程中，一个分配点可以在不同的调用语境下创建多个对象；
- 不同的对象（有相同的分配点分配）可能会携带不同的数据流被操作，比如说在它们的字段中存储不同的值。
- 在指针分析中，不带堆上下文分析这样的代码可能会由于合并不同上下文中的数据流到一个抽象对象中而丢失精度。
- 相比之下，通过堆上下文来区分同一个分配点的不同对象能够获得不少的精度。

比如对于下面这个例子：

```java
n1 = new One();
n2 = new Two();
x1 = newX(n1);
x2 = newX(n2);
n = x1.f;

X newX(Number p) {
    X x = new X();
    x.f = p;
    return x;
}
class X {
    Number f;
}
```

我们对上面的程序作3次指针分析进行对比，分别是：上下文敏感，不考虑堆上下文、上下文敏感，考虑堆上下文、上下文不敏感，考虑堆上下文。

![cs-heap](https://s2.loli.net/2022/09/08/NWcwozZTLyu2Bfp.png)

从第一次分析中，我们会发现，在将 $3:p$ 和 $4:p$ 合并给 $o_8$ 的时候造成了虚假的数据流，从而导致了对 $n$ 分析的误差。

在第二次分析中，我们可以看到，考虑了堆上下文之后，第一次分析的问题就被解决了，我们分别考虑了在程序第8行分配点处分配的两个对象，用分配点的上下文修饰（这里上下文暂定为调用点的位置），即 $3:o_8$ 和 $4:o_8$ ，从而避免了数据流合并带来的精度丢失。

而第三次分析告诉我们，在不考虑调用上下文的情况下考虑堆上下文是无法提高精度的，因为堆上下文是由于调用上下文而产生的。

# 上下文敏感的指针分析规则

和7.1一样，我们将用一系列推导式的形式来形式化地定义上下文敏感的指针分析规则。

## 定义和记号

为了表示方便，我们依然是要引入一些记号的，表示方法和7.1.1类似，这里我就直接列出，不加赘述了。

|内容|记号|
|:-:|:-:|
|上下文（Context）|$c, c', c'' \in C$|
|上下文敏感方法（C.S. Methods）|$c:m\in C\times M$|
|上下文敏感变量（C.S. Variables）|$c:x, c':y \in C\times V$|
|上下文敏感对象（C.S. Objects）|$c:o_i, c':o_j\in C\times O$|
|字段（Fields）|$f, g\in F$|
|实例字段（Instance Fields）|$c:o_i.f, c':o_j.g \in C\times O\times F$|
|上下文敏感指针（C.S. Pointers）|$CSPointer \subseteq (C\times V)\cup(C\times O\times F)$|
|指向关系（Points-to Relations）|$pt: CSPointer \to P(C\times O)$|

其中，$M$ 是程序中所有方法的集合， $V$ 是程序中所有变量的集合， $O$ 是程序中会产生的所有对象的集合。 $P(S)$ 表示集合 $S$ 的幂集， $f: S_1 \to S_2$ 表示从集合 $S_1$ 到集合 $S_2$ 的映射，满足 $f \subseteq S_1 \times S_2$ 。

## 规则

先看前4种基本的指针影响型语句（见定义6.9）：

|类型|语句|规则（在上下文 $c$ 下）| 图示|
|:-:|:-:|:-:|:-:|
|创建|`i: x = new T()`| $\overline{c:o_i \in pt(c:x)}$|{% inlineImg https://s2.loli.net/2022/09/08/iDqUX89PZkl4yAO.png 50px %}|
|赋值|`x = y`|$\underline{c':o_i\in pt(c:y)}$<br/>$c':o_i \in pt(c:x)$|{% inlineImg https://s2.loli.net/2022/09/08/uJ4SgThOipU7Yyv.png 55px %}|
|存储|`x.f = y`|$\underline{c':o_i\in pt(c:x), c'':o_j\in pt(c:y)}$<br/>$c'':o_j\in pt(c':o_i.f)$|{% inlineImg https://s2.loli.net/2022/09/08/tTIkYUD9xCFzlbh.png 60px %}|
|载入|`y = x.f`|$\underline{c':o_i\in pt(c:x), c'':o_j\in pt(c':o_i.f)}$<br/>$c'':o_j\in pt(c:y)$|{% inlineImg https://s2.loli.net/2022/09/08/NEgA7xwCmeFWOiR.png 60px %}|

其中，和上一讲一样，红色虚线依然表示作为前提条件的指向关系，黑色实线依然表示作为结论的指向关系。

前面四个语句其实和上下文不敏感的规则除了在指针和对象面前增加了上下文的修饰以外别无二致。

不过调用语句就又些别的区别了，因为调用语句除了在原本上下文不敏感的规则之上增加了上下文之外，还有负责被调用者上下文的生成，我们用 `Select` 函数来表示调用者根据调用点处的信息生成被调用者上下文的过程。

|类型|语句|规则（在上下文 $c$ 下）|
|:-:|:-:|:-:|
|调用|`l: r = x.k(a1, ..., an)`|$c':o_i\in pt(c:x),$<br/>$m = Dispatch(o_i, k), c^t = Select(c, l, c':o_i)$<br/>$c'':o_u\in pt(c:a_j), 1\le j\le n$<br/>$\underline{c''':o_v\in pt(c^t:m_{ret})}$<br/>$c':o_i\in pt(c^t:m_{this})$<br/>$c'':o_u\in pt(c^t:m_{p_j}), 1\le j\le n$<br/>$c''':o_v\in pt(c:r)$|

其中， $c$ 是调用者的上下文， $c^t$ 是被调用者，即目标方法的上下文， $c^t:m_{this}$ 是方法 $c^t:m$ 的 `this` 变量， $c^t:m_{p_j}$ 是方法 $c^t:m$ 的第j个形参， $c^t:m_{ret}$ 是存放 $c^t:m$ 返回值的变量。

-  $Dispatch(o_i, k)$ 和上一讲一致，根据 $o_i$ 的类型对虚调用 $k$ 做方法派发，从而获得一个目标方法。
-  $Select(c, l, c':o_i)$ 根据在调用点 $l$ 处可获得的信息为目标方法 $m$ 选择一个上下文，即目标上下文的生成函数。
    -  `Select` 的输入是调用点 $l$ 、调用点处的上下文 $c$ 、接收对象 $c':o_i$；
    - `Select` 的输出是目标方法的的上下文 $c^{t}$ 。

<img src="https://s2.loli.net/2022/09/09/K4yl7sdxeX5tWVZ.png" alt="select-eg" style="zoom:60%;" />

> 关于 `Select` ，我们暂时将其视为一个抽象，详细的内容，我们会在第4部分具体讨论。

# 上下文敏感的指针分析算法

## 实现思路

再上一讲中，我们是通过构建指针流图、在指针流图上传递指向信息这两个步骤相互依赖的方式来进行上下文不敏感的指针分析的。上下文敏感的指针分析我们也是采用类似的思路，只不过对于PFG上的结点以及指向集中的对象，我们会用上下文修饰来加以区分，不同修饰下的变量或者对象都有单独的一份克隆。

![md-cs](https://s2.loli.net/2022/09/09/CseVtoh7G5yYjng.png)

{% note blue 'fas fa-book' flat %}
{% label 定义8.3 blue %} **上下文敏感的指针流图（Pointer Flow Graph with C.S.）**是一个有向图，表达了不同堆上下文下的对象是怎样在不同调用上下文下的变量之间流动的。

- PFG中的结点 $n$ 代表了一个上下文敏感的变量或者一个上下文敏感的对象的某个字段，即程序中的被上下文修饰的指针。
    $$n \in CSPointer \subseteq (C\times V)\cup(C\times O\times F)$$
- PFG中的边 $x\to y \in CSPointer \times CSPointer$ 表示指针 $x$ 指向的对象可能会流向指针 $y$ 的指向集合。

其中， $C$ 表示所有上下文的集合， $V$ 表示所有变量的集合， $O$ 表示左右对象的集合， $F$ 表示所有字段的集合。
{% endnote %}

比如说下面这个例子中，PFG-C.S.中包含了两个方法 `id` 中变量 `n` 的结点，每个上下文各有一个节点。

![cs-pfg-eg](https://s2.loli.net/2022/09/09/rlXZgt7MHp1jO2h.png)

> 为了表达简洁，本讲后续的PFG都默认是上下文敏感的PFG。

|类型|语句|规则（在上下文 $c$ 下）|PFG边|
|:-:|:-:|:-:|:-:|
|创建|`i: x = new T()`| $\overline{c:o_i \in pt(c:x)}$|$N/A$|
|赋值|`x = y`|$\underline{c':o_i\in pt(c:y)}$<br/>$c':o_i \in pt(c:x)$|$c:y \to c:x$|
|存储|`x.f = y`|$\underline{c':o_i\in pt(c:x), c'':o_j\in pt(c:y)}$<br/>$c'':o_j\in pt(c':o_i.f)$|$c:y \to c':o_i.f$|
|载入|`y = x.f`|$\underline{c':o_i\in pt(c:x), c'':o_j\in pt(c':o_i.f)}$<br/>$c'':o_j\in pt(c:y)$|$c':o_i.f\to c:y$|
|调用|`l: r = x.k(a1, ..., an)`|$c':o_i\in pt(c:x),$<br/>$m = Dispatch(o_i, k), c^t = Select(c, l, c':o_i)$<br/>$c'':o_u\in pt(c:a_j), 1\le j\le n$<br/>$\underline{c''':o_v\in pt(c^t:m_{ret})}$<br/>$c':o_i\in pt(c^t:m_{this})$<br/>$c'':o_u\in pt(c^t:m_{p_j}), 1\le j\le n$<br/>$c''':o_v\in pt(c:r)$|$c:a_1\to c^t:m_{p_1}$<br/>$... ...$<br/>$c:a_n \to c^t:m_{p_n}$<br/>$c^t:m_{ret}\to r$|

## 算法设计

{% note orange 'fas fa-calculator' flat %}
{% label 算法8.1 orange %}：过程间上下文敏感的全程序指针分析算法

![cs-pta-alg](https://s2.loli.net/2022/09/09/IOEQZud4Tb7vJjC.png)

<!--
    \begin{algorithm}
    \caption{Interprocedural Context-Sensitive Whole-Program Pointer Analysis}
    \begin{algorithmic}
    \INPUT Entry method $m^{entry}$ and a context generator \CALL{Select}{$c, l, c':o_i$}.
    \OUTPUT Pointer flow graph $PFG$ and point-to set of $pt(x)$ and call graph $CG$, all context-sensitive.
    \STATE $WL := []$ \COMMENT{work list, initialized as empty}
    \STATE $PFG := \{\}$ \COMMENT{context-sensitive pointer flow graph, initialized as empty}
    \STATE $S := \{\}$ \COMMENT{set of reachable statements, initialized as empty}
    \STATE $RM := \{\}$ \COMMENT{set of context-sensitive reachable methods, initialized as empty}
    \STATE $CG := \{\}$ \COMMENT{context-sensitive call graph, initialized as empty}
    \STATE
    \PROCEDURE{Solve}{$m^{entry}$} \COMMENT{main algorithm}
        \STATE \CALL{AddReachable}{$[]:m^{entry}$}
        \WHILE{$WL$ \textbf{is not} empty}
            \STATE remove $(n, pts)$ from $WL$
            \STATE $\Delta := pts - pt(n)$
            \STATE \CALL{Propagate}{$n, \Delta$}
            \IF{$n$ represents a variable $c:x$}
                \FOR{\textbf{each} $c':o_i \in \Delta$}
                    \FOR{\textbf{each} $x.f = y \in S$}
                        \STATE \CALL{AddEdge}{$c:y, c':o_i.f$}
                    \ENDFOR
                    \FOR{\textbf{each} $y = x.f \in S$}
                        \STATE \CALL{AddEdge}{$c':o_i.f, c:y$}
                    \ENDFOR
                    \STATE \CALL{ProcessCall}{$c:x, c':o_i$}
                \ENDFOR
            \ENDIF
        \ENDWHILE
    \ENDPROCEDURE
    \STATE
    \PROCEDURE{AddReachable}{$c:m$}
        \IF{$c:m \notin RM$}
            \STATE add $c:m$ to $RM$
            \STATE $S = S \cup S_{m}$ \COMMENT{$S_m$ is the set of statements in method $m$.}
            \FOR{\textbf{each} pointer $x \in S_{m}$}
                \STATE $pt(c:x) := \{\}$ \COMMENT{poinrt-to set of each context-sensitive pointer, initialized as empty}
            \ENDFOR
            \FOR{\textbf{each} $i:x=new\ T()\in S_{m}$}
                \STATE add $(c:x, \{c:o_i\})$ to $WL$
            \ENDFOR
            \FOR{\textbf{each} $x=y\in S_{m}$}
                \STATE \CALL{AddEdge}{$c:y, c:x$}
            \ENDFOR
        \ENDIF
    \ENDPROCEDURE
    \STATE
    \PROCEDURE{ProcessCall}{$c:x, c':o_i$}
        \FOR{\textbf{each} $l: r = x.k(a_1, ..., a_n) \in S$}
            \STATE $m :=$ \CALL{Dispatch}{$o_i, k$}
            \STATE $c^t :=$ \CALL{Select}{$c, l, c':o_i$}
            \STATE add $(c^t: m_{this}, \{c':o_i\})$ to $WL$
            \IF{$c:l \to c^t:m \notin CG$}
                \STATE add $c:l \to c^t:m$ to $CG$
                \STATE \CALL{AddReachable}{$c^t:m$}
                \FOR{\textbf{each} parameter $p_i$ \textbf{of} $m$}
                    \STATE \CALL{AddEdge}{$c:a_i, c^t:p_i$}
                \ENDFOR
                \STATE \CALL{AddEdge}{$c^t:m_{ret}, c:r$}
            \ENDIF
        \ENDFOR
    \ENDPROCEDURE
    \end{algorithmic}
    \end{algorithm}
-->

{% endnote %}







# 不同的上下文判定方法


# 自检问题

1. 上下文敏感（Context Sensitivity, C.S.）是什么？
2. 上下文敏感堆（C.S. Heap）是什么？
3. 为什么 C.S. 和 C.S. Heap 能够提高分析精度？
4. 上下文敏感的指针分析有哪些规则？




