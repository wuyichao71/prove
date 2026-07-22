# Liouville's Theorem

> A cleaned-up write-up of handwritten notes. It proves that phase-space density is conserved under Hamiltonian flow, via two independent methods.

---

## 1. Introduction

In Hamiltonian mechanics, the complete state of a system is fixed by its generalized coordinate $q$ and generalized momentum $p$. The space they span is the **phase space**, and the time evolution of the system traces out a trajectory in it.

In statistical mechanics we usually care not about a single system but about an **ensemble** of a large number of identical systems. The distribution of the ensemble over phase space is described by the **phase-space density** $\rho(t, q, p)$: the quantity $\rho\,dq\,dp$ is the number (fraction) of systems lying in the volume element $dq\,dp$ at time $t$.

**Liouville's Theorem** states that as one moves along the Hamiltonian flow, the phase-space density stays constant; equivalently, the phase-space volume element is preserved under the evolution. The intuitive picture is that the "probability fluid" in phase space is **incompressible**.

Two independent proofs follow. They correspond to the two pages of notes and are, at heart, two faces of the same fact.

---

## 2. Preliminaries: Hamilton's Equations

Let the Hamiltonian be $H(q, p)$ (allowing time dependence, $H(t,q,p)$, does not affect the derivation). The equations of motion are **Hamilton's canonical equations**:

$$
\dot q = \frac{\partial H}{\partial p}, \qquad \dot p = -\frac{\partial H}{\partial q}.
$$

Define the phase-space **velocity field**:

$$
\vec v \equiv (\dot q,\ \dot p) = \left(\frac{\partial H}{\partial p},\ -\frac{\partial H}{\partial q}\right).
$$

The divergence operator in phase space is $\nabla \equiv \left(\dfrac{\partial}{\partial q},\ \dfrac{\partial}{\partial p}\right)$.

---

## 3. Statement

Liouville's Theorem has two equivalent formulations:

- **Density conserved along trajectories:**
$$
\frac{d\rho}{dt} = 0.
$$

- **Phase-space volume conserved:** if a (canonical) transformation maps $(q,p)$ to $(Q,P)$, then
$$
dQ\,dP = dq\,dp.
$$

**Proof 1** (continuity equation) establishes the first; **Proof 2** (canonical transformations) establishes the second.

---

## 4. Proof 1 — Continuity Equation

> Corresponds to the first page of notes. Treat the ensemble as a fluid in phase space and argue with the language of fluid conservation (a continuity equation).

### 4.1 Expand the total derivative

The total time derivative of the density $\rho(t,q,p)$ along a trajectory is

$$
\frac{d\rho}{dt}
= \frac{\partial \rho}{\partial t}
+ \frac{\partial \rho}{\partial q}\,\dot q
+ \frac{\partial \rho}{\partial p}\,\dot p .
$$

Substituting Hamilton's equations $\dot q = \partial H/\partial p$ and $\dot p = -\partial H/\partial q$:

$$
\frac{d\rho}{dt}
= \frac{\partial \rho}{\partial t}
+ \frac{\partial \rho}{\partial q}\frac{\partial H}{\partial p}
- \frac{\partial \rho}{\partial p}\frac{\partial H}{\partial q}.
\tag{4.1}
$$

### 4.2 The continuity equation (physical premise)

The total number of systems in the ensemble is conserved — representative points are neither created nor destroyed. For any fixed region of phase space, the rate of decrease of the enclosed systems equals the net outward flux through its boundary; recast in differential form via the divergence theorem, this gives the **continuity equation** in phase space:

$$
\boxed{\ \frac{\partial \rho}{\partial t} + \nabla\cdot(\rho\vec v) = 0\ }
\tag{4.2}
$$

This has exactly the form of the mass-conservation equation in fluid mechanics; it is the **physical starting point** of the proof — supplied by the fact that the number of systems is conserved, not obtained by algebra.

To prepare for substitution, expand the density-flux divergence into an identity with the product rule:

$$
\nabla\cdot(\rho\vec v)
= \frac{\partial}{\partial q}\big(\rho\,\dot q\big)
+ \frac{\partial}{\partial p}\big(\rho\,\dot p\big)
= \underbrace{\left(\frac{\partial \rho}{\partial q}\dot q + \frac{\partial \rho}{\partial p}\dot p\right)}_{(\vec v\cdot\nabla)\rho}
+ \rho\underbrace{\left(\frac{\partial \dot q}{\partial q} + \frac{\partial \dot p}{\partial p}\right)}_{\nabla\cdot\vec v}.
$$

