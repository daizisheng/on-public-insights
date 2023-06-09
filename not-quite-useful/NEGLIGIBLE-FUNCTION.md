$...$

# 1. 引言

一个函数 $`g: \N \to \R`$ 被称为 **可忽略函数** ，如果它比任何多项式的倒数更快地趋近于零。也就是说，对于每个 $`c \in \N`$ ，存在一个整数 $n_c$ ，使得对于所有 $`n \ge n_c`$ ，有 $`g(n) \le n^{−c}`$ 。在理论密码学中，我们通过要求对手的成功概率是安全参数的可忽略函数来形式化“成功概率太小以至于不重要”的概念。

$...$

## 1.1 单向函数的问题

**两个概念。** 设  $`f: \{0, 1\}^* \to \{0, 1\}^*`$  是一个多项式时间可计算的、保持长度的函数。 $f$ 的逆函数是一个概率性的、多项式时间的算法  $I$ 。（我们将在后面讨论非均匀情况，其中逆函数是一个多项式大小的电路族。）对于任何逆函数 $I$ ，我们将其关联的函数 $Inv_I$ 称为其成功概率，对于安全参数的任何值 $`n \in \N`$ ，其定义为 $`Inv_I(n) = Pr[f(I(f(x))) = f(x)]`$ ，其中概率是在从 $`\{0,1\}^n`$ 中随机选择 $x$ 并对 $I$ 的硬币投掷进行计算的基础上得出的。以下是标准定义：

我们称函数 $f$ 是 **单向** 函数，如果对于任何逆函数 $I$ ，函数 $Inv_I$ 都是可忽略的。

### 注
>
> $Inv_I(n)$ 一方面是基于所有 $x$ 的选择，一方面还基于 $I$ 在执行过程中，使用到的所有随机比特。
>

我们还可以考虑另一种形式化 $f$ 为“单向”的方式。为了描述这种方式，我们首先引入以下术语和符号。我们说 $`g_1: \N \to \R`$ 在最终上小于 $`g_2: \N \to \R`$ ，记作 $`g_1 \le_{ev} g_2`$ ，如果存在一个整数 $k$ ，对于所有 $`n \ge k`$ ，有 $`g_1(n) \le g_2(n)`$ 。现在：

我们说 $f$ 是 **一致单向** 的，如果存在一个可忽略函数 $\delta$ ，对于任何逆函数 $I$ ，有 $`Inv_I \le_{ev} \delta`$ 。

### 注
>
> 确实，**单向** 并不意味着 **一致单向** ，因为单向函数序列的极限可能并不一定是单向函数。
>

换句话说，存在一个可忽略函数 $\delta$ ，它是任何逆函数成功概率最终变得“小”的“证明”。更准确地说，对于每个逆函数 $I$ ，存在一个整数 $k_I$ ，使得对于所有 $`n \ge k_I`$ ，有 $`Inv_I(n) \le \delta(n)`$ 。我们将 $`s(·) = 1/\delta(·)`$  称为“安全级别”。

**讨论。** 另一种观察上述定义的方式是量化顺序不同：

| 单向类型 | 量化
|-|-
| $f$ 单向 | $`\forall I, \exists 可忽略函数 \delta_I : Inv_I \le_{ev} \delta_I`$
| $f$ 一致单向 | $`\exists 可忽略函数 \delta, \forall I : Inv_I \le_{ev} \delta`$

另一种区分这两种定义的方式是通过对定义进行逆否操作，这是我们在基于假设 $f$ 是单向或一致单向的前提下证明定理时必须进行的操作。如果存在一个逆函数 $I$ 和一个常数  $c$ ，使得 $`Inv_I(n) > n^{−c}`$ 对于无限多个 $n$ 成立，则函数 $f$ 不是单向的。也就是说，存在某个逆函数其成功概率是可观的。另一方面，如果对于每个可忽略函数 $\delta$ ，存在一个逆函数 $`I_\delta`$ ，使得 $`Inv_{I_\delta}(n) > \delta(n)`$ 对于无限多个 $n$ 成立，则函数 $f$ 不是一致单向的。这并不直接说明存在一个逆函数能够实现非可忽略的成功率。

