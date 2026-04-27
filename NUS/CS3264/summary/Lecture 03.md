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
- **Model**: $y(x)=\sigma(w^\top x)$.

---

# Logistic Regression

- **Data**: $D = \{(x_n, t_n)\}_{n=1}^{N}$
- **Model**:
    - **Targets**: $t_n \in \{0, 1\}$
    - **Prediction**: $y(x) = \sigma(w^{\top} \phi(x))$
    - **Sigmoid**: $\displaystyle \sigma(z)=\frac{1}{1+e^{-z}}$, with
    $\sigma(-z)=1-\sigma(z)$ and $\frac{d\sigma}{dz}=\sigma(z)\big(1-\sigma(z)\big)$.
    - **Likelihood**: $p(t \mid X, w) = \prod_{n=1}^{N} \operatorname{Bern}(t_n \mid y(x_n))$
    - **Prior**: $p(w)=\mathcal{N}(w\mid0,\alpha^{-1}I)$
- **Loss**:
    - **MLE**:
        $\mathcal{L}_{\text{MLE}}(w) = -\left[\sum_{n=1}^{N} t_n \log y_n + (1-t_n) \log(1-y_n)\right]$
        
    - **MAP**:
        $\mathcal{L}_{\text{MAP}}(w) = \mathcal{L}_{\text{MLE}}(w) + \frac{\alpha}{2} \|w\|_2^2$
        
- <details>
    <summary><b>Proof</b>:</summary>
    
    **MLE**:
    
    $\operatorname*{arg\,max}_w p(t \mid X, w) = \operatorname*{arg\,max}_w \prod_{n=1}^{N} y_n^{t_n} (1-y_n)^{1-t_n}$
    
    $\operatorname*{arg\,max}_w \log p(t \mid X, w) = \operatorname*{arg\,max}_w \sum_{n=1}^{N} \left[t_n \log y_n + (1 - t_n) \log(1-y_n)\right]$
    
    Therefore, $\mathcal{L}_{\text{MLE}}(w) = - \sum_{n=1}^{N}\left[t_n \log y_n + (1 - t_n) \log(1-y_n)\right]$
    
    **MAP**:
    
    $\operatorname*{arg\,max}_w p(w \mid X, t) \propto \operatorname*{arg\,max}_w p(t \mid X, w) p(w)$
    
    $$
        \begin{align*}
        \operatorname*{arg\,max}_w \log p(w \mid X, t)
        &\propto \operatorname*{arg\,max}_w \left[- \mathcal{L}_{\text{MLE}}(w) + \log p(w)\right]
        \newline
        &=\operatorname*{arg\,max}_w \left[- \mathcal{L}_{\text{MLE}}(w) - \frac{\alpha}{2}\|w\|_2^2 + \text{const}\right]
        \end{align*}
    $$
    
    Therefore, $\mathcal{L}_{\text{MAP}}(w) = \mathcal{L}_{\text{MLE}}(w) + \frac{\alpha}{2} \|w\|_2^2$

    </details>

---
