# Lecture 02

This lecture explains how to compare cash flows that occur at different
times by moving their values to a common time.

Assume $a(t)$ is the accumulation function. When the effective interest
rate per period is constant, write it as $r$.

## Variable Definitions

Unless stated otherwise, time is measured in periods matching the interest
rate.

$$
a(t) = \text{accumulation function from time } 0 \text{ to time } t
$$

$$
r = \text{effective interest rate per period}
$$

$$
c = \text{single cash flow amount}
$$

$$
t = \text{time at which a cash flow is valued or paid}
$$

$$
C = \text{cash flow stream}
$$

$$
D = \text{another cash flow stream used for comparison}
$$

$$
c_i = \text{amount of the } i\text{th cash flow}
$$

$$
t_i = \text{time of the } i\text{th cash flow}
$$

$$
n = \text{number of cash flows or payments}
$$

$$
i = \text{index for cash flows}
$$

$$
j = \text{index used in annuity or perpetuity payment sums}
$$

$$
\mathrm{PV}(C) = \text{present value of cash flow stream } C
$$

$$
\mathrm{TV}_t(C) = \text{time value of } C \text{ at time } t
$$

$$
s = \text{another time point used when moving values between times}
$$

$$
\mathrm{NPV}(C) = \text{net present value of cash flow stream } C
$$

$$
(c_0,c_1,\ldots,c_n) = \text{signed cash flows at integer times } 0,1,\ldots,n
$$

$$
x = \frac{1}{1+r}
$$

$$
\mathrm{IRR} = \text{internal rate of return}
$$

$$
f(z) = \text{function whose root is being approximated}
$$

$$
z = \text{input variable for } f(z)
$$

$$
f'(z) = \text{derivative of } f(z)
$$

$$
\alpha_k = \text{Newton-Raphson approximation after } k \text{ iterations}
$$

$$
k = \text{index for Newton-Raphson iterations or cash-flow conditions}
$$

$$
A = \text{annuity or loan payment per period}
$$

$$
L = \text{original loan amount}
$$

$$
m = \text{number of loan payments already made}
$$

$$
L_m^{\text{Bal}} = \text{loan balance immediately after the } m\text{th payment}
$$

For formulas with $\frac{1}{r}$, assume $r \ne 0$. In most financial
settings here, $r>0$.

## Present Value

Present value asks: how much is a future payment worth today?

For a single payment $c$ at time $t \ge 0$:

$$
\mathrm{PV} = \frac{c}{a(t)}
$$

If the effective interest rate is $r$:

$$
\mathrm{PV} = \frac{c}{(1+r)^t}
$$

For a cash flow stream

$$
C = \{(c_1,t_1),(c_2,t_2),\ldots,(c_n,t_n)\},
$$

the present value is:

$$
\mathrm{PV}(C) = \sum_{i=1}^n \frac{c_i}{a(t_i)}
$$

If the effective interest rate is $r$:

$$
\mathrm{PV}(C) = \sum_{i=1}^n \frac{c_i}{(1+r)^{t_i}}
$$

Finding the present value of a future payment is called discounting.

<details>
<summary>Intuition / proof</summary>

If one unit at time $0$ grows to $a(t)$ units at time $t$, then the amount
at time $0$ that grows to $c$ at time $t$ must be:

$\frac{c}{a(t)}$

because:

$\frac{c}{a(t)}a(t)=c$

When the effective interest rate is constant:

$a(t)=(1+r)^t$

so:

$\mathrm{PV}=\frac{c}{(1+r)^t}$

For a stream of cash flows, present value is additive. Each cash flow is
discounted to time $0$, then added:

$\mathrm{PV}(C)=\sum_{i=1}^n \frac{c_i}{a(t_i)}$

</details>

## Time Value

The time value of cash flow $C$ at time $t$, denoted $\mathrm{TV}_t(C)$,
is:

$$
\mathrm{TV}_t(C) = \mathrm{PV}(C)a(t)
$$

