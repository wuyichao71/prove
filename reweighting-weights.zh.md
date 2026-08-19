# 有偏系综与无偏系综之间的重加权

> 手写笔记的整理稿。本文推导重要性采样的**权重**，用它把从*有偏*玻尔兹曼分布中采得的样本，重新换算为*无偏*目标分布下的期望值；并进一步把它推广到多个有偏模拟的联合——用最大似然导出 WHAM 方程。

---

## 1. 遍历假设与大数定律

两条经典结果共同保证了可以用有限的模拟来估计系综平均。

**遍历假设。** 对于动力学系统足够长的轨迹，观测量的*时间*平均等于其*系综*平均。对相空间上的观测量 $A(\mathbf{x})$，沿轨迹 $\mathbf{x}(t)$ 的时间平均为

$$
\overline{A}
= \lim_{T \to \infty} \frac{1}{T} \int_{0}^{T} A(\mathbf{x}(t)) dt,
$$

而系综平均定义为

$$
\langle A\rangle = \int A(\mathbf{x}) \rho(\mathbf{x}) d\mathbf{x},
$$

其中 $\rho(\mathbf{x})$ 是系统在相空间中的平衡分布。遍历假设断言

$$
\overline{A} = \langle A\rangle,
$$

只要系统是遍历的——即轨迹按平衡分布 $\rho$ 遍历可达相空间。由于时间的均匀性，轨迹上每一点权重相同；这正是分子动力学中可用时间平均代替热力学系综平均的依据。

**大数定律。** 对独立同分布、且期望 $\mathbb{E}[X]$ 有限的随机变量序列 $\{X_i\}_{i=1}^{N}$，样本平均收敛到该期望：

$$
\overline{X}_N = \frac{1}{N}\sum_{i=1}^{N} X_i
\xrightarrow[N \to \infty]{}  \mathbb{E}[X].
$$

这使得*有限*样本成为平均值的合理估计。

两者结合，观测量的系综平均便可用模拟生成的构型 $\{\mathbf{x}_i\}$ 来估计：

$$
\langle A\rangle = \int A(\mathbf{x}) \rho(\mathbf{x}) d\mathbf{x}
\approx  \frac{1}{N}\sum_{i=1}^{N} A(\mathbf{x}_i).
$$

**自由能则不同。** 上述估计只对真正的观测量成立。自由能*并非*观测量：它依赖配分函数——一个高维积分，无法从归一化的采样分布中直接得到，因为概率密度本身就是通过配分函数定义的，而对密度取平均也没有物理意义。这一空缺正是重加权要解决的：第 4 节的重加权估计就是把上面的样本平均给每一项乘上一个重要性权重，未知的归一化只以配分函数比 $c_b/c$ 的形式出现（第 4–5 节），并在权重归一化后相消。当偏置 $b\equiv 0$ 时，它退化为这里的普通样本平均。

---

## 2. 设定

考察构型 $\mathbf{x}$（构型空间中的一点），比较两个玻尔兹曼型概率密度。

**目标（无偏）分布。** 记约化势能为 $u_0(\mathbf{x})$，

$$
p(\mathbf{x}) = \frac{1}{c} \exp\big(-u_0(\mathbf{x})\big),
\qquad
c = \int \exp\big(-u_0(\mathbf{x})\big) d\mathbf{x} .
$$

其中 $c$ 是配分函数，使 $p$ 的积分归一。

**有偏分布。** 在 $u_0$ 上叠加一个偏置势 $b(\mathbf{x})$，

$$
p_b(\mathbf{x}) = \frac{1}{c_b} \exp\big(-u_0(\mathbf{x}) - b(\mathbf{x})\big),
\qquad
c_b = \int \exp\big(-u_0(\mathbf{x}) - b(\mathbf{x})\big) d\mathbf{x} .
$$

采样（例如增强采样或伞形采样模拟）在 $p_b$ 下进行，而我们想要的是 $p$ 下的平均值。

以上两式对应笔记的前两行。

---

## 3. 重加权因子

要用有偏样本还原无偏平均，需要两个密度的逐点比值，即**重要性权重**：

