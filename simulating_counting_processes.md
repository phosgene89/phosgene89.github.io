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


## Simulating Hawkes Processes

Hawkes processes are a generalisation of Poisson processes that allow past events to increase the intensity. The method of simulation is conceptually the same as for an inhomogeneous Poisson process, but we now need to account for a more complex intensity, , that is dependant on past events. The main implication is that we need to be a bit more careful in choosing our $\lambda_{homogeneous}$, which requires us to generate arrival times one by one, in contrast to the vectorized examples above.

The reason we need to be more careful about sampling is that for any given $\lambda_{homogeneous}$, it is possible for a string of excitation events to push $\lambda(t|history)$ above $\lambda_{homogeneous}$ and ruin our rejection sampling process.

The general procedure will be to restrict our samples to certain time frames (rejecting sampled times outside of it) in order to allow us to calculate $max(\lambda(t|history))$ over a given time frame. This will allow us to choose an appropriate intensity for our homogeneous Poisson process. Note that we may have had to do this in the previous example if our $\lambda(t)$ had been unbounded, though this would not represent a realistic scenario.

