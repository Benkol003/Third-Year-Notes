Word senses are the different meanings to a word:

- **Play** at the theatre
- **Play** foosball

*Word sense disambiguation (WSD)* - Find the meaning of a word given it's context in text. Applicable to machine language translation, search engines.

Approaches:
- Knowledge based: Use lexical resources - dictionaries, thesaurus's, wordNet - build rules from these to disambiguate
- Supervised machine learning: Training corpus containing different labelled word senses for the same words.

#### Lesk Algorithm
knowledge based approach that works by assuming words in close proximity will have similar meaning.

**Simplified Lesk algorithm**: Correct word sense is determined by the sense that overlaps the most with the current context.

Example:
For doing WSD on the word **bank**, from dictionary definitions you can see we have two **overlapping** words in the surrounding context that appear to the first definition (bank in monetary sense).

![](misc/Pasted%20image%2020231021144751.png)

- Good idea to weight word's importance based on inverse document frequency: definition score = $\sum\limits_{w\in I} idf_w$, where $I$ is all the overlapping words between context and definition.

**Sense tagged corpora**: 
- SemCor - https://www.sketchengine.eu/semcor-annotated-corpus/
- Sense tagged text - https://www.d.umn.edu/~tpederse/data.html


#### WSD naïve bayes classifier
For an input sample of text (ambiguous word and context words) x, and a possible sense class y, calculate joint probability $P(x,y)$

$P(word|class)=\frac{count(word,class)+1}{count(class) + |V|}$  - +1 and |V| (vocabulary size) for additive smoothing/never non-zero probability - lowest is $\frac{1}{|V|}$.  


#### Sequence Labelling
WSD can be treated as a sequence labelling problem - with this approach (may also use for POS tagging), & we consider word order in the output.
- Hidden Markov Models
- Recurrent Neural Networks