$$
w(\mathbf{x})  =  \frac{p(\mathbf{x})}{p_b(\mathbf{x})}
= \frac{\dfrac{1}{c} \exp\big(-u_0(\mathbf{x})\big)}
{\dfrac{1}{c_b} \exp\big(-u_0(\mathbf{x}) - b(\mathbf{x})\big)} .
$$

指数因子通过其宗量相减合并，

$$
-u_0(\mathbf{x})  -  \big(-u_0(\mathbf{x}) - b(\mathbf{x})\big)  =  b(\mathbf{x}),
$$

公共的 $-u_0(\mathbf{x})$ 相消，指数上只剩下偏置项：

$$
w(\mathbf{x})
= \frac{1}{c}\cdot\frac{\exp\big(b(\mathbf{x})\big)}{ 1/c_b }
= \boxed{ \frac{c_b}{c} \exp\big(b(\mathbf{x})\big)  }.
$$

这对应笔记的中间部分。权重由一个常数前因子 $c_b/c$（两个配分函数之比）与依赖构型的因子 $\exp\big(b(\mathbf{x})\big)$ 相乘而成，后者恰好抵消了先前施加的偏置。

---

## 4. 归一化权重

实际计算中前因子 $c_b/c$ 未知，因为配分函数并不直接算出。真正用到的是**自归一化权重** $W(\mathbf{x})$——把同一个比值中的有偏密度 $p_b$（即 $c_b^{-1}\exp(-u_0 - b)$）展开写在分母上，也就是笔记中标注 “mbar” 的写法：

$$
W(\mathbf{x})
= \frac{1}{c} 
\frac{\exp\big(-u_0(\mathbf{x})\big)}
{ c_b^{-1}\exp\big(-u_0(\mathbf{x}) - b(\mathbf{x})\big) }
= \frac{c_b}{c} \exp\big(b(\mathbf{x})\big).
$$

由于分母恰为 $p_b(\mathbf{x})$，结果与第 3 节相同： $W(\mathbf{x}) = w(\mathbf{x}) = \dfrac{c_b}{c}\exp\big(b(\mathbf{x})\big)$。这对应笔记的最后一行。

只要在样本集 $\{\mathbf{x}_i\}$ 上对权重做归一化，未知常数 $c_b/c$ 就自动消去：

$$
\widehat{W}(\mathbf{x}_i) = \frac{w(\mathbf{x}_i)}{\sum_j w(\mathbf{x}_j)}
= \frac{\exp\big(b(\mathbf{x}_i)\big)}{\sum_j \exp\big(b(\mathbf{x}_j)\big)} .
$$

于是任意观测量 $A$ 的无偏系综平均都可仅用偏置值来估计：

$$
\langle A\rangle_p  \approx  \sum_i \widehat{W}(\mathbf{x}_i) A(\mathbf{x}_i)
=  \frac{\sum_i A(\mathbf{x}_i) e^{ b(\mathbf{x}_i)}}{\sum_i e^{ b(\mathbf{x}_i)}}.
$$

---

## 5. 多个有偏模拟的联合：WHAM 方程（最大似然推导）

前四节把“一个”有偏分布 $p_b$ 重加权回无偏 $p$。实际中常常有 $S$ 个不同偏置的模拟（例如伞形采样的多个窗口），需要把它们各自的直方图联合成同一条无偏分布。**加权直方图分析法（WHAM）** 正是这一联合的最大似然解。本节整理自另一页手写笔记（多项似然 + 拉格朗日乘子）。

**记号。** 设有 $S$ 个模拟窗口（ $i=1,\dots,S$），把构型空间划分为 $M$ 个箱（ $j=1,\dots,M$）。窗口 $i$ 落入箱 $j$ 的计数记为 $n_{ij}$，该窗口的总样本数 $N_i=\sum_{j} n_{ij}$。已知的偏置因子记为 $C_{ij}$（例如 $C_{ij}=e^{-b_i(\mathbf{x}_j)}$，由所加偏置决定）。待求量是无偏的箱概率 $P_j^{0}$ 与各窗口的归一化常数 $f_i$；模型假设窗口 $i$ 在箱 $j$ 处的概率为

$$
P_{ij}=P_j^{0} C_{ij} f_i .
$$

**多项似然。** 每个窗口的计数服从多项分布：

