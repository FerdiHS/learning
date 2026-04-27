# Lecture 06

This lecture extends random asset returns to portfolios of risky assets.

The key idea is that once several risky assets are combined, portfolio
return depends on weighted combinations of individual returns, while portfolio
risk depends on both individual variances and how the assets move
together.

## Variable Definitions

$$
n = \text{number of available assets}
$$

$$
i,j = \text{asset indices}
$$

$$
X_0 = \text{initial wealth invested at time } 0
$$

$$
X_1 = \text{wealth at the end of the investment period}
$$

$$
X_{0i} = \text{amount invested in asset } i \text{ at time } 0
$$

$$
w_i = \text{portfolio weight of asset } i
$$

$$
w = (w_1,w_2,\ldots,w_n) = \text{portfolio weight vector}
$$

$$
r_i = \text{rate of return of asset } i
$$

$$
r_p = \text{portfolio rate of return}
$$

$$
\mu_i = E(r_i) = \text{mean return of asset } i
$$

$$
\mu_p = E(r_p) = \text{mean return of the portfolio}
$$

$$
\sigma_i^2 = \operatorname{Var}(r_i) = \text{variance of the return of asset } i
$$

$$
\sigma_i = \operatorname{SD}(r_i) = \text{standard deviation of the return of asset } i
$$

$$
\sigma_{ij} = \operatorname{Cov}(r_i,r_j)
$$

$$
\sigma_{12} = \operatorname{Cov}(r_1,r_2)
$$

$$
\sigma_p^2 = \operatorname{Var}(r_p) = \text{portfolio variance}
$$

$$
\sigma_p = \operatorname{SD}(r_p) = \text{portfolio standard deviation}
$$

$$
\rho_{ij} = \operatorname{Corr}(r_i,r_j) = \frac{\sigma_{ij}}{\sigma_i \sigma_j} \text{ when } \sigma_i,\sigma_j > 0
$$

$$
\alpha = w_1 = \text{weight of asset 1 in a two-asset portfolio}
$$

$$
1-\alpha = w_2 = \text{weight of asset 2 in a two-asset portfolio}
$$

$$
\alpha^* = \text{weight of asset 1 in the global minimum-variance portfolio}
$$

The corresponding weight of asset 2 in the GMVP is:

$$
1-\alpha^*
$$

## Portfolio

Suppose there are $n$ assets available and the investor starts with initial
wealth $X_0$.

A portfolio is formed by choosing amounts

$$
X_{0i}, \qquad i = 1,2,\ldots,n
$$

to invest in each asset, such that:

$$
\sum_{i=1}^n X_{0i} = X_0
$$

Here $X_{0i}$ is the amount invested in asset $i$.

If short-selling is allowed, some $X_{0i}$ may be negative. Otherwise:

$$
X_{0i} \ge 0
$$

<details>
<summary>Intuition / proof</summary>

A portfolio is just a way of splitting total wealth across several assets.

The condition

$$
\sum_{i=1}^n X_{0i}=X_0
$$

says all initial wealth must be allocated somewhere.

If short-selling is allowed, one of the amounts can be negative, meaning
the investor has effectively borrowed that asset rather than bought it.

So the portfolio is a "master asset" built from the underlying assets.
</details>

## Portfolio Weights

The invested amounts can be written as fractions of total wealth:

$$
X_{0i} = w_i X_0
$$

where $w_i$ is the portfolio weight of asset $i$.

The weights satisfy:

$$
\sum_{i=1}^n w_i = 1
$$

If short-selling is allowed, then some weights may be negative:

$$
w_i < 0
$$

Otherwise:

$$
w_i \ge 0
$$

<details>
<summary>Intuition / proof</summary>

The weights tell us what fraction of total wealth is placed in each asset.

Since

$$
X_{0i}=w_iX_0,
$$

summing over all assets gives:

$$
\sum_{i=1}^n X_{0i}
= X_0\sum_{i=1}^n w_i.
$$

But the left-hand side equals $X_0$, so:

$$
\sum_{i=1}^n w_i = 1.
$$

So weights always add up to 100% of the investor's wealth, even if some
positions are negative because of short-selling.

Negative weights represent short positions, while weights above $1$ can
occur when short-selling is used to lever a long position elsewhere.
</details>

## Portfolio Return

Let the rate of return of asset $i$ be $r_i$.

The end-of-period wealth is:

$$
X_1
= \sum_{i=1}^n w_i X_0(1+r_i)
= X_0 \sum_{i=1}^n w_i(1+r_i)
$$

If $r_p$ is the portfolio return, then:

$$
X_1 = X_0(1+r_p)
$$

Comparing the two expressions gives the portfolio return:

$$
r_p = \sum_{i=1}^n w_i r_i
$$

So the portfolio return is a weighted combination of the asset returns.

### Two-Asset Portfolio

For two assets, let:

$$
w_1 = \alpha, \qquad w_2 = 1-\alpha
$$

Then:

$$
r_p = \alpha r_1 + (1-\alpha)r_2
$$

<details>
<summary>Intuition / proof</summary>

Each asset grows its allocated wealth by its own return factor. Asset $i$
starts with $w_iX_0$ and ends with:

$$
w_iX_0(1+r_i)
$$

Adding over all assets gives total end wealth.

Since portfolio return $r_p$ is defined by:

$$
X_1=X_0(1+r_p),
$$

dividing both sides by $X_0$ gives:

$$
1+r_p=\sum_{i=1}^n w_i(1+r_i)
$$

and because $\sum_i w_i=1$, this simplifies to:

$$
r_p=\sum_{i=1}^n w_i r_i.
$$

So portfolio return is a weighted combination of the asset returns.

In the two-asset case, once $\alpha$ is chosen, the whole portfolio is
determined because the second weight must be $1-\alpha$.
</details>

