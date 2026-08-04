# 泊松过程的两种严格定义与等价性证明

> 记录（齐次）泊松过程的两种等价定义——**无穷小（公理化）定义**与**泊松增量定义**——以及二者互推的标准方法。它为“把稀有跃迁建模为泊松过程、首次通过时间服从指数分布”提供了严格基础（见 [`mfpt-mle.zh.md`](./mfpt-mle.zh.md)）。

---

## 1. 记号与预备

**计数过程** $\{N(t): t\ge 0\}$ 指： $N(t)$ 表示到时刻 $t$ 为止发生的事件数，取非负整数、右连续、非降，且 $N(0)=0$。增量 $N(t)-N(s)$（ $s<t$）即区间 $(s,t]$ 内的事件数。速率参数记为 $\lambda>0$。记号 $o(h)$ 表示当 $h\to 0$ 时满足 $o(h)/h\to 0$ 的量。

两种定义都会用到以下两条关于增量的性质：

- **独立增量：** 对任意 $0\le t_0<t_1<\dots<t_m$，增量 $N(t_1)-N(t_0),\ N(t_2)-N(t_1),\ \dots,\ N(t_m)-N(t_{m-1})$ 相互独立。
- **平稳增量：** $N(s+t)-N(s)$ 的分布只依赖于区间长度 $t$，与起点 $s$ 无关。

---

## 2. 两种定义

**定义一（无穷小 / 公理化）。** 计数过程 $N(t)$（ $N(0)=0$）称为速率 $\lambda$ 的泊松过程，若它具有独立增量、平稳增量，并且当 $h\to 0^+$ 时

$$
P(N(h)=1) = \lambda h + o(h), \qquad P(N(h)\ge 2) = o(h) .
$$

（等价地 $P(N(h)=0)=1-\lambda h+o(h)$。）直观含义：在极短时间内至多发生一次事件，且发生一次的概率正比于时长。

**定义二（泊松增量）。** 计数过程 $N(t)$（ $N(0)=0$）称为速率 $\lambda$ 的泊松过程，若它具有独立增量，并且任一长度为 $t$ 的区间内的事件数服从参数 $\lambda t$ 的泊松分布：

$$
P\big(N(s+t)-N(s)=n\big) = e^{-\lambda t}\frac{(\lambda t)^n}{n!}, \qquad n=0,1,2,\dots
$$

此式只依赖区间长度 $t$，因此平稳增量自动成立。

---

## 3. 定义一 $\Rightarrow$ 定义二（先建立微分方程再求解）

记 $P_n(t)=P(N(t)=n)$。由 $N(0)=0$ 有初值 $P_0(0)=1$，且 $P_n(0)=0$（ $n\ge1$）。

**$n=0$ 的情形。** 区间 $(0,t+h]$ 内无事件，等价于 $(0,t]$ 内无事件且 $(t,t+h]$ 内无事件；由独立增量与平稳增量，

$$
P_0(t+h) = P_0(t)  P_0(h) = P_0(t)\big(1-\lambda h+o(h)\big) .
$$

移项、除以 $h$ 并令 $h\to0$，得

$$
\frac{dP_0}{dt} = -\lambda P_0(t) \quad\Longrightarrow\quad P_0(t)=e^{-\lambda t} .
$$

**$n\ge1$ 的情形。** 按 $(t,t+h]$ 内发生 $0$、 $1$、 $\ge2$ 个事件分解，并用独立、平稳增量，

$$
P_n(t+h) = P_n(t)P_0(h) + P_{n-1}(t)P_1(h) + \sum_{k\ge2}P_{n-k}(t)P_k(h) .
$$

代入 $P_0(h)=1-\lambda h+o(h)$、 $P_1(h)=\lambda h+o(h)$、 $\sum_{k\ge2}P_k(h)=o(h)$，得

$$
P_n(t+h) = P_n(t)(1-\lambda h) + P_{n-1}(t)\lambda h + o(h) .
$$

同样取极限，得到生灭过程的**前向方程**：

$$
\frac{dP_n}{dt} = -\lambda P_n(t) + \lambda P_{n-1}(t), \qquad n\ge1 .
$$

引入积分因子 $e^{\lambda t}$：令 $Q_n(t)=e^{\lambda t}P_n(t)$，则方程化为 $Q_n'(t)=\lambda Q_{n-1}(t)$，且 $Q_0(t)=1$、 $Q_n(0)=0$。逐次积分（数学归纳法）：若 $Q_{n-1}(t)=\dfrac{(\lambda t)^{n-1}}{(n-1)!}$，则

$$
Q_n(t)=\int_0^t \lambda Q_{n-1}(s)  ds = \frac{(\lambda t)^n}{n!} .
$$

因此 $P_n(t)=e^{-\lambda t}Q_n(t)$，即

$$
\boxed{\ P_n(t)=e^{-\lambda t}\frac{(\lambda t)^n}{n!}\ } .
$$

对一般区间，由平稳增量把 $N(s+t)-N(s)$ 换回 $N(t)$，即得定义二。 $\blacksquare$

---

## 4. 定义二 $\Rightarrow$ 定义一（泰勒展开验证无穷小条件）

独立增量是定义二的直接假设；平稳增量也已由“概率只依赖区间长度”给出。于是只需验证三条无穷小条件。由 $N(h)\sim\mathrm{Poisson}(\lambda h)$，

$$
P(N(h)=0) = e^{-\lambda h} = 1-\lambda h+\frac{(\lambda h)^2}{2}-\dots = 1-\lambda h+o(h) ,
$$

$$
P(N(h)=1) = e^{-\lambda h}\lambda h = \lambda h\big(1-\lambda h+\dots\big) = \lambda h+o(h) ,
$$

$$
P(N(h)\ge2) = 1-P(N(h)=0)-P(N(h)=1) = 1-\big(1-\lambda h+o(h)\big)-\big(\lambda h+o(h)\big) = o(h) .
$$

其中 $\lambda h$ 一次项恰好相消，余项为 $O(h^2)=o(h)$。三条无穷小条件成立，故 $N(t)$ 满足定义一。 $\blacksquare$

---

## 5. 注记

- **第三种等价刻画（间隔时间）：** 相继事件的间隔时间 $T_1,T_2,\dots$ 相互独立且都服从指数分布 $\mathrm{Exp}(\lambda)$。它与上述两种定义等价，正是把首次通过时间建模为指数分布的依据（见 [`mfpt-mle.zh.md`](./mfpt-mle.zh.md)）。
- **平稳增量的地位：** 定义二的泊松增量已隐含平稳性，定义一则需单独假设。若放弃平稳增量、把常数 $\lambda$ 换成 $\lambda(t)$，两套定义仍等价，给出**非齐次泊松过程**——只需把结果中的 $\lambda t$ 换成 $\int_0^t\lambda(s)  ds$。
- **可微性说明：** 第 3 节求导默认了 $P_n(t)$ 可微。由无穷小条件可先证 $P_n$ 连续、再证右可导进而可微，此处从略。

---

*英文版见 [`poisson-process.en.md`](./poisson-process.en.md)。*
