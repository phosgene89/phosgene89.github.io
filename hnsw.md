# <center>A Primer on Hierarchical Navigable Small Worlds</center>

## Overview

k-nearest neighbours (k-NN) is an effective and commonly used method in data science. However, search times grow rapidly with high dimensional, high volume datasets. This restricts the use of kNN algorithms to smaller datasets. Approximate nearest neighbours (ANN) provides faster search times in exchange for returning only *approximate* nearest neighbours. In other words, we can't be sure that the k neighbours returned are the nearest ones. But for many applications this is good enough. The current state of the art in high dimensional approximate k-NN is hierarchical navigable small worlds (HNSW), proposed by Malkov & Yashunin (2017). The general procedure of HNSW is to build a multilayer network and to then search for approximate nearest neighbours in a top-down, greedy fashion. This article will expand on HNSW and provide some code to enable you to try it out in your own projects. 

## Background
#### Milgram's Small World Experiment
A famous experiment by Stanley Milgram tasked participants with delivering a package to someone that they most likely did not know. If they did not personally know the target, they were instructed to pass the package onto someone that they did know. This person would then repeat the procedure until the package found its intended recipient. Milgran found that the average number of times the package exchanged hands was 6 - much smaller than expected. The interpretation of this was that locally connected groups of people often had individuals in them who were connected in some way to people very far away. These long range connections allowed the packages to reach the recipient faster than the exclusive usage of short range connections. 

The figure below demonstrates how this helps. Suppose we start at point A and want to reach point B, but we can only get through by traversing through nodes we are locally connected to. In the first figure, all connections are short-range, hence we will have to take x steps to get to our target. In the second figure, the presence of a few short range connections allows us to bypass many steps and reach our target in only y steps. This is the heart of HNSW.

#### NSW
Navigable small world networks were created to model networks that displayed small world characteristics similar to the ones in Milgram's experiment. NSW networks perform nearest neighbour searches by navigating through local connections to "hub", then moving from these hubs toward the target. This means that NSW networks start moving through short-range connections in order to reach hubs, then move through long range conncetions to cover large distances quickly and then revert back to traversing short range connections to hone in on the target. 

While NSW does allow for more efficient nearest-neighbour searches, their complexity scaling with network size is polylogarithmic at best. Ideally we would like it to be logarithmic.

## Hierarchical Navigable Small Worlds
#### Overview
HNSW improves on the efficiency of NSW by starting the search from hubs (the nodes with long range connections), which cuts out the initial "zoom out" phase of NSW. The "zoom in" phase makes use of a hierarchical structure in which longer links are traversed initially, gradually zooming in through shorter links until an approximate nearest neighbour is found. In comparison to NSW, the complexity of HNSW grows logarithmically with network size. Additionally, HNSW is fully graph based, needing no additional search structures.

The general procedure of HNSW is to create a layered graph (hierarchies) of connected points, with lower layers being more populated than higher layers. Each layer models node to node links of different scales, with the longest range links occurring in the top layer and the shortest range links occurring in the bottom layer. A node may be present in multiple layers, meaning it has both long and short range links. [show picture of long range links from short range small worlds]

<img src="https://github.com/phosgene89/phosgene89.github.io/blob/master/hnsw/hnsw.PNG">


After constructing this hierarchy, a nearest neighbour does a greedy search for the closest point in the top layer. It then enters the next layer through this point and begins another greedy search for a nearest neighbour - with the constraint that it compares itself only to points connected to the current node. The process repeats until k approximate nearest neighbours are found. A key point in this search is that the number of links to a node is constant. This is crucial to the logarithmic complexity scaling.

An important feature of HNSW (and NSW) is that long range links are not uniformly random. Were this the case, we would expect that half the time a long range link would lead us closer to our target, but lead us further the other half - leading to no net benefit.

#### Graph Construction

select neighbours by a heuristic

use exponentially decaying probability to of an element being in a layer.

HNSW reduces to NSW if we merge all layers.

heuristic for creating links: choose link, then greedily search elements, adding links if they are closer than any of the current links

When constructing a graph, the lower layer an element appears in is randomly chosen according to an exponential distribution.
NN-Search

Greedy layer-by-layer search. When local minimum found in one layer, use located element as entry point to next layer. Repeat.

#### Reason for approximate search
As we are doing local, greedy searches, it is possible that we will take a "wrong turn" in te higher levels of the graph and therefore be cut off from a pathway to the actual nearest neighbour. However, we can still get something that is quite close, which may be good enough.


#### Construction parameters
Main parameter to optimise is the number of layers in the graph. Lowering the number of layers used means that there is less overlap (improving performance), but increases the average hop number (which decreases performance). Hence the number of layers must be tuned. A good starting point is 1/ln(M).

The max number of connections for each node in the graph must be tuned for recall vs efficiency. Higher numbers for this parameter result in decreased search performance, but improved recall. A good starting point is 2*M.

efConstruction is straightforward to tweak. It should lead to >0.95 recall on a sample set during the construction phase.

