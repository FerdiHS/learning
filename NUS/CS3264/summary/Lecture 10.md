# Lecture 10: K-Means EM

## Clustering with k-means

- Group into clusters
- Data: $\mathcal{D} = \{x_1, \dots, x_N\}$ with $x_n \in \mathbb{R}^M$
- Model:
    - Assume $K$ clusters
    - Cluster centers $\{\mu_1, \dots, \mu_K\}$ with $\mu_k \in \mathbb{R}^M$
    - Assignments $\{r_1, \dots, r_n\}$ with $r_n \in \mathbb{R}^M$
- Loss:
    
    $$
    \argmin_{\theta = \{r_n, \mu_k\}} J(\theta) = \argmin_{\theta = \{r_n, \mu_k\}} \sum_{n=1}^N \sum_{k=1}^K r_{nk} \mid \mid x_n - \mu_k \mid \mid \quad \text{s.t. } \sum_{k=1}^K r_{nk} = 1
    $$
    
- Algorithm:
    1. **Inilization**
        
        Randomly pick $\{\mu_1, \dots, \mu_K\}$
        
    2. **Assignment**
        
        Assign each point to its’ closest cluster $r_{nk} = \begin{cases} 1 & \text{if } k = \argmin_k \mid\mid x_n - \mu_k \mid\mid \\ 0 & \text{otherwise} \end{cases}$
        
    3. **Update**
        
        Set each cluster center as the average points in the cluster
        
- Weakness:
    - Have to choose $K$
    - Deterministic assignments
    - Spherical, equal-sized clusters

---

## Gaussian Mixture Model (GMM)

- Data Generator:
    - Assume we know $K$
    - For each data point $n$:
        - Sample the label $z_n$
        - Given the label sample, sample the feature
    - “Throw away” the label
- Learner:
    - Given only the $x_n$’s, try to learn the model parameters
- Data:
    - $\mathcal{D} = \{x_1, \dots, x_N\}$
- Model:
    - $p_{\theta}(X, Z) = \prod_{n=1}^N \prod_{k=1}^K \pi^{z_{nk}} \mathcal{N}(x_n; \mu_k, \Sigma_k)^{z_{nk}}$
- Loss:
    - $\argmax_{\theta} \mathcal{L}(\theta) = \argmax_{\theta} \sum_{n=1}^N \log \sum_{k=1}^K \pi_k \mathcal{N}(x_n; \mu_k, \Sigma_k)$
- Assumpttions
    - $p(z_n) = Cat(z_n, \pi)$ (Categorical)
    - $p(x_n | z_n) = \mathcal{N}(x_n, \mu_{zn}, \sigma_{zn})$ (Multivariate Gaussians)
    - $z_{nk} = \begin{cases} 1 & \text{if data points $n$ is labeled $k$} \\ 0 & \text{otherwise} \end{cases}$
- Responsibility:
    - $\gamma(z_k) = p(z_k = 1 \mid x) = \frac{\pi_k \mathcal{N}(x_n; \mu_k, \Sigma_k)}{\sum_{j=1}^K \pi_j N(x_n; \mu_j, \Sigma_j)}$
    - $N_k = \sum_{n=1}^N \gamma(z_{nk})$
    - $\pi_k = \frac{N_k}{N}$
- Solution:
    - $\mu_k = \frac{1}{N_k} \sum_{n=1}^N \gamma(z_{nk})x_n$
    - $\Sigma_k = \frac{1}{N_k} \sum_{n=1}^N \gamma(z_{nk}) (x_n - \mu_k) (x_n - \mu_k)^{\top}$

---

## Expectation Maximization (EM) for GMM

Steps:

1. **Initialize** $\mu_k$, $\Sigma_k$, and $\pi_k$.
2. **Expectation** step:
    
    Compute $\gamma(Z)$ using the current parameter values $\hat{\theta}$
    
3. **Maximization** step:
    
    Estimate new $\theta$ using the current $\gamma(Z)$
    
4. **Repeat** step 2 and 3 until convergence

---

## Expectation Maximization (EM)

1. **Initialize**: parameters $\theta_t$
    
    For distribution $q(z)$, decompose $\log p(X; \theta) = L(q, \theta) + D_{KL}[q][p]$
    
    where $L(q, \theta) = \sum_Z q(Z) \log \frac{p(X, Z; \theta)}{q(Z)}$ is called the lower bound and $D_{KL}[q][p] = \sum_Z q(Z) \log \frac{q(Z)}{p(Z \mid X; \theta)}$ is called the KL divergence
    
2. **Expectation**: Evaluate $p(Z \mid X; \theta_t)$
    
    Fix $\theta_t$, optimize $q_{t+1} = \argmax_q L(q, \theta_t)$
    
    Set $q(Z) = p(Z \mid X; \theta_t)$ since we want to set $D_{KL}[q][p]=0$
    
3. **Maximization**: Compute $\theta_{t+1}$
    
    Fix $q_{t+1}$, optimize $\theta_{t+1} = \argmax_\theta L(q_{t+1}, \theta)$
    
    $\theta_{t+1} = \argmax_\theta E_{Z ~ p(Z \mid X; \theta_t)}[\log p(X, Z; \theta)]$