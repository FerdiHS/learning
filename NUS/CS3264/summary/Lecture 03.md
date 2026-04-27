# Lecture 03: Linear Classification

---

# Bernoulli Distribution

For $t\in\{0,1\}$ with success prob $\mu$:

$p(t\mid \mu)=\mu^t(1-\mu)^{1-t}$.

Dataset $\mathcal D=\{(x_n,t_n)\}_{n=1}^N$

$p(\mathbf t\mid \mu)= \mu^{\sum_{n=1}^{N}t_n}(1-\mu)^{N-\sum_{n=1}^{N}t_n}$.

---

# Binary Classification

- **Data**: $D = \{(x_n, t_n)\}_{n=1}^{N}$
- **Targets**: $t_n \in \{-1, +1\}$
- **Model**: $y(x)=\mathrm{sign}(w^\top x)$.

---

# Logistic Regression

- **Data**: $D = (X, \mathbf{t}) = \{(x_n, t_n)\}_{n=1}^{N}$
- **Model**:
    - **Targets**: $t_n \in \{0, 1\}$
    - **Prediction**: $y(x) = \sigma(w^{\top} \phi(x))$
    - **Sigmoid**: $\displaystyle \sigma(z)=\frac{1}{1+e^{-z}}$, with
    $\sigma(-z)=1-\sigma(z)$ and $\frac{d\sigma}{dz}=\sigma(z)\big(1-\sigma(z)\big)$.
    - **Likelihood**: $p(\mathbf{t} \mid X, w) = \prod_{n=1}^{N} \mathrm{Bern}(t_n \mid y(x_n))$
    - **Prior**: $p(w)=\mathcal{N}(w\mid0,\alpha^{-1}I)$
- **Loss**:
    - **MLE loss**

        $$
        \mathcal{L}_{\text{MLE}}(w) = -\left(\sum_{n=1}^{N} t_n \log y_n + (1-t_n) \log(1-y_n)\right)
        $$

    - **MAP loss**

        $$
        \mathcal{L}_{\text{MAP}}(w) = \mathcal{L}_{\text{MLE}}(w) + \frac{\alpha}{2}\|w\|_2^2
        $$

<details>
<summary><strong>Proof</strong></summary>

**MLE:** maximizing $p(\mathbf{t} \mid X, w)$ is equivalent to maximizing

$$
\prod_{n=1}^{N} y_n^{t_n} (1-y_n)^{1-t_n}
$$

Taking logs gives

$$
\log p(\mathbf{t} \mid X, w) = \sum_{n=1}^{N} \left(t_n \log y_n + (1 - t_n) \log(1-y_n)\right)
$$

Therefore

$$
\mathcal{L}_{\text{MLE}}(w) = - \sum_{n=1}^{N}\left(t_n \log y_n + (1 - t_n) \log(1-y_n)\right)
$$

**MAP:** maximizing $p(w \mid X, \mathbf{t})$ is proportional to maximizing
$p(\mathbf{t} \mid X, w) p(w)$.

This gives

$$
\log p(w \mid X, \mathbf{t}) \propto - \mathcal{L}_{\text{MLE}}(w) - \frac{\alpha}{2}\|w\|_2^2 + \text{const}
$$

Therefore

$$
\mathcal{L}_{\text{MAP}}(w) = \mathcal{L}_{\text{MLE}}(w) + \frac{\alpha}{2} \|w\|_2^2
$$

</details>

---
