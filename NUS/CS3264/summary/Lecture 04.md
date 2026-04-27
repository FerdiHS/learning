# Lecture 04: Naïve Bayes

---

# Discriminative vs Generative (big picture)

- **Discriminative:** learn $p(t\mid x,\theta)$ directly (e.g., logistic regression).
- **Generative:** learn the **joint** via $p(t, x\mid\theta)=p(x\mid t,\theta)\,p(t\mid\theta)$.
- Why generative?
    - Handles **missing features** naturally (marginalize absent $x_m$).
    - Can **sample** $x$ given $t$ or from the joint.
    - Often simpler/faster to fit; may have higher **bias** but lower **variance**.

---

# Gradient Descent

$w_{k+1} = w_k - \eta (\sum_{n=1}^{N} (y_n - t_n) \Phi_n + \alpha w)$

Where $\eta$: Learning Rate

<details>

<summary><b>Proof</b>:</summary>
    
We know, $\mathcal{L}_{MAP}(w) = \sum_{n=1}^{N}\big( -t_n \log y_n - (1 - t_n) \log(1-y_n) \big) + \frac{\alpha}{2}w^\top w$

Therefore,

$$
\begin{align*}
\nabla_w \mathcal{L}_{MAP}(w)
&= \nabla_w \left(\sum_{n=1}^{N}\big( -t_n \log y_n - (1 - t_n) \log(1-y_n) \big)\right) + \nabla_w (\frac{\alpha}{2} w^\top w)\newline
\newline
&= \sum_{n=1}^{N} \big( -t_n(1 - y_n) \Phi_n + (1 - t_n) y_n \Phi_n \big) + \alpha w\newline
&= \sum_{n=1}^{N} (y_n - t_n) \Phi_n + \alpha w\end{align*}
$$

</details>

---
    

# Naïve Bayes

- **Data**: $\mathcal D=\{(x_n,t_n)\}_{n=1}^N$, with $t_n\in\{0,1\}$, $x_n=(x_{n}^{1},\dots,x_{n}^{M})$.
- **Model**:
    - **Joint distribution**: $p(t, x \mid \theta) = p(x \mid t, \theta) p(t \mid \theta)$
    - **Class Conditional Distribution**: $p(x \mid t, \theta) = \prod_{m=1}^{M} \mathcal{N}(x^m \mid \mu_t^m, v_t^m)$
    - **Class Prior**: $p(t\mid \theta) = \operatorname{Bern}(t \mid \pi)$
- **Loss**:
    - **MLE**:
    $\mathcal{L}_{\text{NB}}(\pi, \theta) = - \sum_{n=1}^{N} \left[t_n \log \pi + (1 - t_n) \log (1 - \pi) + \sum_{m=1}^{M} \log p(x_n^m \mid t_n, \theta)\right]$
    - **MAP**:
    $\mathcal{L}_{\text{MAP}}(\pi, \theta) = \mathcal{L}_{\text{NB}}(\pi, \theta) - \log p(\pi) - \sum_{t, m} \log p(\mu_t^m, v_t^m)$
- **Solution**:
    - **MLE**:
        
        $\pi_{\text{MLE}} = \frac{N_1}{N}$
        
        $\mu_{t, \text{MLE}}^m = \bar{x}_t^m$
        
        $v_{t, \text{MLE}}^m = \frac{1}{N_t}\sum_{n\in I_t}\big(x_n^m - \bar{x}_t^m\big)^2$
        
    - **MAP**:
        
        $\pi_{\text{MAP}}=\frac{N_1+a-1}{N+a+b-2}$ assuming $\pi \sim \text{Beta}(a, b)$
        

---

## NB vs Logistic Regression

- **NB:** stronger structural assumption (feature independence) ⇒ **higher bias**, **lower variance**; small-data friendly; fast, closed-form fits.
- **Logistic:** fewer assumptions ⇒ **lower bias**, **higher variance**; needs iterative optimization; typically better with enough data.
- Classic observation (Ng & Jordan, 2002): NB can win with **small $N$**; logistic often overtakes as $N$ grows.

---
