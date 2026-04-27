# Lecture 07

This lecture introduces forward and futures contracts as derivatives whose
values depend on an underlying asset.

The key idea is that both contracts fix a price for future delivery, but
forwards settle only at maturity while futures are marked to market daily.

## Variable Definitions

$$
t = \text{current time or contract initiation time}
$$

$$
T = \text{expiration, maturity, or delivery date}
$$

$$
0 = \text{initial time}
$$

$$
N = \text{number of daily settlement intervals from time } 0 \text{ to maturity } T
$$

$$
i = \text{daily settlement index, where } i=0,1,\ldots,N
$$

$$
S_0 = \text{spot price of the underlying asset today}
$$

$$
S_t = \text{spot price of the underlying asset at time } t
$$

$$
S_T = \text{spot price of the underlying asset at maturity}
$$

$$
S(T) = S_T = \text{spot price at maturity}
$$

$$
F(t,T) = \text{forward or futures delivery price agreed at time } t \text{ for delivery at } T
$$

$$
F(0,T) = \text{delivery price agreed today for maturity } T
$$

$$
F(i) = \text{futures price on day } i
$$

$$
F(N)=F(T)=\text{futures price on the final settlement day, which is maturity}
$$

$$
F(0) = \text{initial futures price}
$$

$$
r = \text{risk-free interest rate per annum}
$$

$$
R = e^{r/365} = \text{daily accumulation factor under continuous compounding}
$$

$$
Q = \text{contract size}
$$

$$
V_{\text{ct}} = \text{total contract value of a futures position}
$$

$$
m_I,m_M = \text{initial margin rate and maintenance margin rate}
$$

$$
IM,MM = \text{initial margin requirement and maintenance margin requirement}
$$

$$
A_T^{\text{long}},A_T^{\text{short}}
= \text{accrued profit at maturity for long and short futures positions}
$$

$$
U,V = \text{two portfolios or trading strategies}
$$

$$
U_0,V_0 = \text{time-0 prices of portfolios } U \text{ and } V
$$

$$
U_T,V_T = \text{time-}T \text{ payoffs of portfolios } U \text{ and } V
$$

$$
F_{\text{forward}}(t,T) = \text{forward delivery price}
$$

$$
F_{\text{future}}(t,T) = \text{futures delivery price}
$$

Unless otherwise stated, the lecture assumes no money changes hands when
entering a forward contract, so its initial cost is zero.

## Forward Market

A forward contract is an obligation to buy or sell an underlying asset at
a specific price and at a specific time in the future.

The buyer of the forward contract is said to be long. The seller is said
to be short.

The spot market is the market for immediate delivery. The spot price is
the price for immediate delivery:

$$
S_t
$$

The forward price is the price that applies at future delivery:

$$
F(t,T)
$$

Unless otherwise stated, the lecture assumes no money changes hands
before maturity. Under this assumption, the forward payoff and profit are
the same.

Long forward payoff:

$$
S_T - F(t,T)
$$

Short forward payoff:

$$
F(t,T) - S_T
$$

<details>
<summary>Intuition / proof</summary>

A forward contract fixes the transaction price now, but the actual exchange
happens later at time $T$.

Under the lecture's assumption, the contract costs nothing to enter at
time $t$. So:

profit = payoff - initial cost = payoff.

If you are long, you agree to buy at price $F(t,T)$. At maturity, the
asset is worth $S_T$ in the market, so your net gain is:

$S_T-F(t,T)$

If you are short, you agree to sell at price $F(t,T)$, so your gain is:

$F(t,T)-S_T$

This also explains the sign:

- if $S_T>F(t,T)$, the long benefits because it buys below market value
- if $S_T<F(t,T)$, the short benefits because it sells above market value

These are exact opposites, which makes sense because every long position
is paired with a short position in the same contract.
</details>

## No-Arbitrage Principle and Law of One Price

An arbitrage opportunity is a strategy that makes a riskless profit.

The no-arbitrage principle says there cannot be two portfolio strategies
with the same initial cost such that one always gives a better final payoff
than the other.

The law of one price follows from no-arbitrage:

If two portfolios $U$ and $V$ have the same payoff at time $T$,

$$
U_T = V_T
$$

then they must have the same price at time $0$:

$$
U_0 = V_0
$$

Otherwise, investors could buy the cheaper portfolio and sell the more
expensive portfolio to earn arbitrage profit.

<details>
<summary>Intuition / proof</summary>

The no-arbitrage principle is the backbone of derivative pricing.

If two strategies end with exactly the same payoff, then paying more for
one than the other would be irrational in a competitive market.

Suppose $U_T=V_T$ but $U_0<V_0$. Then an investor could:

- buy $U$
- sell $V$

The initial cash flow would be positive, since $V$ is sold for more than
$U$ costs, while the final payoff would cancel out because the two
portfolios have the same payoff.

That creates arbitrage, so equal payoffs must imply equal prices.

In practice, derivative pricing often works by:

1. building a portfolio that replicates the derivative payoff
2. using the law of one price to say the derivative and the replicating
   portfolio must have the same value
</details>

## Forward Price by No-Arbitrage

Consider a non-dividend-paying asset with current spot price $S_0$.
Assume the risk-free interest rate is $r$ per annum, compounded
continuously.

By no-arbitrage, the forward price is:

$$
F(0,T) = S_0 e^{rT}
$$

For a general time $t$:

$$
F(t,T) = S_t e^{r(T-t)}
$$

Important point:

$$
F(t,T)
$$

depends on the spot price, risk-free rate, and time to maturity. It does
not depend on the expected rate of return of the underlying asset.

<details>
<summary>Intuition / proof</summary>

To lock in delivery of the asset at time $T$, there are two equivalent
strategies:

1. buy the asset now for $S_0$ and carry it to time $T$
2. enter a forward contract today for delivery at time $T$

If you buy the asset now using borrowed money at risk-free rate $r$, then
the time-$T$ cost of that strategy is:

$S_0 e^{rT}$

No arbitrage says the forward delivery price must match that carrying
cost, so:

$F(0,T)=S_0e^{rT}$

The same logic at a later time $t$ gives:

$F(t,T)=S_t e^{r(T-t)}.$

Another way to see this is by arbitrage bounds:

- if $F(0,T)>S_0e^{rT}$, then sell the forward, buy the asset now, and
  finance the purchase by borrowing
- if $F(0,T)<S_0e^{rT}$, then buy the forward, short-sell the asset now,
  and invest the short-sale proceeds at the risk-free rate

In either case, a mismatch between forward price and carrying cost creates
a riskless profit opportunity.

This formula comes from replication and no-arbitrage, not from guessing
where the asset price is expected to go.
</details>

## Futures

A futures contract is a standardized legal contract to buy or sell an
underlying asset at a predetermined price for delivery at a specified
future time.

Unlike forwards, futures are traded on organized exchanges. The exchange
standardizes the contract and reduces counterparty risk through a
clearinghouse, margins, and daily settlement.

<details>
<summary>Intuition / proof</summary>

A forward contract is private and customized. A futures contract is
standardized and exchange-traded.

Standardization makes it easier to trade in and out of the position, while
the clearinghouse reduces the risk that the other party defaults.

A useful mental picture is:

- in a forward, your direct counterparty matters
- in a futures contract, the exchange clearinghouse effectively stands
  between buyers and sellers
</details>

## Forwards vs Futures

| Feature | Forward Contract | Futures Contract |
|---|---|---|
| Trading venue | Over-the-counter private agreement | Exchange-traded |
| Contract terms | Customized | Standardized |
| Liquidity | Lower, harder to exit early | Higher, easier to buy or sell |
| Counterparty risk | Higher, depends on the other party | Lower, clearinghouse guarantees trade |
| Settlement | Physical or cash settlement at maturity | Can be settled daily or before maturity |
| Margin and collateral | Typically no margin, depends on contract | Requires initial and maintenance margin |
| Marking to market | No daily settlement | Marked to market daily |

<details>
<summary>Intuition / proof</summary>

The economic role of forwards and futures is similar: both set a price for
future delivery.

The operational differences matter in practice:

- forwards are flexible but less liquid and riskier counterparty-wise
- futures are standardized, easier to trade, and safer because of exchange
  clearing and margining

So the main difference is usually not the payoff idea, but the trading
mechanism and the timing of cash flows.
</details>

## Margin

Futures traders must open a margin account with a broker.

### Initial Margin

The initial margin is the amount that must be deposited when the futures
position is opened.

It is usually a fraction of the total contract value:

$$
IM = m_I V_{\text{ct}}
$$

### Maintenance Margin

The maintenance margin is the minimum account value that must be
maintained.

$$
MM = m_M V_{\text{ct}}
$$

If the margin account falls below the maintenance margin, a margin call is
issued. The trader must add funds, otherwise the futures position may be
closed out.

<details>
<summary>Intuition / proof</summary>

Margin is a performance bond, not the full purchase price of the asset.

- the initial margin opens the position
- the maintenance margin is the minimum balance needed to keep it open

Because futures are marked to market daily, the margin account absorbs
daily gains and losses. A margin call happens when losses push the account
below the required maintenance level.

So margin exists because exchange-traded futures do not wait until maturity
to discover gains and losses. The market wants enough cash in the account
to cover day-to-day adverse price moves.
</details>

## Daily Settlement

Daily settlement, or marking to market, means futures gains and losses
are settled every day.

Let $F(i)$ be the futures price on day $i$, where $i=0,1,\ldots,N$ and
day $N$ is the maturity date $T$.

For a long futures position, the daily cash flow from day $i-1$ to day $i$
is:

$$
F(i) - F(i-1)
$$

For a short futures position, the daily cash flow is:

$$
F(i-1) - F(i)
$$

At maturity, the futures price converges to the spot price:

$$
F(N) = F(T) = S(T)
$$

