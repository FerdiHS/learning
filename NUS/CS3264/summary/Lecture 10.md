# Lecture 10: K-Means EM

## Clustering with k-means

- Group into clusters
- Data: $\mathcal{D} = \{x_1, \dots, x_N\}$ with $x_n \in \mathbb{R}^M$
- Model:
    - Assume $K$ clusters
    - Cluster centers $\{\mu_1, \dots, \mu_K\}$ with $\mu_k \in \mathbb{R}^M$
    - Assignments $\{r_1, \dots, r_N\}$ with $r_n \in \{0, 1\}^K$
- Loss:
    
    Objective: minimize
    $J(\theta)=\sum_{n=1}^N \sum_{k=1}^K r_{nk} \|x_n - \mu_k\|^2$
    subject to $\sum_{k=1}^K r_{nk} = 1$.
    
- Algorithm:
    1. **Initialization**
        
        Randomly pick $\{\mu_1, \dots, \mu_K\}$
        
    2. **Assignment**
        
        Assign each point to its closest cluster: set $r_{nk}=1$ for the closest center and $r_{nk}=0$ otherwise.
        
    3. **Update**
        
        Set each cluster center as the average of the points in the cluster
        
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
    - $p_{\theta}(X, Z) = \prod_{n=1}^N \prod_{k=1}^K \pi_k^{z_{nk}} \mathcal{N}(x_n; \mu_k, \Sigma_k)^{z_{nk}}$
- Loss:
    - Maximize $\mathcal{L}(\theta) = \sum_{n=1}^N \log \sum_{k=1}^K \pi_k \mathcal{N}(x_n; \mu_k, \Sigma_k)$
- Assumptions
    - $p(z_n) = \mathrm{Cat}(z_n \mid \pi)$ (Categorical)
    - $p(x_n \mid z_n) = \mathcal{N}(x_n \mid \mu_{z_n}, \Sigma_{z_n})$ (Multivariate Gaussians)
    - $z_{nk}=1$ if data point $n$ is labeled $k$, and $z_{nk}=0$ otherwise
- Responsibility:
    - $\gamma(z_{nk}) = p(z_{nk} = 1 \mid x_n) = \frac{\pi_k \mathcal{N}(x_n; \mu_k, \Sigma_k)}{\sum_{j=1}^K \pi_j \mathcal{N}(x_n; \mu_j, \Sigma_j)}$
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
    
    For distribution $q(Z)$, decompose $\log p(X; \theta) = L(q, \theta) + D_{KL}[q(Z) \| p(Z \mid X; \theta)]$
    
    where $L(q, \theta) = \sum_Z q(Z) \log \frac{p(X, Z; \theta)}{q(Z)}$ is called the lower bound and $D_{KL}[q(Z) \| p(Z \mid X; \theta)] = \sum_Z q(Z) \log \frac{q(Z)}{p(Z \mid X; \theta)}$ is called the KL divergence
    
2. **Expectation**: Evaluate $p(Z \mid X; \theta_t)$
    
    Fix $\theta_t$, then optimize $q_{t+1}$ to maximize $L(q, \theta_t)$
    
    Set $q(Z) = p(Z \mid X; \theta_t)$ since we want to set $D_{KL}[q(Z) \| p(Z \mid X; \theta_t)]=0$
    
3. **Maximization**: Compute $\theta_{t+1}$
    
    Fix $q_{t+1}$, then optimize $\theta_{t+1}$ to maximize $L(q_{t+1}, \theta)$
    
    Equivalently, maximize
    $\mathbb{E}_{Z \sim p(Z \mid X; \theta_t)}[\log p(X, Z; \theta)]$
