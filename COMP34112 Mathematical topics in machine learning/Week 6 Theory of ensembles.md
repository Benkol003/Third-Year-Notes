This week's content is entirely in this [paper's](https://jmlr.org/papers/volume24/23-0041/23-0041.pdf) work by the lecturers.

notation:

![](misc/Pasted%20image%2020240504220623.png)

 - $D$ is the distribution of all possible training sets
 - $\mathring{q}$ or the centroid of the model distribution, for the expected model across $D$, 
 - BRUH IDK WHATS GOING ON HERE, WHY WE USING ARGMIN, MAKES NO SENSE.

![](misc/Pasted%20image%2020240505003714.png)

 - the centroid combiner is the label with the lowest loss, of a prebuilt model ensemble $q$

- "diversity is a measure of ensemble member disagreement, independent of the label."

we have a *bias-variance-diversity* tradeoff for ensembles.


![](misc/Pasted%20image%2020240504201424.png)

- ensemble diversity


- generalised ambiguity decomposition for a loss function
- 
![](misc/Pasted%20image%2020240505001156.png)
for the typical bias-variance decomposition depending on the loss used we get different versions:

![](misc/Pasted%20image%2020240504235940.png)

decomposition for squared risk:

![](misc/Pasted%20image%2020240505000011.png)

and for KL divergence:

![](misc/Pasted%20image%2020240505000048.png)

however we use bregman divergences to generalise this for bias-variance-*diversity* tradeoff:

![](misc/Pasted%20image%2020240505001232.png)

#### centroid combiner

![](misc/Pasted%20image%2020240505000327.png)