**是否存在单一的安全级别呢？** 很容易看出，如果 $f$ 是一致单向的，则它也是单向的，但无法确定反之是否成立。也许不同的逆函数具有不同的成功概率，都是可忽略的，但是对于任何特定的可忽略函数 $\delta$，总会存在某个逆函数在无限多次中表现优于 $\delta$ 。

**等价性。** 本论文中的结果表明上述两种定义是等价的，即如果 $f$ 是单向的，则它也是一致单向的，反之亦然。这意味着对于给定的单向函数 $f$ ，存在一个单一的可忽略函数 $\delta$ ，使得任何逆函数的成功概率最终小于 $\delta$ 。因此，每个单向函数都具有一个“特定的”相关安全级别。换句话说，量化的顺序是没有关系的。

### 注
>
> 不得不说，这一点还是有点让人吃惊的。
>

## 1.2 可忽略函数族

事实证明，上述内容的底层技术问题的与单向函数甚至密码学无关，它只涉及可忽略函数。让我们首先明确问题的表述，然后将其与上述内容联系起来。

设 $`\textbf{F} = \{F_i: i \in \N\}`$ 是一个函数集合，其中的每个函数将 $\N$ 映射到 $\R$ 。我们将考虑函数集合 $\textbf{F}$ 的两种“可忽略性”定义。第一种很简单：只需要求每个函数各自是可忽略的。形式上，我们称 $\textbf{F}$ 是逐点可忽略的，如果对于每个 $`i \in \N`$ ， $F_i$ 都是可忽略的。第二种是要求集合“统一”地可忽略，即不同的函数都符合某个共同的极限点。形式上，$\textbf{F}$ 是统一可忽略的，如果存在一个可忽略函数 $\delta$ （称为极限点），使得对于所有 $`i \in \N`$ ，有 $`F_i \le_{ev} \delta`$ 。也就是说，对于足够大的 $n$ ，每个 $F_i$ 都降至 $\delta$ 以下。（这里的术语类比于实分析中函数集合的逐点收敛和一致收敛的概念，但并非严格相同。）

很容易看出，如果函数集合 $\textbf{F}$ 是统一可忽略的，则它也是逐点可忽略的。但反之是否成立呢？定理 3.2 证明了答案是肯定的：两种可忽略函数集合的概念是等价的。

需要强调的是，这里考虑的函数集合是 **可数** 的。对于不可数集合，这个结果并不成立。

## 1.3 应用于密码学概念

**应用于单向函数。** 现在，这如何与单向函数的问题相关？设 $`\textbf{I} = \langle I_i : i \in \N \rangle`$ 是所有逆函数的一个枚举。（由于逆函数是概率性的、多项式时间的算法，逆函数的数量是可数的。对于非均匀情况，即存在 **不可数多** 个逆函数的情况，请参见第 1.4 节和第 4 节。）对于每个 $`i \in \N`$ ，定义函数 $F_i$  为 $`F_i(n) = Inv_{I_i} (n)`$ ，其中 $`Inv_{I_i}(n)`$  是第 1.1 节中定义的 $I_i$  的成功概率。令 $`\textbf{F} = \{F_i : i \in \N\}`$ 。观察到，如果集合 $\textbf{F}$ 是逐点可忽略的，则函数 $f$ 是单向的，而如果集合 $\textbf{F}$ 是统一可忽略的，则函数 $f$ 是一致单向的。因此，定理 3.2 暗示着函数 $f$ 是单向的当且仅当它是一致单向的。

