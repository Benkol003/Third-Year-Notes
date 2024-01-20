Examples: chatGPT, alexa, siri, google assistant, chatbots

(lets be honest, we are only concerned with GPT here)


General architecture of conversation AI:

![](misc/Pasted%20image%2020240120165801.png)

### Variations in speech recognition (for conversational AI)

- Speech disfluency: self corrections, repititions, pauses which make spoken text not fluent. Increases with duration of speech:
- 
![](misc/Pasted%20image%2020240120190037.png)

#### BERT wait states
Allows BERT to detect disfluencies. In the following example we perform a self contradiction, so for the word no we produce a wait state, before completing the output of the processed order

![](misc/Pasted%20image%2020240120190058.png)

- Speech synthesis: For conversational AI we want to synthesise in real time, and have enough tonal, accent, emotion variation for it to sound realistic. Not achieved by non-ML TTS but neural network models tend to sound more realistic.

#### Linked audio & visual representations

- Viseme: speech sounds that look the same when lip reading
- SSML: speech synthesis markup language

Following diagram shows how to construct both synthesised audio, and a virtual avatar that can be lip-synced.

![](misc/Pasted%20image%2020240120190422.png)

### Converstaional Context

Types of conversation:

- Task-oriented conversations: clear goal, e.g. for answering questions, logical

![](misc/Pasted%20image%2020240120193045.png)

- Open-domain conversations: no exact goal, longer, unpredictable, for emotive. Typically makes greater use of context

May have multiple possible responses from which we choose the best for a *retreival architecture*; or can use generative transformer models (GPT).

![](misc/Pasted%20image%2020240120193057.png)


### Usage of LLM's

Timeline of LLM's - all of these are based on the transformer architecture:

![](misc/Pasted%20image%2020240120193301.png)

- Foundation models - general-purpose LLM models such as GPT, BERT that can be used as-is or fine-tuned, or pre-trained with foundation model weight initialisation to a domain (e.g. BioBERT)

- Conversation consistency: LLM's can frequently forget, or be told to believe contradicting information 
![](misc/Pasted%20image%2020240120193410.png)

![](misc/Pasted%20image%2020240120193459.png)

#### AI misinformation & hallucinations

- Generative models are very good at producing plausible-sounding information even it is wrong, called *hallucinations*.

- Hallucinations: GPT model makes up information or provides false information to a question

(France did not gift a TV tower)

![](misc/Pasted%20image%2020240120193816.png)

- Intrinsic hallucination: answer that directly contradicts the answer / source material (probably from misinterpreation)
- Extrinsic hallucination - Provides data that is not related in any way i.e. it is unfounded

![](misc/Pasted%20image%2020240120193924.png)

Prompt engineering techniques to avoid hallucinations:

- Chain-of-Thought prompt engineering can help alleviate this.
- Can also ask generative model to break down a problem / work on it step by step, rather than asking to provide the entire answer up front.
- Longer context windows (of the input) also improve performance.
- Having useful / important information at beginning / end of sequence improves performance

![](misc/Pasted%20image%2020240120194423.png)

- Retrieval-augmented generation (RAG): take a query and augment it by retrieving relevant data and passing in as context for the LLM to work with. We can then reduce context & can help reduce hallucinations (but can also go the other way). Better for short term memory

- Domain-specific / fine-tuned models (e.g. BioGPT) will reduce hallucinations as its more tailored. Can be used in tandem with RAG. Better for long-term memory

![](misc/Pasted%20image%2020240120194926.png)

- RAGA (RAG assessment) scores: score performance on 1. data retrieval component 2. text generation component; of provided answers from LLM's.

#### LLM competence

LLM's are formally competent - capable of producing very fluid text; however, not functionally / logically competent as well (e.g. 2+2 = 5 earlier)

![](misc/Pasted%20image%2020240120195653.png)

This example albeit is done with prompt engineering / gaslighting but still works:

![](misc/Pasted%20image%2020240120195716.png)


#### Transfer learning

![](misc/Pasted%20image%2020240120195803.png)

Have seen this with fine-tuning BERT on the assignment - you can choose to freeze lower layers and only fine-tune higher ones

#### In-context learning
(Another form of prompt engineering)
(Note this is for a **fixed weight** model i.e. no further training)

Basically we provide new information to GPT in the context (window) of the input question being provided, that GPT can use to answer the question; this is basically what RAG does.



#### Prompt engineering
The quality of a response from a generative model is very dependent on the quality of its input query; prompt engineering are any methods utilised to optimise queries to improve the performance of the response from the generative model.