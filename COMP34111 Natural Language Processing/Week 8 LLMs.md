- LLM - Large language Model

- BERT LLM - uses the transformer encoder block for its model, to produce contextual word embeddings
- GPT LLM - uses the transformer decoder block for a generative text task
- T5 Model uses transformer encoder-decoder model; performs text-to-text tasks, e.g. language translation, or casting classification into a text-to-text task.

### Multilayer LLM's

![](misc/Pasted%20image%2020231219160053.png)

- BERT uses a multilayer encoder; all inputs $x_1, \dots  x_5$ are passed in and propagate up to the top layer $z_1, \dots z_5$.
- GPT uses a multilayer decoder; here instead data works through multiple layer sequentially; input context ($x_0$) passes through layer to top layer to produce $z_1$, which is then used as input $x_1$ for next input context
- T5 model uses both of multilayer encoder - decoder together.

## LLM Training

### Bert Pretraining

Uses two datasets:
- BooksCorpus - 7000 unpublished free books, 800 million words
- Wikipedia - 2.5 Billion words

*pretrained* with two learning tasks:
- Masked Language Model (MLM) pretraining
- Next Sentence Prediction (NSP) pretraining

####  Masked Language Model
[paper](https://arxiv.org/abs/2104.06644)

Helps BERT build relationships between words

1. Randomly select 15% of token positions.
2. For each position:
	- 10% probability we dont change the word
	- 80% probability replace with *[MASK]* token
	- 10% probability replace with random token/word

BERT model then used to predict the masked tokens and use the error in training.

#### Next Sentence Prediction
[paper](https://arxiv.org/abs/2109.03564)

Helps BERT establish longer-distance dependencies across sentences.

choose two sentences, A & B:
- 50% probability that sentence B is *directly after* sentence A.
- 50% probability that sentence B is not directly after A, can be any other sentence in the corpus (may be behind or a distance ahead).

and provide BERT with an input [CLS] A [SEP] B [EOS].

Use BERT with a classifier & train this to guess correctly whether B follows A:

![](misc/Pasted%20image%2020240117172557.png)

### BERT Finetuning
We can use GLUE finetuning for a generalised, 'out-of-the-box' model; or we can use our own dataset if the application is  very specific.

#### Finetuning with GLUE

- GLUE benchmark consists of 9 english sentence understanding tasks, converted to classification tasks.

Single sentence classification:
- CoLA - Predict whether a sentence is linguistically acceptable or not
- SST2 - Predict positive/negative sentiment of movie reviews

Sentence pair classification
(Semantic equivalence - sentences have same meaning)

- MRPC - Predict if pair of sentences are semantically equivalent
- QQP - Predict if two questions taken from Quora are semantically equivalent
- STS-B - Predict sentence pair semantic similarity with a 1-5 score.

Natural Language Inference 
- MNLI & RTE (smaller dataset) - Predict one of 3 things: if 2nd sentence (B) is an entailment of A; i.e. A can imply B; or, if B is neutral; or a contradiction of A.
- SQuAD: Given a question & passage of wikipedia containing the answer, predict the text span of the answer in the passage.
- (Winograd schema challenge is in GLUE, not explained in notes, probably bc BERT paper does not use it due to issues with the dataset.)


![](misc/Pasted%20image%2020240117182615.png)

#### SQuAD
The other training tasks are easily done with a classifier layer; however for SQuAD, where we are performing a *machine comprehension task*, is more complicated. From this [paper](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/reports/default/15792151.pdf) we use an additional layer known as BiDAF (Bidirectional Attention Flow for Machine Comprehension) (in this [paper](https://arxiv.org/pdf/1611.01603.pdf)).

### GPT training
[GPT paper](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf)

As we know GPT is *autoregressive decoder*; Our goal is to find a sequence of predicted tokens $w_i,\dots w_{i-k}$ that maximises the probability of those tokens occurring given any previous tokens, including those generated or those provided as input context. (I.e. the predicted token is added to the previous input tokens).

![](misc/Pasted%20image%2020240117182757.png)

GPT is also trained on the GLUE benchmark like BERT.

Notes make no effort at all to explain how this works, so heres a screenshot of the slide to memorise the key points:

![](misc/Pasted%20image%2020240118164441.png)

Somehow we pass concatenated input & output & use a final representation vector passed to a classifier (architecture of this not explained)

### Instruction Fine Tuning

- FLAN - Fine Tuned Language Net
- Task Category - setup of the use of the data with the model, e.g. extractive question asnwering, query generation, context generation (these can be used for SQuAD)
- Task is a pair of \<dataset, task category\>

#### Chain-of-Thought Annotation
(prompt engineering) With our training data, instead of having a question and an answer we annotate the chain of thought that allows the LLM to be trained on the logical train of thought to reach the answer; enhances the LLM's ability to handle multi-step reasoning tasks. The LLM will also be able to provide a response that is backed up with its own chain of thought rather than just an answer.

![](misc/Pasted%20image%2020240118175535.png)