Otherwise, an arbitrage opportunity would exist.

<details>
<summary>Intuition / proof</summary>

Marking to market updates the contract each day as if gains and losses are
immediately realized.

This is the lecture's idea of "revising the contract as the price
environment changes." Instead of keeping one old delivery price all the way
to maturity, the contract is effectively reset each day by paying out that
day's gain or loss in cash.

For a long position:

- if the futures price rises, the position gains value, so the holder
  receives cash
- if the futures price falls, the holder pays cash

That is why the daily cash flow is:

$F(i)-F(i-1)$

At maturity, the futures contract becomes a contract for immediate
delivery, so the futures price must equal the spot price:

$F(N)=F(T)=S(T)$

If they differed, an investor could lock in an arbitrage by combining a
futures position with immediate spot-market trading.
</details>

## Accrued Profit

Accrued profit is the total accumulated profit from all daily settlement
cash flows, including interest earned or paid on those cash flows.

Let the daily accumulation factor be:

$$
R = e^{r/365}
$$

For a long futures position held from time $0$ to maturity $T$, the accrued
profit at time $T$ is:

$$
A_T^{\text{long}}
= \sum_{i=1}^{N}
\left[F(i)-F(i-1)\right]R^{N-i}
$$

For a position with contract size $Q$, multiply by $Q$:

$$
QA_T^{\text{long}}
= Q\sum_{i=1}^{N}
\left[F(i)-F(i-1)\right]R^{N-i}
$$

For a short futures position, the accrued profit is:

$$
A_T^{\text{short}}
= -\sum_{i=1}^{N}
\left[F(i)-F(i-1)\right]R^{N-i}
$$

If the interest rate is zero, then $R=1$ and the long futures profit
telescopes:

$$
A_T^{\text{long}}
= \sum_{i=1}^{N}
\left[F(i)-F(i-1)\right]
= F(N)-F(0)
$$

Since $F(N)=S(T)$ at maturity:

$$
A_T^{\text{long}} = S(T)-F(0)
$$

This is the same as the profit from a long forward contract with delivery
date $T$ and delivery price equal to the initial futures price $F(0)$ when
interest is zero.

<details>
<summary>Intuition / proof</summary>

The key difference between futures and forwards is timing of cash flows.

A futures gain received early can earn interest for longer, while a loss
paid early has financing consequences too.

That is why accrued profit is not just the sum of daily cash flows, but
the accumulated value of those cash flows at time $T$:

$A_T^{\mathrm{long}}=\sum_{i=1}^{N}[F(i)-F(i-1)]R^{N-i}$

The factor $R^{N-i}$ means the cash flow received on day $i$ is carried
forward for the remaining $N-i$ days until maturity.

If $r=0$, then $R=1$, so the expression simplifies to a telescoping sum:

$[F(N)-F(N-1)] + \cdots + [F(1)-F(0)] = F(N)-F(0).$

Since $F(N)=S(T)$, the total futures profit becomes:

$A_T^{\mathrm{long}}=S(T)-F(0),$

which is exactly the long forward profit. This is the first hint of
forward-futures equivalence.
</details>

## Forward-Futures Equivalence

Forwards and futures both set a price for future delivery. The main
difference is their cash flow timing:

- Forwards have no cash flow until maturity.
- Futures have daily cash flows due to marking to market.

In theory, forward and futures contracts on the same underlying asset and
with the same maturity should have the same delivery price if:

- there is no arbitrage;
- there are no transaction costs;
- interest rates are deterministic;
- interest rates are not correlated with the underlying asset price.

For this course, the simplifying assumption is:

$$
F_{\text{forward}}(t,T)
= F_{\text{future}}(t,T)
= F(t,T)
= S_t e^{r(T-t)}
$$

<details>
<summary>Intuition / proof</summary>

In general, forwards and futures need not be exactly identical because
futures generate interim cash flows while forwards do not.

Those interim cash flows matter when:

- interest rates are random, or
- interest rates are related to the underlying asset price

For example, if gains tend to arrive exactly when interest rates are high,
daily settlement becomes more valuable because those gains can be reinvested
at better rates.

But under the lecture's simplifying assumptions, the reinvestment effect
does not distort prices, so the two delivery prices coincide:

$F_{\mathrm{forward}}(t,T)=F_{\mathrm{future}}(t,T)=F(t,T).$

So for this course, forwards and futures can be priced using the same
no-arbitrage formula.
</details>

## Key Takeaways

- A forward is a private agreement for future delivery.
- A future is standardized, exchange-traded, and marked to market daily.
- No-arbitrage gives $F(t,T)=S_t e^{r(T-t)}$ for a non-dividend-paying asset under continuous compounding.
- Long forwards and futures benefit when the underlying or futures price rises.
- Short forwards and futures benefit when the underlying or futures price falls.
- Futures require initial margin and maintenance margin.
- Accrued profit tracks the accumulated value of daily settlement cash flows.
- Under the course assumptions, forward and futures delivery prices are treated as equal.
