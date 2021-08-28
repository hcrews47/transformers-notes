# NLP and Transformers 

## The 3 eras of AI

### Symbolic

- 1950s to 1980s roughly
- set of rules to define all of AI
- laborious
- readable and very simple, lots of rules to make something complex
- can't function when there's one unique symbol

### Statistical

- 1980s to 2010
- logistical regression, naive bayes
- can generalize, less domain knowledge
- still somewhat limited
- this guy kinda thinks this stuff is out of date

### Neural

- with new computational power and data we could do this
- NNs need the data and comp power
- deep learning started to outperform stastical data
- adaptable, accurate, ease-of-use
- still a brittle system in many ways
- very little interpretability

## Word Vectors

- in 2013, mikolov introduced word2vec
- originally had one-hot encoding of whole words
- word2vec uses a NN to generate a value for each word, similar words have similar values
- king-man+woman=queen
- numerical representation of language that we can do math on

## Recurrent Neural Nets

- at t=0 we have our first word
- t=0 going to t=1 generates an output state

### Vanishing gradient

- RNNs you have to update weights behind as you go on
- results in NNs having trouble computing because of increasing complexity
- LSTM provides a solution to gradient problem
	- cell state
	- information much earlier in the seq can be obtained much later

## Encoder-Decoder Attention

- Encoder used to pass data directly to Decoder
- Now we have Encoder hidden states that pass to the Attention
- allows us to see how relevant word at `t` is to the words at `t-1`, `t-2`, etc.
- Creates an attention matrix where we can see how similar two words are
- similarity between encoder and decoder states gives us the attention
- Three tensors, Query, Key, Value
	- these work together to create the context vector $c_t$
	- the context then goes back to decoder hidden state

## Self Attention

### 2017 - Attention is all you need

- several modifications needed to be made to attention
	- self-attention
	- multihead attention
	- positional encoding
- these components constitute a transformer
- a transformer can pick up on the connections between words based on context
- RNNs allow us to "look back" over the course of a sentence

### multi-head attention

- produces multiple linear layers with different weights
- rather than have a single representation of each word, we get multiple reps of each word
- concatentation layer aggregates reps
- concat layer gets fed through linear layer to create the tensor

### Positional encoded word attention

- don't have sequential stuff in transformers, all parallel
- positional encoding is added to word embedding
- sine and cosine waves represent token position on the x-axis and encoding activation on the y-axis

$$PE_{(pos,2i)} = sin({\frac{pos}{1000^{\frac{2i}{d_{model}}}}})$$

$$PE_{pos,2i+1)} = cos({\frac{pos}{1000^{\frac{2i}{d_{model}}}}})$$

- where `i` is the embedding dimension
- even indices have sine, odd have cosine
- frequency of waves decreases

<div align="center">
	<img src="assets/first_transformer.png" alt="Transformer" style="width: 200px;" />
</div>

## Transformer Heads

### Mask Language Modeling

- Will mask a specific token and then BERT or something else will  have to guess the mask
- mostly for training models
- BERT will output a hidden_state tensor to a Linear Layer
- Softmax takes linear layer output and makes it so they vector adds to one
- the argmax is the largest value


### Question and Answering

- pass in a question and a context
- takes q and c and says that in this context the answer you are looking for is here
- defines a start token and end token as the answer within the context

### Classification

- in the linear layer we get labels for things like sentiment analysis

