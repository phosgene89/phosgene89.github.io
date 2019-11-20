# <center>A Primer on SARIMA</center>

## Overview

Seasonal Autoregressive Integrated Moving Average (SARIMA) models are an extension of ARIMA models that are designed to take seasonality into account.

As SARIMA is a generalization of simpler time series models, we first go through more specific cases in order of complexity. These are AR, MA, ARMA and ARIMA

Check out the [statsmodels](https://www.statsmodels.org/stable/index.html) package to start using SARIMA in Python.

## Precursors to SARIMA

### Autoregressive (AR) Models
Suppose we have a time series given by $\{ y_{t} \}$. An $AR(p)$ model can be specified by

$$ y_{t} = \mu + \sum\limits_{i=1}^p \theta_{i} y_{t-i} $$

This can be made more concise through the use of the lag operator, $L$.

$$L^{n} y_{t} = y_{t-n}$$

Taking $\Theta(L)^{p}$ to be an order p polynomial function of $L$, we can instead define an autoregressive model by

$$ y_{t} = \Theta(L)^{p} y_{t} $$

Taking note that the constant has been absorbed into the polynomial $\Theta$.

### Moving average (MA) Models.
The famous "6 degrees of acquaintances. 


### Autoregressive Moving Average (ARMA) Models
Navigable small world d like it to be logarithmic.

### Autoregressive Integrated Moving Average (ARIMA) Models
HNSW improves on the 


## SARIMA
The procedure for finding connections in the above step 3. is as follows:

1. From a set of candidate nodes found using a greedy search, find their immediate neighbours and add them to the set of candidate nodes.
1. Extract the closest element in the candidate list and create a connection to it if it is closer to the inserted element than it is to any node in the list of candidate nodes.
1. Repeat 2. until the number of candidates is exhausted or the number of connections reaches its limit.
1. If required, add discarded candidates to reach the required number of connections by choosing the closest ones.

#### k-NN Search Through HNSW Graph
The k-NN search is simply a greedy search through the HNSW graph.

#### Why Is This Approximate?
As HNSW is based on local greedy searches, it is possible that our algorithm will settle at a local minimum. For a more concrete example, our NN search may lead us down path $X$ as, according to our local knowledge, it will bring us closer to our target than any other path. However, it is possible that path $X$ is not connected to our target at all. e.g. Driving down a road that points directly at your destination, but is blocked in the middle. 

#### Construction parameters
##### Size of candidate list to search for nearest neighbours from
The number of candidates to search when forming connections is set using a small sample. Select a value that results in a recall of 0.95 during this process.

##### Number of nearest neighbours to find during construction
When constructing the graph, we set a limit on the number of nearest neighbours to search for as potential connections, $M$. Typical values for $M$ are between 5 - 48. Smaller $M$ works well for low recall or low dimensional data, while higher $M$ works better for high recall or high dimensional data.

##### Maximum number of connections per layer, per node
The max number of connections per layer , per node (denoted $M_{max}$) in the graph must be tuned for a good balance between recall and efficiency. Higher numbers for this parameter result in decreased search performance, but improved recall. A good starting point is $2M$.

$M_{max}$ is greater than $M$ as we create connections based on both the $M$ nearest neighbours <i>in addition</i> to nodes that are connected to these nearest neighbours.

##### Normalization factor
The normalization factor, $m_{L}$ controls the probability distribution of elements being assigned a particular minimum layer. In other words, it determines how many total layers there are. Reducing the normalization factor reduces overlap between layers which in turn improves performance. On the other hand, reducing it too far results in high average hops to reach the target node, thereby reducing performance. Hence the normalisation factor must be tuned for each individual problem. A good starting point for $m_{L}$ is $1/ln(M)$.

## Performance benchmarks

[Erik Bernhardsson](https://github.com/erikbern) has performed extensive benchmarks between current nearest neighbour search algorithms (you can find them all [here](https://github.com/erikbern/ann-benchmarks)). These benchmarks show that HNSW is a consistent strong-performer.

<center>
<p>
<figure align="center">
 <img src="https://phosgene89.github.io/hnsw/hnsw3.PNG">
 <figcaption>
  <font size="-1">Performance comparison between HNSW and Facebook AI Similarity Search (FAISS)</font>
 </figcaption>
</figure>
 </p>
 </center>


## References

[1] Y. A. Malkov & D. A. Yashunin, <i>Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs</i> (2018), https://arxiv.org/abs/1603.09320

[2] D. J. Watts & S. H. Strogatz, <i>Collective dynamics of 'small-world’ networks</i> (1998), http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf

[3] S. Milgran, The Small-World Problem (1967), http://snap.stanford.edu/class/cs224w-readings/milgram67smallworld.pdf

