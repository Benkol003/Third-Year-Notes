# Information Extraction

1. Identify entities & relations 
2. Fill in table of facts

![](misc/Pasted%20image%2020240119004715.png)

1. Identifying entities - **Named entity recognition**. I.e. finding names in text & classifying them

- An entity is an instance of a certain thing of interest, e.g. location a or persons; the name is typically in the text.
- Typically we are only interested in specific entities and tailor identification to this
- We can use dictionary methods, rule based methods, or use ML models

3. Entity relations - **Relation Extraction***** from text
- Extract facts, relations, events by linking entities
- Use either rule-based (regexes, grammars) or ML models

Methods:
- Rule-based: again e.g. regex, grammars. Based around important verbs - e.g. x greeted y indicates relation of event between x & y
- Machine learning

## Named Entity Recognition (NER)

Examples on named entities:

![](misc/Pasted%20image%2020240119154325.png)

#### Dictionary-based approach (gazetter)
Have lists of names that are mapped to specific class

- Advantages: Simple, language independent, easy to create from thesauruses, lexicons...
- Disadvantages: impossible to enumerate all names (e.g. monetary amounts); Doesn't handle name variants well; does not resolve ambiguity, making it dumb / bad performance; lists must be maintained & kept up to date.

#### Rule-based approaches
Use context words to indicate the entity type.
E.g. \<title\> \<capitalised word\>+ -> personal name -> Mr John Smith

- Disadvantages: Rules are ambiguous:

![](misc/Pasted%20image%2020240119154934.png)

![](misc/Pasted%20image%2020240119155044.png)

As we can see names can belong to multiple entity classes: try to use context to resolve this ambiguity.

Example rules:

![](misc/Pasted%20image%2020240119155020.png)

These rules resolve some but not all ambiguity using context.

### Machine Learning Models

We consider names as contiguous tokens; each token labelled with a class.

Two encoding examples:

![](misc/Pasted%20image%2020240119170957.png)

- Inside-Outside (IO) encoding
- Inside-Outside-Beginning (IOB) encoding allows us to tag names in a way such that two different names of the same entity are separable by using B-\<entity\> to denote the start of the new name.