Finally, the number of nearest neighbours to search from candidates in algorithm 4. This must also be balanced between recall and performance time.

##### Performance benchmarks and potential issues

Benchmarks between current nearest neighbour search algorithms show that HNSW is a consistent strong-performer (https://github.com/erikbern/ann-benchmarks)

![alt text](https://github.com/phosgene89/phosgene89.github.io/blob/master/hnsw/hnsw3.PNG)

A recent paper places doubt on the advantages of HNSW over NSW. Speed efficiency seems to be present only for d<8. I am not an expert on nearest neighbour searches, so I cannot offer any commentary on this, but it's worth knowing about. https://arxiv.org/abs/1904.02077

http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf



## Algorithm descriptions

#### Algorithm 1 - Graph construction
For inserting point q into hnsw graph

##### Requires:

New element, q

hnsw graph

number of established connections, M

Maximum number of connections per node, per layer,  M_max

Size of dynamic candidate list, ef_candidate

normalisation factor, m_l

##### Returns:

##### Procedure:

First initialize empty list of nearest neighbours, W

Get an enter point, ep

Get top layer, L

Get lowest layer for inserted element, l

Search through layers from L to l: (find entry point)

    W <- Find nearest neighbours in layer.
    
    ep <- closest element in W to q
    
Create connections between q and other nodes in each layer. Shrink connections if necesarry


#### Algorithm 2 - Search layer
This algorithm is used for both graph construction and NN search.

Algorithm two searches a layer for connection candidates. It starts with the enter point, then searches through connected nodes for further nearest neighbour candidates. At each step, candidate nodes are added to the NN list if they are closer to the inserted element than the furthest current NN. If the list of current NNs is filled, the furthest NN is removed. This procedure terminates when the closest current candidate is further away from the inserted element than the furthest current NN.

##### Requires:

query element, q

enter points, ep

number of nearest to q elements to return, ef

layer to search, l

##### Returns:

ef closest neighbours to q in layer l

##### Procedure:

v <- ep

C <- ep

W <- ep

while |C| > 0:

    c <- get nearest neighbour from C to q
    f <- get furthest element from W to q
    
    if distance(c,q) > distance(f,q):
        
        break
       
    for each e in neighbourhood(c) at layer l:
    
        if e is not in v:
        
            v <- union(v, e)
            f <- furthest element in W to q
            
            if distance(e,q) < distance(f,q) or |W| < ef:
            
                C <- union(C, e)
                W <- union(W,e)
                if |W| > ef:
                    remove furthest element from W to q
                    
return W


#### Algorithm 4 - Nearest neighbours heuristic

Algorithm 4 takes a list of candidate nearest neighbours candidates and then checks their nearest neighbours. If any are closer than the furthest current list of candidate NNs, they are added to the list of current NN and the current list of NN is pruned appropriately.

Algorithm 4 differs from algorithm 2 in that algorithm 2 exhaustively searches candidates, whereas algorithm 4 stops once a set number of nearest neighbours are found. Algorithm 4 enables longer range connections to be formed.
##### Requires:

base element, q

candidate elements, C

number of nearest neighbours to return, M

layer number, l_c

binary indicator whether to extend candidate list, extendCandidates

binary indicator whether to add discarded elements, keepPrunedConnections

##### Returns:

M nearest neighbours to q

##### Procedure:
R <- empty list

W <- C

if extendCandidates == True:

    for each e in C:
    
        for each adjacent node in layer c within neighborhood of e:
            
            if adjacent node not in W:
            
                W <- merge(W, adjacent node)

W_discardedcandidates <- empty list

while |W| > 0 and |R| < M:

    e <- nearest element in W to q
    
    if e is closer to q compared to any element from R:
    
        R <- merge(R, e)
        
    else:
    
        W_discardedcandidates <- merge(W_discardedcandidates, e)
        
if keepPrunced Connections == True:

    while |W_discardedcandidates| > 0 and |R| < M:
    
        R <- merge(R, nearest element from W_discardedelements to q)
        
return R

#### Algorithm 5 - kNN search
##### Requires:

hnsw graph

query element, q

number of nearest neighbours to return, k

size of dynamic candidate list, ef

##### Returns:

K approximate nearest neighbours to q

##### Procedure:

W < - empty list # list of nearest elements

ep <- get enter point to HNSW graph

L <- top layer of HNSW graph

for each layer, from L to lowest:

    W <- nearest neighbours in layer (algorithm 2)
    
    ep <- get nearest element to q from W
    
return K nearest elements to q from W


## References

[1] Yu. A. Malkov & D. A. Yashunin, <i>Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs</i> (2018), https://arxiv.org/abs/1603.09320

[2] D. J. Watts & S. H. Strogatz, <i>Collective dynamics of 'small-world’ networks</i> (1998), http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf

[X] S. Milgran, The Small-World Problem (1967), http://snap.stanford.edu/class/cs224w-readings/milgram67smallworld.pdf

[X] N. Name, Title (Year), Source

[X] N. Name, Title (Year), Source
