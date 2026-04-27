# Lecture 08

This lecture explains how forwards and futures can be used to reduce or
eliminate exposure to price risk.

The main theme is:

$$
\text{hedge}=\text{position that offsets unwanted risk}
$$

## Variable Definitions

$$
t = \text{current time}
$$

$$
T = \text{future date at which the exposure is realized or the hedge is closed}
$$

$$
0 = \text{initial time}
$$

$$
W = \text{number of units of the asset exposure}
$$

$$
A,B = \text{asset being hedged and futures asset used for hedging}
$$

$$
S_T = S_A(T) = \text{spot price of asset } A \text{ at time } T
$$

$$
F_0 = F_B(0) = \text{futures price of asset } B \text{ at time } 0
$$

$$
F_T = F_B(T) = \text{futures price of asset } B \text{ at time } T
$$

$$
F(0,T) = \text{forward price agreed at time } 0 \text{ for delivery at } T
$$

$$
h = \text{futures position size used in the hedge}
$$

$$
y = y_T = \text{cash flow of the hedged position at time } T
$$

$$
\beta = \frac{\operatorname{Cov}(S_T,F_T)}{\operatorname{Var}(F_T)}
= \text{optimal hedge ratio per unit of exposure}
$$

$$
\rho_{S,F} = \operatorname{Corr}(S_T,F_T) \text{ when } \sigma_S,\sigma_F > 0
$$

$$
\rho_{F,S}=\rho_{S,F}
$$

$$
\sigma_S = \operatorname{SD}(S_T), \qquad
\sigma_F = \operatorname{SD}(F_T), \qquad
\sigma_y = \operatorname{SD}(y)
$$

$$
\operatorname{Var}(S_T),\operatorname{Var}(F_T),\operatorname{Var}(y)
= \text{variances of } S_T,\ F_T,\ \text{and } y
$$

$$
\operatorname{Cov}(S_T,F_T)
= \text{covariance between the spot exposure and the futures price}
$$

$$
\operatorname{Corr}(S_T,F_T)
= \text{correlation between the spot exposure and the futures price}
$$

$$
\operatorname{basis} = \text{spot price of asset to be hedged}
- \text{futures price of contract used}
$$

Unless otherwise stated, the minimum-variance hedge derivation assumes
interest rates are zero.

## Hedging with a Forward Contract

Using a forward contract to hedge is conceptually straightforward:
you lock in today the future buying or selling price of the asset.

If you will sell the asset in the future, the risk is that the spot price
falls. A short forward position offsets that risk.

If you will buy the asset in the future, the risk is that the spot price
rises. A long forward position offsets that risk.

For a seller of $W$ units, the unhedged time-$T$ revenue is:

$$
WS_T
$$

If the seller shorts a forward for $W$ units at delivery price $F(0,T)$,
the forward payoff is:

$$
W\bigl(F(0,T)-S_T\bigr)
$$

So the total hedged revenue is:

$$
WS_T + W\bigl(F(0,T)-S_T\bigr) = WF(0,T)
$$

For a buyer of $W$ units, the unhedged cost is:

$$
-WS_T
$$

If the buyer goes long a forward, the forward payoff is:

$$
W\bigl(S_T-F(0,T)\bigr)
$$

So the total hedged cash flow is:

$$
-WS_T + W\bigl(S_T-F(0,T)\bigr) = -WF(0,T)
$$

<details>
<summary>Intuition / proof</summary>

A forward hedge works by replacing an unknown future spot price with a
known delivery price.

For a future seller:

- without hedging, revenue is uncertain because it depends on $S_T$
- with a short forward, the loss from a lower spot price is exactly offset
  by a higher forward payoff

That is why:

$WS_T + W(F(0,T)-S_T)=WF(0,T).$

For a future buyer, the same cancellation happens with opposite sign:

$-WS_T + W(S_T-F(0,T))=-WF(0,T).$

In both cases, the spot-price uncertainty disappears completely.

The trade-off is that hedging removes downside risk but also gives up the
upside benefit from a favorable price move.
</details>

## Perfect Hedge with Futures

The primary use of futures contracts is to hedge against risk.

A perfect hedge completely eliminates the risk associated with a future
commitment to deliver or receive an asset by taking an opposite position
in the futures market.

Here "opposite" means opposite to the direction of risk:

- a future buyer hedges by taking a long futures position;
- a future seller hedges by taking a short futures position.

The lecture notes that a perfect hedge is only possible if:

- the delivery date of the asset matches the relevant futures date;
- the asset being hedged matches the futures underlying exactly;
- the amount hedged is an integral multiple of the futures contract size;
- the futures market is liquid enough.

If these conditions fail, risk may still be reduced, but not fully
eliminated.

<details>
<summary>Intuition / proof</summary>

A perfect hedge means the risky part of the original position and the risky
part of the futures position cancel exactly.

