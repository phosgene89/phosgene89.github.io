### Homogeneous Poisson Process
We will first simulate a homogeneous Poisson process. This is fairly simple. It proceeds as follows:

   Step 1. Take the pdf (probability density function) of the time between events (where $\lambda$ is the rate of events):

$$ f(t) = \lambda e^{- \lambda t} $$

   Step 2. Find the associated cdf (cumulative distribution function).

$$ F(t) = \int_{0}^{t} \lambda e^{- \lambda s} ds = 1 - e^{- \lambda t} $$

   Step 3. Find the inverse of the cdf.

$$ F^{-1}(p(t<T)) = -\frac{ln(1-p(t<T))}{\lambda} $$

   Step 4. Generate a random, uniformly distributed number $U$, between 0 and 1, to plug into $F^{-1}$.

   Step 5. Use the inverse cdf to get a corresponding time since last event.

$$ \Delta t = -\frac{ln(U)}{\lambda} $$

   Step 6. Repeat 4. and 5. until a sufficient number of interarrival times have been generated.

This process will yield a sequence of time between events, which can be converted into arrival times or total counts.



### Inhomogeneous Poisson Process

The process is easy enough for a homogeneous Poisson process, but when the intensity $\lambda$ is time-varying (i.e. a function of $t$, $\lambda(t)$), calculating $F^{-1}$ can become a very difficult (or impossible) task. Poisson processes with time-varying intensities are referred to as inhomogeneous.

Instead of sampling directly from the inhomogeneous process, we will sample from a homogeneous Poisson process with $\lambda_{homogeneous}>\lambda(t)$ for all $t$. We then probabilistically reject events based on a comparison of $\lambda_{homogeneous}$ and $\lambda(t)$. This is possible due to the fact that we can easily evaluate $\frac{\lambda_{homogeneous}}{\lambda(t)}$ for any value of t.

The trick is to generate another random, uniformly distributed number $U_{1}$,between 0 and 1, each time we generate an event and reject that event whenever $U_{1}>\frac{\lambda_{homogeneous}}{\lambda(t)}$. This accounts for "extra" events that are generated in periods where $\lambda_{homogeneous}$ is higher than $\lambda(t)$.

For this example, we will assume

$$\lambda(t) = \beta sin(\alpha t) + |\beta| $$

