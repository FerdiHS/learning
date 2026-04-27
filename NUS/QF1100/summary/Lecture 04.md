# Lecture 04

This lecture relaxes the earlier assumption that interest rates are
constant. Instead, interest rates may vary with time to maturity.

Unless stated otherwise, the lecture uses yearly compounding, so rates are
quoted per year and a maturity of $t$ means $t$ years.

## Variable Definitions

$$
0 = \text{today, or the present time}
$$

$$
t = \text{a future time or maturity, measured in years}
$$

$$
j,k = \text{future times with } k>j>0
$$

$$
s_t = \text{spot rate for maturity } t
$$

$$
s_1,s_2,\ldots,s_n = \text{spot rates for maturities } 1,2,\ldots,n
$$

$$
f_{j,k} = \text{forward rate agreed today for the period from time } j \text{ to time } k
$$

$$
f_{0,t} = \text{forward rate starting today and ending at time } t,\text{ which equals } s_t
$$

$$
C_t = \text{cash flow paid at time } t
$$

$$
C_1,C_2,\ldots,C_n = \text{cash flows paid at times } 1,2,\ldots,n
$$

$$
\operatorname{PV} = \text{present value}
$$

$$
(1+s_t)^t = \text{accumulation factor from time } 0 \text{ to time } t
$$

$$
P = \text{price of a bond or present value of a cash flow}
$$

$$
R = \text{redemption or face value paid at maturity}
$$

$$
n = \text{number of coupon or discounting periods up to maturity}
$$

$$
P_n = \text{price of an } n\text{-period coupon-paying bond used in bootstrapping}
$$

$$
F = \text{face value of a coupon-paying bond}
$$

$$
c = \text{nominal annual coupon rate}
$$

$$
m = \text{number of coupon payments per year}
$$

$$
\frac{cF}{m} = \text{coupon payment each period for a coupon-paying bond}
$$

$$
\frac{cF}{m}+R = \text{final cash flow consisting of the last coupon plus redemption value}
$$

When payments occur more frequently than once per year, the cash-flow dates
and spot-rate subscripts are adjusted to the actual times, such as
$1/m,2/m,\ldots,n/m$.

The term structure of interest rates is the collection of spot rates for
different maturities, such as:

$$
\{s_1,s_2,s_3,\ldots\}
$$

## Spot Rates

A spot rate, or zero rate, is the annual interest rate for money held from
today, time $0$, until a future time $t$.

The spot rate for maturity $t$ is denoted:

$$
s_t
$$

Under the yearly compounding convention used in the lecture, $s_t$ is
defined so that:

$$
(1+s_t)^t
$$

is the accumulation factor for holding money from time $0$ to time $t$.

Equivalently, if a cash flow $C_t$ is paid at time $t$, its present value is:

$$
\operatorname{PV}
= \frac{C_t}{(1+s_t)^t}
$$

The collection of spot rates defines the term structure of interest rates.

<details>
<summary>Intuition / proof</summary>

The spot rate $s_t$ is the annualized rate implied for maturity $t$.

If one unit of money grows for $t$ years at the spot rate for maturity
$t$, then under yearly compounding it becomes:

$\displaystyle (1+s_t)^t$

So if a future payment $C_t$ is due at time $t$, the amount today that
grows to that payment must be:

$\displaystyle \frac{C_t}{(1+s_t)^t}$

This is why spot rates are fundamental: each maturity has its own
discount rate, rather than forcing all cash flows to use one constant
yield.

In other words, $s_t$ is not just "the rate at time $t$". It is the rate
that links values today to values at maturity $t$.

</details>

## Spot Rate from a Zero Coupon Bond

The spot rate is the yield to maturity of a zero coupon bond with that
maturity.

For an $n$-period zero coupon bond with price $P$ and redemption value
$R$:

$$
P = \frac{R}{(1+s_n)^n}
$$

Solving for $s_n$:

$$
s_n
= \left(\frac{R}{P}\right)^{1/n} - 1
$$

