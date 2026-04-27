# Lecture 09

This lecture introduces options, option payoffs and profits, no-arbitrage
bounds, option trading strategies, put-call parity, and binomial option
pricing.

## Variable Definitions

$$
t = \text{current time}
$$

$$
T = \text{expiration date or maturity date}
$$

$$
S_0 = \text{current price of the underlying asset}
$$

$$
S_T = \text{price of the underlying asset at maturity}
$$

$$
S = S_0 = \text{current stock price in the binomial model}
$$

$$
K = \text{strike price or exercise price}
$$

$$
C = \text{current price of a European call option}
$$

$$
P = \text{current price of a European put option}
$$

$$
\text{premium} = \text{up-front fee paid by the holder to the writer}
$$

$$
\text{holder} = \text{buyer of the option contract}
$$

$$
\text{writer} = \text{seller of the option contract}
$$

$$
\text{exercise} = \text{holder's act of using the option right}
$$

$$
TV_T(C),\ TV_T(P)
= \text{time-}T \text{ values of the call and put premiums}
$$

$$
D = \text{discount factor from time } T \text{ to time } 0
$$

$$
r = \text{risk-free interest rate under the compounding convention used in the current section}
$$

$$
x^+ = \max(x,0)
$$

$$
K_1,K_2,K_3 = \text{different strike prices used in spread strategies}
$$

$$
u,d = \text{up and down stock-price factors in the binomial model}
$$

$$
R = 1+r = \text{one-period risk-free accumulation factor in the binomial section}
$$

$$
p = \text{real-world probability of an up move}
$$

$$
q = \frac{R-d}{u-d} = \text{risk-neutral probability}
$$

$$
C_u,\ C_d = \text{option values in the up and down child states of a one-step binomial pricing step}
$$

$$
C_{\text{node}} = \text{option value at a generic binomial node}
$$

$$
C_{\text{up}},\ C_{\text{down}}
= \text{option values at the two child nodes of a given node}
$$

$$
C_{uu}, C_{ud}, C_{dd}
= \text{option values at later binomial nodes}
$$

$$
x = \text{dollars invested in the stock in the replicating portfolio}
$$

$$
b = \text{dollars invested in the risk-free asset in the replicating portfolio}
$$

$$
\sigma = \text{annualized volatility in the CRR model}
$$

$$
\Delta t = \text{length of one time step in the CRR model}
$$

$$
PV_A,\ PV_B = \text{present values of the two portfolios used in put-call parity}
$$

Unless otherwise stated, the lecture assumes European options on one unit
of a non-dividend-paying underlying asset.

## Options

An option is a contract that gives the holder the right, but not the
obligation, to buy or sell an underlying asset under specified terms.

A call option gives the holder the right to buy the underlying asset.

A put option gives the holder the right to sell the underlying asset.

The holder is the buyer of the option. The writer is the seller of the
option.

Exercise is the act of using the option right:

- a call holder pays $K$ to buy the asset;
- a put holder sells the asset and receives $K$.

<details>
<summary>Intuition / proof</summary>

The crucial difference between a forward and an option is obligation.

- a forward forces both parties to transact at maturity
- an option lets the holder choose whether exercising is favorable

That is why option payoffs use the positive-part operator:

$$
x^+=\max(x,0).
$$

If exercising would create a loss, the holder simply does not exercise.

That is why an option has asymmetric payoff: the downside is truncated at
zero, but favorable outcomes are still kept.
</details>

## European and American Options

A European option can be exercised only at maturity.

An American option can be exercised at any time up to maturity.

Unless stated otherwise, the lecture assumes European options.

<details>
<summary>Intuition / proof</summary>

European versus American affects timing, not the basic payoff formula.

- European: exercise only at time $T$
- American: exercise any time up to $T$

European options are easier to price, which is why the lecture develops
pricing theory in that setting.

For this lecture, European options are especially convenient because all
payoff comparisons happen at one common date, namely maturity.
</details>

## Payoffs at Maturity

The payoff of a European call at maturity is:

$$
(S_T-K)^+ = \max(S_T-K,0)
$$

The payoff of a European put at maturity is:

$$
(K-S_T)^+ = \max(K-S_T,0)
$$

<details>
<summary>Intuition / proof</summary>

For a call:

- if $S_T \le K$, buying at strike $K$ is not attractive, so the holder
  does not exercise and payoff is $0$
- if $S_T > K$, the holder buys at $K$ and receives something worth $S_T$,
  so payoff is $S_T-K$

For a put:

- if $S_T \ge K$, selling at strike $K$ is not attractive, so payoff is $0$
- if $S_T < K$, the holder can sell for $K$ something worth only $S_T$, so
  payoff is $K-S_T$

This is exactly why the call payoff is increasing in $S_T$, while the put
payoff is decreasing in $S_T$.

It also explains why calls benefit from high final stock prices, while puts
benefit from low final stock prices.
</details>

## Option Positions and Profits

A long option position means buying the option.

A short option position means writing, or selling, the option.

If the premium is paid at time $0$, then profit at time $T$ equals payoff
minus the time value of the premium.

Long call profit:

$$
\max(S_T-K,0) - TV_T(C)
$$

Long put profit:

$$
\max(K-S_T,0) - TV_T(P)
$$

Short call profit:

$$
-\max(S_T-K,0) + TV_T(C)
$$

Short put profit:

$$
-\max(K-S_T,0) + TV_T(P)
$$

<details>
<summary>Intuition / proof</summary>

Payoff tells you what the option itself produces at maturity.
Profit also accounts for what you paid or received at time $0$.

For a long call, for example:

$$
\text{profit}=\text{call payoff}-\text{time-}T \text{ value of premium}.
$$

The short position always has the opposite payoff and opposite profit from
the corresponding long position.

This is why an option can have positive payoff but still negative profit:
the payoff may be too small to recover the premium paid upfront.
</details>

## Upper Bounds for European Option Prices

Assume:

- no transaction costs;
- the same tax rate for all investors;
- borrowing and lending are possible at the risk-free rate;
- the underlying asset pays no dividends.

Then the no-arbitrage upper bounds are:

$$
C \le S_0
$$

$$
P \le DK
$$

With continuous compounding:

$$
D = e^{-rT}
$$

so:

$$
P \le Ke^{-rT}
$$

<details>
<summary>Intuition / proof</summary>

A call cannot be worth more than the stock itself, because owning the
stock gives at least as much upside as owning the right to buy the stock
later.

If $C>S_0$, one could:

- buy the stock for $S_0$
- sell the call for $C$

to receive positive cash now, and at maturity the stock position always
covers the call obligation.

A put cannot be worth more than the present value of $K$, because the most
a put can ever deliver at maturity is $K$.

If $P>DK$, one could:

- write the put
- invest the premium proceeds at the risk-free rate

Since the worst possible maturity obligation is paying $K$, the invested
proceeds are enough to cover it and still leave arbitrage profit.
</details>

## Lower Bounds for European Option Prices

The call lower bound is:

$$
C \ge \max(0,S_0-DK)
$$

With continuous compounding:

$$
C \ge \max(0,S_0-Ke^{-rT})
$$

The put lower bound is:

$$
P \ge \max(0,DK-S_0)
$$

With continuous compounding:

$$
P \ge \max(0,Ke^{-rT}-S_0)
$$

<details>
<summary>Intuition / proof</summary>

The lower bounds say an option cannot be too cheap relative to a portfolio
that can dominate it.

For the call, if

$$
C < S_0-DK,
$$

then one can:

- short the stock and receive $S_0$
- buy the call for $C$
- invest the difference $S_0-C$

At maturity, either the call is exercised to close the short stock
position, or the stock is bought in the market. In both cases, the
investment is enough to leave positive profit.

For the put, if

$$
P < DK-S_0,
$$

then one can:

- borrow enough money to buy the stock and the put
- buy the stock
- buy the put

At maturity, either the put is exercised for $K$ or the stock itself is
worth at least $K$, so the final cash flow covers the loan and leaves
positive profit.
</details>

## Trading Strategies

A principal-protected note combines a zero-coupon bond with a call option.
The bond protects principal, while the call provides upside exposure.

A spread is a strategy involving two or more options of the same type.

A combination is a strategy involving both calls and puts.

## Bull Spread

A bull spread profits from an increase in the underlying price, but with
limited upside and limited downside.

Using calls, it is formed by:

- buying one call with lower strike $K_1$;
- selling one call with higher strike $K_2$;
- assuming $K_1<K_2$.

Ignoring premiums, the payoff is:

$$
\begin{cases}
0, & S_T \le K_1, \\
S_T-K_1, & K_1 < S_T < K_2, \\
K_2-K_1, & S_T \ge K_2.
\end{cases}
$$

<details>
<summary>Intuition / proof</summary>

The long low-strike call benefits from price increases first.
The short high-strike call starts offsetting gains only after the stock
moves above $K_2$.

So the payoff:

- is flat when the stock stays too low
- rises between $K_1$ and $K_2$
- becomes capped at $K_2-K_1$

This is why a bull spread expresses a moderately bullish view, not an
unlimited-upside view.
</details>

## Bear Spread

A bear spread profits from a decrease in the underlying price, but both
gain and loss are limited.

It can be formed using either calls or puts, and its payoff moves in the
opposite direction of a bull spread.

<details>
<summary>Intuition / proof</summary>

A bear spread is the mirror image of a bull spread.

Instead of benefiting from moderate increases in price, it benefits from
moderate decreases in price.

Its payoff is still bounded, which is why it represents a directional view
with limited risk and limited reward.
</details>

## Butterfly Spread

A butterfly spread is useful when the investor expects the underlying
price to stay near a middle strike.

Using calls, let:

$$
K_1 < K_2 < K_3
$$

Usually:

$$
K_1 + K_3 = 2K_2
$$

The strategy is:

- buy one call with strike $K_1$;
- sell two calls with strike $K_2$;
- buy one call with strike $K_3$.

Ignoring premiums, the payoff is:

$$
\begin{cases}
0, & S_T \le K_1, \\
S_T-K_1, & K_1 < S_T \le K_2, \\
K_3-S_T, & K_2 < S_T < K_3, \\
0, & S_T \ge K_3.
\end{cases}
$$

<details>
<summary>Intuition / proof</summary>

A butterfly spread is built to profit when the stock ends near the middle
strike $K_2$.

- the low-strike long call helps when the price rises toward the middle
- the two short middle-strike calls hurt if the price moves too far beyond
  the center
- the high-strike long call limits the loss from very large upward moves

So the payoff peaks in the middle and is small at both extremes.

This is why butterfly spreads are useful when the investor expects little
movement and wants to profit from the stock staying near a target range.
</details>

## Box Spread

A box spread combines a bull spread and a bear spread using the same pair
of strike prices.

Its payoff is riskless, so its present value must equal the present value
of the certain payoff. Otherwise, there is an arbitrage opportunity.

<details>
<summary>Intuition / proof</summary>

Because a box spread creates a fixed payoff at maturity, it behaves like a
synthetic zero-coupon bond.

So no-arbitrage says its current price must equal the present value of that
fixed maturity payoff.

If the market price differed, one could buy the cheaper of the box spread
and the matching risk-free payoff and sell the more expensive one.
</details>

## Covered Call, Protective Put, and Long Straddle

A covered call is formed by holding the underlying asset and writing a
call option on it.

Ignoring premiums, its payoff is:

$$
S_T-(S_T-K)^+ = \min(S_T,K)
$$

A protective put is formed by holding the underlying asset and buying a
put option on it.

Ignoring premiums, its payoff is:

$$
S_T+(K-S_T)^+ = \max(S_T,K)
$$

A long straddle is formed by buying a call and a put with the same strike
and maturity.

Ignoring premiums, its payoff is:

$$
(S_T-K)^+ + (K-S_T)^+ = |S_T-K|
$$

<details>
<summary>Intuition / proof</summary>

These strategies combine options with the stock to reshape risk:

- a covered call sacrifices upside beyond $K$
- a protective put creates a floor at $K$
- a long straddle profits when the stock moves far away from $K$ in either
  direction

The formulas simplify because the option payoff either truncates the stock
payoff above $K$, lifts it up to $K$, or measures distance from $K$.

In particular, the straddle payoff becomes $|S_T-K|$, so it is small near
the strike and large when the stock ends far away from the strike in either
direction.
</details>

## Put-Call Parity

For a European call and put on the same non-dividend-paying underlying,
with the same strike $K$ and maturity $T$:

$$
C+DK = P+S_0
$$

With continuous compounding:

$$
D=e^{-rT}
$$

so:

$$
C+Ke^{-rT}=P+S_0
$$

<details>
<summary>Intuition / proof</summary>

Consider two portfolios:

- Portfolio A: one call plus a zero-coupon bond paying $K$ at time $T$
- Portfolio B: one put plus one share of stock

Their present values are:

$$
PV_A=C+DK,\qquad PV_B=P+S_0.
$$

At maturity:

- if $S_T \ge K$, Portfolio A pays $S_T-K+K=S_T$, while Portfolio B pays
  $0+S_T=S_T$
- if $S_T < K$, Portfolio A pays $0+K=K$, while Portfolio B pays
  $K-S_T+S_T=K$

So both portfolios have the same payoff in every state.
By the law of one price, they must have the same present value, which
gives put-call parity.

This is the cleanest example in the lecture of pricing by payoff
equivalence: same future payoff implies same present value.
</details>

## Single-Period Binomial Model

Let the stock price today be:

$$
S=S_0
$$

After one period, it moves either up to $uS$ or down to $dS$, where:

$$
u>d>0
$$

Let:

$$
R=1+r
$$

Here, $r$ is the one-period effective risk-free rate, so $R$ is the
one-period accumulation factor.

For no-arbitrage, the lecture requires:

$$
u>R>d
$$

For a call option, the one-period up-state and down-state option values
are terminal payoffs:

$$
C_u=\max(uS-K,0)
$$

$$
C_d=\max(dS-K,0)
$$

The one-period call price is:

$$
C=\frac{1}{R}\left(qC_u+(1-q)C_d\right)
$$

where:

$$
q=\frac{R-d}{u-d}
$$

Important point:

$$
q \ne p \text{ in general}
$$

<details>
<summary>Intuition / proof</summary>

The binomial model prices the option by replacing uncertain payoffs with a
no-arbitrage equivalent valuation rule.

The condition

$$
u>R>d
$$

prevents trivial arbitrage between the stock and the risk-free asset.

If $R\ge u$, even the up state of the stock underperforms the bank account,
so one could short the stock and invest risk-free. If $d\ge R$, even the
down state does not underperform the bank account, so one could buy the
stock instead of lending risk-free.

The quantity

$$
q=\frac{R-d}{u-d}
$$

is not the real probability of an up move. It is the pricing weight that
makes the discounted expected stock price under $q$ match no-arbitrage:

$$
S=\frac{1}{R}\bigl(quS+(1-q)dS\bigr).
$$

That is why the option price depends on $q$, not on the real-world
probability $p$.
</details>

## Replicating Portfolio

The one-period option price can also be found by building a portfolio
that replicates the option payoff.

Let:

$$
ux+Rb=C_u
$$

$$
dx+Rb=C_d
$$

Solving gives:

$$
x=\frac{C_u-C_d}{u-d}
$$

$$
b=\frac{uC_d-dC_u}{R(u-d)}
$$

Therefore:

$$
C=x+b
$$

<details>
<summary>Intuition / proof</summary>

The idea is to choose holdings in stock and the risk-free asset so that the
portfolio payoff matches the option payoff in both possible future states.

Once:

$$
ux+Rb=C_u, \qquad dx+Rb=C_d,
$$

the portfolio and the option are indistinguishable at maturity.
By no-arbitrage, they must have the same current price.

Solving the two linear equations gives:

$$
x=\frac{C_u-C_d}{u-d},\qquad
b=\frac{uC_d-dC_u}{R(u-d)}.
$$

This is the core replication idea behind derivative pricing.
</details>

## Multi-Period Binomial Pricing

For multiple periods, first compute option payoffs at maturity, then work
backward one node at a time.

At each node:

$$
C_{\text{node}}
= \frac{1}{R}\left(qC_{\text{up}}+(1-q)C_{\text{down}}\right)
$$

For two periods:

$$
C_u=\frac{1}{R}\left(qC_{uu}+(1-q)C_{ud}\right)
$$

$$
C_d=\frac{1}{R}\left(qC_{ud}+(1-q)C_{dd}\right)
$$

Then:

$$
C=\frac{1}{R}\left(qC_u+(1-q)C_d\right)
$$

<details>
<summary>Intuition / proof</summary>

The same one-step pricing logic applies at every node of the tree.

So the pricing procedure is:

1. compute terminal payoffs at maturity
2. move backward one step at a time
3. apply the one-period pricing formula at each node

This backward induction is the main computational method for binomial
option pricing.

Because the tree recombines, many different paths can lead to the same
intermediate node, which keeps the calculation manageable.
</details>

## CRR Binomial Model

In the Cox-Ross-Rubinstein model:

$$
u=e^{\sigma\sqrt{\Delta t}}
$$

$$
d=\frac{1}{u}
$$

As $\Delta t \to 0$, the CRR binomial model approaches the
Black-Scholes model.

The lecture notes that Black-Scholes itself is not required for the final
exam.

<details>
<summary>Intuition / proof</summary>

The CRR model chooses the up and down factors so that the tree recombines
neatly and the one-step volatility matches the intended volatility level
$\sigma$.

The relation

$$
d=\frac{1}{u}
$$

makes the up and down moves multiplicatively symmetric, which is why an
up move followed by a down move returns to the same node.

As the time step $\Delta t$ gets smaller, the CRR tree becomes a finer and
finer approximation to continuous-time option pricing.
</details>

## Key Takeaways

- A call payoff is $(S_T-K)^+$ and a put payoff is $(K-S_T)^+$.
- Profit equals payoff adjusted for the time-0 premium.
- No-arbitrage gives the option price bounds and put-call parity.
- A bull spread has limited upside and downside; a butterfly spread peaks near the middle strike.
- In the binomial model, no-arbitrage requires $u>R>d$.
- The risk-neutral probability is $q=\frac{R-d}{u-d}$, and in general it is not the real-world probability.
- Binomial option pricing can be done either by risk-neutral valuation or by replication.
- In the CRR model, $u=e^{\sigma\sqrt{\Delta t}}$ and $d=1/u$.