For example:

- if you must buy later, rising prices hurt you, so you want a hedge that
  profits when prices rise
- if you must sell later, falling prices hurt you, so you want a hedge that
  profits when prices fall

That is why the "opposite position" is defined relative to price risk, not
simply by whether you are buying or selling in the spot market.

The hedge is called perfect only when the risky part cancels exactly, not
merely approximately. That is why matching maturity, underlying asset, and
contract size matters so much.
</details>

## Basis

One measure of hedging imperfection is the basis:

$$
\operatorname{basis}
= \text{spot price of asset to be hedged}
- \text{futures price of contract used}
$$

Basis measures the mismatch between the asset exposure and the futures
contract used in the hedge.

If the hedged asset is identical to the futures underlying, then at the
delivery date the basis is zero because spot and futures prices coincide.

<details>
<summary>Intuition / proof</summary>

If the hedge were perfect, the futures price used for hedging would move
exactly with the spot price exposure.

Basis tells you how far reality is from that ideal.

- small basis means the hedge tracks the exposure closely
- large or unstable basis means the hedge leaves more residual risk

So basis risk is the core reason why a hedge using a related but different
futures contract may fail to eliminate risk completely.
</details>

## Minimum-Variance Hedge

When a perfect hedge is unavailable, the goal is no longer to make the
cash flow riskless. Instead, the goal is to choose the futures position
that makes the cash flow as stable as possible.

Suppose you have an obligation to purchase $W$ units of asset $A$ at
time $T$, but only futures on asset $B$ are available.

Assume the cash flow of the hedged position is:

$$
y = -WS_T + h(F_T-F_0)
$$

Here:

- $-WS_T$ is the cost of buying the asset at time $T$;
- $h(F_T-F_0)$ is the profit from the futures position.

The variance of the cash flow is:

$$
\operatorname{Var}(y)
= W^2\operatorname{Var}(S_T)
- 2W\operatorname{Cov}(S_T,F_T)h
+ \operatorname{Var}(F_T)h^2
$$

This is a quadratic function of $h$.

To minimize the risk, differentiate with respect to $h$ and set the
result equal to zero:

$$
-2W\operatorname{Cov}(S_T,F_T)
+ 2\operatorname{Var}(F_T)h = 0
$$

Hence the minimum-variance hedge is:

$$
h
= W\frac{\operatorname{Cov}(S_T,F_T)}
{\operatorname{Var}(F_T)}
$$

Using correlation and standard deviations, when $\sigma_F > 0$,

$$
h
= W\rho_{S,F}\frac{\sigma_S}{\sigma_F}
$$

The optimal hedge ratio per unit of exposure, when
$\operatorname{Var}(F_T)>0$, is:

$$
\beta
= \frac{\operatorname{Cov}(S_T,F_T)}
{\operatorname{Var}(F_T)}
$$

so:

$$
h = W\beta
$$

<details>
<summary>Intuition / proof</summary>

The unhedged position has random cash flow because $S_T$ is random.
Adding futures introduces another random term, $h(F_T-F_0)$.

We choose $h$ so that the futures profit moves in the direction that best
offsets the original exposure.

The algebra comes from:

$y=-WS_T+hF_T-hF_0.$

Since $F_0$ is known at time $0$, it is a constant, so it does not affect
variance. Therefore:

$\mathrm{Var}(y)=\mathrm{Var}(-WS_T+hF_T).$

Expanding with
$\mathrm{Var}(aX+bY)=a^2\mathrm{Var}(X)+b^2\mathrm{Var}(Y)+2ab\mathrm{Cov}(X,Y)$
gives:

$\mathrm{Var}(y) =W^2\mathrm{Var}(S_T)+h^2\mathrm{Var}(F_T)-2Wh\mathrm{Cov}(S_T,F_T).$

Because this is a quadratic in $h$ with positive coefficient
$\mathrm{Var}(F_T)$, its minimum occurs where the derivative is zero.

The formula

$h=W\frac{\mathrm{Cov}(S_T,F_T)}{\mathrm{Var}(F_T)}$

has a natural interpretation:

- if $S_T$ and $F_T$ move together strongly, covariance is large, so the
  hedge should be larger
- if futures prices are very volatile on their own, $\mathrm{Var}(F_T)$
  is large, so fewer futures are needed for the same offsetting effect

This is exactly the same logic as choosing the slope coefficient in a
linear least-squares fit: find the multiple of futures price changes that
best explains the spot exposure.
</details>

## Minimum Variance and Hedge Effectiveness

Substituting the optimal $h$ into the variance formula gives:

$$
\operatorname{Var}(y)
= W^2\operatorname{Var}(S_T)
- \frac{W^2\operatorname{Cov}(S_T,F_T)^2}
{\operatorname{Var}(F_T)}
$$

Equivalently:

$$
\operatorname{Var}(y)
= W^2\operatorname{Var}(S_T)(1-\rho_{S,F}^2)
$$

Hence the resulting standard deviation is:

$$
\sigma_y
= \sqrt{1-\rho_{S,F}^2}\,W\sigma_S
$$

The unhedged risk is:

$$
W\sigma_S
$$

So the hedge reduces risk by the factor:

$$
\sqrt{1-\rho_{S,F}^2}
$$

This factor measures hedge effectiveness.

<details>
<summary>Intuition / proof</summary>

The key determinant is the correlation between the spot exposure and the
futures contract.

- if $|\rho_{S,F}|$ is close to $1$, the futures contract tracks the
  exposure closely, so the remaining risk is small
- if $\rho_{S,F}$ is close to $0$, the futures contract does not move with
  the exposure, so the hedge does little

The term

$1-\rho_{S,F}^2$

is the fraction of variance that remains after optimal hedging.

This comes from substituting

$\mathrm{Cov}(S_T,F_T)=\rho_{S,F}\sigma_S\sigma_F$

into the minimized variance formula:

$W^2\mathrm{Var}(S_T) - \frac{W^2\mathrm{Cov}(S_T,F_T)^2}{\mathrm{Var}(F_T)} = W^2\sigma_S^2-\frac{W^2\rho_{S,F}^2\sigma_S^2\sigma_F^2}{\sigma_F^2}.$

That simplifies to:

$W^2\sigma_S^2(1-\rho_{S,F}^2).$

So:

- $\rho_{S,F}^2$ measures how much risk is explained or removed
- $\sqrt{1-\rho_{S,F}^2}$ measures how much standard deviation remains
</details>

## Sign of the Hedge

The formula

$$
h = W\beta
$$

was derived for an obligation to purchase $W$ units, with cash flow

$$
y = -WS_T + h(F_T-F_0)
$$

If instead you plan to sell $W$ units of the asset, then the spot exposure
changes sign, and the hedge becomes:

$$
h = -W\beta
$$

Equivalently, the sign of $h$ automatically reflects both:

- whether the exposure is a future purchase or a future sale;
- whether $\rho_{S,F}$ is positive or negative.

<details>
<summary>Intuition / proof</summary>

The hedge sign must oppose the direction of risk.

- if you will buy later, rising spot prices hurt you, so you want a futures
  position that tends to profit when futures prices rise
- if you will sell later, falling spot prices hurt you, so you want the
  opposite sign

Also, if the correlation is negative, then the futures contract already
moves in the opposite direction of the exposure, so the sign of the hedge
flips automatically through $\beta$.

This is why the lecture’s strategy table depends on both:

- whether the underlying exposure is a purchase or sale, and
- whether correlation is positive or negative

The sign of $\beta$ packages that information into one formula.
</details>

## Perfect Hedge as a Special Case

Suppose the futures asset is identical to the spot asset being hedged.
For the purchase-obligation setup used above, this gives:

$$
F_T = S_T
$$

Hence:

$$
\operatorname{Cov}(S_T,F_T)
= \operatorname{Cov}(S_T,S_T)
= \operatorname{Var}(S_T)
$$

Therefore:

$$
\beta = 1
$$

and:

$$
h = W
$$

The minimum variance becomes:

$$
\operatorname{Var}(y)=0
$$

So the hedge is perfect.

If instead the exposure were a future sale, then the matching perfect
hedge would have the opposite sign:

$$
h = -W
$$

<details>
<summary>Intuition / proof</summary>

When the futures contract and the spot exposure are really the same risk,
there is no basis mismatch left at the hedge date.

Then one unit of futures offsets one unit of spot exposure, so the optimal
hedge ratio becomes exactly $1$.

In the formula:

$\beta=\frac{\mathrm{Cov}(S_T,F_T)}{\mathrm{Var}(F_T)},$

setting $F_T=S_T$ gives:

$\beta=\frac{\mathrm{Var}(S_T)}{\mathrm{Var}(S_T)}=1.$

Then the hedged cash flow becomes deterministic, so its variance is zero.

This is why perfect hedges are best seen as a special case of the
minimum-variance hedge where the correlation is effectively complete and
the basis problem disappears.
</details>

## Key Takeaways

- Hedging reduces unwanted price risk by adding an offsetting position.
- Forward hedging locks in a known future buying or selling price.
- A perfect futures hedge completely eliminates risk, but requires a very close match between exposure and contract.
- Basis measures the mismatch between the spot exposure and the futures contract used.
- Minimum-variance hedging chooses the futures position that minimizes the variance of the hedged cash flow.
- The optimal hedge ratio is $\beta=\operatorname{Cov}(S_T,F_T)/\operatorname{Var}(F_T)$ when $\operatorname{Var}(F_T)>0$.
- Hedge effectiveness depends on $\rho_{S,F}^2$: stronger absolute correlation means a better hedge.
