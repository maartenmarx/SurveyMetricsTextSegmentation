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

## Metrics based on confusion table.

With these metrics we mean the satand precision, recall and their harmonic mean F1. Given two sets $h$ and $t$ these are defined as usual as 

\begin{eqnarray}
 P(h,t)&= &\frac{\vert h\cap t\vert}{\vert h\vert} \\
 R(h,t) &= &\frac{\vert h\cap t\vert}{\vert t\vert} \\
 F1(h,t)&=& \frac{\vert h\cap t\vert}{\vert h\cap t\vert + .5 (
 \vert h\setminus t\vert + \vert t\setminus h\vert ) } 
\end{eqnarray}


### Bcubed

Define $B=\{(t,h)\in T\times H\mid \exists e\in E: e \in t\cap h\}$. Then the BCubed precision is  simply the mean $P(t,h)$ over all pairs in $B$. And similarly for recall and F1. We denote these by the subscript $BC$. 

In the literature the BCUBED measure is the mean F1 value over $B$, i.e.

$$ F1_{BC} = \frac{\Sigma_{(t,h)\in B} F1(t,h)}{\vert B\vert}. $$
Note that $1\leq \vert B\vert \leq \vert E\vert$, corersponding to the two extreme degenerate clusterings. 

The BCUBED metric bypasses the need for aligning hypothesised and true segments by computing something different: for each element $e$ in the domain, it compares the (two unique) hypothesized and true segments containing that element using the standard confusion table metrics. So it computes these metrics relative to that element $e$: i.e., precision means how many of the elements predicted in the same cluster as $e$ are indeed in the true cluster of $e$. Then the mean over all elements is taken. Note that the above definition is "more efficient" as it does not recompute values repeatedly as in the common definition in which one normalizes by dividing by $\vert E\vert$. 

### Panoptic Quality

The Panoptic Quality metric first makes an alignment between true and hypothesized segments and then computes the metrics over these alignments. 
Alignments are partial isomorphisms; every predicted segment is mached to at most one true segment and vice-verse.
Kirilov et al require that this alignment is both natural and efficiently computable and propose that $t$ and $h$ are aligned if the overlap between $t$ and $h$ is larger than the non-overlap, in formulas $\vert t\cap h\vert > \vert t\symdiff h\vert$. This indeed induces an alignment but there is a weaker natural condition possible that also guarantes an algignment and which can be shown to be the \emph{weakest possible} condition:

$$ t \mbox{ and } h \mbox{ are aligned iff } \vert t\cap h\vert > \vert t\setminus h\vert \mbox{ and } \vert t\cap h\vert > \vert h\setminus t\vert.$$

Let $B$  consists of all pairs in $T\times H$ which are thus aligned.  
<!--Define $TP=B, FP= \{h\in H\mid \not exists t : (h,t) \in B\}$ and FP= \{t\in T\mid \not exists h : (h,t) \in B\}$, and define precision, recall and F1 using these notions.  -->
The panoptic quality metrics are defined directly from $H$ and $T$ in the standard manner, except that the True Positives (which are in essence the relation $B$) are not exactly equal but aligned. We solve this by substituting the the true part of an aligned pair by its predicted segment. Thus let $\bar{T}$ be $\{t\in T\mid \not \exists h: (h,t)\in B\} \cup \{h\in H\mid \exists t: (h,t)\in B\}$. Then P, R and $F1$ are simply defuined using $H$ and $\bar{T}$. We denote these by the subscript $PQ$.

Now, these maeasures do not take the quality of the match or the fit into account: whenever two segments can be aligned they are a true positive and they count fully as one. Within the Panptic Quality way of measuring this is repaied by \emph{weighting} each true positive pair $(t,h)$ by its \emph{IoU} value, which is simply its Jaccard distance $\frac{\vert h\cap t\vert}{\vert h\cup t\vert}$. So the weighted P, R and F1 values are derived by multiplying them by the mean $IoU$ value calcultaed over all true positives in $B$.  

\todo{Ruben, wat denk je hiervan? Het is wel meer uniform toch? We kunnen die plaatjes uit ELM en PQ papers alicht nog inzetten}.

\todo{WinPR moet er nog in. Ruben kan je daar naar kijken? En ook naar die verbeteringen van Pk en windowdiff die zijn voorgesteld. Ik gebruik die verbeteringen in mijn notebook waar ik die exeirmenten doe om die claim onderuit te halen. Dat is allemaal flauw gedoe maarwel belangrijk om nu voor eens en voor altijd netjes op te schrijven. }

\todo{Het moet ons met dit apparaat toch lukken dat winPR goed te krijgen!!!!}
