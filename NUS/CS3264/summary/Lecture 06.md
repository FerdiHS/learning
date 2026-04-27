# Lecture 06: PAC Primer

## Basic Notions

- **Hypothesis space**: $\mathcal{H}$
- **Classifier / hypothesis**: $y \in \mathcal{H}$
- Dataset: $\mathcal{D}$

---

## Hoeffding’s Inequality

- Dataset:
    
    $\mathcal{D} = \{t_n\}_{n=1}^N$ where $t_n \sim \mathrm{Bern}(\mu)$
    
- Estimate Sample Average:
    
    $\hat{\mu} = \frac{\sum_{n=1}^N t_n}{N}$
    
- Then, $p(|\hat{\mu} - \mu| > \epsilon) \le 2 \exp(-2N\epsilon^2)$

---

## Hoeffding’s Inequality (General)

Suppose $X_1, X_2, \dots, X_N$ are **independent random variables** such that $a_n \le X_n \le b_n$.

Consider $S_n = X_1+X_2+\dots+X_N$. For all $\epsilon > 0$:

$$
p(|S_N - \mathbb{E}[S_N]| > \epsilon) \le 2 \exp \big(-\frac{2\epsilon^2}{\sum_{n=1}^N(b_n-a_n)^2}\big)
$$

---

## Union Bound (Boole’s Inequality)

Given events $E_1, E_2, \dots, E_N$

$$
p\big(\bigcup_{n=1}^N E_n) \le \sum_{n=1}^N p(E_n)
$$

---

## Binary Classification Setting

**Data.** $\mathcal{D} = \{(x_n, t_n)\}_{n=1}^N$ where $(x, t) \sim p(x, t)$.

**Model.** $y \in \mathcal{H}$, assuming $\mathcal{H}$ is finite with size $M_\mathcal{H}$.

**Loss.** $l(x, t, y)=0$ if $y(x)=t$, and $l(x, t, y)=1$ otherwise.

**Dataset Loss.** $\mathcal{L}_\mathcal{D}(y) = \frac{1}{N} \sum_{n=1}^N l(x_n, t_n, y)$.

**Generalization Loss.** $\mathcal{L}_p(y) = \mathbb{E}_{p(x,t)}[l(x, t, y)]$.
    

### PAC-Bound

If the learning algorithm selects $\hat{y} \in \mathcal{H}$ that minimizes $\mathcal{L}_\mathcal{D}(y)$

and we choose $N \ge \frac{1}{2\epsilon^2} \log \big(\frac{2 M_\mathcal{H}}{\delta} \big)$. Then,

$$
p(|\mathcal{L}_\mathcal{D}(\hat{y}) - \mathcal{L}_p(\hat{y})| > \epsilon) \le \delta
$$

---
