# Lecture 01 - Theory of Interest

This lecture introduces how money grows over time under different interest
conventions.

## Variable Definitions

Unless stated otherwise, time is measured in years.

$$
P = \text{principal, or initial amount invested}
$$

$$
A(t) = \text{accumulated amount at time } t
$$

$$
a(t) = \text{accumulation function for one unit of money}
$$

$$
t = \text{time elapsed}
$$

$$
r = \text{interest rate under the stated compounding convention}
$$

$$
r^{(p)} = \text{nominal annual interest rate compounded } p \text{ times per year}
$$

$$
p = \text{number of compounding periods per year}
$$

$$
r_e = \text{effective annual interest rate}
$$

$$
R^{(q)} = \text{another nominal annual rate compounded } q \text{ times per year}
$$

$$
q = \text{another compounding frequency}
$$

$$
x = \text{dummy variable used for compounding frequency in limits or proofs}
$$

$$
e = \text{Euler's number, used for continuous compounding}
$$

$$
\delta(t) = \text{force of interest at time } t
$$

$$
\delta = \text{constant force of interest}
$$

$$
a'(t) = \text{derivative of } a(t) \text{ with respect to } t
$$

$$
a(s,t) = \text{accumulation factor from time } s \text{ to time } t
$$

The variable $s$ is a starting time in $a(s,t)$. The variable $u$ is a
dummy time variable used inside integrals.
The variables $t_0,t_1,\ldots,t_n$ are ordered time points in the
consistency principle.

## Core Accumulation

If $P$ is the principal and $a(t)$ is the accumulation function:

$$
A(t) = P a(t), \qquad a(0) = 1
$$

<details>
<summary>Intuition / proof</summary>

The accumulation function $a(t)$ tells us what one unit of money becomes
after time $t$.

So if one unit becomes $a(t)$, then $P$ units become:

$$
P \times a(t)
$$

Hence:

$$
A(t) = Pa(t)
$$

Also, at time $0$, no time has passed, so one unit is still one unit:

$$
a(0)=1
$$

</details>

## Simple Interest

Simple interest is earned only on the principal.

$$
a(t) = 1 + rt
$$

where $r$ is the annual interest rate.

<details>
<summary>Intuition / proof</summary>

Under simple interest, interest does not earn further interest.

After $t$ years, the interest earned on principal $P$ is:

$$
Prt
$$

So the accumulated amount is:

$$
A(t) = P + Prt = P(1+rt)
$$

Since:

$$
A(t)=Pa(t)
$$

we get:

$$
a(t)=1+rt
$$

Simple interest grows linearly because the interest is always based only
on the original principal.

</details>

## Compound Interest

Interest earned after one period is reinvested.

$$
a(t) = (1+r)^t
$$

Unless stated otherwise, QF1100 assumes compound interest.

<details>
<summary>Intuition / proof</summary>

Under compound interest, each period multiplies the current amount by:

$$
1+r
$$

After one period:

$$
a(1)=1+r
$$

After two periods:

$$
a(2)=(1+r)(1+r)=(1+r)^2
$$

After $t$ years, when $r$ is the effective annual rate:

$$
a(t)=(1+r)^t
$$

For positive $r$, compound interest matches simple interest at $t=1$ and
exceeds it for $t>1$ because interest is added back into the balance and
earns more interest later. For $0<t<1$, under $a(t)=(1+r)^t$, compound
accumulation is lower than simple accumulation.

</details>

## Nominal Rate Compounded $p$ Times Per Year

If the nominal annual rate is $r^{(p)}$ and interest is compounded $p$
times per year:

$$
\text{periodic rate} = \frac{r^{(p)}}{p}
$$

The accumulation formulas are:

$$
\begin{aligned}
a\left(\frac{1}{p}\right) &= 1 + \frac{r^{(p)}}{p}, \\
a(1) &= \left(1 + \frac{r^{(p)}}{p}\right)^p, \\
a(t) &= \left(1 + \frac{r^{(p)}}{p}\right)^{pt}.
\end{aligned}
$$

Common values:

- $p = 2$: semi-annual compounding
- $p = 4$: quarterly compounding
- $p = 12$: monthly compounding

<details>
<summary>Intuition / proof</summary>

The nominal annual rate $r^{(p)}$ is split evenly across $p$ compounding
periods in a year.

So the interest rate per compounding period is:

$$
\frac{r^{(p)}}{p}
$$

Each compounding period multiplies money by:

$$
1+\frac{r^{(p)}}{p}
$$

There are $p$ compounding periods in one year, so:

$$
a(1)=\left(1+\frac{r^{(p)}}{p}\right)^p
$$

There are $pt$ compounding periods in $t$ years, so:

$$
a(t)=\left(1+\frac{r^{(p)}}{p}\right)^{pt}
$$

</details>

## Effective Annual Interest Rate

The effective annual interest rate $r_e$ is the actual one-year growth
rate.

$$
1 + r_e = \left(1 + \frac{r^{(p)}}{p}\right)^p
$$

Equivalently:

$$
r_e = \left(1 + \frac{r^{(p)}}{p}\right)^p - 1
$$

For positive rates and integer $p \ge 1$:

$$
r_e \ge r^{(p)}
$$

<details>
<summary>Intuition / proof</summary>

The effective annual rate asks: what single annual rate gives the same
one-year growth?

For a nominal rate compounded $p$ times per year, one-year accumulation
is:

$$
\left(1+\frac{r^{(p)}}{p}\right)^p
$$

For an effective annual rate, one-year accumulation is:

$$
1+r_e
$$

Equating the two gives:

$$
1+r_e=\left(1+\frac{r^{(p)}}{p}\right)^p
$$

For $r^{(p)} \ge 0$, the inequality $r_e \ge r^{(p)}$ comes from the fact
that compounding adds interest on interest.

Algebraically, let:

$$
x=\frac{r^{(p)}}{p}
$$

Then:

$$
(1+x)^p \ge 1+px
$$

So:

$$
\left(1+\frac{r^{(p)}}{p}\right)^p
\ge
1+r^{(p)}
$$

Subtracting $1$ gives:

$$
r_e \ge r^{(p)}
$$

</details>

## Equivalent Nominal Rates

Two nominal rates are equivalent if they produce the same one-year
accumulation.

$$
\left(1 + \frac{r^{(p)}}{p}\right)^p
=
\left(1 + \frac{R^{(q)}}{q}\right)^q
$$

Use this when converting between compounding frequencies.

<details>
<summary>Intuition / proof</summary>

Equivalent rates must make the same amount of money after one year.

The nominal rate $r^{(p)}$ compounded $p$ times per year gives:

$$
\left(1+\frac{r^{(p)}}{p}\right)^p
$$

The nominal rate $R^{(q)}$ compounded $q$ times per year gives:

$$
\left(1+\frac{R^{(q)}}{q}\right)^q
$$

If these two one-year accumulation factors are equal, then both rates are
financially equivalent over one year.

</details>

## Continuous Compounding

As the compounding frequency tends to infinity:

$$
\lim_{x \to \infty} \left(1 + \frac{r}{x}\right)^x = e^r
$$

For continuous compounding:

$$
a(1) = e^r, \qquad a(t) = e^{rt}
$$

Here $r$ is the continuously compounded rate.

Equivalence with a nominal rate compounded $p$ times per year:

$$
\left(1 + \frac{r^{(p)}}{p}\right)^p = e^r = 1 + r_e
$$

<details>
<summary>Intuition / proof</summary>

Continuous compounding is the limiting case of compounding more and more
frequently.

With $x$ compounding periods per year, the one-year accumulation is:

$$
\left(1+\frac{r}{x}\right)^x
$$

As $x$ becomes infinitely large, the compounding periods become
infinitesimally small. The limit is:

$$
e^r
$$

For $t$ years, the same logic gives:

$$
a(t)=e^{rt}
$$

This is why continuous compounding and a constant force of interest use
the same exponential form.

</details>

## Force of Interest

The force of interest is the instantaneous relative growth rate.

$$
\delta(t) = \frac{a'(t)}{a(t)}
= \frac{d}{dt}\left[\ln a(t)\right]
$$

If $\delta(t) = \delta$ is constant, then:

$$
a(t) = e^{\delta t}
$$

So a constant force of interest is the same idea as a continuously
compounded rate.

<details>
<summary>Intuition / proof</summary>

The derivative $a'(t)$ measures the instantaneous rate at which the
accumulation function is changing.

Dividing by $a(t)$ turns this into a relative growth rate:

$$
\frac{a'(t)}{a(t)}
$$

That relative growth rate is the force of interest:

$$
\delta(t)=\frac{a'(t)}{a(t)}
$$

Also:

$$
\frac{d}{dt}\ln a(t)=\frac{a'(t)}{a(t)}
$$

If the force is constant, then:

$$
\frac{d}{dt}\ln a(t)=\delta
$$

Integrating from $0$ to $t$ gives:

$$
\ln a(t)-\ln a(0)=\delta t
$$

Since $a(0)=1$, we have $\ln a(0)=0$, so:

$$
\ln a(t)=\delta t
$$

Therefore:

$$
a(t)=e^{\delta t}
$$

</details>

## Recovering Accumulation from Force of Interest

If the force of interest is known:

$$
a(t) = \exp\left(\int_0^t \delta(u)\,du\right)
$$

For accumulation from time $s$ to time $t$:

$$
a(s,t) = \frac{a(t)}{a(s)}
= \exp\left(\int_s^t \delta(u)\,du\right)
$$

<details>
<summary>Intuition / proof</summary>

From the definition of force of interest:

$$
\delta(t)=\frac{d}{dt}\ln a(t)
$$

Integrate both sides from $0$ to $t$:

$$
\int_0^t \delta(u)\,du
=
\ln a(t)-\ln a(0)
$$

Since $a(0)=1$, $\ln a(0)=0$, so:

$$
\ln a(t)=\int_0^t \delta(u)\,du
$$

Exponentiating both sides gives:

$$
a(t)=\exp\left(\int_0^t \delta(u)\,du\right)
$$

For accumulation from $s$ to $t$, divide the total accumulation to time
$t$ by the total accumulation to time $s$:

$$
a(s,t)=\frac{a(t)}{a(s)}
$$

Using the force-of-interest formula:

$$
\frac{a(t)}{a(s)}
=
\frac{
\exp\left(\int_0^t \delta(u)\,du\right)
}{
\exp\left(\int_0^s \delta(u)\,du\right)
}
=
\exp\left(\int_s^t \delta(u)\,du\right)
$$

</details>

## Consistency Principle

For $t_0 < t_1 < t_2 < \cdots < t_n$:

$$
a(t_0,t_n)
= a(t_0,t_1)a(t_1,t_2)\cdots a(t_{n-1},t_n)
$$

<details>
<summary>Intuition / proof</summary>

Accumulating from $t_0$ to $t_n$ should be the same as accumulating in
smaller consecutive steps.

Using:

$$
a(s,t)=\frac{a(t)}{a(s)}
$$

we get:

$$
a(t_0,t_1)a(t_1,t_2)\cdots a(t_{n-1},t_n)
=
\frac{a(t_1)}{a(t_0)}
\frac{a(t_2)}{a(t_1)}
\cdots
\frac{a(t_n)}{a(t_{n-1})}
$$

All middle terms cancel, leaving:

$$
\frac{a(t_n)}{a(t_0)}
=a(t_0,t_n)
$$

So the consistency principle is just the idea that growth over a long
interval can be split into consecutive shorter intervals.

</details>

## Must Remember

- Simple interest grows linearly: $a(t) = 1 + rt$.
- Compound interest grows exponentially: $a(t) = (1+r)^t$.
- Effective rate converts nominal compounding into actual annual growth.
- Equivalent rates have the same one-year accumulation.
- Continuous compounding uses $e^{rt}$.
- Force of interest is $\delta(t) = a'(t) / a(t)$.
