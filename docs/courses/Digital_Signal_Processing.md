# Digital Signal Processing

## Example 1
!!! example
    (i)  Compute the 4-point DFT of $x[n]=[2\;3\;-3\;-2]$
    using the matrix form.  
    (ii) Compute the 4-point DFT of$x[n]$ in part (i)
    using the 4-point decimation-in-time (DIT)FFT.  
    (iii) Determine the number of multiplications and additions required
    in part (i) and part (ii)respectively.  
    (Note that any +1 or -1 appearing in part (ii) can be realized through
    one "addition", without any multiplication.)

**Answer:**

## Example 2
!!! example
    Consider an LTI system that is casual and for which the system function is

    $$ H(z) = \frac{3-7z^{-1}+5z^{-2}}{1-2.5z^{-1}+z^{-2}} $$

    Suppose $x[n]$, the input to the system, is a unit step sequence.  
    (i) Find a CCDE (1). Using the CCDE, find the output$y[n]$ for n = 1,2 and 3.  
    (ii) Find the output$y[n]$ for all n by
    computing the inverse z-transform of $Y(z)$.  
    { .annotate }

    1. CCDE means (linear) Constant-Coefficient Difference Equation.

**Answer:**  
(i)

$$ Y(z) \cdot {(1-2.5z^{-1}+z^{-2})} = X(z) \cdot ({3-7z^{-1}+5z^{-2}}) $$

Therefore,

$$ y[n] - 2.5y[n-1] + y[n-2] = 3x[n] -7x[n-1] + 5x[n-2] $$

Since the system is casual, input $x[n]=0$ for $n<0$.
So output $y[n]=0$ for $n<0$. We can calculate that $y[1]=\frac{7}{2}$,
$y[2]=\frac{27}{4}$ and $y[3]=\frac{115}{8}$.

(ii) $H(z)$ is casual with poles at $\frac{1}{2}$ and 2. So the ROC is
$R_H: |z| > 2$. Because $x[n] = u[n]$, so

$$ X(z) = \mathcal{Z}\{x(n)\} = \frac{1}{1-z^{-1}}, |z|>1 $$

Therefore,

$$ Y(z) = X(z)H(z) = \frac{1}{1-z^{-1}} \cdot \frac{3-7z^{-1}+5z^{-2}}{1-2.5z^{-1}+z^{-2}} $$

has poles at $\frac{1}{2}$ and 2.
ROC of $Y(z)$ must contain $R_x \cap R_H = |z| > 2$.
Therefore, ROC of $Y(z)$ is $|z|>2$.

$$ Y(z) = \frac{-2}{1-z^{-1}} + \frac{2}{1-2z^{-1}} + \frac{3}{1-0.5z^{-1}} $$

By using inverse z-transform, we get:

$$ y[n] = -2u[n]+2\cdot2^{n}u[n] + 3\cdot{(\frac{1}{2})}^{n}u[n] $$

## Example 3
!!! example
    How many complex multiplications are required
    to compute the 1024-point DFT using the direct computation?
    How will these required numbers change if the DIT FFT
    algorithm is used? How many stages are involved in this 1024-point DIT FFT?

**Answer:**

Direct Computation: $1024^{2}$ multiplications
and $1024*1023(\approx 1024^{2})$ additions.

Using DIT FFT: $\frac{N}{2} \cdot \log_2 N$ multiplications
and $N \cdot \log_2 N$ additions.

So when $N=1024$, the number of multiplications is reduced to
$\frac{1024}{2} \cdot \log_2 1024 = 5120$.
The number of additions is reduced to $1024 \cdot \log_2 1024 = 10240$.

The number of stages is $\log_2 1024 = 10$.

## Question 1
!!! question
    When we draw the magnitude response of a given filter, why drawing the positive frequency from 0 to $\pi$ is enough?

**Answer:**

From DTFT(1), we know that the spectrum is periodic with period $2\pi$, since:
{ .annotate }

1. DTFT means Discrete-Time Fourier Transform.

$$ X(e^{j(\omega+2m \pi)}) = \sum_{n=-\infty}^{\infty}x[n]e^{-j(\omega+2m \pi)n} = \sum_{n=-\infty}^{\infty}x[n]e^{-j\omega n} \cdot e^{-j2m \pi n} = X(e^{j\omega}) $$

Therefore the frequency range is limited by $2 \pi$ from $-\pi$ to $\pi$.

Recall that magnitude response is symmetric, $|H(e^{j\omega})| = |H(e^{-j\omega})|$.

Therefore, plotting the positive frequency from 0 to $\pi$ is enough.

## Question 2
!!! question
    Why ideal filters are not realizable?

**Answer:**

1. Ideal filters are non-casual.
2. Ideal filters' impulse response is extends from $-\infty$ to $\infty$.

## Question 3
!!! question
    What's the difference between **Group Delay** and **Time Delay**?

**Answer:**

**Time Delay of the filter** is how much the sinusoid is shifted at its output; **Group Delay of the filter** gives how much the envelope of the burst is shifted in time.

Also, the time delay can be calculated by the following formula:

$$ -\frac{\angle H(e^{j \omega_0})}{\omega_0} $$

And the group delay can be calculated by the following formula:

$$ -\frac{d}{d\omega} \angle H(e^{j \omega})$$

In general, group delay = time delay for filters with **linear phase response** of the form

$$ \angle H(e^{j \omega}) = const \cdot \omega $$

## Question 4
!!! question
    What's the difference between IIR(1) Filter and FIR(2) Filter?
    { .annotate }

   1. IIR Filter: Infinite Impulse Response Filter
   2. FIR Filter: Finite Impulse Response Filter

**Answer:**
