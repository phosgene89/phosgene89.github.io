# <center>A Primer on Hierarchical Navigable Small Worlds</center>

## Overview

k-nearest neighbours (k-NN) is an effective and commonly used method in data science. However, search times grow rapidly with high dimensional, high volume datasets. This restricts the use of kNN algorithms to smaller datasets. Approximate nearest neighbours (ANN) provides faster search times in exchange for returning only *approximate* nearest neighbours. In other words, we can't be sure that the k neighbours returned are the nearest ones. But they are usually pretty close and for many applications this all that is needed. The current state of the art in high dimensional approximate k-NN is hierarchical navigable small worlds (HNSW), proposed by Malkov and Yashunin <sup>[1]</sup>. The general procedure of HNSW is to build a multilayer network and to then search for approximate nearest neighbours in a top-down, greedy fashion.

Check out the [hnswlib](https://github.com/nmslib/hnswlib) and/or [nmslib](https://github.com/nmslib/nmslib) packages to start using HNSW in Python.

## Quick Comparison of HNSW vs K-Dimensional Tree
Before saying any more about HNSW, a quick demonstration of the benefits over a traditional nearest neighbour search will be performed.

First we generate our data.

    import numpy as np
    import hnswlib
    from sklearn.neighbors import NearestNeighbors
    
    dim = 512
    num_elements = 10000

    X = np.random.uniform(0,5,(num_elements,dim))
    
Now we build our 512 dimension tree and find the 5 nearest neighbours for each record.

    nbrs = NearestNeighbors(n_neighbors=5, algorithm='kd_tree').fit(X)
    
    %%time
    distances, indices = nbrs.kneighbors(X)
    CPU times: user 1min 30s, sys: 193 ms, total: 1min 30s
    Wall time: 1min 30s
    
Let's try it again with HNSW. First we need to construct the graph.

    data_labels = np.arange(num_elements)

    p = hnswlib.Index(space = 'l2', dim = dim) # possible options are l2, cosine or ip
    p.init_index(max_elements = num_elements, ef_construction = 200, M = 30)
    p.add_items(X, data_labels)
    p.set_ef(50)
    
Now we get the 5 approximate nearest neighbours for each record.

    %%time
    labels, distances = p.knn_query(X, k = 5)
    CPU times: user 37.8 s, sys: 352 ms, total: 38.1 s
    Wall time: 3.64 s

Much faster! Let's learn why.

## Background
#### Milgram's Small World Experiment
The famous "6 degrees of separation" experiment by Stanley Milgram tasked participants with delivering a package to someone that they most likely did not know <sup>[2][3]</sup>. If they did not personally know the target, they were instructed to pass the package onto someone that they did know. This person would then repeat the procedure until the package found its intended recipient. Milgram found that the average number of times the package exchanged hands was 6 - much smaller than expected. The interpretation of this was that locally connected groups of people often had individuals in them who were connected in some way to people very far away. These long range connections allowed the packages to reach the recipient faster than the exclusive usage of short range connections. 

If liken this to climbing stairs, then we can think of the short range, local connections as being a single step up to the stair directly above the one you are currently on. The long range connections can be thought of as "skipping" multiple steps and going straight up by 3-4 stairs. Assuming that moving to the next stair in the sequence and moving up by 3 take the same amount of effort, then clearly being able to skip stairs will help us to reach our destination quicker. In reality our assumption of equal effort is obviously false, but for a nearest neighbour search, we are dealing with distance calculations that will take approximately the same time. In realistic problems, we will also be unsure of which direction we need to take the multi-steps in, as opposed to this example where we are only going one way.


<center>
<p>
<figure align="center">
     <img src="https://phosgene89.github.io/hnsw/skipsteps.jpg" width = "300">
 <figcaption>
  <font size="-1">Skipping steps speeds up nearest neighbour searches...and shows dominance</font>
 </figcaption>
</figure>
 </p>
 </center>

#### NSW
Navigable small world networks were created to model networks that displayed small world characteristics similar to the ones in Milgram's experiment. NSW networks perform nearest neighbour searches by navigating through local connections to "hub", then moving from these hubs toward the target. This means that NSW networks start moving through short-range connections in order to reach hubs, then move through long range conncetions to cover large distances quickly and then revert back to traversing short range connections to hone in on the target. 

While NSW does allow for more efficient nearest-neighbour searches, their complexity scaling with network size is polylogarithmic at best. Ideally we would like it to be logarithmic.

## Overview of Hierarchical Navigable Small Worlds
HNSW improves on the efficiency of NSW by starting the search from hubs (the nodes with long range connections), which cuts out the initial "zoom out" phase of NSW. The "zoom in" phase makes use of a hierarchical structure in which longer links are traversed initially, gradually zooming in through shorter links until an approximate nearest neighbour is found. In comparison to NSW, the complexity of HNSW grows logarithmically with network size. Additionally, HNSW is fully graph based, needing no additional search structures.

The general procedure of HNSW is to create a layered graph (hierarchies) of connected points, with lower layers being more populated than higher layers. Each layer models node to node links of different scales, with the longest range links occurring in the top layer and the shortest range links occurring in the bottom layer. A node may be present in multiple layers, meaning it has both long and short range links.

<center>
<p>
<figure align="center">
 <img src="https://phosgene89.github.io/hnsw/hnsw.PNG">
 <figcaption>
  <font size="-1">Hierarchical graph structure of HNSW</font>
 </figcaption>
</figure>
 </p>
 </center>

After constructing this hierarchy, a nearest neighbour does a greedy search for the closest point in the top layer. It then enters the next layer through this entry point and begins another greedy search for an entry point to the next layer. The process repeats until the inserted point's final layer is reached. Then k connections are formed using a heuristic. The heuristic allows for longer range connections to form between clusters of tightly linked nodes, which helps the algorithm to recover from bad path choices in the approximate nearest neighbours search. Another thing to remember is that the number of connections to a node, k, is constant. This is crucial to the logarithmic complexity scaling.

<center>
<p>
<figure align="center">
 <img src="https://phosgene89.github.io/hnsw/hnsw2.PNG">
 <figcaption>
  <font size="-1">Long range links generated by heuristic</font>
 </figcaption>
</figure>
 </p>
 </center>

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

