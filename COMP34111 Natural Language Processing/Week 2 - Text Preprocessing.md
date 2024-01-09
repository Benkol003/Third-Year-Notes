Free NLP [book!](https://web.stanford.edu/~jurafsky/slp3/)

### Language identification and Genre/Domain
Identify the language being used in the text (English), and the area of discourse for the material (news, computer science).

### Tokenization
Break down language into a whitespace-delimited sequence. However some languages e.g. Chinese aren't delimited by whitespace. However we may have punctuation and other non-text symbols (punctuation, numbers) accompanying the tokenized text.

##### Tokens of equivalent form
	Abbreviations: They are speaking, They're speaking. We could convert one to the other e.g. I'm -> I am, However one may be considered more formal speech.
	Manchester-based - is this one or two tokens?
	Possesives: Tory's fuckup - The two words are associated but also seperate tokens in their own right

There are no hard rules for how to peform tokenization, however it is important to be **consistent** throughout your system, and knowing where to *split*, not when to combine (which is double the work) - avoid over-splitting.

#### Normalisation
- Walk, walking, walked - we could consider these equivalent.
- Case folding i.e. convert to lowercase - important in some situations e.g. search engines.

- **Lemmetisation**: This is computationally intensive. We also have an issue when a word appears that is not in the dictionary (Out-of-Dictionary, OOV).
	  {am,are,is} -> be.
	  {ball, balls, ball's, balled} -> ball
- **Morphological Analysis**: Words are not the most basic unit of text, instead we have *morphemes* - prefix, suffix, stem. 
	demoralisation -> de, moral, isation.
**Word inflection**: Words are modified for different uses e.g. temporal: *shot, shooting.*
![](misc/Pasted%20image%2020231003103949.png)

**Stemming**: We *remove* additional morphological data by removing suffixes (and possibly prefixes). However we can commit under/over-stemming.

*Porter Stemmer*: One of most used and accurate stemming algorithms for english language.
![](misc/Pasted%20image%2020231003104545.png)

#### Alternative Tokenization methods
- Character-level tokenization: Character n-grams
- Subword tokenization: tokenize text into subwords such as syllables, pre/affixes.

**Byte-pair encoding**: We have a *token learner* and a *token segmenter*.
- **Token learner**: Generates a vocabulary of tokens from a training corpus.
	- **Token Learner Algorithm**
	1. Choose two symbols (character n-grams) that occur the most frequently in the document.
	2. Merge the two symbols, A & B, and add to vocabulary / merge list.
	3. Replace all occurences of A B with AB.
	4. Repeat for *k* iterations.

 - **Token segmenter**: Apply the merged vocabulary to test document data. Split the document into its symbols, then merge them into tokens. The segmenter may merge symbols into full words or split words as tokens.
	 Example: for [new, er_, newer_], l o w e r _ -> low, er_; 
	 n e w e r _ -> new er_ -> newer_

https://huggingface.co - Website for language models.

**Bag-of-Words (BoW) Representation**: Reduce text to a dictionary of words and their occurences. However we will lose information from discarding word order; However is computationally efficient. We can add importance to words based on their occurence frequency.

**Zipf's law**: Frequency of a word inversely proportional to its word rank/importance. word frequency $\propto$ word rank$^{-1}$.

**Luhn's Hypothesis**: Words in rank above the upper limit are considered too common, and those below the lower limit are too sparse to be of any significance. We only consider words within a certain frequency range.
![](misc/Pasted%20image%2020231003112122.png)

**Stop words**: These can be removed as they have low distinguishing power for text documents. Examples: *the, and, it, a*. We can have different stop word lists for different types of domains. https://gist.github.com/sebleier/554280.

#### Vector Representation

For a vocabulary vector |V| of words, a document D is represented as a **vector of weights** to signify importance of words in D. We use *inverse document frequency* to calculate weights.
**Document frequency**: df$_{i}$ = No. of documents that contain term i. 
**Inverse document frequency**: idf$_{i}$ = log$_{10}$(N/df$_{i}$). N -> No. of documents.
IDF increases with rarity / inverse proportion to No. of occurences of i in all documents in our corpus.

**tf.idf weighting**: Adjusts for the fact that some words occur more often in general. Increases with no. of occurences of term i in our document.
![](misc/Pasted%20image%2020231003113834.png)

**Word n-grams**: consider combinations of words. Allow us to preserve localised word order. 
	This is a sentence -> 2-gram -> this is, is a, a sentence.
Higher n-grams become very sparse. https://books.google.com/ngrams/info

### Probabilistic language modelling 
A function that assigns a higher probability to pieces of text that appear more natural.
- Unigram probabilistic model: Assign a probability to each word, and multiply together. We are assuming the words appeared independently and identically distrubuted in terms of occurence. Later apply bayesian inference? 
 ![](misc/Pasted%20image%2020231003114351.png)
- **Calculating word probabilities**: Use word frequency / total no. of terms in corpus.
- **Chain rule**: We can calculate overall probability of some text by using the probability of a word given surrounding (previous?) words to preserve order.
- 
	For a sequence of tokens $w_1, w_2, ... w_L$, $p(w1,w2,...w_L)=p(w_1)p(w_2|w1)p(w_3|w_1, w_2)\ldots p(w_L|w1,\ldots w_{L-1})$
	$\implies \prod\limits_{i=1}^{L} p(w_i|w1,\ldots w_{i-1})$
	- **Markov Chain of order n** : Assumption that a token is only independent on previous n (e.g. 2) tokens (bigram model):
	$p(w_i|w_1,\ldots w_{i-1})=p(w_i | w_{i-1}, w_{i-2})$
	$\implies p(w_{1:L}) = \prod\limits_{i=1}^{L}p(w_i|w_{i-1})$
**N-gram probabilities**: We need a method to assign probabilities of occurence for our N-grams. Sophisticated methods can use
1. Hidden Markov Models with gaussian mixture model
2. Neural Networks
3. Count-based estimation

**Count-based estimation**: For a N-gram model, we count the number of occurence of a word $w_k$ with a N-1 previous words $w_{k-1},\ldots w_{k-N+1}$, we calculate the maximum likelihood probability thus:
![](misc/Pasted%20image%2020231003144340.png)
N-gram models can generate text; trigram models produce most readable (but nonsensical) text. Does not account for long-distance dependencies in words.

**Overfitting**: The test dataset often does not look like the training dataset. There are methods to mitigate this:
1. Laplace smoothing: tackles problem of words that are unseen in training dataset having a 0 probability.
	$$P^*(w_n|w_{n-1}) = \frac{Count(w_{n-1}w_n))}{Count(w_{n-1}+ |V|)})$$
	(|V| denotes vocabulary size)

2. **Backoff**: Use different N-gram in different contexts: use trigram if have evidence that it will work, otherwise bigram, otherwise unigram
3. **Interpolation**: Usually works better than backoff; Combine the probabilities for a word in unigram, bigram, trigram models.
	$$P(w_n|w_{n-2}w_{n-1}) = \lambda_3P(w_n|w_{n-2}w_{n-1})+\lambda_2P(w_n|w{n-1})+\lambda_1P(w_n)$$

	For linear interpolation, $\sum\limits_i\lambda_i=1$

**Unknown words**: Subwords can help avoid these. One method to mitigate for these:
	1. Choose a fixed vocabulary V
	2. (Optional) replace (ideally low-probability/unsignificant), words to replace with \<UNK> token. Could be randomly chosen or all those below a certain probability threshold.
	3. When training probabilistic model, all unknown tokens are counted as \<UNK>
	4. When decoding on test data replace unrecognised tokens with \<UNK>, calculate probability accordingly.







