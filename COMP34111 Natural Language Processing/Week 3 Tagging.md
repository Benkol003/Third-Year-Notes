### Part-of-Speech (POS) Tagging
**Part of speech**: Linguistic class of words: nouns, verbs, adjectives, etc.

**Open word class**: a POS that constantly acquires new words.
*New verbs*: google, tweet, self-isolate.

**Closed word class**: A POS that does not acquire new words (often).
e.g. Pronouns, he/him, they/them , she/her.

**Content words**: Words that carry useful meaning in a sentence. Usually covers most open word classes - e.g. nouns and verbs, (but not  e.g. auxiliary verbs such as *was* or *do*).

**Function words**: Words that contribute to syntax, and act as 'linguistic glue': This is most closed word classes - e.g. conjunctions (and, or, but).

**POS tagging**: Given a list of words/tokens, assign a POS category tag, e.g. {noun, verb, adjective...} to each token.

#### POS tag sets 
A defined set of tags we can use on our grammar.

List of tag sets:
- Universal tag set (for english). The typical {verbs, nouns, …}. https://universaldependencies.org/u/pos/
- Penn Treebank - 45 tags. https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html
- C5 (CLAWS5) set - 61 tags. http://www.natcorp.ox.ac.uk/docs/c5spec.html
- C7 (CLAWS7) set - 141 tags (complicated). http://www.natcorp.ox.ac.uk/docs/claws7.html

Words can be ambiguous in the tag set they belong to.
- We can beans - verb
- We can go - auxiliary 
- Pick up that can. - noun

POS taggers must resolve ambiguity given the context of the token. This can be done by looking for common *phrase structures* in the text. 

#### POS tagger cues
**Internal Cues**: (morphology) use affixes:
- devi**ous** - -*ous* affix is used for adjectives
- overdos**ed** -*ed* affix common for verbs

**External Cues**: Use words around surrounding word to be tagged to provide context; Use **phrase structure rules** in a **rule-based tagger**:
- I *will* **read** lecture notes - common to see a verb after the use of the word **will**.


#### Types of POS taggers:
- **Rule-based tagger**: uses pre-written rules such as phrase structures to be able to assign tags in ambiguous cases.
- **Stochastic taggers**: Uses probability (e.g. bayes rule for surrounding words or) to find the most likely tag/sequence of tags.
- **Transformation taggers/Brill tagger algorithm**: Takes an existing stochastic tagger, and then is applied to training data; we then identify words that have been incorrectly tagged, and we form *transformational rules* to correct these errors.

#### Rule-based Taggers
Process:
1. We have a dictionary of words with all *possible* tags for that word. Assign these to words in text.
2. Using a large list of *disambiguation rules*, narrow down the possible tags to having one tag per word in the text.

#### Stochastic Taggers
We want to find the most likely sequence of tags, $\hat{t^n_1}=t_1,t_2,\dots t_n$.

With bayes rule:
$$\hat{t^n_1}=\arg\max\limits_{t^n_1} P(t^n_1|w^n_1) = \frac{P(w^n_1|t^n_1)P(t^n_1)}{P(w^n_1)}
\implies P(w^n_1|t^n_1)P(t^n_1)$$
(We can remove $P(w^n_1)$ as it is constant for any tag sequence - not dependent on $t^n_1$).

- **Likelihood term**: Assume word is independent of surrounding words & tags - **naïve bayes assumption** *of independence*.
$$P(w^n_1|t^n_1) = \prod\limits_{i=1}^n p(w_i|t_i)$$
- **Prior term**: 
$$P(t_1^n) = \prod\limits_{i=1}^n P(t_i|t_{i-1})$$
We assume a *nth order* (here 2nd order) **Markov chain** to then use chain rule of probability.

#### Tag Transition Probabilities
Need to estimate $P(t_i|t_{i-1})$. We can use a *training corpus for this* - $P(t_i|t_{i-1}) = \frac{C(t_{i-1},t_i)}{C(t_{i-1})} =$ No. of Occurrences of tag $i$ & $i-1$ / Occurrences of tag $i$.



#### Viterbi Algorithm
(Optional Reading)
(Also used in digital signal processing.)
https://www.youtube.com/watch?v=6JVqutwtzmo
https://medium.com/mlearning-ai/intro-to-the-viterbi-algorithm-8f41c3f43cf3

Finds the most optimal sequence of states for observations. In our case, we want to estimate the best sequence of tags given a sequence of  n words.

**Graph Visualisation:**
Here we have all possible routes as a graph.

![](misc/Pasted%20image%2020231019004505.png)

By eliminating all except the best connections between two nodes we can clearly see the best route (shortest route ends at Daytona beach):

![](misc/Pasted%20image%2020231019004540.png)



**Algorithm (in context of tagging)**:

- First we need to be able to calculate transition probabilities - e.g. (as Markov chain order 1) use $P(t_i|t_{i-1},w_i,w_{i-1})$. 

Starting from the first word, for all words $w_1,\dots w_n$:
	For a possible tag $t_{i}=$<*tag*> for a given word $w_i$, we choose the out of all possible previous tags for $t_{i-1}$, the one that gives the highest probability for the sequence $t_1,\dots t_i$;  assuming we have previously calculated the probability of **best possible sequence to** $t_{i-1}$, represented $\delta_{t_{i-1}}$. Calculate as
	$$\delta_{t_i} = \max\limits_{t_{i-1}} P(t_i|t_{i-1},w_i,w_{i-1})\delta_{t_{i-1}}$$
We then work from the last word, picking the tag with the highest probability; $\hat{t_n}=\arg\max\limits_{t_n}\delta_{t_n}$, and then working backward to find best tags $\hat{t_{n-1}}=\arg\max\limits_{t_{n-1}}\delta_{t_{n-1}}$, etc. upto $\hat{t_1}$.