### 4.3 Key lemma: the flow is divergence-free

Compute the divergence of the phase-space velocity field, substituting Hamilton's equations:

$$
\nabla\cdot\vec v
= \frac{\partial \dot q}{\partial q} + \frac{\partial \dot p}{\partial p}
= \frac{\partial}{\partial q}\!\left(\frac{\partial H}{\partial p}\right)
+ \frac{\partial}{\partial p}\!\left(-\frac{\partial H}{\partial q}\right)
= \frac{\partial^2 H}{\partial q\,\partial p} - \frac{\partial^2 H}{\partial p\,\partial q}.
$$

As long as the second partials of $H$ are continuous, the mixed partials commute (Clairaut/Schwarz theorem), so

$$
\boxed{\ \nabla\cdot\vec v = 0\ }
\tag{4.3}
$$

**This is the heart of the whole proof:** the Hamiltonian flow is divergence-free; the phase-space "fluid" is incompressible.

### 4.4 Conclusion

Substitute (4.3) back into the continuity equation (4.2). Since $\nabla\cdot(\rho\vec v) = (\vec v\cdot\nabla)\rho + \rho\,(\nabla\cdot\vec v)$ and the second term vanishes,

$$
\frac{\partial \rho}{\partial t}
= -\,\nabla\cdot(\rho\vec v)
= -\,\vec v\cdot\nabla\rho
= -\frac{\partial \rho}{\partial q}\dot q - \frac{\partial \rho}{\partial p}\dot p .
$$

Rearranging gives

$$
\frac{d\rho}{dt}
= \frac{\partial \rho}{\partial t}
+ \frac{\partial \rho}{\partial q}\dot q
+ \frac{\partial \rho}{\partial p}\dot p
= 0 .
\qquad\blacksquare
$$

**Conclusion:** an observer co-moving with the flow sees a constant local density, $d\rho/dt = 0$.

---

## 5. Proof 2 — Volume Invariance under Canonical Transformations

> Corresponds to the second and third pages of notes. **First prove that a general canonical transformation preserves phase-space volume**, then connect back by treating time evolution as an infinitesimal canonical transformation.

### 5.1 A canonical transformation preserves phase-space volume

Consider a transformation $(q,p)\to(Q,P)$. A **canonical transformation** is one that keeps the equations of motion in Hamiltonian form in the new variables:

$$
\dot Q = \frac{\partial H}{\partial P},
\qquad
\dot P = -\frac{\partial H}{\partial Q}.
$$

Old and new volume elements are related by the Jacobian determinant:

$$
dQ\,dP = \left|\frac{\partial(Q,P)}{\partial(q,p)}\right|\,dq\,dp,
\qquad
\frac{\partial(Q,P)}{\partial(q,p)}
= \frac{\partial Q}{\partial q}\frac{\partial P}{\partial p}
- \frac{\partial Q}{\partial p}\frac{\partial P}{\partial q}
\equiv \{Q, P\},
$$

where the last step recognizes this as the **Poisson bracket** $\{Q,P\}$. Thus it suffices to prove $\{Q,P\}=1$ to obtain $dQ\,dP = dq\,dp$.

**Proof that $\{Q,P\}=1$** (following the notes: expand the time derivatives of the new variables with the chain rule, then match them against the Hamiltonian equations they satisfy).

On one hand, the time derivative of $Q=Q(q,p)$ along a trajectory follows from the chain rule, then substitute Hamilton's equations in the old variables:

$$
\dot Q
= \frac{\partial Q}{\partial q}\dot q + \frac{\partial Q}{\partial p}\dot p
= \frac{\partial Q}{\partial q}\frac{\partial H}{\partial p}
- \frac{\partial Q}{\partial p}\frac{\partial H}{\partial q}.
\tag{5.1}
$$

On the other hand, regard $H$ as a function of the new variables $(Q,P)$ and expand $\partial H/\partial P$ by the chain rule:

$$
\frac{\partial H}{\partial P}
= \frac{\partial H}{\partial q}\frac{\partial q}{\partial P}
+ \frac{\partial H}{\partial p}\frac{\partial p}{\partial P}.
\tag{5.2}
$$

Being canonical requires (5.1) = (5.2), i.e. $\dot Q = \partial H/\partial P$. Since this must hold for an arbitrary Hamiltonian $H$, matching the coefficients of $\partial H/\partial q$ and $\partial H/\partial p$ yields two sets of relations:

$$
\frac{\partial Q}{\partial p} = -\frac{\partial q}{\partial P},
\qquad
\frac{\partial Q}{\partial q} = \frac{\partial p}{\partial P}.
\tag{5.3}
$$

Doing the same for $\dot P = -\partial H/\partial Q$ gives

$$
\frac{\partial P}{\partial p} = \frac{\partial q}{\partial Q},
\qquad
\frac{\partial P}{\partial q} = -\frac{\partial p}{\partial Q}.
\tag{5.4}
$$

Substituting (5.3) and (5.4) into the Jacobian:

$$
\{Q,P\}
= \frac{\partial Q}{\partial q}\frac{\partial P}{\partial p}
- \frac{\partial Q}{\partial p}\frac{\partial P}{\partial q}
= \frac{\partial p}{\partial P}\frac{\partial q}{\partial Q}
- \left(-\frac{\partial q}{\partial P}\right)\!\left(-\frac{\partial p}{\partial Q}\right)
= \frac{\partial q}{\partial Q}\frac{\partial p}{\partial P}
- \frac{\partial q}{\partial P}\frac{\partial p}{\partial Q}.
$$

The right-hand side is exactly the Jacobian of the inverse transformation, $\dfrac{\partial(q,p)}{\partial(Q,P)}$. But the forward and inverse Jacobians are reciprocals:

$$
\{Q,P\} = \frac{\partial(Q,P)}{\partial(q,p)},
\qquad
\frac{\partial(q,p)}{\partial(Q,P)} = \left(\frac{\partial(Q,P)}{\partial(q,p)}\right)^{-1},
$$

so $\{Q,P\} = \{Q,P\}^{-1}$, i.e. $\{Q,P\}^2 = 1$. Taking the branch continuously connected to the identity transformation, $\{Q,P\}=+1$. Therefore

$$
\boxed{\ \{Q,P\} = 1\ } \quad\Longrightarrow\quad \boxed{\ dQ\,dP = dq\,dp\ }.
$$

**This is volume invariance under canonical transformations:** any canonical transformation preserves the phase-space volume element.

> In matrix language, the Jacobian matrix $J$ of a canonical transformation satisfies the **symplectic condition** $J^{\mathsf T}\Omega J = \Omega$, where $\Omega = \begin{pmatrix}0&1\\-1&0\end{pmatrix}$ is the symplectic form. Taking determinants of both sides gives $(\det J)^2 = 1$, consistent with the result above.

### 5.2 Time evolution is an infinitesimal canonical transformation

Time evolution is itself a one-parameter family of canonical transformations: after an infinitesimal time $dt$, a phase point moves according to Hamilton's equations,

$$
(q,\ p)\ \longrightarrow\ (Q,\ P)
= \left(\,q + \frac{\partial H}{\partial p}\,dt,\ \ p - \frac{\partial H}{\partial q}\,dt\,\right)
= (q + \dot q\,dt,\ \ p + \dot p\,dt).
$$

By the general result of §5.1 it must preserve volume. One can also verify this directly — compute the Jacobian of this map and expand to $O(dt)$:

$$
\frac{\partial Q}{\partial q} = 1 + \frac{\partial \dot q}{\partial q}\,dt,
\quad
\frac{\partial Q}{\partial p} = \frac{\partial \dot q}{\partial p}\,dt,
\quad
\frac{\partial P}{\partial q} = \frac{\partial \dot p}{\partial q}\,dt,
\quad
\frac{\partial P}{\partial p} = 1 + \frac{\partial \dot p}{\partial p}\,dt,
$$

so that

$$
\det J
= \left(1 + \frac{\partial \dot q}{\partial q}dt\right)\!\left(1 + \frac{\partial \dot p}{\partial p}dt\right)
- \left(\frac{\partial \dot q}{\partial p}dt\right)\!\left(\frac{\partial \dot p}{\partial q}dt\right)
= 1 + \left(\frac{\partial \dot q}{\partial q} + \frac{\partial \dot p}{\partial p}\right)dt + O(dt^2).
$$

