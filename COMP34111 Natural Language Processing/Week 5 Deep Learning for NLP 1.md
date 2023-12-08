**Deep Learning**: Emphasises neural networks with a large number of hidden layers; Usually concerned with building a model that is end-to-end: Input -> hidden layers -> output. DL is widely used in NLP.

### Deep Learning Tasks

- **Sequence distribution**: E.g. a sequence of words; Calculate the probability of a sequence.
	Useful for text generation/completion - maximise a sequence probability given missing words.

- **Sequence Classification**: Learn a representation vector of a sequence (similar to word embedding); (to be used to classify the sequence).
	Uses: *Sentiment analysis*, e.g. spam filtering.

- **Sequence Labelling**: Learn a representation vector for each element in a sequence; i.e. produce a word vector for all words in a sequence. Use these to produce a class label for each state.
	Uses: POS tagging; *name entity recognition*, assigning words to name classes/topic categories.
	
- **Seq2Seq Learning**: Convert between an input sequence & output sequence. Usually encode important input sequence information (encoder); then use a decoder to generate output sequence.
	Uses: Machine translation (e.g. English -> Chinese); audio to text.

	Encoder-Decoder looks like:

	![](misc/Pasted%20image%2020231024153752.png)

	Where we have neural networks (typically RNN's/LSTM's) for the encoder & decoder

#### Activation Functions

- Sigmoid function
	- Logistic function $\in [0,1]$  
	- Hyperbolic tangent $\in [-1,1]$ 
- ReLU
- Parametric ReLU

**Multilayer Perceptron / Feedforward neural network**: We have input layer, *at least* one hidden layer, & an output layer.
A **single layer perceptron** directly connects the input to the output layer.


#### Neural Language Model

![](misc/Pasted%20image%2020231024154939.png)

For k words in the context, we take their word vectors $x_1,x_2,\dots x_k$ and concatenate the features together to form the input for the network as $x = [x1,\dots x_k]$.

### Recurrent Neural Networks

#### Representation of one layer in RNN

![](misc/Pasted%20image%2020231024155918.png)

You can see we combine the input, initial state, and the cell operation function/gating mechanism to produce the output vector. Between each layer we have a unique set of weights; $w_0,w_1,\dots w_n$.
Note we commonly use the hidden state $h_n$ as the output at layer $n$.

**Vanilla RNN Cell**: We use a single neuron layer; for an activation function $\varphi$, the output state $h_k = \varphi(W_xx_k+W_hh_{k-1}+b)$.  Not commonly used due to lack of memory and gradient problem...

**Vanishing/exploding gradient**: When we backpropagate, because the gradients travel through so many layers they are susceptible to becoming very large/small due to the *chain rule* as they reach the input layer, causing slow training or instability.

**Batch Normalisation** can solve exploding & vanishing gradient problems. We can also use **gradient clipping** to solve exploding gradients.


### LSTM & GRU
LSTM, & it's simplified parameter version GRU, are effective methods for solving a vanishing gradient problem.
For each layer we have a more complex model of more formulas and parameters:

Comparison of cell structures:

![](misc/Pasted%20image%2020231024172624.png)

#### LSTM formulas

![](misc/Pasted%20image%2020231024223450.png)

Comparison to Activation function:

![](misc/Pasted%20image%2020231024182320.png)

- $\sigma$ - sigmoid function 
- $\circ$ - element-wise multiplication/Hadamard product
- Activation function is set as hyperbolic tangent sigmoid (tanh)
- As the functions are in the range $\sigma(x)\in(0,1)$ & $tanh(x)\in(-1,1)$ we can infer the range of values of the vector variables respectively; in practice they are close to the range limits, as they act as **gates**, which control adding and removing internal state, and what state to output, by selective choice via the gate functions of which features to use/change.
- Also the hidden state $h_k$ is updated 

- $o_k$ - output gate; $f_k$ - forget gate; $i_k$  - input/update gate; $c_k$ - internal cell state/long term memory; $h_k$ - hidden state/short-term memory that is passed to the next layer.

#### Gated Recurrent Units (GRU)
Are a simplification of LSTM:

Formulas:

![](misc/Pasted%20image%2020231024223511.png)

![](misc/Pasted%20image%2020231024222555.png)

This time we only have gates: $r_k$ - reset gate, & $u_k$ update gates, to control cell ($c_k$) & hidden ($h_k$) state.


### Neural Language Models with RNN's

An assortment of neural architectures used in NLP.

#### 1. Feedforward NN model word prediction

![](misc/Pasted%20image%2020231024224159.png)

The word vectors for the previous words are concatenated together in order and used as the input features to the neural net.
(Or more naively we can use *raw* representation via one-hot encoding of vocabulary words as features)

#### 2. RNN word prediction

![](misc/Pasted%20image%2020231024224314.png)

The n'th word is passed in at the n'th layer; predicted word vector is produced at final layer.

#### RNN POS tagger

![](misc/Pasted%20image%2020231025002625.png)

We use the hidden state to produce an output at each stage for predicted tag.

#### Seq2Seq with RNN-based Encoder-Decoder
(Used for machine translation)

We provide the input sequence to the encoder.
We then use it's output as the first input for the decoder. Then for the remaining future layers, output from one layer is used as the input for the one in front. This is called **autoregressive encoding**.

![](misc/Pasted%20image%2020231025004954.png)

Machine Translation example:

![](misc/Pasted%20image%2020231025004719.png)

(**Note**: Diagram is missing annotation: $\hat{e_0} = h_3$)

(https://arxiv.org/pdf/1409.3215v3.pdf)

**Information bottleneck**: Occurs between the encoder & decoder; above the information is bottlenecked through the hidden state $\tilde{h_0}\equiv h_3$. Forces only relevant information to be conserved; helps prevent overfitting.

#### Bi-directional RNN

Allows use to use both left and right sided context in a sequence.
We use two RNN's; one is passed a reversed input sequence; then concatenate the output vectors of both RNN's into one output vector.

![](misc/Pasted%20image%2020231025010358.png)

BERT: bidirectional RNN model used to produce word embeddings for words given left + right context. Models like word2vec and gloVe combine all ambiguous senses of a word into one word embedding; However BERT can disambiguate by using context to produce different word embeddings for the same word.

#### Stacked/multi-layer RNN

![](misc/Pasted%20image%2020231025011024.png)

We stack RNN's with single connection between layers at the same depth; akin to single vs multi-layer perceptron's; Gives better performance and can have a more complex trained model; Most don't exceed 4 stacked RNN's.

