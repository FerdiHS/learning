# Lecture 05

This lecture moves from fixed-income securities to investments whose
future value is uncertain.

Returns are therefore studied using probability and random variables.

## Variable Definitions

$$
t = \text{time}
$$

$$
\Omega = \text{sample space of the random experiment}
$$

$$
\omega = \text{an outcome in the sample space}
$$

$$
P_0 = \text{price of the asset at time } 0
$$

$$
P_1 = \text{price of the asset at time } 1
$$

$$
R = \text{total return}
$$

$$
r = \text{rate of return}
$$

$$
X,Y,Z = \text{random variables}
$$

$$
x,y = \text{possible values taken by random variables}
$$

$$
p(x) = P(X=x) = \text{probability mass function of } X
$$

$$
p(x,y) = P(X=x,Y=y) = \text{joint probability mass function of } X \text{ and } Y
$$

$$
P(X=x),\ P(X=x,Y=y) = \text{probabilities of single or joint events}
$$

$$
E(X) = \text{expected value, or mean, of } X
$$

$$
\mu = E(X) = \text{mean of } X
$$

$$
\operatorname{Var}(X) = \text{variance of } X
$$

$$
\operatorname{SD}(X) = \text{standard deviation of } X
$$

$$
\sigma^2,\sigma_X^2 = \text{common notation for } \operatorname{Var}(X)
$$

$$
\sigma,\sigma_X = \text{common notation for } \operatorname{SD}(X)
$$

$$
\operatorname{Cov}(X,Y) = \text{covariance of } X \text{ and } Y
$$

$$
\rho_{X,Y} = \operatorname{Corr}(X,Y) = \text{correlation coefficient of } X \text{ and } Y
$$

$$
a,b,c = \text{constants}
$$

## Assets

An investment instrument that can be bought and sold is called an asset.

Suppose an investor buys an asset at time $t=0$ for price $P_0$ and sells
it at time $t=1$ for price $P_1$.

<details>
<summary>Intuition / proof</summary>

The lecture uses a simple one-period investment horizon:

- buy at time $0$
- sell at time $1$

This is enough to define return. More complicated settings build on the
same idea period by period.
</details>

## Returns

The total return is defined as:

$$
R = \frac{\text{amount received}}{\text{amount invested}}
= \frac{P_1}{P_0}
$$

The rate of return is defined as:

$$
r
= \frac{\text{amount received} - \text{amount invested}}
{\text{amount invested}}
= \frac{P_1 - P_0}{P_0}
$$

The two are related by:

$$
R = 1 + r
$$

Hence:

$$
P_1 = (1+r)P_0
$$

So a rate of return behaves like an interest rate over a single period.

<details>
<summary>Intuition / proof</summary>

Total return tells us the multiplicative growth factor of the investment:

$\displaystyle R=\frac{P_1}{P_0}$

Rate of return tells us the gain relative to the initial investment:

$\displaystyle r=\frac{P_1-P_0}{P_0}$

Since

$\displaystyle \frac{P_1}{P_0} = \frac{P_0+(P_1-P_0)}{P_0} = 1+\frac{P_1-P_0}{P_0},$

we get:

$\displaystyle R=1+r$

Rearranging gives:

$\displaystyle P_1=(1+r)P_0,$

which is why a one-period return acts like a one-period interest rate.
</details>

## Short Sales

Short-selling means selling an asset that the investor does not currently
own.

The mechanics are:

1. Borrow the asset from someone who owns it.
2. Sell the borrowed asset and receive $P_0$.
3. Later, buy it back for $P_1$ and return it to the lender.

Dollar profit from a short sale:

$$
\text{profit} = P_0 - P_1
$$

So a profit is made when:

$$
P_1 < P_0
$$

The lecture notes that short-selling is risky because the potential loss is
unlimited: the asset price can rise without bound.

For short sales, using the lecture's sign convention for total return:

$$
R = \frac{-P_1}{-P_0} = \frac{P_1}{P_0}
$$

Under this convention, the short-sale dollar profit is still measured by
$P_0-P_1$. So unlike a long position, a profitable short sale has
$R<1$ and $r<0$.

<details>
<summary>Intuition / proof</summary>

In a short sale, you benefit when the asset price falls.

You first receive $P_0$ from selling the borrowed asset, then later pay
$P_1$ to buy it back. So the dollar profit is:

$\displaystyle P_0-P_1$

That is positive exactly when:

$\displaystyle P_1<P_0$

The "return on short sale" formula can look strange because the lecture
treats the initial and final cash flows using negative signs:

$\displaystyle R=\frac{-P_1}{-P_0}=\frac{P_1}{P_0}$

