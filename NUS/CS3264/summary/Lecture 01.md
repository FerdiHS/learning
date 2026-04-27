# Lecture 01: Introduction + Regression I

# What is Machine Learning (ML)?

- *Definition*: Study of models and algorithms for finding patterns in data.
- Common traits in ML problems:
    1. We have data.
    2. We want to find patterns/regularities.
    3. The pattern is hard to express mathematically.
- Applications span finance, robotics, computer vision, natural language processing, self-driving cars, etc.

---

# Core Ingredients of ML Models

Every ML model involves three components:

1. **Data**
2. **Model**
3. **Loss function ($\mathcal{L}$)**

---

# Linear Regression

- **Data**: 
$\mathcal{D} = \{(x_n, t_n)\}_{n=1}^N$
- **Model**:
$y(x) = w^\top x$
- **Loss** (squared error):
$\mathcal{L}(w) = \tfrac{1}{2}\sum_n (w^\top x_n - t_n)^2$
- **Solution** (normal equation):
$w_{\text{ML}} = (X^\top X)^{-1}X^\top t$

<details>
<summary><strong>Proof</strong></summary>

$$
\nabla_w \mathcal{L}(w)=X^\top(Xw - t)
$$

At the optimum, set the gradient to zero:

$$
X^\top X\,w=X^\top t
\quad\Rightarrow\quad
w=(X^\top X)^{-1}X^\top t
$$

</details>
---

# Nonlinear Regression

- Extend regression with **basis functions** $\phi(x)$:
$y(x) = w^\top \phi(x)$
- Example: polynomial basis $\phi(x) = (1, x, x^2, \ldots, x^p)^\top$.
- **Loss** (squared error):
$\mathcal{L}(w) = \tfrac{1}{2}\sum_n (w^\top \phi(x_n) - t_n)^2$
- Solution:
$w_{\text{ML}} = (\Phi^\top \Phi)^{-1}\Phi^\top t$

<details>
<summary><strong>Proof</strong></summary>

If $\Phi^\top\Phi$ is invertible, then

$$
w_{\text{ML}}=(\Phi^\top\Phi)^{-1}\Phi^\top t
$$

If $\Phi^\top\Phi$ is singular, use the Moore-Penrose pseudoinverse:

$$
w=\Phi^{+}t
$$

where $\Phi^+$ is the pseudoinverse of $\Phi$.

</details>

---

# Overfitting & Regularization

- **Key Idea**: Training error ≠ Test error.
- We care about generalization performance.
- To mitigate overfitting, add **regularization**:
$\mathcal{L}(w) = \tfrac{1}{2}\sum_n (w^\top \phi(x_n) - t_n)^2 + \tfrac{\lambda}{2}\|w\|^2$
- Solution (ridge regression):
$w_{\text{RR}} = (\Phi^\top \Phi + \lambda I)^{-1}\Phi^\top t$

<details>
<summary><strong>Proof</strong></summary>

Differentiate:

$$
\nabla_w \mathcal{L}_\lambda(w)=\Phi^\top(\Phi w - t)+\lambda w
$$

Set to zero:

$$
(\Phi^\top\Phi+\lambda I)w=\Phi^\top t
$$

For any $w\neq 0$ and $\lambda>0$,

$$
w^\top(\Phi^\top\Phi+\lambda I)w
= \|\Phi w\|_2^2 + \lambda\|w\|_2^2 \;>\; 0
$$

so $\Phi^\top\Phi+\lambda I$ is symmetric positive definite and hence invertible.

Therefore,

$$
w=(\Phi^\top\Phi+\lambda I)^{-1}\Phi^\top t
$$

is the unique minimizer.

</details>

---
