Chunking: The process of grouping multiple words together into **phrases** based on a set of criteria; We break down text into structured tree of groups of words that are labelled with grammatical tags; e.g. noun phrases (NP) or verb phrases (VP).

Different from tagging as we look at  multiple words, and use different algorithms; though tagsets will include tags for both tagging and chunking.

![](misc/Pasted%20image%2020231020165912.png)

### Grammatical Structures
**Do not** consider this a complete list; There are too many to list extensively, this is an introduction.
I am just doing the ones mentioned in the notes.

- The **head** of a phrase determines it's category - e.g. noun phrase has noun as it's head.

#### Verb Phrase (VP)
Contains the verb, and other words that compliment/modify the verb action.

Examples:
- Master **has given** master a sock
- I **have not gone** to lectures

#### Noun Phrase (NP)
Performs the same *grammatical function* as it's noun.
Examples:

![](misc/Pasted%20image%2020231020002748.png)

A group of words that can be replaced by a pronoun is a noun phrase.
Substitution examples:

![](misc/Pasted%20image%2020231020002808.png)

Example Structure:

![](misc/Pasted%20image%2020231020010708.png)

#### Determiners
(Group of) Words of what the noun is referencing.

Example determiners: for a noun *car*:
- **A** car
- **John Doe's** car
- ***John's sister's brother's third cousin removed** car

#### Nominals#
Group of words in a NP containing the noun, and all of its pre- and post- modifiers.

![](misc/Pasted%20image%2020231020011446.png)

#### Pre-modifiers
- Quantifiers: Ordinals, cardinals i.e. any type of counting; **TWO** cars, **several** war crimes
- Adjectives - **large** cars, **red** blood

WE have ordering of pre-modifiers: Say **three small** balls, not **small three** balls

#### Post-modifiers
- Pre-positional phrases: describe position of a object(noun) e.g. location, time: 
	- I am **at** the lecture
	- Cat is **in front of** the fridge
	- Born **in** 1975
	- **In the midst of** 9/11
- Non-finite clauses: phrase containing a verb that is used irrespective of the position e.g. time:
	- I went to **watch a movie**
	- I am going to **watch a movie**
- Relative clauses: Modifies a noun phrase to refer to it - Stating a clause relative to the noun:
	- I met a guy **who also listens to grindcore

### Chunking Models

#### Context Free grammars
We can use CFG's to produce our (constituents/) phrases;
- Terminals - words
- Non-terminals - (constituents/) phrases
- Rules - structures / rules for our phrases

*Example noun phrase rule*s: 
- NP -> determiner nominal.
- NP -> (Predeterminer) (NP)

*Example Nominal Rules*: 
- (Nominal) -> (Nominal) (Preposition Phrase)
- (Nominal) -> (Nominal) (Gerund VP)
- (Nominal) -> (Nominal) (Relative Phrase)

You can see that we can produce a large complex set of grammatical rules for breaking down grammars (too many to learn); We will also formulate different rules for different *tagsets*.

#### Rule agreement
Constraints based on the specific words used (not just grammatical structures) s.t. our text makes sense.

These NP's are in agreement:

- This flight
- Those flights

But these are not:
- Those flight
- This flights

We can introduce rules specific for e.g. singular and plural forms:
- Plural NP -> (Plural Determiner) (Plural Nominal)
- Singular NP -> (Singular Determiner) (Singular Nominal)

However tedious/does not scale well. Instead we use:

#### Treebanks
Is a parse tree of the grammatical structure of sentences of a text corpus, with tags for different words & phrases, often constructed with manual checking from a linguist. We can use these to train our chunking (& tagging) models.

#### Probabilistic CFG's

We extract phrase rules and corresponding probabilities from a treebank for those rules, aswell as POS tagging.

Given a non-terminal *A*:
- For each rule $\forall i|A \rightarrow \alpha_i$ we have a value for $P(\alpha_i|A)=p_i$.
- Simple method to calculate probability is count-based estimation: $P(\alpha_i|A)$ = Occurrences($A\rightarrow\alpha_i$)/Occurrences($A$).
- Sum of probabilities for all rules $\sum\limits_{\forall i} P(\alpha_i|A)=1$.
- Probability of a sentence = product of recursively non-terminals e.g. A and its production rule $A\rightarrow\alpha$:

![](misc/Pasted%20image%2020231020181136.png)

and use the **Viterbi algorithm** to choose the sequence of rules with highest probability.