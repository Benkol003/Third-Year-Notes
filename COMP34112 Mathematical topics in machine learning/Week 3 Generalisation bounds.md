Given our model with a value for emperical risk, we want to say within a certain *confidence probability parameter* $\delta$ what the *upper bound* population risk will be - i.e. the worst case scenario:

- $\delta$ is the probability that the population risk is outside this estimated bound
- $\epsilon$ - difference in population vs emperical risk

![](misc/Pasted%20image%2020240404123558.png)

### Hoeffding's inequality

![](misc/Pasted%20image%2020240404123359.png)

Increasing the sample size for a fixed bound also increases our confidence:

![](misc/Pasted%20image%2020240404123725.png)

Hoeffding's inequality terms of model risk:

![](misc/Pasted%20image%2020240404124448.png)

The bound is usually very **loose** - typically dont need a lot of samples, & overestimates a lot:

w.r.t. confidence parameter:

![](misc/Pasted%20image%2020240404124515.png)

![](misc/Pasted%20image%2020240404124752.png)

w.r.t. tolerance $\epsilon$:

![](misc/Pasted%20image%2020240404124717.png)

w.r.t. samples:

![](misc/Pasted%20image%2020240404124735.png)

### Generalisation bound for finite function classes

#### Proof of probability or

Independent events $A,B$. $P(A \cup B) = P(A) + P(B) - P(A \cap B)$.

![](misc/Pasted%20image%2020240404131240.png)

 $P(A) = P(A \cap \neg B) + P(A \cap B)$  and vice versa.
Therefore we have $P(A)+P(B) = P(A \cap \neg B) + P(A \cap B) + P(\neg A \cap B) + P(A \cap B) = P(A \cup B) + P(A \cap B)$. (added the centre area AB twice, so subtract $P(A \cap B)$ for the full area.



#### Boole's inequality

As $P(V_1 \cap V_2) \ge 0$ then $P(V_1 \cup V_2) \le P(V_1) + P(V_2)$.
This holds to any number events, as *booles inequality* - which we can also apply to hoeffding's inequality:

![](misc/Pasted%20image%2020240404141611.png)

However, this is only useful after we finish training our model. It is more useful to get a garunteed upper bound on population risk *before* we spend time training our model, so we know we have chosen the right model family i.e. architecture.

For a function class $\mathcal{F}$ of models, the probability that there exists a model $f_i$ that breaks the generalisation bound is given by 

![](misc/Pasted%20image%2020240404141736.png)

or denoting function class size as $|\mathcal{F}|$ - we get the **uniform deviation bound**:

![](misc/Pasted%20image%2020240404141939.png)

WE can now extract the formula relating the variables. However, **note we are only interested in the case where the population risk is higher than empirical risk** (we will take a model that does better on the population anyday).
Therefore we can adapt this to a one-tailed test (From two-tailed) to finally get the **Generalisation bound**:

#### Generalisation bound

- $P(\exists f \in \mathcal{F}, |R(f)-\hat{R}(f,S_n)| \ge \epsilon) \le \delta = |\mathcal{F}|exp(-2n\epsilon^2)$
(we remove the two as a two-tailed test takes a parameter for probability split across each sides i.e. $\frac{\delta}{2}$ - the 2 gets moved to the RHS.)

![](misc/Pasted%20image%2020240404183920.png)

![](misc/Pasted%20image%2020240404183927.png)

