# Lecture 02: Probabilistic Modeling for Linear Regression

---

# Probability Preliminaries

- **Chain/product rule:** $p(x,y)=p(x\mid y)p(y)$
- **Sum rule (marginalization):** $p(x)=\sum_y p(x,y)$ or $\int p(x,y)\,dy$
- **Independence:** $p(x\mid y)=p(x)\ \Leftrightarrow\ p(x,y)=p(x)p(y)$

# **Gaussian**

- **Univariate**: $x\sim\mathcal N(\mu,\sigma^2)$,
$p(x)=\dfrac{1}{\sqrt{2\pi\sigma^2}}\exp\!\big(-\tfrac{(x-\mu)^2}{2\sigma^2}\big)$
- **Multivariate**: $x\sim\mathcal N(\mu,\Sigma)$,
$p(x)=\dfrac{1}{(2\pi)^{D/2}|\Sigma|^{1/2}}\exp\!\big(-\tfrac12 (x-\mu)^\top\Sigma^{-1}(x-\mu)\big)$

---

# Gaussian Model

- **Data:** $\mathcal D=\{x_n\}_{n=1}^N$, assume i.i.d. $x_n\sim \mathcal N(\mu,\sigma^2)$.
- **Likelihood:** $p(\mathcal D\mid \theta)=\prod_{n=1}^N \mathcal N(x_n\mid \mu,\sigma^2)$ with $\theta = \{\mu, \sigma^2\}$
- **Loss: $\mathcal{L}(\theta) = \frac{1}{2} \sum_{n=1}^{N}(t_n - \mu)^2$**
- **MLE (Maximum Likelihood Estimation):**
$\hat\mu_{\text{ML}}=\frac1N\sum_{n=1}^N x_n,\qquad
\hat\sigma^2_{\text{ML}}=\frac1N \sum_{n=1}^N (x_n-\hat\mu_{\text{ML}})^2$.
- <details>
    <summary><b>Proof</b>:</summary>
    
    **1) Loss Function**
    
    $$
    \begin{align*}
    \log p(\mathcal D\mid\mu,\sigma^2)
    &=\log \prod_{n=1}^N \mathcal N(x_n\mid \mu,\sigma^2)\newline
    &=\sum_{n=1}^{N} \log \frac{1}{\sqrt{2\pi\sigma^2}}
    \exp\!\Big(-\frac{(x_n-\mu)^2}{2\sigma^2}\Big)\newline
    &=N\log\frac{1}{2\pi\sigma^2}-\sigma^2\sum_{n=1}^{N}\frac{(x_n-\mu)^2}{2}
    \end{align*}
    $$
    
    Therefore, $\mathcal{L}(\theta):=\frac{1}{2}\sum_{n=1}^{N}(x_n-\mu)^2$
    
    **2) MLE for the mean $\mu$**
    
    Differentiate $\mathcal{L}$ w.r.t. $\mu$ and set to zero:
    
    $\frac{\partial \mathcal{L}}{\partial \mu}
    = \frac{1}{2}\left(2\sum_{n=1}^{N}\left(x_n-\mu\right)\right)=\sum_{n=1}^{N}(x_n-\mu)=0$.
    
    Hence,
    $\sum_{n=1}^N x_n - N\mu = 0
    \quad\Rightarrow\quad
    \boxed{\ \hat\mu_{\text{ML}}=\frac{1}{N}\sum_{n=1}^N x_n\ }$ .
    
    Second derivative:
    $\displaystyle \frac{\partial^2 \mathcal{L}}{\partial \mu^2}=-N<0$,
    so this critical point is a (global) maximizer in $\mu$.
    
    **3) MLE for the variance $\sigma^2$**
    
    Differentiate $\ell$ w.r.t. $\sigma^2$ (treat $S(\mu)$ as constant for this step):
    
    $\frac{\partial \ell}{\partial \sigma^2}
    = -\frac{N}{2}\cdot\frac{1}{\sigma^2} - \frac{1}{2}\cdot\frac{S(\mu)}{\sigma^4}
    = -\frac{N}{2\sigma^2}+\frac{S(\mu)}{2\sigma^4}=0$
    
    Multiply by $2\sigma^4$:
    
    $-N\sigma^2 + S(\mu)=0
    \quad\Rightarrow\quad
    \boxed{\ \hat\sigma^2_{\text{ML}}=\frac{1}{N}\,S(\hat\mu_{\text{ML}})
    =\frac{1}{N}\sum_{n=1}^N\big(x_n-\hat\mu_{\text{ML}}\big)^2\ }$
    
    (Second derivative at the solution is negative, confirming a maximum.)

    </details>
    

> Takeaway: MLE gives us a principled “loss” (negative log-likelihood) instead of picking a loss arbitrarily.
> 

---

# Bayesian

- **Data:** $\mathcal D=\{(x_n,t_n)\}_{n=1}^N$. Use basis $\phi:\mathbb R^d\to\mathbb R^M$; stack rows into $\Phi\in\mathbb R^{N\times M}$; targets $t\in\mathbb R^N$.
- **Model:**
    - **Target**: $t_n = w^\top \phi(x) + \varepsilon, \quad \varepsilon\sim \mathcal N(0,\beta^{-1})$
    - **Likelihood:** $p(\mathbf t\mid X,w)=\mathcal N(\Phi w,\ \beta^{-1} I)$
    - **Prior**: $p(w \mid 0, \alpha^{-1}I)$
    - **Posterior**:
        - $p(w \mid D) = N(w \mid m_N, S_N)$
        - $m_N = \beta S_N \Phi^\top t$
        - $S_N = \left(\beta \Phi^\top\Phi + \alpha I\right)^{-1}$