**更一般地说。** 现在我们看到几乎任何密码学原语都适用相同的结论。对于任何原语的（渐近）安全性定义具有以下形式。对于任何“对手” $A$ 和安全参数 $n$ 的值，在某个实验下会有一个关联的成功概率 $`Succ_A(n)`$ 。（目前，对手是一个均匀算法。）如果对于每个对手 $A$ ，函数 $`Succ_A`$ 都是可忽略的，那么该原语将被称为“安全”的。将这个定义放入我们一直在研究的框架中，设 $`\textbf{A} = \{A_i : i \in \N \}`$ 是所讨论的所有对手的枚举。（由于对手是均匀算法，对手的数量是可数的。）令 $`F_i(n) = Succ_{A_i}(n)`$ 。然后我们可以看到，上述定义要求函数集合 $`\textbf{F} = \{F_i : i \in \N \}`$ 是逐点可忽略的。然而，同样合理的是要求函数集合是统一可忽略的。这意味着存在一个特定的可忽略函数 $\delta$ ，使得任何对手 $A$ 的成功概率 $`Succ_A(·)`$ 最终小于 $\delta$ 。在这里，一个特定的安全级别与原语相关联，所有对手最终都必须符合该级别。这看起来可能不同，但定理 3.2 表明这两个概念是等价的。特别地，它表示对于任何原语，即使定义中没有明确要求，总是可以找到这样一个特定的安全级别。

**在协议中的错误概率。** 对于单向函数，与函数关联的单个“安全级别”似乎具有一定的重要性，但我们并没有看到将定义实际以新方式表述的特定优势。然而，在 **计算可靠性证明** 和 **知识证明** 中，第二种形式似乎更加自然。

在讨论 **计算可靠性交互式证明** 等协议时，我们经常谈论“错误概率”。这个术语表明我们想象与给定的交互式证明系统（由给定的验证者定义）相关联的是一个单一的实体（函数），称为其错误概率。在[2]中给出了基于这种观点的“可忽略错误参数”的定义。然而，在早期的研究中，出现了其他定义，对于可忽略错误的情况下并没有这种错误概率的观点：每个证明者都有一个不同的相关“错误概率”，因此协议的“错误概率”并没有具体实现。然而，应用上述结果，我们可以证明这两种表述是等价的。请参见第4.2节。类似地，我们将与[1]中提出的可忽略知识错误的 **计算知识证明** 两个概念相关联。请参见第4.3节。

## 1.4 非均匀对手和不可数性

在上面我们提到，我们希望将每个对手关联到一个函数，这个函数是对手的成功概率，并考虑由此产生的函数集合的“可忽略性”。当对手类别仅包括均匀算法时，对手的数量，因此函数的数量是可数的，并且适用于1.2节中提到的结果。然而，对手的集合可能是所有非均匀多项式时间算法的集合。这个集合是不可数的，因此相关函数的集合也是不可数的。在这种情况下，我们不能直接应用我们的主要结果。然而，我们将看到，通过考虑每个特定多项式大小限制下的“最佳可能”非均匀对手，仍然可以应用等价性，并获得所需的结果。这将把不可数的情况“归约”到可数的情况下。

第2节的处理是一般性的，适用于可数集合或不可数集合。我们在第3节证明了可数情况下的等价性，并在不可数情况下给出了一种刻画，使我们能够将后者归约为前者。

# 2 定义和基本事实

设 $`\N = \{1, 2, 3, ...\}`$ 为正整数的集合，$\R$ 为实数集。除非另有说明，函数将正整数映射到实数。有时我们将函数视为所有函数空间中的一个“点”并以此方式引用它。所谓“整数”指的是正整数，即 $\N$ 的一个元素。我们从一个有用的简写开始：

**定义2.1** 如果 $f,g$ 是函数，我们称 $f$ 在最终上小于 $g$ ，记作 $`f \le_{ev} g`$ ，如果存在一个整数 $k$ ，使得对于所有 $`n \ge k`$ ，都有 $`f(n) \le g(n)`$ 。 $\blacksquare$

值得注意的是，这个关系是可传递的：

