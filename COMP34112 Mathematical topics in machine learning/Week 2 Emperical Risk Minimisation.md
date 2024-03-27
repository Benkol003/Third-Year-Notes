- (Probability) Simplex: Is a probability distribution involving $k$ classes; probability of all classes sum to 1. (It is also a generalisation of a triangle to n dimensions.)
![](misc/Pasted%20image%2020240304144247.png)

If we have a $k$-class classifier, it forms a $k$-simplex and has an output space of $[0,1]^k$ (though we are restricted on combinations of each variable to sum to 1.)

A 3-class classifier can output values of the face of this triangle:

![](misc/Pasted%20image%2020240304144616.png)

- Models: A function $f: \mathcal{X}\rightarrow\mathcal{Y}$ 
- Family Models: Neural network, SVM, decision tree etc, aswell as their hyperparameters e.g. layer + layer size distuingish them as different families of models. The set of all possible models is denoted $\Omega$.
- Input/Output spaces. MNIST is $\mathcal{X},\mathcal(Y)$