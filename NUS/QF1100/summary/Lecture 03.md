# Lecture 03

This lecture explains how bonds are priced from their future cash flows
and how duration summarizes the timing of those cash flows.

Unless stated otherwise, time is measured in years, while payment indices
count coupon periods.

## Variable Definitions

$$
P = \text{current price of the bond}
$$

$$
F = \text{face value, or par value, of the bond}
$$

$$
R = \text{redemption value, or maturity value, paid at maturity}
$$

$$
c = \text{nominal annual coupon rate}
$$

$$
m = \text{number of coupon payments per year}
$$

$$
N = \text{term of the bond in years}
$$

$$
n = \text{total number of payment or discounting periods to maturity}
$$

$$
\lambda = \text{nominal annual bond yield}
$$

$$
\frac{\lambda}{m} = \text{yield per coupon period}
$$

$$
\frac{cF}{m} = \text{coupon payment made each coupon period}
$$

$$
i = \text{payment index}
$$

$$
i/m = \text{time in years of the } i\text{th coupon payment}
$$

$$
k = \text{number of coupon payments already made}
$$

$$
P_k = \text{bond price immediately after the } k\text{th coupon payment}
$$

$$
K = \text{present value of the face value in the Makeham formula}
$$

$$
C = \text{general cash flow stream}
$$

$$
\operatorname{PV}(C) = \text{present value of cash flow stream } C
$$

$$
\operatorname{PV}(\{(c_i,t_i)\}) = \text{present value of the single cash flow } c_i \text{ at time } t_i
$$

$$
r = \text{generic per-period discount rate used in annuity notation}
$$

$$
a_{\overline{n}|\,r} = \text{present value of an ordinary annuity of } 1 \text{ per period for } n \text{ periods at rate } r
$$

$$
c_i = \text{amount of the } i\text{th cash flow}
$$

$$
t_i = \text{time of the } i\text{th cash flow}
$$

$$
q = \text{number of cash flows in a general cash flow stream}
$$

$$
D = \text{Macaulay duration}
$$

$$
w_i = \text{present-value weight of the } i\text{th cash flow}
$$

$$
T = \text{maturity time of a zero coupon bond when written separately}
$$

$$
\mu = \frac{\lambda}{m} = \text{per-period yield}
$$

$$
\gamma = \frac{c}{m} = \text{coupon rate per payment period}
$$

In most examples in this lecture, the bond redeems at par, so $R=F$.

## Bond Basic Notation

A bond is a contract where the issuer borrows money from investors and
promises future payments.

If the bond lasts $N$ years and pays coupons $m$ times per year, then:

$$
n = Nm
$$

Each coupon payment is:

$$
\frac{cF}{m}
$$

The per-period yield used for discounting is:

$$
\frac{\lambda}{m}
$$

Usually, unless stated otherwise:

$$
R = F
$$

<details>
<summary>Intuition / proof</summary>

The coupon rate $c$ is quoted annually, but coupons are paid only $m$
times per year, so each coupon is the annual coupon amount $cF$ split
across $m$ payments:

$\frac{cF}{m}$

If the bond lasts $N$ years and pays $m$ times per year, then there are
$Nm$ payment dates, which gives:

$n=Nm$

The nominal yield $\lambda$ is also quoted annually, so the matching
per-period discount rate is:

$\frac{\lambda}{m}$

This is why coupon bonds are naturally priced period by period: both the
coupon cash flows and the discounting are expressed on the same payment
schedule.

</details>

## Valuing Bonds: Initial Price Formula

The price of a bond is the present value of all future coupon payments
plus the present value of the redemption value.

For a coupon bond:

$$
P
= \sum_{i=1}^{n}
\frac{cF/m}{(1+\lambda/m)^i}
+ \frac{R}{(1+\lambda/m)^n}
$$

Interpretation:

- $\frac{cF}{m}$ is each coupon payment.
- $R$ is the redemption value paid at maturity.
- $\frac{\lambda}{m}$ is the per-period yield.
- $n$ is the total number of coupon periods to maturity.

If $R = F$, then:

$$
P
= \sum_{i=1}^{n}
\frac{cF/m}{(1+\lambda/m)^i}
+ \frac{F}{(1+\lambda/m)^n}
$$

Using the ordinary annuity formula:

$$
P
= F\left[
\frac{1}{(1+\lambda/m)^n}
+ \frac{c}{\lambda}
\left(1-\frac{1}{(1+\lambda/m)^n}\right)
\right]
$$

Equivalently:

$$
P
= F
+ F\left(\frac{c-\lambda}{\lambda}\right)
\left[
1-\frac{1}{(1+\lambda/m)^n}
\right]
$$

<details>
<summary>Intuition / proof</summary>

Bond pricing is just present value:

Price = PV of coupons + PV of redemption value.

Each coupon equals $\frac{cF}{m}$ and arrives one period apart, so we
discount the $i$th coupon by $(1+\lambda/m)^i$. The redemption value is
paid at the final date, so it is discounted by $(1+\lambda/m)^n$.

When $R=F$, the coupon part becomes an ordinary annuity, which gives the
compact annuity form above. The last equivalent form is especially useful
for intuition: it shows that price differs from par because the coupon
rate $c$ may differ from the yield $\lambda$.

More explicitly, the coupon leg is:

$\sum_{i=1}^{n}\frac{cF/m}{(1+\lambda/m)^i} = \frac{cF}{m}a_{\overline{n}|\,\lambda/m}$

and the redemption leg is:

$\frac{R}{(1+\lambda/m)^n}$

so the full price is just the sum of those two pieces.

</details>

## Makeham Formula

Let:

$$
K = \frac{F}{(1+\lambda/m)^n}
$$

Here $K$ is the present value of the face value paid at maturity.

The Makeham formula is:

$$
P = K + \frac{c}{\lambda}(F-K)
$$

This is another way to write the bond price when $R = F$.

<details>
<summary>Intuition / proof</summary>

The term $K$ isolates the discounted redemption payment. Then $F-K$
captures the gap between the face value and its present value, and the
factor $\frac{c}{\lambda}$ adjusts that gap according to how large the
coupon rate is relative to the yield.

So the Makeham formula is just a rearranged bond-pricing formula, but it
makes the roles of redemption and coupons easier to separate.

Starting from

$P = F[ \frac{1}{(1+\lambda/m)^n} + \frac{c}{\lambda} (1-\frac{1}{(1+\lambda/m)^n}) ],$

define

$K=\frac{F}{(1+\lambda/m)^n}.$

Then

$F-K = F(1-\frac{1}{(1+\lambda/m)^n}),$

so the formula becomes

$P = K + \frac{c}{\lambda}(F-K).$

</details>

## Price Immediately After $k$ Payments

Let $P_k$ be the bond price immediately after the $k$th coupon payment
has been made.

At that point, there are $n-k$ coupon payments remaining, plus the
redemption value at maturity. Hence:

$$
P_k
= \sum_{i=1}^{n-k}
\frac{cF/m}{(1+\lambda/m)^i}
+ \frac{R}{(1+\lambda/m)^{n-k}}
$$

If $R = F$, this becomes:

$$
P_k
= \sum_{i=1}^{n-k}
\frac{cF/m}{(1+\lambda/m)^i}
+ \frac{F}{(1+\lambda/m)^{n-k}}
$$

The lecture also gives the recursive relation:

$$
P_{k+1}
= P_k\left(1+\frac{\lambda}{m}\right)
- \frac{cF}{m}
$$

Interpretation: after one period, the current price grows by the
per-period yield, then the coupon payment is paid out.

<details>
<summary>Intuition / proof</summary>

Immediately after the $k$th coupon is paid, all earlier cash flows are
gone. So the bond is worth only the present value of the remaining
coupons and the final redemption payment.

The recursive formula says:

next ex-coupon price = current ex-coupon price grown one period - coupon
just paid.

So the bond first earns one period of yield, then loses value when the
next coupon is detached and paid to the bondholder.

Algebraically, take the price formula for $P_k$, multiply it by
$1+\lambda/m$, and compare it with the remaining-cash-flow formula for
$P_{k+1}$. The only extra term is the coupon paid at the next date, which
is why we subtract $\frac{cF}{m}$.

</details>

## Premium, Par, and Discount Bonds

A bond is purchased at a premium if:

$$
P > F
$$

A bond is purchased at par if:

$$
P = F
$$

A bond is purchased at a discount if:

$$
P < F
$$

When $R = F$, the criteria are:

$$
P > F \iff c > \lambda
$$

$$
P = F \iff c = \lambda
$$

$$
P < F \iff c < \lambda
$$

<details>
<summary>Intuition / proof</summary>

If the coupon rate $c$ is higher than the yield $\lambda$, the bond pays
relatively attractive coupons, so investors are willing to pay more than
par:

$P>F$

If the coupon rate exactly matches the yield, the bond is fairly priced at
par:

$P=F$

If the coupon rate is below the yield, the coupons are unattractive
relative to the market, so the price must fall below par:

$P<F$

This also follows directly from the equivalent form

$P = F + F(\frac{c-\lambda}{\lambda}) [ 1-\frac{1}{(1+\lambda/m)^n} ].$

The bracketed term is positive, so the sign of $P-F$ is determined by the
sign of $c-\lambda$.

</details>

## Zero Coupon Bond

A zero coupon bond pays no coupons.

The only cash flow is the redemption value $R$ at maturity. If maturity is
at time $N$ and the bond discounts once per year, so $m=1$, then the
nominal annual yield $\lambda$ is also the per-year yield, and:

$$
P = \frac{R}{(1+\lambda)^N}
$$

If the yield is nominal convertible $m$ times per year and there are $n$
periods, the matching form is:

$$
P = \frac{R}{(1+\lambda/m)^n}
$$

For a zero coupon bond, all value comes from the final redemption
payment.

<details>
<summary>Intuition / proof</summary>

A zero coupon bond has only one future cash flow, so its price is just the
present value of that single payment:

$P$ is the present value of $R$.

There are no interim coupons, so the entire return comes from buying the
bond now and receiving the redemption value at maturity.

This is why zero coupon bonds are the simplest building blocks in term
structure work: there is exactly one payment date and no intermediate cash
flows to separate out.

</details>

## Perpetual Bond

A perpetual bond, or consol, is a bond that never matures.

It pays coupons forever. Since each coupon is $\frac{cF}{m}$ and the
per-period yield is $\frac{\lambda}{m}$:

$$
P
= \frac{cF/m}{\lambda/m}
= \frac{cF}{\lambda}
$$

So:

$$
P = \frac{cF}{\lambda}
$$

<details>
<summary>Intuition / proof</summary>

A perpetual bond is just a perpetuity. The payment each period is
$\frac{cF}{m}$ and the discount rate per period is $\frac{\lambda}{m}$,
so the perpetuity formula gives:

So the perpetuity rule "payment divided by rate" becomes
$P = \frac{cF/m}{\lambda/m}$.

The $m$ cancels, leaving:

$P=\frac{cF}{\lambda}$

Intuitively, the price is large when the coupon stream is large and small
when the market yield is high, exactly like any perpetuity.

</details>

## Macaulay Duration

Macaulay duration is the weighted average time of the cash flows, using
present-value weights.

The weights are based on the present value of each cash payment.

For a general cash flow stream with $q$ cash flows:

$$
C = \{(c_i,t_i): i = 1,2,\ldots,q\}
$$

the Macaulay duration $D$ is:

$$
D
= \frac{
\sum_{i=1}^q t_i \operatorname{PV}(\{(c_i,t_i)\})
}{
\operatorname{PV}(C)
}
$$

Equivalently:

$$
D = \sum_{i=1}^q w_i t_i
$$

where:

$$
w_i
= \frac{
\operatorname{PV}(\{(c_i,t_i)\})
}{
\operatorname{PV}(C)
}
$$

If all cash flows are non-negative, then:

$$
t_1 \le D \le t_q
$$

For a zero coupon bond with maturity $T$:

$$
D = T
$$

Macaulay duration is also related to how sensitive a bond price is to
changes in interest rates.

<details>
<summary>Intuition / proof</summary>

Duration is a time average, but not an ordinary average. Each payment time
is weighted by how important that cash flow is in present-value terms.

Since the weights $w_i$ are present-value proportions, they satisfy:

$w_i \ge 0$ and $\sum_{i=1}^q w_i = 1$

So duration is a weighted average of payment times, which explains why it
must lie between the earliest and latest payment times when all cash flows
are non-negative.

For a zero coupon bond, there is only one cash flow at maturity, so all
the weight is on that one date and:

$D=T$

