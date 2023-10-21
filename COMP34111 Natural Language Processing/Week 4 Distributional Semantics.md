Linguistic Item - Can be any of: sub words, words, word senses, word phrases, large pieces of text...

A distributional semantic model aims to build a **feature vector** to represent the meaning of an item; this is done by looking at the distribution of the item within text (training corpus). Words with similar meanings will appear in similar contexts.
**Word embedding**: Representing single words as feature vectors.
We can later compare item similarity using vector similarity operations - inner product, cosine.

**Distributional hypothesis**: We can infer the meaning of a word solely based off context. This is the basis of distributional semantics, in opposition to formal linguistics.

### Vector Space Model (VSM)
Simplest approach; each feature is a linguistic item, holding a frequency value.

**Document-term matrix**: Each row is a document; each column is a vocabulary word; Count the frequency of each word in each document.

**Term-context occurrent matrix**: We have frequencies for contexts instead; these may be scoped to a sentence, paragraph, documents (multiple items may share the same context/topic). 

We use the row vectors to compare similarity between words. 

**context window size**: Changes the size of context we are looking at i.e. no. of surrounding words to consider. A small window (1-3 words) captures syntax, larger windows will capture more semantics & meaning. ChatGPT's context window size has increased from 2k tokens (ChatGPT-3) to 8k tokens (ChatGPT 4).

#### Sparseness and Dimensionality
VSM's are very high-dimensional sparse vectors - lot of terms equal to zero. Is inefficient & noisy. A lower-dimensional model will be more efficient, less noisy (but possibly at the loss of data.)

### Latent Semantic Analysis / Singular Value Decomposition (SVD)

Latent Semantic Analysis represents e.g. a Document-term matrix in a lower dimensional form, by (somehow) revealing relationships numerically, using **singular value decomposition**, which is a generalisation of **eigendecomposition**.

#### SVD Method 
Given our VSM matrix $M$, SVD calculates $M_{m\times n} = U_{m\times r}\Sigma_{r\times r} V_{n\times r}^T$, where $r\leq\min(m,n)$. $r$ is the minimum dimensionality possible to represent the data with.
where we have properties:
- $U^TU=V^TV=I$ (Identity matrix). 
- $\Sigma$ is a square diagonal matrix, where the diagonal values are decreasing towards the bottom-right direction. (These are eigenvalues).

#### Truncated SVD
Given our VSM matrix $M$, we compute a *k rank approximation*, with (minimal) error/data loss, $M_{m\times n} \approx U_{m\times k}\Sigma_{k\times k} V_{n\times k}^T$; 
For our k-th rank approximation, we only keep the first k rows in $U,\Sigma,V$. Our k-th rank is the new dimensionality of our *approximated* data.


### Predictive Word Embedding Models

These models have a **predictive task** which predicts the occurring word given the context; The models are trained to optimise their predictions, & to produce optimal word feature vectors (word embeddings) that we can use.

Example Models:
- Word2Vec - Two models - Predicts either the target word (continuous bag-of-words model) or the surrounding context words (continuous skip-gram model).
- gloVe - Aims to predict occurrences of words within the context of a target word; It considers relations between only pairs of words.

### Continuous Bag-of-Words model (CBOW)

The model runs continuously over text with it's context window + target word:

![](misc/Pasted%20image%2020231021193753.png)

- We use context words as features and the target word is predicted on the output layer.
- For our model we arbitrarily choose the word vector dimensionality as N.
- For a context window of size C, we have C input vectors.
- Input feature vectors are of size V, which are encoded using one-hot encoding.
- We share the same weight matrix $W_{V\times N}$ across all input features.
- The hidden layer is of size N; it's input is the average from all the context input features.
- The output layer is of size V. Logistic regression is used as the target word classifier.


### Continuous Skip-gram model
This is works in a similar manner to CBOW.

![](misc/Pasted%20image%2020231021195112.png)

Note the model has to predict context words in the *correct order*.
Each context word is assigned in order to an output layer vector, to then perform backpropagation.

### gloVe Model

Uses a bilinear regression model with the following objective function:

![](misc/Pasted%20image%2020231021201522.png)

Train using gradient descent to optimise the word and context word vectors.