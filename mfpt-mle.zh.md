# 用最大似然法估计平均首次通过时间（MFPT）

> 从零推导。把首次通过时间建模为指数分布，用最大似然给出平均首次通过时间（MFPT）的估计量：先给出完整观测下的结果，再推广到含“未完成轨迹”（右删失）的情形，并用 Fisher 信息给出估计的统计误差。

---

## 1. 背景与记号

设系统从状态 A 出发、首次到达状态 B 所需的时间为**首次通过时间** $T$，其平均值即**平均首次通过时间** $\tau=\langle T\rangle$（MFPT）。在稀有事件动力学中——例如分子模拟里存在明显的时间尺度分离时——A→B 的跃迁近似为无记忆的泊松过程，速率记为 $k$，相应的首次通过时间服从指数分布。目标是由有限的模拟数据估计 $\tau$，这等价于估计 $k$。

---

## 2. 模型：指数分布的首次通过时间

无记忆假设下，首次通过时间的概率密度为

$$
p(t \mid k) = k e^{-k t}, \qquad t \ge 0 .
$$

其生存函数（到时刻 $t$ 仍未跃迁的概率）为

$$
S(t) = P(T > t) = e^{-k t} .
$$

由密度求平均，MFPT 正是速率的倒数：

$$
\tau = \langle T\rangle = \int_0^\infty t  p(t)  dt = \frac{1}{k} .
$$

因此“估计 $\tau$”与“估计 $k$”等价。

---

## 3. 完整观测下的最大似然估计

设有 $N$ 条相互独立的轨迹，全部分别在时间 $t_1,\dots,t_N$ 到达 B。由独立性，联合似然为

$$
L(k) = \prod_{i=1}^{N} k e^{-k t_i} = k^{N} \exp\Big(-k \sum_{i=1}^{N} t_i\Big) .
$$

取对数得对数似然

$$
\ell(k) = \ln L(k) = N \ln k - k \sum_{i=1}^{N} t_i .
$$

令其对 $k$ 的导数为零：

$$
\frac{d\ell}{dk} = \frac{N}{k} - \sum_{i=1}^{N} t_i = 0 \quad\Longrightarrow\quad k_{\mathrm{ML}} = \frac{N}{\sum_{i=1}^{N} t_i} .
$$

二阶导数 $\dfrac{d^2\ell}{dk^2} = -\dfrac{N}{k^2} < 0$，确为极大。于是 MFPT 的最大似然估计恰是**样本均值**：

$$
\boxed{\ \tau_{\mathrm{ML}} = \frac{1}{k_{\mathrm{ML}}} = \frac{1}{N} \sum_{i=1}^{N} t_i\ } .
$$

---

## 4. 含删失：未完成的轨迹

真实模拟时长有限：一部分轨迹在时间 $t_i$ 到达 B（称为**事件**），另一部分到其观测窗口结束（时间 $T_j$）仍未到达（**右删失**）。事件贡献密度 $p(t_i)=k e^{-k t_i}$；删失轨迹只提供“到 $T_j$ 为止尚未跃迁”这一信息，贡献生存概率 $S(T_j)=e^{-k T_j}$。于是似然为

$$
L(k) = \prod_{i\in\mathrm{event}} k e^{-k t_i} \prod_{j\in\mathrm{cens}} e^{-k T_j} = k^{N_e} \exp(-k \mathcal{T}) ,
$$

其中 $N_e$ 是事件（成功跃迁）数， $\mathcal{T}=\sum_i t_i+\sum_j T_j$ 是所有轨迹的**总观测时间**。对数似然为 $\ell(k)=N_e\ln k - k\mathcal{T}$，同样令导数为零得

$$
k_{\mathrm{ML}} = \frac{N_e}{\mathcal{T}} , \qquad \boxed{\ \tau_{\mathrm{ML}} = \frac{\mathcal{T}}{N_e}\ } .
$$

即 MFPT 的估计等于**总观测时间除以跃迁次数**。当没有删失（ $N_e=N$、 $\mathcal{T}=\sum_i t_i$）时，即回到第 3 节的结果。

---

## 5. 统计误差（Fisher 信息）

对数似然的曲率给出 Fisher 信息

$$
I(k) = -\frac{d^2\ell}{dk^2} = \frac{N_e}{k^2} .
$$

由最大似然估计的渐近性质， $k_{\mathrm{ML}}$ 的方差约为 $I^{-1}$：

$$
\mathrm{Var}(k_{\mathrm{ML}}) \approx \frac{k^2}{N_e} \quad\Longrightarrow\quad \frac{\sigma_k}{k} \approx \frac{1}{\sqrt{N_e}} .
$$

因 $\tau=1/k$，相对误差在倒数变换下不变，故

$$
\boxed{\ \frac{\sigma_\tau}{\tau} \approx \frac{1}{\sqrt{N_e}}\ } .
$$

关键结论：估计精度由**观测到的跃迁次数** $N_e$ 决定，而非单纯由模拟时长决定——延长模拟只有在带来更多跃迁事件时才提升精度。

---

## 6. 注记

- **泊松计数视角：** 在总时间 $\mathcal{T}$ 内发生 $N_e$ 次事件本身就是一个泊松过程， $k_{\mathrm{ML}}=N_e/\mathcal{T}$ 正是标准的计数速率估计。
- **偏差：** 完整观测下 $\tau_{\mathrm{ML}}=\frac{1}{N}\sum_i t_i$ 对 $\tau$ 无偏，即 $\mathbb{E}[\tau_{\mathrm{ML}}]=\tau$；而速率估计 $k_{\mathrm{ML}}$（样本均值的倒数）略有偏，指数分布下 $\mathbb{E}[k_{\mathrm{ML}}]=\frac{N}{N-1}k$，大 $N$ 时可忽略。
- **适用范围：** 以上依赖单指数（无记忆）假设，需时间尺度分离才成立；多指数或非马尔可夫的首次通过时间需要更复杂的模型。

---

*英文版见 [`mfpt-mle.en.md`](./mfpt-mle.en.md)。*
