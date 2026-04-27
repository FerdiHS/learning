# Lecture 05: Bias Variance Decomposition

---

---

## Setup & Notation

- **Data**: $\mathcal D=\{(x_n,t_n)\}_{n=1}^N$, with $(x,t)\sim p(x,t)$.
- A learning algorithm outputs a predictor $y(x;\mathcal D)$.
- Define
    - Population (generalization) squared loss: $\mathcal L_p(y)=\mathbb E_{p(x,t)}[(y(x)-t)^2]$.
    - Dataset (empirical) loss: $\mathcal L_{\mathcal D}(y)=\frac1N\sum_{n=1}^N (y(x_n)-t_n)^2$.
- Let the **average predictor** $\bar y(x)=\mathbb E_{p(\mathcal D)}[y(x;\mathcal D)]$ and the **Bayes target** $\bar t(x)=\mathbb E[t\mid x]$.

---

## Bias–Variance–Noise Decomposition

**Generalization Error = Bias + Variance + Noise**

$$
\mathbb{E}_{p(x, t)p(D)}\left[(y(x; D) - t)^2\right] = \mathbb{E}_{p(x)}\left[(\bar{y}(x)-\bar{t}(x))^2\right] + \mathbb{E}_{p(x)p(D)}\left[(y(x; D) - \bar{y}(x))^2\right] + \mathbb{E}_{p(x, t)}\left[(\bar{t}(x) - t)^2\right]
$$

where,

- **Generalization Error**: $\mathbb{E}_{p(x, t)p(D)}\left[(y(x; D) - t)^2\right]$
- **Bias**: $\mathbb{E}_{p(x)}\left[(\bar{y}(x)-\bar{t}(x))^2\right]$
- **Variance**: $\mathbb{E}_{p(x)p(D)}\left[(y(x; D) - \bar{y}(x))^2\right]$
- **Noise**: $\mathbb{E}_{p(x, t)}\left[(\bar{t}(x) - t)^2\right]$

**Intuition**

- **Bias**: error from a **misspecified**/too-simple model (systematic gap from Bayes).
- **Variance**: sensitivity to the particular **dataset** you happened to draw.
- **Noise**: irreducible randomness in the labels.
- As **model complexity** grows: bias ↓, variance ↑; with **more data**: variance ↓.

---