**命题2.2** 关系 $`\le_{ev}`$ 是可传递的：如果 $`f_1 \le_{ev} f_2`$ 且 $`f_2 \le_{ev} f_3`$ ，则 $`f_1 \le_{ev} f_3`$ 。 $\blacksquare$

回想一下，如果对于每个整数 $c$ ，存在一个整数 $n_c$ 使得对于所有 $`n \ge n_c`$ ，都有 $`f(n) \le n^{−c}`$ ，那么函数 $f$ 是可以忽略的。使用上述简写，可以另一种说法是：

**定义2.3**  如果对于每个整数 $c$ ，都有 $`f \le_{ev} (·)^{−c}`$ ，那么函数 $f$ 是可以忽略的。 $\blacksquare$

这里的 $`(·)^{−c}`$ 表示函数 $`n \mapsto n^{−c}`$ 。值得注意以下内容：

**命题2.4** 如果函数 $f$ 是可以忽略的，当且仅当存在一个可忽略函数 $g$ ，使得 $`f \le_{ev} g`$ 。 $\blacksquare$

**命题2.4的证明：** 如果 $f$ 是可忽略的，那么另一个条件通过设置 $`g = f`$ 来满足。反之，假设存在一个可忽略的函数 $g$ ，使得 $`f \le_{ev} g`$ 。我们想要证明 $f$ 是可忽略的。因此，设 $`c \in \N`$ 。根据假设，$`f \le_{ev} g`$ ，而由于 $g$ 是可忽略的，我们有 $`g \le_{ev} (·)^{-c}`$ （原文为 $`(·)^{c}`$ ，疑似有误）。根据命题2.2，我们有 $`f \le_{ev} (·)^{-c}`$ 。因此，根据定义2.3， $f$ 是可忽略的。 $\blacksquare$

**函数族** 是一个包含有可数或不可数个函数的集合。

**定义2.5** 函数族 $\textbf{F}$ 是逐点可忽略的，如果对于 $\textbf{F}$ 中的每个函数 $F$ ，它都是一个可忽略的函数。 $\blacksquare$

这意味着对于 $`F \in \textbf{F}`$ 中的每个 $F$ 和每个整数 $c$ ，存在某个依赖于 $F$ 和 $c$ 的数 $k(F,c)$ ，使得当 $n \ge k(F,c)$ 时， $`F(n) \le n^{−c}`$ 。换句话说，不同的函数可能以不同的速度下降，尽管每个函数最终都低于任何倒数多项式，但发生这种情况的时间取决于函数本身和定义倒数多项式的 $c$ 的值。接下来我们定义的概念更强，它要求存在一个可忽略函数 $\delta$ ，作为函数集合最终变得很小的"证明"。所有的函数最终都必须低于 $\delta$ 。

**定义2.6** 函数族 $\textbf{F}$ 是一致可忽略的，如果存在一个可忽略函数 $\delta$ ，对于 $\textbf{F}$ 中的每个 $F$ ，都有 $`F \le_{ev} \delta`$ 。 $\blacksquare$

换句话说，如果存在一个可忽略函数 $\delta$ ，对于 $\textbf{F}$ 中的每个 $F$ ，都存在一个整数 $k(F)$ ，使得对于所有 $`n \ge k(F)`$ ，都有 $`F(n) \le \delta`$ 。请注意，$F$ 低于 $\delta$ 的点可以依赖于 $F$ ，并且可能因族中的不同函数而有所不同。

**定义2.7** 设 $\textbf{F}$ 为一个函数族， $\delta$ 为一个函数。如果对于族中的每个 $F$ ，都有 $`F \le_{ev} \delta`$ ，则我们说 $\delta$ 是 $\textbf{F}$ 的极限点。 $\blacksquare$

下面的命题是显然成立的：

**命题2.8** 如果一个函数族 $\textbf{F}$ 是一致可忽略的，当且仅当它有一个可忽略的极限点。 $\blacksquare$

请注意，极限点不是唯一的。