For $0 < s < t$:

$$
\mathrm{TV}_t(C) = \frac{a(t)}{a(s)}\mathrm{TV}_s(C)
$$

If the effective interest rate is $r$:

$$
\mathrm{TV}_t(C) = \sum_{i=1}^n \frac{c_i}{(1+r)^{t_i-t}}
$$

and:

$$
\mathrm{TV}_t(C) = \mathrm{TV}_s(C)(1+r)^{t-s}
$$

<details>
<summary>Intuition / proof</summary>

The present value of $C$ is its value at time $0$. To move that value to
time $t$, accumulate it by $a(t)$:

$\mathrm{TV}_t(C)=\mathrm{PV}(C)a(t)$

To move from time $s$ to time $t$, multiply by the accumulation factor
from $s$ to $t$:

$a(s,t)=\frac{a(t)}{a(s)}$

So:

$\mathrm{TV}_t(C) =\frac{a(t)}{a(s)}\mathrm{TV}_s(C)$

When $a(t)=(1+r)^t$:

$\frac{a(t)}{a(s)} =\frac{(1+r)^t}{(1+r)^s} =(1+r)^{t-s}$

For each cash flow $c_i$ at time $t_i$, its value at time $t$ is:

$\frac{c_i}{(1+r)^{t_i-t}}$

If $t_i>t$, this discounts backward. If $t_i<t$, the exponent is negative,
so the expression accumulates forward.

</details>

## Principle of Equivalence

Two cash flow streams are equivalent if and only if they have the same
present value, assuming the interest rate and accumulation method are the
same.

Equivalent cash flow streams also have the same time value at any time
$t$.

<details>
<summary>Intuition / proof</summary>

If two cash flow streams $C$ and $D$ have the same present value, say:

$\mathrm{PV}(C)=\mathrm{PV}(D)$

then their time values at time $t$ are:

$\mathrm{TV}_t(C)=\mathrm{PV}(C)a(t)$

and:

$\mathrm{TV}_t(D)=\mathrm{PV}(D)a(t)$

Since the present values are equal, the time values are also equal.

So equality at one time implies equality at every time, as long as the
same accumulation method is used.

</details>

## Net Present Value

Net present value, or $\mathrm{NPV}$, is the present value of all cash
flows in an investment project, including both negative and positive cash
flows.

For an investment cash flow

$$
C = \{(c_i,t_i)\}_{i=1}^n,
$$

the net present value is:

$$
\mathrm{NPV}(C) = \sum_{i=1}^n \frac{c_i}{a(t_i)}
$$

If the effective interest rate is $r$:

$$
\mathrm{NPV}(C) = \sum_{i=1}^n \frac{c_i}{(1+r)^{t_i}}
$$

Decision rules:

- Invest if $\mathrm{NPV} > 0$.
- Project A is preferred to Project B if $\mathrm{NPV}(A) > \mathrm{NPV}(B)$.

<details>
<summary>Intuition / proof</summary>

NPV is just present value applied to signed cash flows.

Positive cash flows are benefits and negative cash flows are costs.
Discount every cash flow to time $0$, then add:

$\mathrm{NPV}(C)=\sum_{i=1}^n \frac{c_i}{a(t_i)}$

If $\mathrm{NPV}>0$, the benefits are worth more today than the
costs, using the chosen discount rate.

</details>

## Equation of Value

An equation of value compares two cash flow streams by moving them to the
same focal time and setting those values equal.

At time $0$, if cash flow streams $C$ and $D$ are equivalent, then:

$$
\mathrm{PV}(C)=\mathrm{PV}(D)
$$

For a cash flow stream written as $(c_0,c_1,\ldots,c_n)$ at integer
times, a common special case is the equation of value against the zero
cash flow stream:

$$
\sum_{i=0}^n \frac{c_i}{(1+r)^i}=0
$$

