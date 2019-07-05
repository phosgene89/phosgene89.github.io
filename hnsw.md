## HNSW 

k-nearest neighbours (k-NN) is a commonly used method in data science. An issue with k-NN is that it is inefficient in high dimensional, high volume datasets. One trick to improve performance is to search for approximate nearest neighbours rather than exact nearest neighbours. This means that instead of garuanteeing that our algorithm returns the k-nearest neighbours, we guarantee that it will return k neighbours that are less than a pre-specified distance away. This is often good enough and provides a sizable speed boost. The current state of the art in high dimensional approximate k-NN is hierarchical navigable small worlds (HNSW), proposed by Malkov & Yashunin (2017). This article will introduce the concept and provide some code to enable you to try it out in your own projects.

The general idea is to build a multilayer network and to search for the approximate nearest neighbour in a top-down, greedy fashion. 

#### Background
###### Navigable small world
A famous experiment by Stanley Milgram (https://en.wikipedia.org/wiki/Small-world_experiment) tasked participants with delivering a package to someone that they most likely did not know. If they did not personally know the target, they were instructed to pass the package onto someone that they did know. This person would then repeat the procedure until the package found its intended recipient. Milgran found that the average number of times the package exchanged hands was 6 - much smaller than expected. The interpretation of this was that locally connected groups of people often had individuals in them who were connected in some way to people very far away. These long range connections allowed the packages to reach the recipient faster than the exclusive usage of short range connections. 

The figure below demonstrates how this helps. Suppose we start at point A and want to reach point B, but we can only get through by traversing through nodes we are locally connected to. In the first figure, all connections are short-range, hence we will have to take x steps to get to our target. In the second figure, the presence of a few short range connections allows us to bypass many steps and reach our target in only y steps. This is the heart of HNSW.

###### Hierarchical Navigable small world
Hierarchical navigable small worlds creates a layered graph (hierarchies) of connected points, with lower layers being more populated than higher layers. After constructing this hierarchy, a nearest neighbour does a greedy search for the closest point in the bottom layer. It then enters the next layer through this point and begins another greedy search for a nearest neighbour - with the constraint that it compares itself only to points connected to the current node. The process repeats until k neighbours a maximum of epsilon away are found.

One of the crucial features of HNSW is that long range links are not uniformly random. Were this the case, we would expect that half the time a long range link would lead us closer to our target, but lead us further the other half - leading to no net benefit. 

##### Reason for approximate search

##### Coded example

##### Performance benchmarks and potential issues

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


# NSW

path length grows polylogarithmically with network size.

For scale free, the scaling can be as bad as a power law

While somewhat efficient for searching, constructing the graphs requires global knowledge of the link distribution, making their computation very difficult.


Kleinberg models of NSW requires knowledge of data distribution beforehand as well.


scale free models go from zoom out to zoom in phases...hnsw sets up the graph so that it starts directly at the zoom in phase, skipping the zoom out phase entirely.
