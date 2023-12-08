TODO pretty dense & confusing topic, notes will only explain it generally, and are not finished

This topic is mainly concerned with this [paper](https://arxiv.org/abs/1706.03762) which proposes both the attention mechanism(s) and the architecture of the transformer model.

## Attention Mechanisms
### Classical Attention formulation, for an RNN

https://www.youtube.com/watch?v=mDZil99CtSU&t=795s

![](misc/Pasted%20image%2020231207194114.png)

Normally we would use the hidden state directly as the decoded word embedding to produce a word; 

- $h_i$ - encoder hidden state (hidden state for i'th input word)
- $\tilde{h_i}$ - decoder hidden state (hidden state for i'th output word) 

1. Calculate similarity between decoder hidden states & all encoder hidden states.

$s_i^1 = similarity(h_i,\tilde{h_1})$ calculates the similarity between encoder hidden state $i$ & decoder hidden state 1 (first output word). We have flexibility over the similarity function, might use e.g. cosine similarity or outer product.

$[a1,a2,a3, \dots] = softmax(s_1^1, s_2^1,\dots)$ - attention values between decoder hidden state and all encoder hidden states; attention values are probabilities with a sum of 1 due to the SoftMax. This has good interpretability as we can see the strength of the connection/contribution between all the input words for the current output word.

2. Formulate another new representation vector as an attention-based hidden state:

$\tilde{h_1^a} = a_1h_1 + a_2h_2 + \dots$ - we combine the encoder states as a sum weighted from their attention values.

3. Formulate the final output representation - $\hat{h_1} = [\tilde{h_1^a},\tilde{h_1}]$ - concatenate the hidden state from the decoder and the attention-based hidden state.

4. We use $\tilde{h_1}$ to predict the output word - pass it through a prediction layer (TODO what is this).

5. We use the output from the prediction layer as the next decoder hidden state - this is known as **autoregression**. (See the red arrows).

Our whole RNN looks like this to formulate the entire output sequence:

![](misc/Pasted%20image%2020231207200243.png)


- [Inner product](https://en.wikipedia.org/wiki/Frobenius_inner_product) - takes two matrices (of the same size) and produces a scalar; multiplies elements at the same position together and sums the elements.
![](misc/Pasted%20image%2020231207205929.png)

![](misc/Pasted%20image%2020231207205941.png)

#### Another representation of the attention RNN

This is leu with the attention mechanism formula provided in the paper

![](misc/Pasted%20image%2020231208004108.png)

Again you can see the attention $Z = softmax(QV^T)V$ is applied for each decoder hidden state.

### Generalised Attention Mechanism

![](misc/Pasted%20image%2020231208004317.png)

In terms of our encoder-decoder RNN:

- $d_k$ is the length of our hidden states
- $Q$ is our encoder states
- $K$ = $V$ = decoder states, however the generalisation allows for two different sets of representations, one for calculating similarity ($K$) & one for calculating the weighted output embedding ($V$ is weighted to compute $Z$). (Yet to see $K$ & $V$ be different things)

### Multi Head attention

(Seriously idk what's going on here)

https://youtu.be/A1eUVxscNq8

We calculate the attention mechanism multiple times, using learnt *linear projections* or sets of weights $\set{W^q_1,W^k_1,W^V_1},\set{W^q_2,W^k_2,W^V_2}, \dots$, called *attention heads* on $Q,K,V$ to transform them. original paper uses 8 attention heads. We calculate and concatenate our attention heads, and then again project the output with $W^O$. This allows having attention drawn to multiple parts of our encoder sequence.

In practice we use the same linear projection matrix for all heads (in the transformer model).

(Presumably the attention head matrices are parameters to be learnt via backpropagation during training.)

![](misc/Pasted%20image%2020231208015117.png)

![](misc/Pasted%20image%2020231208015142.png)

## Transformer Models

- Used by all new state of the art language models such as BERT, GPT, T5.

General architecture overview:

![](misc/Pasted%20image%2020231208021008.png)

### Explanation of encoder block

#### Positional encoding

![](misc/Pasted%20image%2020231208040015.png)

Our multi-head attention is applied directly to the encodings; since we are not using an RNN we encode the position of each word as sine waves.

- $d_{model}$ - the dimensionality of our word embeddings
- $x_i$ - word embedding for i'th word

We produce a position value for an embedding using the formula of $p_i$ - has two cases for odd/even with sine/cosine. We add this value to all elements in the word embedding ($g_i=p_i+x_i$).

#### Self attention

The next step is to pass the positional embeddings to the attention layer - we use self attention - the positional embeddings are used for $Q,K,V$.

![](misc/Pasted%20image%2020231208040722.png)

#### Add & Normalise Layer

Is proposed in this [paper](https://arxiv.org/pdf/1607.06450.pdf).

The idea is to prevent information from being lost from a layer; so it adds input + output data (or $F(x)+x$) and normalises.

![](misc/Pasted%20image%2020231208041129.png)

We then value-shift and scale the data to fit a normal distribution of $a$, with $\mu = 0, \sigma=1$ which are calculated thus:

![](misc/Pasted%20image%2020231208041355.png)

TODO explain terms.

#### Feedforward Neural Network architecture

Contains one hidden layer with ReLU activation, output layer has linear activation. This is the layer that actually learns/creates the output embeddings (the other layers are for complex attention only.)

$FFN(x) = max(0, xW1 + b1)W2 + b2$.


### Explanation of decoder block


#### Masked multi-head attention

https://stackoverflow.com/questions/58127059/how-to-understand-masked-multi-head-attention-in-transformer

Is needed so we can perform parallel operations across all words (as this isn't a RNN).  We run the decoder block $n$ times for n output words; The output probabilities (same as embeddings) are fed back into the input on the next iteration of the decoder to produce the next word.
If the decoder has iterated $i$ times, the output matrix only has useful output word embeddings for the first $i$ words, and the rest are ignored. 

For masked attention we mask s.t. we only calculate attention for $i-1$ words in the output embeddings, the remaining words ahead are masked. I.e. to calculate very first output word on the first decoder iteration the entire output embedding matrix is masked. In the paper masked values are set to $-\infty$ (before SoftMax converts them into probability embeddings).
(also remember that we are still using self attention).


![](misc/Pasted%20image%2020231208044404.png)

#### Combining encoder output in multi-head attention layer

![](misc/Pasted%20image%2020231208050109.png)

Given $Z = Attention(Q,K,V)$, 
- the output of decoder Add&Norm layer is used for the query, Q.
- The output of the encoder is used for both keys and values, $K$ & $V$.

You should be able to figure out the rest of the architecture layers; note dropout is also used on layers for regularisation. 

### Transformer Model Hyperparameters

![](misc/Pasted%20image%2020231208051158.png)


- [BERT](https://arxiv.org/pdf/1810.04805.pdf) uses only the encoder blocks of transformer (bidirectional **encoder** ...!), which are stacked on top of each other.

https://huggingface.co/blog/bert-101#3-bert-model-size--architecture

![](misc/Pasted%20image%2020231208051529.png)

![](misc/Pasted%20image%2020231208051539.png)

- The number of attention is for multi-head attention.
- Hidden size is the size of the internal word embeddings.

How does bert create positional embeddings? it only has input id's to work with a vocabulary