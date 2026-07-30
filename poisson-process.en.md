# Two Rigorous Definitions of the Poisson Process and Their Equivalence

> A record of the two equivalent definitions of the (homogeneous) Poisson process — the **infinitesimal (axiomatic) definition** and the **Poisson-increment definition** — together with the standard method for deriving each from the other. This is the rigorous basis for modeling rare transitions as a Poisson process with exponentially distributed first-passage times (see [`mfpt-mle.en.md`](./mfpt-mle.en.md)).

---

## 1. Notation and preliminaries

A **counting process** $\{N(t): t\ge 0\}$ means: $N(t)$ is the number of events up to time $t$, taking non-negative integer values, right-continuous and non-decreasing, with $N(0)=0$. The increment $N(t)-N(s)$ (for $s<t$) is the number of events in the interval $(s,t]$. The rate parameter is $\lambda>0$. The symbol $o(h)$ denotes a quantity with $o(h)/h\to 0$ as $h\to 0$.

Both definitions use the following two properties of the increments:

- **Independent increments.** For any $0\le t_0<t_1<\dots<t_m$, the increments $N(t_1)-N(t_0),\ N(t_2)-N(t_1),\ \dots,\ N(t_m)-N(t_{m-1})$ are mutually independent.
- **Stationary increments.** The distribution of $N(s+t)-N(s)$ depends only on the interval length $t$, not on the starting point $s$.

---

## 2. The two definitions

**Definition 1 (infinitesimal / axiomatic).** A counting process $N(t)$ (with $N(0)=0$) is a Poisson process of rate $\lambda$ if it has independent increments, stationary increments, and, as $h\to 0^+$,

$$
P(N(h)=1) = \lambda h + o(h), \qquad P(N(h)\ge 2) = o(h) .
$$

(Equivalently $P(N(h)=0)=1-\lambda h+o(h)$.) Intuitively: in an infinitesimal time at most one event occurs, and the probability of exactly one is proportional to the length.

**Definition 2 (Poisson increments).** A counting process $N(t)$ (with $N(0)=0$) is a Poisson process of rate $\lambda$ if it has independent increments and the number of events in any interval of length $t$ is Poisson distributed with mean $\lambda t$:

$$
P\big(N(s+t)-N(s)=n\big) = e^{-\lambda t}\frac{(\lambda t)^n}{n!}, \qquad n=0,1,2,\dots
$$

Since this depends only on the interval length $t$, stationary increments hold automatically.

---

## 3. Definition 1 $\Rightarrow$ Definition 2 (set up the differential equations, then solve)

Write $P_n(t)=P(N(t)=n)$. From $N(0)=0$ the initial values are $P_0(0)=1$ and $P_n(0)=0$ (for $n\ge1$).

**Case $n=0$.** No event in $(0,t+h]$ is equivalent to no event in $(0,t]$ and no event in $(t,t+h]$; by independent and stationary increments,

$$
P_0(t+h) = P_0(t)  P_0(h) = P_0(t)\big(1-\lambda h+o(h)\big) .
$$

Rearranging, dividing by $h$ and letting $h\to0$,

$$
\frac{dP_0}{dt} = -\lambda P_0(t) \quad\Longrightarrow\quad P_0(t)=e^{-\lambda t} .
$$

**Case $n\ge1$.** Split according to whether $0$, $1$, or $\ge2$ events occur in $(t,t+h]$, using independent and stationary increments,

$$
P_n(t+h) = P_n(t)P_0(h) + P_{n-1}(t)P_1(h) + \sum_{k\ge2}P_{n-k}(t)P_k(h) .
$$

Substituting $P_0(h)=1-\lambda h+o(h)$, $P_1(h)=\lambda h+o(h)$, and $\sum_{k\ge2}P_k(h)=o(h)$,

$$
P_n(t+h) = P_n(t)(1-\lambda h) + P_{n-1}(t)\lambda h + o(h) .
$$

Taking the limit gives the **forward equations** of the pure-birth process:

$$
\frac{dP_n}{dt} = -\lambda P_n(t) + \lambda P_{n-1}(t), \qquad n\ge1 .
$$

Introduce the integrating factor $e^{\lambda t}$: with $Q_n(t)=e^{\lambda t}P_n(t)$ the equation becomes $Q_n'(t)=\lambda Q_{n-1}(t)$, with $Q_0(t)=1$ and $Q_n(0)=0$. Integrating successively (by induction): if $Q_{n-1}(t)=\dfrac{(\lambda t)^{n-1}}{(n-1)!}$, then

$$
Q_n(t)=\int_0^t \lambda Q_{n-1}(s)  ds = \frac{(\lambda t)^n}{n!} .
$$

Hence $P_n(t)=e^{-\lambda t}Q_n(t)$, that is,

$$
\boxed{\ P_n(t)=e^{-\lambda t}\frac{(\lambda t)^n}{n!}\ } .
$$

For a general interval, stationary increments turn $N(s+t)-N(s)$ back into $N(t)$, which is Definition 2. $\blacksquare$

---

## 4. Definition 2 $\Rightarrow$ Definition 1 (verify the infinitesimal conditions by Taylor expansion)

Independent increments are assumed directly in Definition 2, and stationary increments follow from the probability depending only on the interval length. So it remains to verify the three infinitesimal conditions. From $N(h)\sim\mathrm{Poisson}(\lambda h)$,

$$
P(N(h)=0) = e^{-\lambda h} = 1-\lambda h+\frac{(\lambda h)^2}{2}-\dots = 1-\lambda h+o(h) ,
$$

$$
P(N(h)=1) = e^{-\lambda h}\lambda h = \lambda h\big(1-\lambda h+\dots\big) = \lambda h+o(h) ,
$$

$$
P(N(h)\ge2) = 1-P(N(h)=0)-P(N(h)=1) = 1-\big(1-\lambda h+o(h)\big)-\big(\lambda h+o(h)\big) = o(h) .
$$

The linear $\lambda h$ terms cancel and the remainder is $O(h^2)=o(h)$. All three infinitesimal conditions hold, so $N(t)$ satisfies Definition 1. $\blacksquare$

---

## 5. Remarks

- **A third equivalent characterization (interarrival times).** The times $T_1,T_2,\dots$ between successive events are independent and each exponentially distributed, $\mathrm{Exp}(\lambda)$. This is equivalent to the two definitions above and is exactly the basis for modeling first-passage times as exponential (see [`mfpt-mle.en.md`](./mfpt-mle.en.md)).
- **The role of stationary increments.** Definition 2's Poisson increment already implies stationarity, whereas Definition 1 needs it as a separate axiom. Dropping stationarity and letting the constant $\lambda$ become $\lambda(t)$ keeps the two definitions equivalent and yields the **inhomogeneous Poisson process** — one just replaces $\lambda t$ by $\int_0^t\lambda(s)  ds$.
- **On differentiability.** §3 assumed $P_n(t)$ differentiable. The infinitesimal conditions first give continuity of $P_n$, then right-differentiability and hence differentiability; the details are omitted.

---

*Chinese version: [`poisson-process.zh.md`](./poisson-process.zh.md).*
