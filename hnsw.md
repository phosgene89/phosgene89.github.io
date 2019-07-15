## HNSW 

k-nearest neighbours (k-NN) is a commonly used method in data science. An issue with k-NN is that it is inefficient in high dimensional, high volume datasets. One trick to improve performance is to search for approximate nearest neighbours rather than exact nearest neighbours. This means that instead of garuanteeing that our algorithm returns the k-nearest neighbours, we guarantee that it will return k neighbours that are less than a pre-specified distance away. This is often good enough and provides a sizable speed boost. The current state of the art in high dimensional approximate k-NN is hierarchical navigable small worlds (HNSW), proposed by Malkov & Yashunin (2017). This article will introduce the concept and provide some code to enable you to try it out in your own projects.

The general idea is to build a multilayer network and to search for the approximate nearest neighbour in a top-down, greedy fashion. 

#### Background
###### Navigable small world
A famous experiment by Stanley Milgram tasked participants with delivering a package to someone that they most likely did not know. If they did not personally know the target, they were instructed to pass the package onto someone that they did know. This person would then repeat the procedure until the package found its intended recipient. Milgran found that the average number of times the package exchanged hands was 6 - much smaller than expected. The interpretation of this was that locally connected groups of people often had individuals in them who were connected in some way to people very far away. These long range connections allowed the packages to reach the recipient faster than the exclusive usage of short range connections. 

The figure below demonstrates how this helps. Suppose we start at point A and want to reach point B, but we can only get through by traversing through nodes we are locally connected to. In the first figure, all connections are short-range, hence we will have to take x steps to get to our target. In the second figure, the presence of a few short range connections allows us to bypass many steps and reach our target in only y steps. This is the heart of HNSW.

# NSW
Navigable small world networks were created to model networks that displayed small world characteristics similar to the ones in Milgram's experiment. NSW networks perform nearest neighbour searches by navigating through local connections to "hub", then moving from these hubs toward the target. While these networks do allow for more efficient nearest-neighbour searches, their complexity scaling with network size is from polylogarithmic to power law - ideally we would like it to be logarithmic.

path length grows polylogarithmically with network size.

For scale free, the scaling can be as bad as a power law

scale free models go from zoom out to zoom in phases...hnsw sets up the graph so that it starts directly at the zoom in phase, skipping the zoom out phase entirely.

###### Hierarchical Navigable small world
HNSW improves on the efficiency of NSW by starting the search from "hubs" with long range connections, thus cutting out the initial "zoom out" phase of NSW. In comparison to NSW, the complexity of HNSW grows logarithmically with network size.

The general procedure of HNSW is to create a layered graph (hierarchies) of connected points, with lower layers being more populated than higher layers. After constructing this hierarchy, a nearest neighbour does a greedy search for the closest point in the bottom layer. It then enters the next layer through this point and begins another greedy search for a nearest neighbour - with the constraint that it compares itself only to points connected to the current node. The process repeats until k neighbours a maximum of epsilon away are found.

!(hnsw1)[https://github.com/phosgene89/phosgene89.github.io/blob/master/hnsw/hnsw.PNG]

One of the crucial features of HNSW is that long range links are not uniformly random. Were this the case, we would expect that half the time a long range link would lead us closer to our target, but lead us further the other half - leading to no net benefit. 

##### Reason for approximate search

# Construction parameters
Main parameter to optimise is the number of layers in the graph. Lowering the number of layers used means that there is less overlap (improving performance), but increases the average hop number (which decreases performance). Hence the number of layers must be tuned. A good starting point is 1/ln(M).

The max number of connections for each node in the graph must be tuned for recall vs efficiency. Higher numbers for this parameter result in decreased search performance, but improved recall. A good starting point is 2*M.

efConstruction is straightforward to tweak. It should lead to >0.95 recall on a sample set during the construction phase.

Finally, the number of nearest neighbours to search from candidates in algorithm 4. This must also be balanced between recall and performance time.

##### Performance benchmarks and potential issues

Benchmarks between current nearest neighbour search algorithms show that HNSW is a consistent strong-performer (https://github.com/erikbern/ann-benchmarks)

A recent paper places doubt on the advantages of HNSW over NSW. Speed efficiency seems to be present only for d<8. I am not an expert on nearest neighbour searches, so I cannot offer any commentary on this, but it's worth knowing about. https://arxiv.org/abs/1904.02077

http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf

# Key stuff

Fully graph based

logarithmic complexity scaling

select neighbours by a heuristic

Alternatives are polylogarithmic at best



HNSW improves on NSW

Core concept is to create multilayer graph separated by length scale, where each node has a set number of connections. A search will thus be independant of the network size, enabling logarithmic scaling.

use exponentially decaying probability to of an element being in a layer.

HNSW reduces to NSW if we merge all layers.



# Algorithm descriptions

### Algorithm 1 - Graph construction
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


### Algorithm 2 - Search layer
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


### Algorithm 4 - Nearest neighbours heuristic
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

### Algorithm 5 - kNN search
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


# References

[1] Yu. A. Malkov & D. A. Yashunin, <i>Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs</i> (2018), https://arxiv.org/abs/1603.09320

[2] D. J. Watts & S. H. Strogatz, <i>Collective dynamics of 'small-world’ networks</i> (1998), http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf

[X] N. Name, Title (Year), Source milgram (http://snap.stanford.edu/class/cs224w-readings/milgram67smallworld.pdf)

[X] N. Name, Title (Year), Source

[X] N. Name, Title (Year), Source
