# 有偏系综与无偏系综之间的重加权

> 手写笔记的整理稿。本文推导重要性采样的**权重**，用它把从*有偏*玻尔兹曼分布中采得的样本，重新换算为*无偏*目标分布下的期望值。

---

## 1. 遍历假设与大数定律

两条经典结果共同保证了可以用有限的模拟来估计系综平均。

**遍历假设。** 对于动力学系统足够长的轨迹，观测量的*时间*平均等于其*系综*平均。对相空间上的观测量 $A(\vec x)$，沿轨迹 $\vec x(t)$ 的时间平均为

$$
\overline{A}
= \lim_{T \to \infty} \frac{1}{T} \int_{0}^{T} A(\vec x(t)) dt,
$$

而系综平均定义为

$$
\langle A\rangle = \int A(\vec x) \rho(\vec x) d\vec x,
$$

其中 $\rho(\vec x)$ 是系统在相空间中的平衡分布。遍历假设断言

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

两者结合，观测量的系综平均便可用模拟生成的构型 $\{\vec x_i\}$ 来估计：

$$
\langle A\rangle = \int A(\vec x) \rho(\vec x) d\vec x
 \approx  \frac{1}{N}\sum_{i=1}^{N} A(\vec x_i).
$$

**自由能则不同。** 上述估计只对真正的观测量成立。自由能*并非*观测量：它依赖配分函数——一个高维积分，无法从归一化的采样分布中直接得到，因为概率密度本身就是通过配分函数定义的，而对密度取平均也没有物理意义。这一空缺正是重加权要解决的：第 4 节的重加权估计就是把上面的样本平均给每一项乘上一个重要性权重，未知的归一化只以配分函数比 $c_b/c$ 的形式出现（第 4–5 节），并在权重归一化后相消。当偏置 $b\equiv 0$ 时，它退化为这里的普通样本平均。

---

## 2. 设定

考察构型 $\vec x$（构型空间中的一点），比较两个玻尔兹曼型概率密度。

**目标（无偏）分布。** 记约化势能为 $u_0(\vec x)$，

$$
p(\vec x) = \frac{1}{c} \exp\big(-u_0(\vec x)\big),
\qquad
c = \int \exp\big(-u_0(\vec x)\big) d\vec x .
$$

其中 $c$ 是配分函数，使 $p$ 的积分归一。

**有偏分布。** 在 $u_0$ 上叠加一个偏置势 $b(\vec x)$，

$$
p_b(\vec x) = \frac{1}{c_b} \exp\big(-u_0(\vec x) - b(\vec x)\big),
\qquad
c_b = \int \exp\big(-u_0(\vec x) - b(\vec x)\big) d\vec x .
$$

采样（例如增强采样或伞形采样模拟）在 $p_b$ 下进行，而我们想要的是 $p$ 下的平均值。

以上两式对应笔记的前两行。

---

## 3. 重加权因子

要用有偏样本还原无偏平均，需要两个密度的逐点比值，即**重要性权重**：

$$
w(\vec x)  =  \frac{p(\vec x)}{p_b(\vec x)}
 = 
\frac{\dfrac{1}{c} \exp\big(-u_0(\vec x)\big)}
     {\dfrac{1}{c_b} \exp\big(-u_0(\vec x) - b(\vec x)\big)} .
$$

指数因子通过其宗量相减合并，

$$
-u_0(\vec x)  -  \big(-u_0(\vec x) - b(\vec x)\big)  =  b(\vec x),
$$

公共的 $-u_0(\vec x)$ 相消，指数上只剩下偏置项：

$$
w(\vec x)
 = 
\frac{1}{c}\cdot\frac{\exp\big(b(\vec x)\big)}{ 1/c_b }
 = 
\boxed{ \frac{c_b}{c} \exp\big(b(\vec x)\big)  }.
$$

这对应笔记的中间部分。权重由一个常数前因子 $c_b/c$（两个配分函数之比）与依赖构型的因子 $\exp\big(b(\vec x)\big)$ 相乘而成，后者恰好抵消了先前施加的偏置。

---

## 4. 归一化权重

实际计算中前因子 $c_b/c$ 未知，因为配分函数并不直接算出。真正用到的是**自归一化权重** $W(\vec x)$——把同一个比值中的有偏密度 $p_b$（即 $c_b^{-1}\exp(-u_0 - b)$）展开写在分母上，也就是笔记中标注 “mbar” 的写法：

$$
W(\vec x)
 = 
\frac{1}{c} 
\frac{\exp\big(-u_0(\vec x)\big)}
     { c_b^{-1}\exp\big(-u_0(\vec x) - b(\vec x)\big) }
 = 
\frac{c_b}{c} \exp\big(b(\vec x)\big).
$$

由于分母恰为 $p_b(\vec x)$，结果与第 3 节相同：$W(\vec x) = w(\vec x) = \dfrac{c_b}{c}\exp\big(b(\vec x)\big)$。这对应笔记的最后一行。

只要在样本集 $\{\vec x_i\}$ 上对权重做归一化，未知常数 $c_b/c$ 就自动消去：

$$
\widehat{W}(\vec x_i) = \frac{w(\vec x_i)}{\sum_j w(\vec x_j)}
= \frac{\exp\big(b(\vec x_i)\big)}{\sum_j \exp\big(b(\vec x_j)\big)} .
$$

于是任意观测量 $A$ 的无偏系综平均都可仅用偏置值来估计：

$$
\langle A\rangle_p  \approx  \sum_i \widehat{W}(\vec x_i) A(\vec x_i)
 =  \frac{\sum_i A(\vec x_i) e^{ b(\vec x_i)}}{\sum_i e^{ b(\vec x_i)}}.
$$

---

## 5. 说明

整个推导只依赖一次相消：因为 $p$ 与 $p_b$ 共享同一个底层势能 $u_0$，它们的比值不再依赖 $u_0$——只剩偏置 $b(\vec x)$ 和配分函数比这个常数。这正是从有偏模拟做重加权在原理上精确的原因：把每个有偏样本乘以 $e^{ b(\vec x)}$（至多相差一个整体归一化因子），无偏统计量便得以还原。

---

*英文版本：[`reweighting-weights.en.md`](./reweighting-weights.en.md)*
