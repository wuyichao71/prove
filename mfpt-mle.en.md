# Maximum-Likelihood Estimation of the Mean First-Passage Time (MFPT)

> A from-scratch derivation. Model the first-passage time as exponentially distributed and use maximum likelihood to obtain an estimator of the mean first-passage time (MFPT): first for complete observations, then generalized to include unfinished trajectories (right censoring), with the statistical error given by the Fisher information.

---

## 1. Background and notation

Let $T$ be the **first-passage time** — the time for the system to reach state B for the first time, starting from state A — and let $\tau=\langle T\rangle$ be its mean, the **mean first-passage time** (MFPT). In rare-event kinetics — for instance when a molecular simulation has a clear separation of timescales — the A→B transition is approximately a memoryless Poisson process with rate $k$, and the first-passage time is exponentially distributed. The goal is to estimate $\tau$ from finite simulation data, which is equivalent to estimating $k$.

---

## 2. Model: exponentially distributed first-passage times

Under the memoryless assumption the probability density of the first-passage time is

$$
p(t \mid k) = k e^{-k t}, \qquad t \ge 0 .
$$

Its survival function (the probability of not having transitioned by time $t$) is

$$
S(t) = P(T > t) = e^{-k t} .
$$

Averaging over the density, the MFPT is exactly the reciprocal of the rate:

$$
\tau = \langle T\rangle = \int_0^\infty t  p(t)  dt = \frac{1}{k} .
$$

So estimating $\tau$ is equivalent to estimating $k$.

---

## 3. Maximum likelihood for complete observations

Suppose we have $N$ independent trajectories that all reach B, at times $t_1,\dots,t_N$. By independence the joint likelihood is

$$
L(k) = \prod_{i=1}^{N} k e^{-k t_i} = k^{N} \exp\Big(-k \sum_{i=1}^{N} t_i\Big) .
$$

Taking the logarithm gives the log-likelihood

$$
\ell(k) = \ln L(k) = N \ln k - k \sum_{i=1}^{N} t_i .
$$

Setting its derivative with respect to $k$ to zero,

$$
\frac{d\ell}{dk} = \frac{N}{k} - \sum_{i=1}^{N} t_i = 0 \quad\Longrightarrow\quad k_{\mathrm{ML}} = \frac{N}{\sum_{i=1}^{N} t_i} .
$$

The second derivative $\dfrac{d^2\ell}{dk^2} = -\dfrac{N}{k^2} < 0$ confirms a maximum. Hence the maximum-likelihood estimate of the MFPT is simply the **sample mean**:

$$
\boxed{\ \tau_{\mathrm{ML}} = \frac{1}{k_{\mathrm{ML}}} = \frac{1}{N} \sum_{i=1}^{N} t_i\ } .
$$

---

## 4. With censoring: unfinished trajectories

Real simulations run for a finite time: some trajectories reach B at time $t_i$ (call these **events**), while others have still not reached B by the end of their observation window at time $T_j$ (**right-censored**). An event contributes the density $p(t_i)=k e^{-k t_i}$; a censored trajectory only tells us "no transition up to $T_j$," contributing the survival probability $S(T_j)=e^{-k T_j}$. The likelihood is therefore

$$
L(k) = \prod_{i\in\mathrm{event}} k e^{-k t_i} \prod_{j\in\mathrm{cens}} e^{-k T_j} = k^{N_e} \exp(-k \mathcal{T}) ,
$$

where $N_e$ is the number of events (successful transitions) and $\mathcal{T}=\sum_i t_i+\sum_j T_j$ is the **total observation time** over all trajectories. With $\ell(k)=N_e\ln k - k\mathcal{T}$, setting the derivative to zero gives

$$
k_{\mathrm{ML}} = \frac{N_e}{\mathcal{T}} , \qquad \boxed{\ \tau_{\mathrm{ML}} = \frac{\mathcal{T}}{N_e}\ } .
$$

That is, the MFPT estimate equals the **total observation time divided by the number of transitions**. With no censoring ($N_e=N$, $\mathcal{T}=\sum_i t_i$) this reduces to the result of §3.

---

## 5. Statistical error (Fisher information)

The curvature of the log-likelihood gives the Fisher information

$$
I(k) = -\frac{d^2\ell}{dk^2} = \frac{N_e}{k^2} .
$$

By the asymptotic properties of maximum-likelihood estimators, the variance of $k_{\mathrm{ML}}$ is about $I^{-1}$:

$$
\mathrm{Var}(k_{\mathrm{ML}}) \approx \frac{k^2}{N_e} \quad\Longrightarrow\quad \frac{\sigma_k}{k} \approx \frac{1}{\sqrt{N_e}} .
$$

Since $\tau=1/k$, the relative error is preserved under the reciprocal, so

$$
\boxed{\ \frac{\sigma_\tau}{\tau} \approx \frac{1}{\sqrt{N_e}}\ } .
$$

Key conclusion: the precision is set by the **number of observed transitions** $N_e$, not by the total simulation time as such — running longer helps only insofar as it produces more transition events.

---

## 6. Remarks

- **Poisson-counting view.** Observing $N_e$ events in a total time $\mathcal{T}$ is itself a Poisson process, and $k_{\mathrm{ML}}=N_e/\mathcal{T}$ is exactly the standard counting estimate of the rate.
- **Bias.** For complete observations $\tau_{\mathrm{ML}}=\frac{1}{N}\sum_i t_i$ is unbiased for $\tau$, i.e. $\mathbb{E}[\tau_{\mathrm{ML}}]=\tau$; the rate estimate $k_{\mathrm{ML}}$ (the reciprocal of the sample mean) is slightly biased — for the exponential distribution $\mathbb{E}[k_{\mathrm{ML}}]=\frac{N}{N-1}k$ — which is negligible for large $N$.
- **Scope.** The above rests on the single-exponential (memoryless) assumption, which requires a separation of timescales; multi-exponential or non-Markovian first-passage times need a richer model.

---

*Chinese version: [`mfpt-mle.zh.md`](./mfpt-mle.zh.md).*