## Portfolio Mean

Let:

$$
\mu_i = E(r_i)
$$

be the mean return of asset $i$.

Then the portfolio mean is:

$$
\mu_p = E(r_p) = \sum_{i=1}^n w_i \mu_i
$$

For two assets:

$$
\mu_p = \alpha \mu_1 + (1-\alpha)\mu_2
$$

<details>
<summary>Intuition / proof</summary>

Since

$$
r_p=\sum_{i=1}^n w_i r_i,
$$

taking expectation and using linearity gives:

$$
E(r_p)
= E\left(\sum_{i=1}^n w_i r_i\right)
= \sum_{i=1}^n w_i E(r_i)
= \sum_{i=1}^n w_i \mu_i.
$$

So the portfolio mean is simply the weighted combination of individual asset
means.

This parallels the return formula: expectation preserves weighted sums.
</details>

## Portfolio Variance

Let:

$$
\sigma_{ij} = \operatorname{Cov}(r_i,r_j)
$$

Then the portfolio variance is:

$$
\sigma_p^2
= \operatorname{Var}(r_p)
= \sum_{i=1}^n \sum_{j=1}^n w_i w_j \sigma_{ij}
$$

For two assets:

$$
\sigma_p^2
= \alpha^2 \sigma_1^2
+ (1-\alpha)^2 \sigma_2^2
+ 2\alpha(1-\alpha)\sigma_{12}
$$

Using the correlation coefficient, when $\sigma_1,\sigma_2 > 0$,

$$
\rho_{12} = \frac{\sigma_{12}}{\sigma_1 \sigma_2}
$$

this becomes:

$$
\sigma_p^2
= \alpha^2 \sigma_1^2
+ (1-\alpha)^2 \sigma_2^2
+ 2\alpha(1-\alpha)\rho_{12}\sigma_1 \sigma_2
$$

<details>
<summary>Intuition / proof</summary>

Portfolio risk is not just a weighted average of individual risks,
because the assets may move together.

Starting from the two-asset formula

$$
r_p=\alpha r_1+(1-\alpha)r_2,
$$

the variance-of-a-sum formula gives:

$$
\operatorname{Var}(X+Y)
= \operatorname{Var}(X)+\operatorname{Var}(Y)+2\operatorname{Cov}(X,Y).
$$

Applying this with
$X=\alpha r_1$ and $Y=(1-\alpha)r_2$ gives:

$$
\sigma_p^2
= \alpha^2\sigma_1^2
+ (1-\alpha)^2\sigma_2^2
+ 2\alpha(1-\alpha)\sigma_{12}.
$$

The covariance term is what captures diversification.

- If the assets tend to move together strongly, covariance is large and
  diversification helps less.
- If they move differently, covariance is smaller and diversification
  reduces risk more.

Rewriting with correlation is often useful because $\rho_{12}$ is unit-free
when it is defined.

In the general $n$-asset case, the double sum

$$
\sum_{i=1}^n \sum_{j=1}^n w_i w_j \sigma_{ij}
$$

collects all own-variance terms ($i=j$) and all cross-covariance terms
($i \ne j$).
</details>

## Global Minimum-Variance Portfolio

For two risky assets, the portfolio variance is a quadratic function of
$\alpha$.

The global minimum-variance portfolio, or GMVP, is the portfolio with the
smallest possible variance.

The minimizing weight is:

$$
\alpha^*
=
\frac{\sigma_2(\sigma_2-\rho_{12}\sigma_1)}
{\sigma_1^2+\sigma_2^2-2\rho_{12}\sigma_1 \sigma_2}
$$

So the GMVP weights are:

$$
w_1 = \alpha^*, \qquad w_2 = 1-\alpha^*
$$

The minimum portfolio variance is:

$$
(\sigma_p^2)^*
=
\frac{\sigma_1^2 \sigma_2^2 (1-\rho_{12}^2)}
{\sigma_1^2+\sigma_2^2-2\rho_{12}\sigma_1 \sigma_2}
$$

The corresponding portfolio mean is:

$$
(\mu_p)^* = \alpha^* \mu_1 + (1-\alpha^*)\mu_2
$$

This portfolio is called the global minimum-variance portfolio because it
has the lowest variance among all portfolios formed from the two assets.

<details>
<summary>Intuition / proof</summary>

For two assets, portfolio variance depends only on the single choice
variable $\alpha$, since the other weight is $1-\alpha$.

That makes $\sigma_p^2$ a quadratic function of $\alpha$, so its graph is
a parabola. The GMVP is the lowest point of that parabola.

The minimizing value $\alpha^*$ can be found by:

- completing the square, or
- differentiating $\sigma_p^2$ with respect to $\alpha$ and setting the
  derivative equal to zero.

The resulting weight

$$
\alpha^*
=
\frac{\sigma_2(\sigma_2-\rho_{12}\sigma_1)}
{\sigma_1^2+\sigma_2^2-2\rho_{12}\sigma_1 \sigma_2}
$$

gives the smallest possible portfolio variance across all two-asset
portfolios.

Intuitively, the GMVP balances the assets so that diversification is used
as much as possible. The lower the correlation, the more diversification
can reduce risk.

The formula for $(\sigma_p^2)^*$ is the minimum value of the quadratic
once $\alpha=\alpha^*$ is substituted back in.
</details>

## Key Takeaways

- A portfolio allocates total wealth across several assets.
- Portfolio weights satisfy $\sum_{i=1}^n w_i = 1$.
- Portfolio return is the weighted sum of asset returns.
- Portfolio mean is the weighted sum of asset means.
- Portfolio variance depends on both individual asset risk and covariance between assets.
- Correlation helps describe how much diversification reduces risk.
- The GMVP is the portfolio with the lowest possible variance.