So this return convention does not track profit direction in the same
intuitive way as a long position. A profitable short sale has a positive
dollar profit $P_0-P_1$, but it corresponds to $R<1$ and $r<0$.

The key danger is that if the asset price rises a lot, then the cost of
buying it back can become arbitrarily large, so losses are not capped.
</details>

## Returns as Random Variables

If the future selling price $P_1$ is uncertain, then the return is random.
So returns can be studied using probability.

<details>
<summary>Intuition / proof</summary>

Once $P_1$ is unknown, formulas such as

$\displaystyle R=\frac{P_1}{P_0}, \qquad r=\frac{P_1-P_0}{P_0}$

become random as well. That is why return analysis naturally leads into
random variables, probability, expectation, and variance.
</details>

## Random Variable

A random variable is a function that assigns a real number to each
outcome in the sample space of a random experiment.

<details>
<summary>Intuition / proof</summary>

A random experiment produces an outcome $\omega \in \Omega$.

A random variable $X$ then converts that outcome into a number:

$\displaystyle X(\omega)$

This lets us describe uncertain quantities numerically, which is why
random variables are the bridge from probability to finance.
</details>

## Discrete Random Variable

A random variable is discrete if its possible values are finite or
countably infinite.

If $X$ is a discrete random variable and $x$ is one of its possible values,
its probability mass function, or pmf, is:

$$
p(x) = P(X=x)
$$

<details>
<summary>Intuition / proof</summary>

A random variable turns uncertain outcomes into numbers that we can work
with mathematically.

In the discrete case, we can list the possible values and assign a
probability to each one. That is exactly what the pmf does:

$\displaystyle p(x)=P(X=x)$

So the pmf is the basic input used to compute expectations, variances, and
other summaries.
</details>

## Expected Value

If $X$ is a discrete random variable with pmf $p(x)$, then its expected
value is:

$$
E(X) = \sum_x x\,p(x)
$$

where the sum is over all possible values of $x$.

The expected value is also called the mean, and is often denoted by:

$$
\mu = E(X)
$$

### Basic Properties of Expectation

$$
E(c) = c
$$

$$
E(cX) = cE(X)
$$

$$
E(X+c) = E(X) + c
$$

$$
E(aX+bY) = aE(X) + bE(Y)
$$

If:

$$
X > 0
$$

then:

$$
E(X) > 0
$$

<details>
<summary>Intuition / proof</summary>

The expected value is a probability-weighted average:

$\displaystyle E(X)=\sum_x x\,p(x)$

It describes the center of the distribution, or the long-run average if
the experiment were repeated many times.

The expectation rules come from distributing sums and constants through
the weighted average. For example:

$\displaystyle E(aX+bY)=aE(X)+bE(Y)$

because expectation is linear.

The sign-preserving property also makes sense intuitively: if $X$ is always
positive, then every value contributing to the weighted average is
positive, so the weighted average itself must be positive.
</details>

## Variance

If $X$ has mean $E(X)$, then the variance of $X$ is:

$$
\operatorname{Var}(X)
= E\left[(X-E(X))^2\right]
$$

Variance is the expected squared deviation from the mean.

Common notation:

$$
\sigma^2, \qquad \sigma_X^2
$$

<details>
<summary>Intuition / proof</summary>

Variance measures spread around the mean.

The deviation from the mean is:

$\displaystyle X-E(X)$

Squaring removes sign and emphasizes larger deviations:

$\displaystyle (X-E(X))^2$

Taking expectation gives the average squared distance from the mean:

$\displaystyle \operatorname{Var}(X)=E\left[(X-E(X))^2\right]$
</details>

## Standard Deviation

The standard deviation of $X$ is the square root of its variance:

$$
\operatorname{SD}(X) = \sqrt{\operatorname{Var}(X)}
$$

Common notation:

$$
\sigma, \qquad \sigma_X
$$

The standard deviation has the same units as $X$.

<details>
<summary>Intuition / proof</summary>

Variance is useful mathematically, but it is measured in squared units.
Standard deviation fixes that by taking the square root:

$\displaystyle \operatorname{SD}(X)=\sqrt{\operatorname{Var}(X)}$

So it measures spread in the same units as the original random variable.
</details>

## Covariance

Covariance measures linear dependence between two random variables $X$
and $Y$.

It is defined by:

$$
\operatorname{Cov}(X,Y)
= E\left[(X-E(X))(Y-E(Y))\right]
$$

It can also be written as:

$$
\operatorname{Cov}(X,Y)
= E(XY) - E(X)E(Y)
$$

In general:

$$
E(XY) \ne E(X)E(Y)
$$

<details>
<summary>Intuition / proof</summary>

Covariance checks whether $X$ and $Y$ tend to move together relative to
their means.