### 注
>
> 感觉这里定义为极限点是不是不妥？更类似一种上界？极限点显然对应于上确界？
>

# 两种可忽略函数族的关系

很容易看出，一致可忽略性蕴含逐点可忽略性。这一点在族是可数的还是不可数的情况下都成立。

**命题3.1** 如果$\textbf{F}$ 是一致可忽略的，则它是逐点不可忽略的。 $\blacksquare$

**证明：** 根据命题2.8，存在一个对 $\textbf{F}$ 而言是极限函数的可忽略函数 $\delta$ 。假设 $`F \in \textbf{F}`$ 。我们知道 $`F \le_{ev} \delta`$ ，因为 $\delta$ 是 $\textbf{F}$ 的极限点，而且我们知道 $\delta$ 是可忽略的，所以根据命题2.4，$F$ 是可忽略的。因此，根据定义2.5， $\textbf{F}$ 是逐点可忽略的。 $\blacksquare$

我们要考虑的问题是这两个概念是否等价。首先我们考虑函数族可数的情况，然后再考虑函数族不可数的情况。

## 3.1 函数族可数的情况

重要的情况是函数族是可数的。在这种情况下，我们将证明这两个可忽略性概念是等价的。

**定理3.2**  设 $`\textbf{F} = \{ F_i : i \in \N \}`$  是一个可数函数族。那么，如果 $\textbf{F}$ 是逐点可忽略的，当且仅当它一致可忽略的。 $\blacksquare$ 

根据命题3.1，如果 $\textbf{F}$ 是一致可忽略的，则它是逐点可忽略的。另一个方向更有趣。假设每个 $F_i$ 都是可忽略的函数。我们声称 $\textbf{F}$ 是一致可忽略的。为了证明这一点，我们将定义 $\textbf{F}$ 的一个可忽略极限点 $\delta$ 。在这之前，先看看为什么一个看似更简单的构造不起作用。

**注3.3** 或许最初的想法是设定

$$` 
\delta(n) = \max \{ F_1(n),F_2(n),...,F_n(n) \}  \tag{1} 
`$$ 

对于每个 $`i \in \N`$ ，显然有  $`F_i \le_{ev} \delta`$ 。但很容易看出，$\delta$ 可能不是可忽略的。例如，假设 $\nu$ 是一个可忽略函数，并且对于 $`j \le i`$ ，设 $`F_i(j)=1`$ ，对于 $j>i$ ，设 $`F_i(j)=\nu(j)`$ 。函数集合 $`\{ F_i : i \in \N \}`$  是逐点可忽略的，但等式(1)中的函数 $\delta$ 是常数函数 $1$ ，绝对不是可忽略的。

**定理3.2的证明：** 假设 $\textbf{F}$ 是逐点可忽略的。我们将构造一个可忽略的极限点 $\delta$ 来证明。这个构造使用了对角线化。我们首先概述思路，然后提供详细的细节。
想象一张表，行由  $`i = 1,2,...`$  索引，列由  $`n = 1, 2, ...`$  索引，表的第  $`(i, n)`$  个条目包含 $`F_i(n)`$ 。我们知道对于任意的 $c$ ，每行的条目最终都会低于  $`n^{−c}`$ 。但是它们下降的位置因行而异。我们通过一种对角线化的方式，在一系列“阶段”中定义  $\delta$ 。在第 $c$ 阶段，我们只考虑列表中的前 $c$ 个函数，即  $`F_1, ... , F_c`$ 。我们将找到一个值  $`h(c)`$ ，使得对于  $`n \ge h(c)`$ ，所有这些函数都小于  $`(·)^{−c}`$ ，通过“向外移动”尽可能多的距离，使得所有 $c$ 个函数都低于我们的目标。我们将这看作是定义了一系列矩形，每个矩形是有限的，但是这个序列最终会覆盖整个表格。（对于一个模糊的示例，请参见图1。）我们将使用 $h$ 来定义 $\delta$ 。也就是说，对于每个 $n$ ，我们定义  $\delta(n)$  为使得与 $n$  “最接近”的列号所在矩形中的函数的最大值。现在，我们来详细给出构造和证明。

