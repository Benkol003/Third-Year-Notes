### Worst generalisation gap

for a family class, the max possible difference between average population and emperical risk; max (supremum) with the worst model in family class.

![](misc/Pasted%20image%2020240506014018.png)

this is dependent on the sample size, model family, and distribution D, but **not the actual size of the model family**!. (starts to answer  the question - are bigger models better? - not always/not necessarily dependent on model size)

- the term 'training' data may seem ambigous (and probably in previous weeks aswell) - sometimes its used in calculating empirical risk/error as test data like we would for say a pytorch model, other times we are choosing a model function from a family class, or we are iteratively updating weights in gradient descent.

## Rademacher complexity

**Rademacher complexity** is the ability for a function class (of models) to be able to fit/generalise to random noise/data.

![](misc/Pasted%20image%2020240506014211.png)

(average Rademacher complexity is just empirical Rademacher complexity averaged over all possible sample 'training' datasets from D).


## Rademacher compelxity bound on the generalisation gap

![](misc/Pasted%20image%2020240506030208.png)

i.e. the generalisation gap $\le$ 2 $\times$ the Rademacher complexity.




### extra resources

https://web.eecs.umich.edu/~cscott/past_courses/eecs598w14/notes/10_rademacher.pdf