- If both are above their means together, the product
  $(X-E(X))(Y-E(Y))$ is positive.
- If one is above its mean while the other is below, the product is
  negative.

So:

$\displaystyle \operatorname{Cov}(X,Y) =E\left[(X-E(X))(Y-E(Y))\right]$

is positive for positive linear association and negative for negative
linear association.

Expanding the product gives the equivalent formula:

$\displaystyle \operatorname{Cov}(X,Y)=E(XY)-E(X)E(Y)$
</details>

## Correlation Coefficient

The correlation coefficient of $X$ and $Y$ is:

$$
\rho_{X,Y}
= \operatorname{Corr}(X,Y)
= \frac{\operatorname{Cov}(X,Y)}
{\operatorname{SD}(X)\operatorname{SD}(Y)}
$$

It rescales covariance to remove the units of $X$ and $Y$.

When $\operatorname{SD}(X)$ and $\operatorname{SD}(Y)$ are both nonzero,
its range is:

$$
-1 \le \rho_{X,Y} \le 1
$$

Interpretation:

- $\rho_{X,Y}$ near $+1$ means strong positive linear relationship.
- $\rho_{X,Y}$ near $-1$ means strong negative linear relationship.
- $\rho_{X,Y}$ near $0$ means little or no linear relationship.

<details>
<summary>Intuition / proof</summary>

Covariance depends on the units of $X$ and $Y$, so its size is hard to
compare across problems.

Correlation removes the scale by dividing by the standard deviations:

$\displaystyle \rho_{X,Y} =\frac{\operatorname{Cov}(X,Y)} {\operatorname{SD}(X)\operatorname{SD}(Y)}$

That is why correlation is unit-free and, when defined, always lies between
$-1$ and $1$.

The closer $\rho_{X,Y}$ is to $\pm 1$, the stronger the linear
relationship.

In effect, correlation is a normalized covariance: it keeps the direction
of association while removing the scale effect.
</details>

## Independence

Two jointly discrete random variables $X$ and $Y$ are independent if:

$$
P(X=x,Y=y) = P(X=x)P(Y=y)
$$

for all $x$ and $y$.

Important relationship:

- If $X$ and $Y$ are independent, then $\operatorname{Cov}(X,Y)=0$. If $\operatorname{SD}(X)$ and $\operatorname{SD}(Y)$ are both nonzero, then $\rho_{X,Y}=0$.
- But $\rho_{X,Y}=0$ does not imply independence in general.

<details>
<summary>Intuition / proof</summary>

Independence means knowing the value of one random variable gives no
information about the other.

For discrete random variables, that is expressed by the factorization:

$\displaystyle P(X=x,Y=y)=P(X=x)P(Y=y)$

Independence implies zero covariance, but the converse fails in general:
two variables can have no linear relationship while still being dependent
in a nonlinear way.

The reason independence implies zero covariance is that independence gives

$\displaystyle E(XY)=E(X)E(Y),$

so:

$\displaystyle \operatorname{Cov}(X,Y)=E(XY)-E(X)E(Y)=0.$
</details>

## Properties of Covariance and Variance

For constants $a,b$ and random variables $X,Y,Z$:

$$
\operatorname{Cov}(X,Y) = \operatorname{Cov}(Y,X)
$$

$$
\operatorname{Cov}(X,X) = \operatorname{Var}(X)
$$

$$
\operatorname{Cov}(aX+bY,Z)
= a\operatorname{Cov}(X,Z) + b\operatorname{Cov}(Y,Z)
$$

For two random variables $X$ and $Y$:

$$
\operatorname{Var}(X+Y)
= \operatorname{Var}(X) + 2\operatorname{Cov}(X,Y) + \operatorname{Var}(Y)
$$

<details>
<summary>Intuition / proof</summary>

These formulas show how covariance and variance behave under algebraic
combinations.

The most important one here is:

$\displaystyle \operatorname{Var}(X+Y) = \operatorname{Var}(X) + 2\operatorname{Cov}(X,Y) + \operatorname{Var}(Y)$

It comes from expanding:

$\displaystyle \operatorname{Var}(X+Y) = E\left[(X+Y-E(X)-E(Y))^2\right]$

The cross term appears twice, which is why the covariance coefficient is
$2$, not $1$.
</details>

## Key Takeaways

- $R$ is total return and $r$ is rate of return.
- $R = 1+r$ and $P_1=(1+r)P_0$.
- For a short sale, profit is made when the asset price falls.
- When returns are uncertain, they are treated as random variables.
- Expectation measures center, while variance and standard deviation measure spread.
- Covariance and correlation describe how two random variables move together.
- Independence implies zero covariance, but zero covariance does not imply independence in general.