![F-1](./images/nf-f-1.jpg "F-1")

**图1：** 这张表格的条目 $(i,n)$ 是 $`F_i(n)`$ 。对于每个行号 $i$ ，我们关联一个列号  $h(i)$  （原文为 $h(i,i)$ 疑似有误），使得对应矩形右侧的所有条目（意思是保持在底边之上！）都被 $`n^{−i}`$ 限制住。

### 注

![F-1-2](./images/nf-f-1-2.jpg "F-1-2")

> FIX: 上图中的 $`< n^{-1}`$  应该是 $\le$ 。类似fix $`< n^{-2}`$ 和 $`< n^{-3}`$ 。

对于每个 $`i,c \in \N`$ ，我们知道 $`F_i \le_{ev} (·)^{−c}`$ 。令 $`N(i,c) \in \N`$ 是满足对于所有 $`n \ge N(i,c)`$ 有 $`F_i(n) \le n^{−c}`$  的值。

### 注
>
> $`N(i,c)`$ 是那个定义 $F_i$ 可忽略性的时候用到 $N$ 。即: $`\forall c, \exists N, n\ge N \implies F_i(n) \le n^{-c}`$
>
> 容易看出, $`N(i,c)`$ 的可选取值是无穷多的。
>

我们现在递归地定义一个函数 $`h: \{0\} \cup \N \to \N`$ ，如下所示。令 $`h(0) = 0`$ ，并且对于 $`c \in \N`$ ，令

$$`
h(c) = \max \{N(1,c),N(2,c),...,N(c,c), 1+h(c−1)\}
\tag{2}
`$$ 

### 注
>
> 这样定义有个好处： $`h(c) > h(c-1)`$ 以及 $`h(c) \ge c`$ 。画上面的图的时候，不会担心框框的交叉。
>

也就是说，$`h(c)`$ 是一个超过前 $c$ 个函数下降到 $`(·)^{−c}`$ 以下的点。以下两个命题从定义上是清楚的：

**命题1：** 对于所有 $`n \ge h(c)`$ 和所有 $`c \in \N`$ ，有 $F_1(n),...,F_c(n) \le n^{−c}`$ 。 $\Box$

**命题2：** $h$ 是一个递增函数，即对于所有 $`c \in \N \cup \{0\}`$ ，有  $`h(c) < h(c + 1)`$ 。$\Box$

对于任意 $`n \in \N `$ ，我们定义 
$$`
g(n) = \max \{j \in \N : h(j) \le n\} \tag{3}
`$$

也就是说，前 $g(n)$ 个函数在输入至少为 $n$ 时下降到  $`(·)^{−g(n)}`$ 以下。需要注意的是，$h$ 是递增的这个事实意味着上述最大化的集合是有限的，所以最大值是有定义的。根据等式 (3)，以下事实是显然的：

**命题3：** $g$ 是一个非递减函数，即对于所有 $`n \in \N， g(n) \le  g(n + 1)`$ 。 $\Box$

直观地说，我们认为 $g$ 是函数 $h$ 的逆。精确的关系由下面的命题 4 和命题 5 提供。根据等式 (3)，命题 4 是显然的：

**命题4：** 对于所有 $`n \in \N`$ ，有  $`h(g(n)) \le n`$ 。 $\Box$

将  $`n = h(c)`$  代入方程 (3) 并使用命题 2，我们还可以得到：

**命题 5：** 对于所有 $`c \in N， g(h(c)) = c`$ 。 $\Box$

现在对于任意 $`n \in \N`$ ，我们定义

$$`
\delta(n) = \max\{F_i(n) : 1 \le i \le g(n)\} \tag{4}
`$$

我们有两个最终的命题，将在下面证明：

$...$