<details>
<summary>Intuition / proof</summary>

A zero coupon bond is the cleanest possible instrument for finding a spot
rate because it has only one cash flow.

Its price must equal the present value of the redemption payment:

$\displaystyle P=\frac{R}{(1+s_n)^n}$

Since $P$ and $R$ are known from the bond, the only unknown is $s_n$.
Rearranging gives:

$\displaystyle (1+s_n)^n = \frac{R}{P}$

and then:

$\displaystyle s_n = \left(\frac{R}{P}\right)^{1/n}-1$

This is why zero coupon bonds "isolate" spot rates directly.

Coupon-paying bonds do not isolate one spot rate so cleanly, because their
prices mix together cash flows from several dates.

</details>

## Forward Rates

A forward rate is an interest rate agreed today for a future time period.

The forward rate from time $j$ to time $k$, where $k > j > 0$, is
denoted:

$$
f_{j,k}
$$

By convention:

$$
f_{0,t} = s_t
$$

So a spot rate can be viewed as a forward rate that starts today.

<details>
<summary>Intuition / proof</summary>

A spot rate starts now. A forward rate starts later, but the rate is fixed
today.

So:

- $s_t$ prices borrowing or lending from time $0$ to time $t$.
- $f_{j,k}$ prices borrowing or lending from time $j$ to time $k$,
  agreed at time $0$.

The convention

$\displaystyle f_{0,t}=s_t$

simply says a spot rate is a special case of a forward rate whose starting
date is today.

So forward rates are the building blocks for future sub-periods, while
spot rates cover the whole period from now to a future date.

</details>

## Spot and Forward Rate Relationship

The key no-arbitrage relationship is:

$$
(1+s_k)^k
= (1+s_j)^j(1+f_{j,k})^{k-j}
$$

for:

$$
k > j > 0
$$

Solving for the forward rate:

$$
f_{j,k}
=
\left[
\frac{(1+s_k)^k}{(1+s_j)^j}
\right]^{1/(k-j)}
- 1
$$

For the one-period forward rate from year $1$ to year $2$:

$$
(1+s_2)^2 = (1+s_1)(1+f_{1,2})
$$

Thus:

$$
f_{1,2}
= \frac{(1+s_2)^2}{1+s_1} - 1
$$

<details>
<summary>Intuition / proof</summary>

The no-arbitrage idea is that two investment strategies with the same
start date and end date should produce the same accumulated value.

To reach time $k$, you can:

1. invest directly from time $0$ to time $k$ at spot rate $s_k$, or
2. invest from time $0$ to time $j$ at spot rate $s_j$, then reinvest
   from time $j$ to time $k$ at forward rate $f_{j,k}$.

Strategy 1 gives:

$\displaystyle (1+s_k)^k$

Strategy 2 gives:

$\displaystyle (1+s_j)^j(1+f_{j,k})^{k-j}$

No arbitrage forces these to be equal, which gives the forward-rate
formula after rearranging.

To solve explicitly:

$\displaystyle (1+f_{j,k})^{k-j} = \frac{(1+s_k)^k}{(1+s_j)^j}$

so:

$\displaystyle 1+f_{j,k} = \left[ \frac{(1+s_k)^k}{(1+s_j)^j} \right]^{1/(k-j)}$

and subtracting $1$ gives the forward-rate formula.

</details>

## Pricing Cash Flows with Spot Rates

When spot rates are known, each cash flow is discounted using the spot
rate matching its payment time.

For cash flows $C_1,C_2,\ldots,C_n$:

$$
P
= \frac{C_1}{1+s_1}
+ \frac{C_2}{(1+s_2)^2}
+ \cdots
+ \frac{C_n}{(1+s_n)^n}
$$

This is different from using one constant yield for every cash flow.

<details>
<summary>Intuition / proof</summary>

Each cash flow has its own maturity, so each cash flow should use the
spot rate for that maturity.

The payment at time $1$ is discounted by $s_1$, the payment at time $2$
is discounted by $s_2$, and so on:

$\displaystyle \frac{C_1}{1+s_1},\quad \frac{C_2}{(1+s_2)^2},\quad \ldots,\quad \frac{C_n}{(1+s_n)^n}$

Then the total price is the sum of these present values.

This is more accurate than discounting every payment at one constant yield
when the market term structure is not flat.

The law of one price is the reason this works: the price of the whole cash
flow stream must equal the sum of the prices of its pieces.

</details>

## Bootstrap Method

Prices of zero coupon bonds for every maturity may not be available.
When that happens, spot rates can be solved from coupon-paying bonds
using the bootstrap method.

The bootstrap method solves spot rates one maturity at a time.

Suppose:

$$
s_1,s_2,\ldots,s_{n-1}
$$

have already been determined. For the yearly-coupon case, consider an
$n$-period coupon-paying bond with price $P_n$ and cash flows:

$$
(-P_n,\ cF/m,\ cF/m,\ \ldots,\ cF/m + R)
$$

When coupons are annual, we have $m=1$, so the pricing equation is:

$$
P_n
= \frac{cF/m}{1+s_1}
+ \frac{cF/m}{(1+s_2)^2}
+ \cdots
+ \frac{cF/m}{(1+s_{n-1})^{n-1}}
+ \frac{cF/m+R}{(1+s_n)^n}
$$

Since $s_1,\ldots,s_{n-1}$ are already known, the only unknown is $s_n$.
So $s_n$ can be solved from the equation.

This is why earlier spot rates must be determined first: later
coupon-paying bonds include earlier cash flows that must be discounted
using the already-known spot rates.

If coupons occur more frequently than once per year, the same idea still
works, but the matching payment dates must be used. For example, with
coupon frequency $m$, the payment times are:

$$
\frac{1}{m},\frac{2}{m},\ldots,\frac{n}{m}
$$

so the corresponding spot rates are:

$$
s_{1/m},s_{2/m},\ldots,s_{n/m}.
$$

In that general case, each cash flow must be discounted using the spot
rate matching its actual payment date.

### Bootstrap Procedure

1. Use a 1-period bond to find $s_1$.
2. Use a 2-period bond and the known $s_1$ to find $s_2$.
3. Use a 3-period bond and the known $s_1,s_2$ to find $s_3$.
4. Continue until the required spot rates are found.

<details>
<summary>Intuition / proof</summary>

Bootstrapping works because once the earlier spot rates are known, the
earlier coupon cash flows of a later bond can already be priced.

In the $n$-period bond formula, all terms except the last one involve
spot rates that were solved earlier:

$\displaystyle s_1,s_2,\ldots,s_{n-1}$

So after subtracting the present value of the earlier coupon payments from
the bond price, the remaining unknown part is the final term involving
$s_n$.

That is why the method must proceed sequentially: first $s_1$, then
$s_2$, then $s_3$, and so on.

More explicitly, once the earlier terms are moved to the left-hand side,
we get:

$\displaystyle P_n - \frac{cF/m}{1+s_1} - \frac{cF/m}{(1+s_2)^2} - \cdots - \frac{cF/m}{(1+s_{n-1})^{n-1}} = \frac{cF/m+R}{(1+s_n)^n}$

and now the only unknown in the equation is $s_n$.

If coupons are more frequent than annual, the same rearrangement works,
but the terms must use the matching dates
$\frac{1}{m},\frac{2}{m},\ldots,\frac{n}{m}$ rather than the annual dates
$1,2,\ldots,n$.

</details>

## Key Takeaways

- A spot rate $s_t$ is the rate from today to time $t$.
- A forward rate $f_{j,k}$ is the rate from future time $j$ to future time $k$, agreed today.
- Spot and forward rates are connected by a no-arbitrage relationship.
- Each cash flow should be discounted using the spot rate matching its payment time.
- Zero coupon bonds isolate spot rates directly because they have only one cash flow.
- Bootstrapping solves spot rates sequentially: first $s_1$, then $s_2$, then $s_3$, and so on.