$$
L_i=\frac{N_i!}{\prod_{j} n_{ij}!}\prod_{j=1}^{M} P_{ij}^{n_{ij}}
=\frac{N_i!}{\prod_{j} n_{ij}!}\prod_{j=1}^{M}\big(P_j^{0} C_{ij} f_i\big)^{n_{ij}} .
$$

各窗口相互独立，总似然 $L=\prod_{i=1}^{S} L_i$。取对数并略去与参数无关的常数：

$$
\ell=\ln L \propto \sum_{i=1}^{S}\sum_{j=1}^{M} n_{ij}\ln\big(P_j^{0} C_{ij} f_i\big) .
$$

**约束与拉格朗日函数。** 每个窗口的分布必须归一，这同时定义了 $f_i$：

$$
\sum_{j=1}^{M} P_j^{0} C_{ij} f_i = 1 .
$$

对每个约束引入乘子 $\lambda_i$，构造

$$
F=\ell+\sum_{i=1}^{S}\lambda_i\Big(\sum_{j=1}^{M} P_j^{0} C_{ij} f_i\Big) .
$$

**对 $f_i$ 求导定乘子。** 令 $\partial F/\partial f_i=0$：

$$
\frac{\partial F}{\partial f_i}=\frac{\sum_{j} n_{ij}}{f_i}+\lambda_i\sum_{j=1}^{M} P_j^{0} C_{ij}=0 .
$$

用归一约束 $\sum_{j} P_j^{0} C_{ij} f_i=1$ 与总数 $\sum_{j} n_{ij}=N_i$，两端乘 $f_i$ 即得 $N_i+\lambda_i=0$，故

$$
\boxed{\ \lambda_i=-N_i\ } .
$$

**对 $P_j^{0}$ 求导定概率。** 令 $\partial F/\partial P_j^{0}=0$：

$$
\sum_{i=1}^{S}\Big(\frac{n_{ij}}{P_j^{0}}+\lambda_i C_{ij} f_i\Big)=0
\quad\Longrightarrow\quad
\frac{\sum_{i} n_{ij}}{P_j^{0}}=-\sum_{i}\lambda_i C_{ij} f_i .
$$

代入 $\lambda_i=-N_i$，解出无偏箱概率：

$$
\boxed{\ P_j^{0}=\frac{\sum_{i=1}^{S} n_{ij}}{\sum_{i=1}^{S} N_i C_{ij} f_i}\ } .
$$

**自洽方程。** 归一约束又把各窗口常数写成

$$
\boxed{\ f_i^{-1}=\sum_{j=1}^{M} C_{ij} P_j^{0}\ } .
$$

两个方框式互相耦合： $P_j^{0}$ 依赖 $f_i$，而 $f_i$ 又依赖 $P_j^{0}$。实际中迭代求解——给定一组 $f_i$ 算出 $P_j^{0}$，再回代更新 $f_i$，直至收敛。这正是 WHAM（及其无箱推广 MBAR）所做的：用一套最大似然自洽方程，把多个有偏采样的直方图拼接成同一条无偏分布 $P_j^{0}$。

**与单偏置的联系。** 当只有一个窗口（ $S=1$）时，上式给出 $P_j^{0}\propto n_{1j}/C_{1j}$——即把这条偏置直方图按 $1/C_{1j}$ 重加权。取 $C_{1j}=e^{-b(\mathbf{x}_j)}$ 便是乘以 $e^{b(\mathbf{x}_j)}$，与第 3–4 节的权重完全一致；WHAM 不过是把这一重加权推广到多条，并用最大似然确定各窗口的相对归一 $f_i$。

---

## 6. 说明

整个推导只依赖一次相消：因为 $p$ 与 $p_b$ 共享同一个底层势能 $u_0$，它们的比值不再依赖 $u_0$——只剩偏置 $b(\mathbf{x})$ 和配分函数比这个常数。这正是从有偏模拟做重加权在原理上精确的原因：把每个有偏样本乘以 $e^{ b(\mathbf{x})}$（至多相差一个整体归一化因子），无偏统计量便得以还原。

---

*英文版本：[`reweighting-weights.en.md`](./reweighting-weights.en.md)*
