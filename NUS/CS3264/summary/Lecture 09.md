# Lecture 09: SVM

## Kernel Ideas

- Replace $w = \sum_n a_n \phi(x_n) = \Phi^\top a$
- $k(x, z) = \phi(x)^\top \phi(z)$

---

## Kernel Linear Regression

- Data:
    
    $\mathcal{D} = \{(x_n ,t_n)\}_{n=1}^N$
    
- Model:
    
    $y(x) = \sum_n a_n \phi(x_n)^\top \phi(x)$
    
- Loss:
    
    $\mathcal{L}(a) = \frac{1}{2}a^\top KKa - a^\top Kt + \frac{1}{2} t^\top t + \frac{\lambda}{2} a^\top Ka$ with $K = \Phi\Phi^\top \in \mathbb{R}^{N \times N}$
    
- Solution:
    
    $a_{\text{MAP}} = (K + \lambda I_N)t$
    
- Prediction:
    
    $y(x_*) = \sigma_n a_n \phi(x_n)^\top \phi(x_*) = a_\text{MAP}^\top k(x_*)$ where $k(x_*) = \begin{bmatrix} k(x_1, x_*) \\ \vdots \\ k(x_N, x_*)\end{bmatrix} \in \mathbb{R}^N$
    

---

## Valid Kernel Function

Kernel Matrix $K$ must be **Symmetric Positive Semi-Definite (PSD)**

- Symmetric: $K = K^\top$
- PSD: $v^\top Kv \ge 0 \quad \forall v \in \mathbb{R}^N \setminus \{0\}$

---

## Kernel Function Properties

If $k_1(x, z)$ and $k_2(x, z)$ are valid kernel functions, then all the following are also valid kernel functions:

- $ck_1(x, z)$ where $c > 0$
- $k_1(x,z) + k_2(x,z)$
- $k_1(x, z) \cdot k_2(x, z)$
- $f(x) k_1(x, z) f(z)$
- $k_1(x, z) + c$
- $\exp(k_1(x,z))$

---

## Support Vector Machine (SVM)

Maximize the margin (the smallest distance between the decision boundary and any of the samples)

### SVM for Binary Classification with Maximum Margin Classifiers

- Data:
    
    $\mathcal{D} = \{(x_n, t_n)\}_{n=1}^N$ with $t_n \in \{1, -1\}$ and the data linearly separable ($t_ny(x_n)>0 \quad \forall n$)
    
- Model:
    
    $y(x) = \sum_{n=1}^N a_nt_nk(x, x_n)+b$
    
- Goal:
    
    $\max_a \mathcal{L}(a) = \max_a \sum_{n=1}^N a_n - \frac{1}{2} \sum_{m=1}^N  a_na_mt_nt_mk(x_n, x_m)$ such that $a_n \ge 0 \quad \forall n \in \{1, 2, \dots, N\}$ and $\sum_n a_n t_n = 0$