This means the stream $(c_0,c_1,\ldots,c_n)$ is equivalent to the zero
cash flow stream $(0,0,\ldots,0)$ at the chosen focal time.

<details>
<summary>Intuition / proof</summary>

The equation of value compares cash flow streams at the same focal time,
usually time $0$.

If two streams are equivalent at time $0$, then:

$\mathrm{PV}(C)=\mathrm{PV}(D)$

If one side is the zero cash flow stream, its present value is $0$, so the
equation becomes a zero-sum present value equation.

Each cash flow $c_i$ is discounted to time $0$:

$\frac{c_i}{(1+r)^i}$

Adding all discounted cash flows gives:

$\sum_{i=0}^n \frac{c_i}{(1+r)^i}$

Setting the sum equal to $0$ means the stream exactly balances the zero
cash flow stream at the chosen rate $r$.

</details>

## Internal Rate of Return

Any non-negative root $r$ of the equation of value is called the yield or
effective internal rate of return, abbreviated $\mathrm{IRR}$.

$$
\sum_{i=0}^n \frac{c_i}{(1+r)^i} = 0
$$

Remarks:

- IRR is determined entirely by the cash flows, not by a prevailing market interest rate.
- If the IRR is unique, it is the rate of return on the investment.
- A cash flow can have multiple IRRs or no non-negative IRR.

Using $x = 1/(1+r)$, the equation of value becomes a polynomial:

$$
c_0 + c_1x + c_2x^2 + \cdots + c_nx^n = 0
$$

This polynomial has at most $n$ distinct roots.

Decision rule when IRR is unique:

$$
\text{Invest if IRR} > \text{required return.}
$$

<details>
<summary>Intuition / proof</summary>

IRR is the rate that makes the net present value equal to zero.

Using:

$x=\frac{1}{1+r}$

we have:

$\frac{1}{(1+r)^i}=x^i$

So:

$\sum_{i=0}^n \frac{c_i}{(1+r)^i}=0$

becomes:

$c_0+c_1x+c_2x^2+\cdots+c_nx^n=0$

This is a polynomial of degree at most $n$, so it has at most $n$ distinct
real roots. This is why cash flows may have more than one IRR.

</details>

## Uniqueness of IRR

The IRR is guaranteed to be unique if the cash flow stream
$(c_0,c_1,\ldots,c_n)$ satisfies:

$$
c_0 < 0
$$

$$
c_k \ge 0 \quad \text{for all } k = 1,\ldots,n
$$

$$
c_0 + c_1 + \cdots + c_n > 0
$$

Intuition: there is one initial investment outflow, followed only by
non-negative inflows, and the total cash flow is positive.

<details>
<summary>Intuition / proof</summary>

Let:

$f(r)=\sum_{i=0}^n \frac{c_i}{(1+r)^i}$

At $r=0$:

$f(0)=c_0+c_1+\cdots+c_n>0$

As $r$ becomes very large, the future cash flows are heavily discounted,
so:

$f(r)\to c_0<0$

Since $f(r)$ moves from positive to negative, at least one root exists.

Also, under the stated cash flow pattern, as $r$ increases, the present
value of the non-negative future inflows decreases while $c_0$ stays
fixed. Therefore $f(r)$ crosses zero only once.

So the IRR is unique.

</details>

## Newton-Raphson Method

For a general cash flow stream with $n \ge 5$, there is no general formula
for IRR. Numerical methods may be used instead.

Newton-Raphson is used to approximate a root of a differentiable function
$f(z)$.

Start with an initial guess $\alpha_0$. Then iterate:

$$
\alpha_{k+1} = \alpha_k - \frac{f(\alpha_k)}{f'(\alpha_k)}
$$

provided:

$$
f'(\alpha_k) \ne 0
$$

For IRR, $f(r)$ is usually the present value equation:

$$
f(r) = \sum_{i=0}^n \frac{c_i}{(1+r)^i}
$$

Repeat until the desired accuracy is reached.

<details>
<summary>Intuition / proof</summary>

Newton-Raphson finds a root by repeatedly replacing a curve with its
tangent line.

At the current guess $\alpha_k$, the tangent line to $f$ is:

$y=f(\alpha_k)+f'(\alpha_k)(z-\alpha_k)$

The next guess $\alpha_{k+1}$ is where this tangent line crosses the
$z$-axis. Set $y=0$:

$0=f(\alpha_k)+f'(\alpha_k)(\alpha_{k+1}-\alpha_k)$

Solving for $\alpha_{k+1}$:

$\alpha_{k+1} =\alpha_k-\frac{f(\alpha_k)}{f'(\alpha_k)}$

</details>

## Annuity

An annuity is a series of equal payments made at regular intervals.

Notation:

$$
A = \text{payment per period}, \qquad r = \text{effective interest rate per period}, \qquad n = \text{number of payments}.
$$

An annuity can be viewed as a cash flow stream:

$$
\{(A,t_1),(A,t_2),\ldots,(A,t_n)\}
$$

where the payments occur at equally spaced times.

## Ordinary Annuity vs Annuity Due

An ordinary annuity has payments at the end of each period:

$$
t = 1,2,\ldots,n
$$

Its present value at $t = 0$ is:

$$
\mathrm{PV} = \sum_{j=1}^n \frac{A}{(1+r)^j} = \frac{A}{r}\left(1-\frac{1}{(1+r)^n}\right)
$$

An annuity due has payments at the beginning of each period:

$$
t = 0,1,\ldots,n-1
$$

Its present value at $t = 0$ is:

$$
\mathrm{PV} = \sum_{j=0}^{n-1} \frac{A}{(1+r)^j} = \frac{A(1+r)}{r}\left(1-\frac{1}{(1+r)^n}\right)
$$

Relationship:

$$
\mathrm{PV}(\text{annuity due}) = (1+r)\mathrm{PV}(\text{ordinary annuity})
$$

The time value of an annuity can be obtained from:

$$
\mathrm{TV}_t = \mathrm{PV}(1+r)^t
$$

<details>
<summary>Intuition / proof</summary>

For an ordinary annuity, payments occur at times:

$1,2,\ldots,n$

So the present value is:

$\mathrm{PV} =A[ \frac{1}{1+r} +\frac{1}{(1+r)^2} +\cdots +\frac{1}{(1+r)^n} ]$

This is a finite geometric series with first term $\frac{1}{1+r}$ and
common ratio $\frac{1}{1+r}$, so:

$\mathrm{PV} =\frac{A}{r}[1-\frac{1}{(1+r)^n}]$

For an annuity due, every payment occurs one period earlier than in an
ordinary annuity. Therefore its present value is one period of
accumulation larger:

PV of an annuity due equals $(1+r)$ times the PV of the corresponding
ordinary annuity.

</details>

## Perpetual Annuity

A perpetual annuity, or perpetuity, pays a fixed amount forever.

For payments at the beginning of each period:

$$
\mathrm{PV} = \sum_{j=0}^{\infty} \frac{A}{(1+r)^j} = \frac{A(1+r)}{r}
$$

For payments at the end of each period:

$$
\mathrm{PV} = \sum_{j=1}^{\infty} \frac{A}{(1+r)^j} = \frac{A}{r}
$$

<details>
<summary>Intuition / proof</summary>

A perpetuity is an annuity with infinitely many payments.

For end-of-period payments:

$\mathrm{PV} =A[ \frac{1}{1+r} +\frac{1}{(1+r)^2} +\cdots ]$

This is an infinite geometric series with first term $\frac{1}{1+r}$ and
common ratio $\frac{1}{1+r}$, so:

$\mathrm{PV}=\frac{A}{r}$

For beginning-of-period payments, the first payment occurs immediately at
time $0$. This makes the value one period larger:

$\mathrm{PV}=\frac{A(1+r)}{r}$

</details>

## Loans

Loans are usually repaid by installment payments at regular intervals. If
$L$ is the loan amount at $t = 0$ and $C$ is the repayment stream:

$$
L = \mathrm{PV}(C)
$$

For equal end-of-period payments $A$ over $n$ periods:

$$
L = \frac{A}{r}\left(1-\frac{1}{(1+r)^n}\right)
$$

So:

$$
A = \frac{Lr}{1-\frac{1}{(1+r)^n}}
$$

<details>
<summary>Intuition / proof</summary>

At the start of the loan, the loan amount must equal the present value of
all repayments:

$L=\mathrm{PV}(C)$

For equal end-of-period repayments, the repayment stream is an ordinary
annuity. Therefore:

$L=\frac{A}{r}[1-\frac{1}{(1+r)^n}]$

Solving for $A$ gives:

$A=\frac{Lr}{1-\frac{1}{(1+r)^n}}$

</details>

## Loan Balance: Prospective vs Retrospective

Let:

$$
\begin{aligned} L &= \text{original loan amount}, \\ A &= \text{installment payment per period}, \\ r &= \text{effective interest rate per period}, \\ n &= \text{total number of payments}, \\ m &= \text{number of payments already made}, \\ L_m^{\text{Bal}} &= \text{loan balance immediately after the } m\text{th payment}. \end{aligned}
$$

### Prospective Method

This looks forward. The loan balance is the present value at time $m$ of
the remaining $n-m$ payments.

$$
L_m^{\text{Bal}} = \frac{A}{1+r} + \frac{A}{(1+r)^2} + \cdots + \frac{A}{(1+r)^{n-m}}
$$

Using the annuity formula:

$$
L_m^{\text{Bal}} = \frac{A}{r}\left(1-\frac{1}{(1+r)^{n-m}}\right)
$$

This method does not require the original loan amount $L$.

### Retrospective Method

This looks backward. Accumulate the original loan to time $m$, then
subtract the accumulated value of the payments already made.

$$
L_m^{\text{Bal}} = L(1+r)^m - \frac{A}{r}\left((1+r)^m - 1\right)
$$

Equivalently, as written using the ordinary annuity PV factor accumulated
to time $m$:

$$
L_m^{\text{Bal}} = L(1+r)^m - \frac{A}{r}\left(1-\frac{1}{(1+r)^m}\right)(1+r)^m
$$

This method requires the original loan amount $L$.

Both methods give the same balance.

<details>
<summary>Intuition / proof</summary>

The prospective method looks forward from time $m$. Immediately after the
$m$th payment, there are $n-m$ payments left. The balance is the present
value at time $m$ of those remaining payments:

$L_m^{\mathrm{Bal}} = \frac{A}{r}[1-\frac{1}{(1+r)^{n-m}}]$

The retrospective method looks backward. Start with the original loan and
accumulate it to time $m$:

$L(1+r)^m$

Then subtract the accumulated value at time $m$ of the $m$ payments
already made:

$A\frac{(1+r)^m-1}{r}$

Therefore:

$L_m^{\mathrm{Bal}} = L(1+r)^m-\frac{A}{r}[(1+r)^m-1]$

Both methods give the same result because they are just two different
ways to value the same remaining loan obligation.

</details>

## Key Takeaways

- Present value discounts future cash flows back to today.
- Time value moves a cash flow's value to another time.
- NPV includes both negative and positive cash flows.
- IRR is a non-negative rate that makes $\mathrm{NPV} = 0$.
- IRR is unique under the standard pattern: one initial outflow, then non-negative inflows, with positive total cash flow.
- Newton-Raphson approximates roots using $\alpha_{k+1} = \alpha_k - f(\alpha_k)/f'(\alpha_k)$.
- Ordinary annuity pays at period ends; annuity due pays at period beginnings.
- Perpetuity pays forever.
- Prospective loan balance looks at future payments; retrospective loan balance looks at the accumulated loan minus accumulated past payments.
