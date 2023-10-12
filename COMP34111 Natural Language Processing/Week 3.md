### Part-of-Speech (POS) Tagging
**Part of speech**: Linguistic class of words: nouns, verbs, adjectives, etc.

**Open word class**: a POS that constantly acquires new words.
*New verbs*: google, tweet, self-isolate.

**Closed word class**: A POS that does not acquire new words (often).
e.g. Pronouns, he/him, they/them , she/her.

**Content words**: Words that carry useful meaning in a sentence. Usually covers most open word classes - e.g. nouns and verbs, (but not  e.g. auxiliary verbs such as *was* or *do*).

**Function words**: Words that contribute to syntax, and act as 'linguistic glue': This is most closed word classes - e.g. conjunctions (and, or, but).

**POS tagging**: Given a list of words/tokens, assign a POS category tag, e.g. {noun, verb, adjective...} to each token.

**POS tag sets:** A defined set of tags we can use on our grammar.
List of tag sets:
- Universal tag set (for english). The typical {verbs, nouns, …}. https://universaldependencies.org/u/pos/
- Penn Treebank - 45 tags. https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html
- C5 set - 61 tags. http://www.natcorp.ox.ac.uk/docs/c5spec.html
- C7/CLAWS7 set - 141 tags (complicated). http://www.natcorp.ox.ac.uk/docs/claws7.html

Words can be ambiguous in the tag set they belong to.
- We can beans - verb
- We can go - auxiliary 
- Pick up that can. - noun

POS taggers must resolve ambiguity given the context of the token. This can be done by looking for common *phrase structures* in the text. TODO example phrase structure.

Types of POS taggers:
- **Rule-based tagger**: uses pre-written rules such as phrase structures to be able to assign tags in ambiguous cases.
- **Stochastic taggers**: Uses probability (e.g. bayes rule for surrounding words or) to find the most likely tag/sequence of tags.
- **Transformation taggers/Brill tagging**: Takes an existing stochastic tagger, and then is applied to training data; we then identify words that have been incorrectly tagged, and we form *transformational rules* to correct these errors.