We can use again a hidden markov model 
[python notebook using HMM for NER](https://www.kaggle.com/code/annsanababy/hidden-markov-model-hmm-on-ner-dataset)

![](misc/Pasted%20image%2020240119175424.png)

However hard to add data features that may help well to discriminate / resolve ambiguity 



Example features we might want to add:

![](misc/Pasted%20image%2020240119175624.png)

& applied to words:

![](misc/Pasted%20image%2020240119175637.png)

#### NER classifiers

![](misc/Pasted%20image%2020240119175813.png)

#### Conditional Random Fields

More generalised model compared to a classifier or HMM; our prediction for an output is dependent on its neighbours (input values or previously predicted), e.g. surrounding text or pixels. Our predictions are co-dependent in a graph-like manner.

CRF can be considered a better / more *global* approach.

We typically use *linear CRF's* - our co-dependency graph is just a linear chain.

[blog post](https://blog.echen.me/2012/01/03/introduction-to-conditional-random-fields/)

Our CRF is defined as follows:

$$P(y|x) = \frac{1}{Z(x)} exp(\sum\limits_{i=1}^{N} \sum\limits_{j=1}^{K}\lambda_j f_j(y_i,y_{i-1},x,i))$$


- Input sequence $x$, output sequence $y$, typically both of same length   
- N - length of sequence for $x$ / $y$; iterate with $i$.

- K - number of features; feature values are generated with *feature functions*; iterate with $j$.
- Feature function, with template $f_i(y_i,y_{i-1},x,i)$ - we typically know $x$, position $i$ beforehand. 
For a **linear chain CRF** We typically know upto $i-1$ output tokens beforehand.  You can also see how the dependency graph of predicted outputs is linear i.e. only connects to the previous token.(General CRFs have a different function template, & have a different graph, but we aren't concerned with these.)

- $\lambda_j$ - weight associated with feature function $j$ - train these with gradient descent.

- $Z(x)$ Normalisation factor - ensures that the sum of probabilities of all possible output sequences of $y$ (given $x$) $Y$, is 1. 
	- $$Z(x) = \sum\limits_{\forall y \in Y} exp(\sum\limits_{i=1}^{N} \sum\limits_{j=1}^{K}\lambda_j f_j(y_i,y_{i-1},x,i))$$

#### BERT for NER
can be used but is considered local.

Typically we pass the output of a classifier like BERT, apply a softmax (i.e. probability for each possible entity label on its own) and pass it to a CRF layer which is more global.

#### Fine-tuning BERT

Typically take BERTs output word embeddings, add a classifier head / dense layer to produce entity class labels,

![](misc/Pasted%20image%2020240120023200.png)

- BERT + CRF

![](misc/Pasted%20image%2020240120023342.png)

- BERT + BiLSTM + CRF

![](misc/Pasted%20image%2020240120023355.png)

#### NER Domain Adaptation

Typically a model is trained for a specific domain of names & entities, e.g. medical names are different to journalism,

![](misc/Pasted%20image%2020240120023856.png)

Typically do with few-shot learning (lol what does this mean in practice).


#### NER adjusted performance metrics

Adjust formulas for calculating performance to allow for partially correct answers. Boundaries for named entities are often misplaced, but still may use correct entity class

![](misc/Pasted%20image%2020240120024158.png)

## Relation Extraction
I.e. a medical paper may state a drug & disease, but does the drug help treat, cause or no effect relating to the disease?

again we can use
- Hand written rules - high precision, low recall
- Machine learning methods - classifier to say if a kind of relation between two entities exists or not


Features we can extract for a machine learning model to use:

![](misc/Pasted%20image%2020240120144137.png)



- Trigger lists: lists of words that you would typically see between two entities that are indicative of a type of relation:

![](misc/Pasted%20image%2020240120144306.png)

### Semi-supervised methods

We need large training datasets for ML methods for effective entity recognition, & even larger datasets for identifying relations. However this is not as feasible if we have to hand annotate all our training data by hand.

#### Relation Bootstrapping

1. Generate a set of **seed pairs**, ie. word pairs that we want to search for specific relations of.

Repeatedly:
2. Find sentences containing these pairs
3. Look & generalise the context (sentence) around the word pairs to create patterns or seed tuples
4. Use the new tuples to search (e.g. grep) for more pairs 

Example

![](misc/Pasted%20image%2020240120144911.png)

Found three sentences from this word pair; create new seed tuples e.g. \<buried,in\>.
However we are biasing our data on the initial seed tuples we use.

Again we may have ambiguity

![](misc/Pasted%20image%2020240120145321.png)

![](misc/Pasted%20image%2020240120145335.png)

- Coreferences: introduces more ambiguity as we use different words such as he,she,they (pronouns) to refer to the same entity

![](misc/Pasted%20image%2020240120145549.png)

### BioBERT

1. Initialise BioBERT (weight initilisation) with normal pre-trained BERT model
2. Pre-train on biomedical data
3. Fine-tune with Biomedical versions of NER, RE, QA tasks

![](misc/Pasted%20image%2020240120150837.png)

#### BERT vs BioBERT performance (slides)

![](misc/Pasted%20image%2020240120151650.png)

![](misc/Pasted%20image%2020240120151708.png)

#### Knowledge Distillation

A smaller model, the *student*, is trained to copy the behaviour of the larger model, *teacher*; WE distil the model into a smaller one that is faster (for both evaluation & training e.g. fine tuning).

![](misc/Pasted%20image%2020240120152629.png)


#### Template-based NER
Few-shot learning approach that allows us to adapt existing NER rules;
create both *entity* & *non-entity* (opposite rule) templates for all entity types to be used.

(entity type e.g. candidate_span is different from entity e.g. location, person)

Can apply these templates to training data to get expected outputs

![](misc/Pasted%20image%2020240120155112.png)

We then use a text generation model such as BART that produces an output in the entity / non entity template format, i.e. produces any of

- Bangkok is a location entity
- Bangkok is a person entity
- Bangkok is not an entity

And we can compare + train with expected template answer.

![](misc/Pasted%20image%2020240120160751.png)

#### PromptNER

Use prompt engineering with chatGPT to perform NER 

Example instruction tuning of the task:

![](misc/Pasted%20image%2020240120161112.png)