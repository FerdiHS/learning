# Lecture 08: CNNs and Transformers

## Convolutional Neural Networks (CNN)

### Properties

- Sparse interactions
    - Kernel/filter only interacts with a **small receptive field**.
    - Reduces parameters drastically compared to MLPs.
- Parameter sharing
    - The same kernel is applied across the whole image.
    - Learned feature detectors apply everywhere (edges, corners, etc.).
- Translation equivariance
    - If $g$ is a translation operation, then $f(g(x)) = g(f(x))$.
        

### Stages

1. **Convolutional Stage**: sliding kernel $\times$ local patch
2. **Detector Stage**: nonlinearity (usually ReLU)
3. **Pooling Stage**: often max-pool (stride 2)

---

## Transformer

### Attention as Soft Lookup

- Instead of choosing 1 translation for each word (hard dictionary)
- Attention looks at *all words simultaneously* and computes a weighted combination

### Steps

1. Compute positional embeddings: $E = [e_1^\top; e_2^\top; \dots; e_T^\top]$.

2. Compute Query, Key, and Value Matrices
    - Query: $Q = E W_Q$
    - Key: $K = E W_K$
    - Value: $V = E W_V$
3. Compute attention weights: $S = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right)$, where $d_k$ is the dimensionality of the key/query vectors.
4. Extract features based on attention: $O = S V$.
    

---

## CNNs vs Transformers

| Aspect | CNN | Transformer |
| --- | --- | --- |
| Inductive Bias | Translation equivariance | Sequence-wide attention |
| Best For | Images | Language / long-range dependencies |
| Parameter Sharing | Yes (kernels) | Yes (projection matrices) |
| Parallelizable | Yes | Yes |
| Handles Variable Length | No (needs padding) | Yes |

---

## Lagrange Multipliers for Inequality Constraints

### Original

$$
\begin{aligned}
\max_x\; &f(x) \\
\text{subject to}\; &g(x) \ge 0
\end{aligned}
$$

$L(x, \lambda) = f(x) + \lambda g(x)$ where $\lambda \ge 0$ is the Lagrange multiplier.

### Karush-Kuhn-Tucker (KKT)

$$
\begin{aligned}
\max_x\min_{\lambda \ge 0}\; &L(x, \lambda) \\
L(x, \lambda) &= f(x) + \lambda g(x) \\
\nabla f(x) + \lambda \nabla g(x) &= 0 \\
g(x) &\ge 0 \\
\lambda &\ge 0 \\
\lambda g(x) &= 0
\end{aligned}
$$
