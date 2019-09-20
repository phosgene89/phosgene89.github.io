<html>
  <head>
    {% include head.html %}
    {% seo %}
  </head>
# <center>A Primer on Hierarchical Navigable Small Worlds</center>

## Overview

k-nearest neighbours (k-NN) is an effective and commonly used method in data science. However, search times grow rapidly with high dimensional, high volume datasets. This restricts the use of kNN algorithms to smaller datasets. Approximate nearest neighbours (ANN) provides faster search times in exchange for returning only *approximate* nearest neighbours. In other words, we can't be sure that the k neighbours returned are the nearest ones. But they are usually pretty close and for many applications this all that is needed. The current state of the art in high dimensional approximate k-NN is hierarchical navigable small worlds (HNSW), proposed by Malkov & Yashunin (2017). The general procedure of HNSW is to build a multilayer network and to then search for approximate nearest neighbours in a top-down, greedy fashion. This article will expand on HNSW and provide some code to enable you to try it out in your own projects. 

## Background
#### Milgram's Small World Experiment
A famous experiment by Stanley Milgram tasked participants with delivering a package to someone that they most likely did not know. If they did not personally know the target, they were instructed to pass the package onto someone that they did know. This person would then repeat the procedure until the package found its intended recipient. Milgran found that the average number of times the package exchanged hands was 6 - much smaller than expected. The interpretation of this was that locally connected groups of people often had individuals in them who were connected in some way to people very far away. These long range connections allowed the packages to reach the recipient faster than the exclusive usage of short range connections. 

The figure below demonstrates how this helps. Suppose we start at point A and want to reach point B, but we can only get through by traversing through nodes we are locally connected to. In the first figure, all connections are short-range, hence we will have to take x steps to get to our target. In the second figure, the presence of a few short range connections allows us to bypass many steps and reach our target in only y steps. This is the heart of HNSW.

#### NSW
Navigable small world networks were created to model networks that displayed small world characteristics similar to the ones in Milgram's experiment. NSW networks perform nearest neighbour searches by navigating through local connections to "hub", then moving from these hubs toward the target. This means that NSW networks start moving through short-range connections in order to reach hubs, then move through long range conncetions to cover large distances quickly and then revert back to traversing short range connections to hone in on the target. 

While NSW does allow for more efficient nearest-neighbour searches, their complexity scaling with network size is polylogarithmic at best. Ideally we would like it to be logarithmic.

## Overview of Hierarchical Navigable Small Worlds
HNSW improves on the efficiency of NSW by starting the search from hubs (the nodes with long range connections), which cuts out the initial "zoom out" phase of NSW. The "zoom in" phase makes use of a hierarchical structure in which longer links are traversed initially, gradually zooming in through shorter links until an approximate nearest neighbour is found. In comparison to NSW, the complexity of HNSW grows logarithmically with network size. Additionally, HNSW is fully graph based, needing no additional search structures.

The general procedure of HNSW is to create a layered graph (hierarchies) of connected points, with lower layers being more populated than higher layers. Each layer models node to node links of different scales, with the longest range links occurring in the top layer and the shortest range links occurring in the bottom layer. A node may be present in multiple layers, meaning it has both long and short range links. [show picture of long range links from short range small worlds]

<img src="https://github.com/phosgene89/phosgene89.github.io/blob/master/hnsw/hnsw.PNG">


After constructing this hierarchy, a nearest neighbour does a greedy search for the closest point in the top layer. It then enters the next layer through this entry point and begins another greedy search for an entry point to the next layer. The process repeats until the inserted point's final layer is reached. Then k connections are formed using a heuristic. A key point in this search is that the number of connections to a node, k, is constant. This is crucial to the logarithmic complexity scaling.

An important feature of HNSW (and NSW) is that long range links are not uniformly random. Were this the case, we would expect that half the time a long range link would lead us closer to our target, but lead us further the other half - leading to no net benefit.

## Graph Construction
#### Overview of Graph Construction
To construct the graph element by element, the procedure is as follows:

1. Get the maximum layer of inserted element q.
1. Do a layer by layer greedy search for an entry point.
1. Create connections from the entry point found in step 2. using the connection building heuristic.
1. If an element has too many connections, discard the ones that are further from q.

#### Creating Connections
The procedure for finding connections in the above step 3. is as follows:

1. From a set of candidate nodes found using a greedy search, find their immediate neighbours and add them to the set of candidate nodes.
1. Extract the closest element in the candidate list and create a connection to it if it is closer to the inserted element than it is to any node in the list of candidate nodes.
1. Repeat 2. until the number of candidates is exhausted or the number of connections reaches its limit.
1. If required, add discarded candidates to reach the required number of connections by choosing the closest ones.

#### k-NN Search Through HNSW Graph
The k-NN search is simply a greedy search through the HNSW graph.

#### Why Is This Approximate?
As HNSW is based on local greedy searches, it is possible that our algorithm will settle at a local minimum. For a more concrete example, our NN search may lead us down path X as, according to our local knowledge, it will bring us closer to our target than any other path. However, it is possible that path X is not connected to our target at all. e.g. Driving down a road that points directly at your destination, but is blocked in the middle. 

#### Construction parameters
##### Normalization factor
The normalization factor controls the probability distribution of elements being assigned a particular minimum layer.
Reducing the normalization factor reduces overlap between layers which in turn improves performance. On the other hand, reducing it too far results in high average hops to reach the target node, thereby reducing performance. Hence the normalisation factor must be tuned for each individual problem. A good starting point is 1/ln(M).

##### Maximum number of connections per node
The max number of connections for each node in the graph must be tuned for a good balance between recall and efficiency. Higher numbers for this parameter result in decreased search performance, but improved recall. A good starting point is 2*M.

##### Maximum number of connections per layer
Typically from 5 - 48. Smaller M works well for low recall or low dimensional data, while higher M works better for high recall or high dimensional data.

##### Size of candidate list for generating connections when constructing HNSW graph
This can be tweaked on a sample of the data and should be chosen to achieve at least 0.95 recall.

## Performance benchmarks

Benchmarks between current nearest neighbour search algorithms show that HNSW is a consistent strong-performer (https://github.com/erikbern/ann-benchmarks)

![alt text](https://github.com/phosgene89/phosgene89.github.io/blob/master/hnsw/hnsw3.PNG)


## References

[1] Yu. A. Malkov & D. A. Yashunin, <i>Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs</i> (2018), https://arxiv.org/abs/1603.09320

[2] D. J. Watts & S. H. Strogatz, <i>Collective dynamics of 'small-world’ networks</i> (1998), http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf

[X] S. Milgran, The Small-World Problem (1967), http://snap.stanford.edu/class/cs224w-readings/milgram67smallworld.pdf

[X] N. Name, Title (Year), Source

[X] N. Name, Title (Year), Source
</html>
