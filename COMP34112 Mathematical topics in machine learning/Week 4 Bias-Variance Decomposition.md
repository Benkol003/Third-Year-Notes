[original paper](https://www.dam.brown.edu/people/documents/bias-variance.pdf)
#### Difference between bias and variance

If we have models from the same family, trained on different training dataset samples $S_n$, (lets assume we find the emperical risk minimiser). If we plot predictions from these different models
it may look like thus:


![](misc/Pasted%20image%2020240404201156.png)

(aka accuracy vs precision)
There is also an additional factor called noise which we cant be represented visually.


#### Expected Risk

**for compactness notes will denote $f(x;S_n)$ as $f(x)$ - the model is still dependently trained on the sample set $S_n$. 

$\mathbb{E}_{S_n}$ - average over all possible training datasets of size $n$
$\mathbb{E}_{(x,y)\sim D}$ - average over all data points in the population (infinite) dataset

The expected risk is the population risk for a model, averaged over all possible training sets (& models trained on each possible sample).
i.e. the expected population risk for an arbitrary model and sample of size n.

![](misc/Pasted%20image%2020240404215333.png)

### Bias-variance decomposition of expected squared risk
(geman et al. 1992)

Note that this is **specific to squred risk**. Decomposition holds for other losses e.g. cross entropy, but with different formula.

![](misc/Pasted%20image%2020240404215714.png)

- Expected model $\mathbb{E}_{S_n}[f(x)]$ - averaged sum of each model trained with sample $S_n$, weighted by $p(S_n)$.
- $E_{y|x}$ - expected value of y conditional on a given **fixed** x - we also take the average of $E_{y|x}$ over $x$ with $E_x[\dots]$ exterior to this
- Infact the noise, bias & variance values are all averaged with $E_x$.

![](misc/Pasted%20image%2020240404221039.png)

![](misc/Pasted%20image%2020240404221400.png)

TODO why not exactly equal to Bayes Risk?

### Decomposition of cross entropy risk

#### Kullbach-Liebler divergence

For n classes: 

$$K(y || f(x)) := \sum\limits_{c=1}^n y_c ln \frac{y_c}{f_c(x)}$$

- $y_c$ denotes the true class distribution e.g. for three classes $[0.1,0.1,0.8]$ - class 3 occurs 80% of the time.
- $f_c$ denotes the probability predicted by model f with input 

#### Converting KL-divergence form to cross-entropy

If we make assumptions:
-  $y$ now represents a one-hot encoded vector for an single input sample $x$
- We assume $0 \ln 0 = 0$ - re more accurately $\lim_{x\rightarrow 0} x \ln x = 0$ - due to [L'Hôpital's rule](https://en.wikipedia.org/wiki/L%27H%C3%B4pital%27s_rule)

Then KL-divergence is equivalent to cross entropy:

![](misc/Pasted%20image%2020240404235622.png)

![](misc/Pasted%20image%2020240404235731.png)

TODO I think these are the proofs/material, TODO read
https://en.wikipedia.org/wiki/Bregman_divergence
https://arxiv.org/pdf/2002.11328.pdf
http://davidpfau.com/assets/generalized_bvd_proof.pdf

### Bias-Variance tradeoff

As we increase model complexity we trade off reduced bias for increased variation;

![](misc/Pasted%20image%2020240405001301.png)

### Model overparameterisation & double descent
The notes are pretty good i cant lie

![](misc/Pasted%20image%2020240405001547.png)