The connection with interest-rate sensitivity is intuitive: if more of the
bond's value comes from cash flows far in the future, then changes in
discount rates affect the present value more strongly.

</details>

## Bond Duration

For a bond with $n$ coupon payments, coupon frequency $m$, nominal
coupon rate $c$, nominal yield $\lambda$, and redemption value $R$, the
bond cash inflows are coupon payments plus the redemption value.

The bond price is:

$$
P
= \sum_{i=1}^{n}
\frac{cF/m}{(1+\lambda/m)^i}
+ \frac{R}{(1+\lambda/m)^n}
$$

The bond duration is:

$$
D
= \frac{1}{P}
\left[
\sum_{i=1}^{n}
\frac{(i/m)(cF/m)}{(1+\lambda/m)^i}
+ \frac{(n/m)R}{(1+\lambda/m)^n}
\right]
$$

If $R = F$, then the face value cancels out:

$$
D
=
\frac{
\sum_{i=1}^{n}
\frac{(i/m)(c/m)}{(1+\lambda/m)^i}
+ \frac{n/m}{(1+\lambda/m)^n}
}{
\sum_{i=1}^{n}
\frac{c/m}{(1+\lambda/m)^i}
+ \frac{1}{(1+\lambda/m)^n}
}
$$

Let:

$$
\mu = \frac{\lambda}{m}, \qquad \gamma = \frac{c}{m}
$$

Then:

$$
D
=
\frac{
\sum_{i=1}^{n}
\frac{(i/m)\gamma}{(1+\mu)^i}
+ \frac{n/m}{(1+\mu)^n}
}{
\sum_{i=1}^{n}
\frac{\gamma}{(1+\mu)^i}
+ \frac{1}{(1+\mu)^n}
}
$$

After simplification:

$$
D
=
\frac{1+\mu}{m\mu}
-
\frac{1+\mu+n(\gamma-\mu)}
{m\gamma\left[(1+\mu)^n-1\right]+m\mu}
$$

If the bond is priced at par, then $P = F$ and $\mu = \gamma$. In this
case:

$$
D
=
\frac{1+\mu}{m\mu}
\left(
1-\frac{1}{(1+\mu)^n}
\right)
$$

<details>
<summary>Intuition / proof</summary>

This is just the Macaulay-duration formula applied to the bond's own cash
flow stream.

Each coupon at time $\frac{i}{m}$ is weighted by its present value, and
the redemption value at time $\frac{n}{m}$ is weighted the same way.

So duration is larger when more of the bond's value comes from later cash
flows. That is why long maturities and low coupons tend to produce higher
duration.

When the bond is priced at par, the formula simplifies because the coupon
rate and the yield match.

The factor $\frac{1}{P}$ appears because the weights must add to $1$.
Equivalently, $w_i$ is the present value of cash flow $i$ divided by the
total bond price $P$.

So bond duration is not a new concept; it is the same weighted-average
idea as Macaulay duration, specialized to coupon-bond cash flows.

</details>

## Duration for Perpetual Bonds

For a perpetual bond, take $n \to \infty$ in the duration formula.

The duration is:

$$
D
= \frac{1+\mu}{m\mu}
$$

Since $\mu = \frac{\lambda}{m}$:

$$
D
= \frac{1+\lambda/m}{\lambda}
$$

where $\lambda$ is the nominal annual yield.

<details>
<summary>Intuition / proof</summary>

Even though a perpetual bond pays forever, discounting makes far-future
coupons matter less and less.

So the weighted-average payment time stays finite, and the perpetual
duration is the limiting value of the finite-bond duration formula as
$n\to\infty$.

This is a good reminder that "infinite maturity" does not force duration
to be infinite. Present-value weights shrink for distant coupons, so the
weighted average can still converge.

</details>

## Key Takeaways

- A bond price is the present value of coupon payments plus redemption value.
- The coupon rate $c$ determines the cash payments; the yield $\lambda$ discounts those payments.
- If $R=F$, then $P>F$ iff $c>\lambda$, $P=F$ iff $c=\lambda$, and $P<F$ iff $c<\lambda$.
- $P_k$ is the price immediately after the $k$th coupon payment.
- A zero coupon bond has duration equal to its maturity.
- Macaulay duration is a present-value-weighted average payment time.
- For a fixed yield convention, higher duration generally means greater price sensitivity to yield changes.
