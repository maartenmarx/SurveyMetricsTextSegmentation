# Text segmentation metrics


Let $E$ be a linearly ordered set. We use $E[i]$ to denote the $i-th$ element of $E$ (with counting starting at 0).
A *segmentation* of $E$ is a partial partition of $E$ in which all blocks consist of consecutive elements. We call the blocks *segments*.
We usually consider a *true* segmentation $T$ and a *hypothesized* or *predicted* segmentation $H$. 
Segmentations are thus sets of segments, but sometimes it is handy to use a  notation based on the *first elements* of the segments. For $T=\{t_0,\ldots,t_n\}$, $\bar{T}$ denotes this set of first elements $\{ t_0[0],\ldots t_n[0]\}$. Obviously $T$ and $\bar{T}$ define the same set of segments.


## Metrics based on a sliding window 
Given a segmentation $P$, for $e,e'\in E$, the *distance* $\delta^P(e,e')$ between them is the number of first elements strictly in between $e$ and $e'$. 
Only if $e$ and $e'$ are in the same segment is their distance $0$, and positive otherwise. 

Let $H$ and $T$ be segmentations of $E$.  Then 

$WindowDiff_k(T,H)$ is the mean number of times that $\delta^P(E[i],E[i+k])$ differ for $P=H$ and $P=T$ and $0\leq i\leq |E|-k$.

$P_k(T,H)$ is the mean number of times that $\delta^P(E[i],E[i+k])$ is either both equal or both unequal to $0$ for $P=H$ and $P=T$.


**TODO 1: ook in formules** 

**TODO 2: ook de aangepaste versie behandelen die van alles repareert.** Dat is de versie die we in de Pk exprimenten egbruiken. 

$WindowDiff$ and $P_k$ are defined for *total* partitions. Adjusting them to partial partitions means we have to decide how to handle elements in $E$ but not in $T$ or $H$. We also need to adjust the definition of distance. We do so as follows: For $P$ a partial segmentation  of $E$, let $P^*$ be the smallest total segmentation which contains $P$. Then each "gap" in $P$ is filled by one new segment in $P^*$. We then define distance over these total segmentations, and compute the metrics using all elements in $E$. In essence we compute the distance and the metrics *as if* all gaps were segments as well, the most natural solution. 