- **Posterior Predictive**: $p(y_* \mid x_*, D) = N(y_* \mid \phi(x_*)^\top m_N, \phi(x)^\top S_N \phi(x))$
- **Loss**:
    - **MLE**:
    $\mathcal L_{\text{MLE}}(w)=\frac{1}{2}\|\Phi w - t\|_2^2$
    - **MAP**:
    $\mathcal L_{\text{MAP}}(w)=\frac{1}{2}\|\Phi w - t\|_2^2 + \frac{\alpha}{2\beta}\|w\|_2^2$
- **Solution**:
    - **MLE**:
    $w_{\text{ML}}=(\Phi^\top \Phi)^{-1}\Phi^\top t$
    - **MAP**:
    $w_{\text{MAP}} = \big(\Phi^\top \Phi + \tfrac{\alpha}{\beta} I\big)^{-1}\Phi^\top t$
- <details>
    <summary><b>Proof</b>:</summary>
    
    **1) MLE:**
    
    **Likelihood**
    $p(t\mid \Phi,w,\beta)
    = (2\pi)^{-N/2}\,\beta^{N/2}\,
    \exp\!\Big(-\tfrac{\beta}{2}\,\|\Phi w - t\|_2^2\Big)$
    
    **Negative log-likelihood (loss)**
    $\mathcal L_{\text{MLE}}(w)
    := -\frac{1}{\beta}\log p(t\mid \Phi,w,\beta)
    = \tfrac{1}{2}\|\Phi w - t\|_2^2 + \text{const}$.
    (“Const” does not depend on $w$ and can be dropped for optimization.)
    
    **Solution (normal equations)**
    $\nabla_w \mathcal L_{\text{MLE}}(w)
    = \,\Phi^\top(\Phi w - t)=0
    \ \Longrightarrow\
    \Phi^\top \Phi\, w = \Phi^\top t.$
    
    If $\Phi^\top \Phi$ is invertible (full column rank),
    $\boxed{\,w_{\text{ML}} = (\Phi^\top \Phi)^{-1}\Phi^\top t\,}$.
    If $\Phi^\top \Phi$ is singular, the minimizers form an affine set; the minimum-norm one is $w=\Phi^+ t$ (Moore–Penrose pseudoinverse).
    
    **2.) MAP**
    
    **Prior on parameters**
    $p(w\mid \alpha)=\mathcal N(0,\alpha^{-1}I)
    \ \ \Longrightarrow\ \
    -\log p(w\mid\alpha)=\tfrac{\alpha}{2}\|w\|_2^2 + \text{const}$.
    
    **Posterior (up to proportionality)**
    $p(w\mid \mathcal D,\alpha,\beta) \ \propto\
    p(t\mid \Phi,w,\beta)\,p(w\mid \alpha)$.
    
    **Negative log-posterior (MAP loss)**
    $\mathcal L_{\text{MAP}}(w)
    := -\frac{1}{\beta}\log p(w\mid \mathcal D,\alpha,\beta)
    = \tfrac{1}{2}\|\Phi w - t\|_2^2 + \tfrac{\alpha}{2\beta}\|w\|_2^2 + \text{const}$.
    So the **regularizer** $\tfrac{\alpha}{2\beta}\|w\|^2$ comes directly from the Gaussian prior.
    
    **Solution**
    $\nabla_w \mathcal L_{\text{MAP}}(w)
    = \beta\,\Phi^\top(\Phi w - t) + \alpha\,w = 0$
    $\Longrightarrow\quad
    (\beta \Phi^\top \Phi + \alpha I) w = \beta \Phi^\top t$
    Since for any $w\neq 0$,
    $w^\top(\beta \Phi^\top\Phi+\alpha I)w
    = \beta\|\Phi w\|^2 + \alpha\|w\|^2 > 0$
    the matrix is SPD and invertible for $\alpha>0$. 
    
    Hence
    $\boxed{\,w_{\text{MAP}}
    = (\beta \Phi^\top \Phi + \alpha I)^{-1}\,\beta \Phi^\top t
    = \big(\Phi^\top \Phi + \tfrac{\alpha}{\beta} I\big)^{-1}\Phi^\top t\,}.$
    
    Identifying $\lambda:=\alpha/\beta$ shows **MAP = ridge regression**.

    </details>
    

> Ridge = MAP: The regularizer comes from the Gaussian prior with $\lambda=\alpha/\beta$.
> 

---

# Frequentist vs Bayesian (coin-flip mini-example)

- **Frequentist MLE:** pick $\theta$ (e.g., coin bias) that maximizes $p(\mathcal D\mid \theta)$.
- **Bayesian:** start with **prior** $p(\theta)$, update via **Bayes’ rule** to get **posterior** $p(\theta\mid\mathcal D)$; predict by averaging over the posterior.
- **MAP** is the “cheap” Bayesian: take the mode instead of integrating.

---

---