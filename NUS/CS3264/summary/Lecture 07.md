# Lecture 07: Neural Networks

## Neural Networks as Adaptive Basis Functions

| Model Type | Output Form |
| --- | --- |
| Linear regression/classification | $y = f(\mathbf{w}^\top \mathbf{x})$ |
| Fixed basis functions | $y = f(\mathbf{w}^\top \phi(\mathbf{x}))$ |
| Neural networks | $y = f(\mathbf{w}^\top \phi_\theta(\mathbf{x}))$ |

---

## Single Hidden Layer NN

$$
y = f(W^{(2)}h(W^{(1)}x))
$$

- $W^{(1)}$: input → hidden
- $W^{(2)}$: hidden → output
- $h(\cdot)$: activation ($\tanh$, sigmoid, ReLU)

---

## Properties of Neural Networks

### Universal Function Approximation

- Neural networks are highly expressive
- Even a single nonlinear hidden layer neural network can approximate any continuous function

### Representation Learning

- Feature hierarchies
- “Automatic” feature engineering

### Optimization and Regularization

- Effective training: Stochastic gradient descent + GPUs
- Other techniques: Dropout, batch norm

## Activation Functions

Used to remove linearity in Neural Networks

### Example

- Sigmoid
    
    $h(a) = \frac{1}{1 + \exp(-a)}$
    
- tanh
    
    $h(a) = \frac{\exp(2a)-1}{\exp(2a)+1}$
    
- ReLU
    
    $h(a) = \max(a, 0)$
    

---

## Universal Function Approximation

For *any* continuous function $f: [0, 1] \rightarrow \mathbb{R}$ and *any* $\epsilon > 0$,

there exists a width $m$ and real numbers $c_1, \{\Delta_i, w_i, b_i\}_{i=1}^m$ such that the single-hidden-layer network with sigmoid activation $\sigma$

$$
\hat{f}(x) = c_1 + \sum_{i=1}^m \Delta_i \sigma(w_i x + b_i)
$$

satisfies $\max_{x \in [0, 1]} |f(x) - \hat{f}(x)| < \epsilon$

---

## Gradient Descent

Assuming the loss function $\mathcal{L}$ is twice-continuously differentiable and L-smooth. We choose $\alpha \le \frac{1}{L}$ then, 

$$
\mathcal{L}(w_t) - \mathcal{L}(w_{t+1}) \ge \frac{\alpha \| \nabla \mathcal{L}(w_t)\|^2}{2}
$$

### Assumptions

- **Constant** learning rate $\alpha$
- $\mathcal{L}(w)$ is **twice continuously differentiable**

Taylor's Theorem: there exists some $\xi \in \mathbb{R}^d$ such that

$$
\mathcal{L}(v) = \mathcal{L}(w) + \nabla \mathcal{L}(w)^\top(v-w) + \frac{1}{2}(v-w)^\top \nabla^2 \mathcal{L}(\xi)(v-w)
$$

- $\mathcal{L}(w)$ is ***L*-smooth**

This means

$$
\| \nabla_w \mathcal{L}(w) - \nabla_v \mathcal{L}(v)\| \le L \|w - v\|
$$

and equivalently

$$
|u^\top \nabla^2\mathcal{L}(w)u| \le L\|u\|^2
$$
    

---

## Stochastic Gradient Descent

- Initialize $w_0$
- While termination conditions not met:
    - Sample random $n_t$
        
        $w_{t+1} = w_t - \alpha \nabla_w \mathcal{L}_{n_t}(w_t)$
        

Assume stochastic gradients have bounded variance $\sigma^2$:

$$
\min_{t \in \{0, \dots, T-1\}} \mathbb{E}[\| \nabla \mathcal{L}(w_t)\|^2] \le \frac{\mathcal{L}(w_0) - \mathcal{L}(w_*)}{\alpha T} + \frac{\alpha \sigma^2 L}{2}
$$