The bracket is precisely the divergence of the flow $\nabla\cdot\vec v$, and Proof 1 already showed $\nabla\cdot\vec v = 0$, hence

$$
\boxed{\ \det J = 1 + (\nabla\cdot\vec v)\,dt + O(dt^2) = 1\ }
\quad\Longrightarrow\quad
dQ\,dP = dq\,dp .
\qquad\blacksquare
$$

For finite times, simply compose the infinitesimal steps; the determinant stays identically 1. This agrees with the canonical-transformation result of §5.1 and ties Proof 2 back to Proof 1: **"divergence-free" and "unit Jacobian" are the same fact, at first order.**

---

## 6. Poisson-Bracket Form

Introducing the **Poisson bracket**

$$
\{A, B\} \equiv \frac{\partial A}{\partial q}\frac{\partial B}{\partial p} - \frac{\partial A}{\partial p}\frac{\partial B}{\partial q},
$$

the result of §4.4 can be written compactly as the **Liouville equation**:

$$
\frac{\partial \rho}{\partial t}
= -\frac{\partial \rho}{\partial q}\frac{\partial H}{\partial p} + \frac{\partial \rho}{\partial p}\frac{\partial H}{\partial q}
= \{H, \rho\}.
$$

> Sign convention: with $\{A,B\}=\partial_q A\,\partial_p B - \partial_p A\,\partial_q B$, we have $\partial\rho/\partial t = \{H,\rho\} = -\{\rho,H\}$. Different textbooks may differ by a sign, so read this against the Poisson-bracket definition in use.

This is in one-to-one correspondence with the **von Neumann equation** obeyed by the density operator $\hat\rho$ in quantum mechanics,

$$
i\hbar\,\frac{\partial \hat\rho}{\partial t} = [\hat H, \hat\rho],
$$

where the classical Poisson bracket $\{\cdot,\cdot\}$ corresponds to the quantum commutator $\tfrac{1}{i\hbar}[\cdot,\cdot]$ — a textbook instance of the classical–quantum correspondence.

---

## 7. Physical Interpretation

- **Incompressible phase flow:** $\nabla\cdot\vec v = 0$ means the "fluid" of representative points is neither compressed nor expanded. A blob of phase points initially occupying an area $A$ can be strongly stretched and distorted (like the deformed parallelogram sketched on the third page of notes), yet its **area/volume always remains $A$**.

- **Ensemble evolution:** density conservation along trajectories, $d\rho/dt=0$, guarantees the self-consistency of describing a statistical ensemble by a phase-space density — probability is neither created from nothing nor destroyed.

- **Foundation of statistical mechanics:** Liouville's Theorem is the dynamical basis for the "equal a priori probability" postulate of the **microcanonical ensemble**: a density $\rho = \text{const}$ that is uniform on the energy shell is a stationary solution of the Liouville equation ($\partial\rho/\partial t = 0$), and hence a natural equilibrium distribution.

- **Unifying the two proofs:** the core of Proof 1, $\nabla\cdot\vec v = 0$, and the core of Proof 2, $\det J = 1$, are **the same fact** — because the Jacobian expanded to first order is exactly
$$
\det J = 1 + (\nabla\cdot\vec v)\,dt + O(dt^2),
$$
so divergence-free $\iff$ zero first-order rate of volume change $\iff$ volume conserved. The former is the differential (local, instantaneous) view, the latter the integral (global, finite-step) view; they are two sides of one coin.

---

### Appendix: Notation

| Symbol | Meaning |
|---|---|
| $q,\ p$ | generalized coordinate & momentum |
| $H(t,q,p)$ | Hamiltonian |
| $\rho(t,q,p)$ | phase-space density |
| $\vec v = (\dot q,\dot p)$ | phase-space velocity field |
| $\nabla\cdot\vec v$ | divergence of the flow |
| $J,\ \det J$ | Jacobian matrix & determinant |
| $\{A,B\}$ | Poisson bracket |
| $(Q,P)$ | evolved / transformed canonical variables |

---

*Chinese version: see `liouville-theorem.zh.md`.*
