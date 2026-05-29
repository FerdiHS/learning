# Lecture 11: Lower Bounds and VAEs

## Variational Autoencoder (VAE)

- Data:
    
    $\mathcal{D} = \{x_1, \dots, x_N\}$
    
- Idea:
    - Each datapoint has an unobserved latent code $z_n$
    - Sample latent code from a simple prior
    - Decode the latent code into the observed datapoint with a neural network

---

## VAE Generative Model

For each datapoint $n$:

$$
z_n \sim \mathcal{N}(0, I)
$$

$$
x_n \mid z_n \sim \mathcal{N}(f_\theta(z_n), \sigma^2 I)
$$

- $f_\theta(\cdot)$ is the **decoder** network
- $\sigma^2$ is assumed known

The joint model is

$$
p_\theta(X, Z) = \prod_{n=1}^N \mathcal{N}(x_n \mid f_\theta(z_n), \sigma^2 I)\mathcal{N}(z_n; 0, I)
$$

---

## MLE Objective

To learn $\theta$, maximize the marginal log-likelihood:

$$
\log p_\theta(X) = \sum_{n=1}^N \log p_\theta(x_n)
$$

where

$$
p_\theta(x_n) = \int p_\theta(x_n, z_n)\, dz_n = \int \mathcal{N}(x_n \mid f_\theta(z_n), \sigma^2 I)\mathcal{N}(z_n; 0, I)\, dz_n
$$

- This integral is generally hard to compute directly
- So direct MLE is intractable

---

## Lower Bound / ELBO

Introduce a variational distribution $q_\psi(Z)$. Then:

$$
\log p_\theta(X) = L(\psi, \theta) + D_{KL}[q_\psi(Z) \,\|\, p_\theta(Z \mid X)]
$$

where

$$
L(\psi, \theta) = \mathbb{E}_{q_\psi(Z)}\left(\log \frac{p_\theta(X, Z)}{q_\psi(Z)}\right)
$$

- Since KL divergence is non-negative, $L(\psi, \theta)$ is a **lower bound**
- This lower bound is also called the **Evidence Lower Bound (ELBO)**

---

## ELBO for VAEs

Using

$$
p_\theta(X, Z) = p_\theta(X \mid Z)p_\theta(Z)
$$

the ELBO becomes

$$
L(\psi, \theta) = \mathbb{E}_{q_\psi(Z)}[\log p_\theta(X \mid Z)] - D_{KL}[q_\psi(Z) \,\|\, p_\theta(Z)]
$$

Assuming factorization over datapoints:

$$
L(\psi, \theta) = \sum_{n=1}^N \mathbb{E}_{q_\psi(z_n)}[\log p_\theta(x_n \mid z_n)] - \sum_{n=1}^N D_{KL}[q_\psi(z_n) \,\|\, p_\theta(z_n)]
$$

- First term: **reconstruction**
- Second term: **regularization toward the prior**

---

## Variational Distribution

A fixed Gaussian $q_\psi(z_n) = \mathcal{N}(z_n; \mu, \Sigma)$ is not expressive enough.

Instead, make the variational distribution depend on the input:

$$
q_\psi(z_n \mid x_n) = \mathcal{N}(z_n \mid g^\mu_\psi(x_n), \mathrm{diag}(g^\sigma_\psi(x_n)^2))
$$

- $g_\psi(\cdot)$ is the **encoder** network
- It outputs the parameters of the latent distribution for each input

So the VAE objective becomes

$$
L(\psi, \theta) = \sum_{n=1}^N \mathbb{E}_{q_\psi(z_n \mid x_n)}[\log p_\theta(x_n \mid z_n)] - \sum_{n=1}^N D_{KL}[q_\psi(z_n \mid x_n) \,\|\, p_\theta(z_n)]
$$

---

## Reconstruction and KL Terms

### Reconstruction Term

$$
\mathbb{E}_{q_\psi(z_n \mid x_n)}[\log p_\theta(x_n \mid z_n)] \approx \frac{1}{M}\sum_{m=1}^M \log \mathcal{N}(x_n \mid f_\theta(z_n^{(m)}), \sigma^2 I)
$$

where $M$ is the number of Monte Carlo samples and

$$
z_n^{(m)} \sim q_\psi(z_n \mid x_n)
$$

### KL Term

$$
D_{KL}[q_\psi(z_n \mid x_n) \,\|\, p_\theta(z_n)] = D_{KL}\left(\mathcal{N}(z_n \mid g^\mu_\psi(x_n), \mathrm{diag}(g^\sigma_\psi(x_n)^2)) \,\|\, \mathcal{N}(z_n \mid 0, I)\right)
$$

- For Gaussian distributions, this KL term can often be computed in closed form

---

## Reparameterization Trick

Problem:

- The reconstruction term requires sampling from $q_\psi(z_n \mid x_n)$
- Sampling is not differentiable, so backpropagation through the encoder is blocked

Key idea:

$$
\epsilon_n^{(m)} \sim \mathcal{N}(0, I)
$$

$$
z_n^{(m)} = g^\mu_\psi(x_n) + g^\sigma_\psi(x_n) \odot \epsilon_n^{(m)}
$$

- Randomness is moved into $\epsilon$
- $z_n^{(m)}$ becomes a differentiable transformation of encoder outputs
- This allows gradients to flow through both encoder and decoder

---

## Training Objective

Optimize the negative ELBO with SGD, i.e. minimize $-L(\psi, \theta)$.

- Encoder: $q_\psi(z \mid x)$
- Decoder: $p_\theta(x \mid z)$
- Prior: $p(z) = \mathcal{N}(0, I)$

---

## Summary

- VAE is a **generative latent-variable model**
- Direct MLE is hard because the marginal likelihood is intractable
- We optimize the **ELBO** instead
- ELBO = reconstruction term $-$ KL regularization term
- The **encoder** defines $q_\psi(z \mid x)$ and the **decoder** defines $p_\theta(x \mid z)$
- The **reparameterization trick** makes SGD training possible
