# Reweighting Between a Biased and an Unbiased Ensemble

> A cleaned-up write-up of handwritten notes. It derives the importance-sampling
> weight that converts samples drawn from a *biased* Boltzmann distribution back
> into expectation values under the *unbiased* target distribution.

---

## 1. Setup

We work with configurations $\vec x$ (a point in configuration space). Two
Boltzmann-type probability densities are compared.

- **Target (unbiased) distribution.** With reduced potential $u_0(\vec x)$,
$$
p(\vec x) = \frac{1}{c}\,\exp\!\big(-u_0(\vec x)\big),
\qquad
c = \int \exp\!\big(-u_0(\vec x)\big)\,d\vec x .
$$
Here $c$ is the partition function that normalizes $p$ to unit integral.

- **Biased distribution.** A bias potential $b(\vec x)$ is added to $u_0$,
$$
p_b(\vec x) = \frac{1}{c_b}\,\exp\!\big(-u_0(\vec x) - b(\vec x)\big),
\qquad
c_b = \int \exp\!\big(-u_0(\vec x) - b(\vec x)\big)\,d\vec x .
$$
Sampling (e.g. an enhanced-sampling or umbrella-sampling simulation) is carried
out under $p_b$; we want averages under $p$.

These two lines are the first two lines of the notes.

---

## 2. The reweighting factor

To recover an unbiased average from biased samples we need the pointwise ratio
of the two densities — the **importance weight**:

$$
w(\vec x) \;=\; \frac{p(\vec x)}{p_b(\vec x)}
\;=\;
\frac{\dfrac{1}{c}\,\exp\!\big(-u_0(\vec x)\big)}
     {\dfrac{1}{c_b}\,\exp\!\big(-u_0(\vec x) - b(\vec x)\big)} .
$$

The exponential factors combine through their arguments,
$$
-u_0(\vec x) \;-\; \big(-u_0(\vec x) - b(\vec x)\big) \;=\; b(\vec x),
$$
so the shared $-u_0(\vec x)$ cancels and only the bias survives in the exponent:

$$
w(\vec x)
\;=\;
\frac{1}{c}\cdot\frac{\exp\!\big(b(\vec x)\big)}{\,1/c_b\,}
\;=\;
\boxed{\,\frac{c_b}{c}\,\exp\!\big(b(\vec x)\big)\, }.
$$

This is the middle block of the notes. The weight is the product of a constant
prefactor $c_b/c$ (a ratio of partition functions) and the configuration-dependent
factor $\exp\!\big(b(\vec x)\big)$, which simply undoes the bias that was applied.

---

## 3. Normalized weight

In practice the prefactor $c_b/c$ is unknown, because the partition functions are
not computed directly. What is used is the **self-normalized weight** $W(\vec x)$,
in which the same ratio is written with the biased density $p_b$ (equivalently
$c_b^{-1}\exp(-u_0 - b)$) spelled out in the denominator — the "mbar"-style form
in the notes:

$$
W(\vec x)
\;=\;
\frac{1}{c}\,
\frac{\exp\!\big(-u_0(\vec x)\big)}
     {\,c_b^{-1}\exp\!\big(-u_0(\vec x) - b(\vec x)\big)\,}
\;=\;
\frac{c_b}{c}\,\exp\!\big(b(\vec x)\big).
$$

Since the denominator is exactly $p_b(\vec x)$, this reduces to the same result
as §2: $W(\vec x) = w(\vec x) = \dfrac{c_b}{c}\exp\!\big(b(\vec x)\big)$. This is the
bottom line of the notes.

The unknown constant $c_b/c$ drops out whenever the weights are normalized over a
sample set $\{\vec x_i\}$,
$$
\widehat{W}(\vec x_i) = \frac{w(\vec x_i)}{\sum_j w(\vec x_j)}
= \frac{\exp\!\big(b(\vec x_i)\big)}{\sum_j \exp\!\big(b(\vec x_j)\big)},
$$
so an unbiased ensemble average of any observable $A$ can be estimated purely from
the bias values:
$$
\langle A\rangle_p \;\approx\; \sum_i \widehat{W}(\vec x_i)\,A(\vec x_i)
\;=\; \frac{\sum_i A(\vec x_i)\,e^{\,b(\vec x_i)}}{\sum_i e^{\,b(\vec x_i)}}.
$$

---

## 4. Remark

The whole derivation rests on one cancellation: because $p$ and $p_b$ share the
same underlying potential $u_0$, their ratio keeps no dependence on $u_0$ — only
the bias $b(\vec x)$ and the constant partition-function ratio remain. That is
what makes reweighting from a biased simulation exact in principle: multiply each
biased sample by $e^{\,b(\vec x)}$ (up to overall normalization) and the unbiased
statistics are restored.

---

*Chinese version: [`reweighting-weights.zh.md`](./reweighting-weights.zh.md)*
