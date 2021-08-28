# Attention

## Introduction

- not totally necessary for implementing these models, theory
- 4 types of attention mechanisms
	- encoder-decoder attention
	- self attention
	- bidirectional attention
	- multi-head atttention

## Alignment with Dot-Product

- sentences with similar meaning would have similar sets of values 
- between similar words we have a higher activation through a similarity function
- can be shown on a heat map as well
- the similarity of word vectors is know as alignment
	- calculated using the dot product $U \dot V$
	- $|U||V|cos\theta = \sum^{n}_{i=1} U_i V_i$
- these vectors can be visualized on a plot of 2 or 3 dimensions
- same as `a*b^T`
	- can use numpy matmul for this

## Dot Product Attention

- For comparing two different sequences
- add padding token in order to be able to do the matrix multiplication
- encoder and decoders pass through linear layer to create matrices that are then multiplied
- softmax is applied to multiplied tensors to create output which is then multiplied by encoder tensor

## Self Attention

- similar to dot product attention except that we use the same corpus or sequences
- used for text summarization or generation
- for this we use a masking tensor before doing the softmax
	- masking tensor has 0s and -infinity
- softmax function is an S-curve where we find the y value based on the x value
	- we use negative infinity to make values equal to 0
	- this makes it so we only get attention for words before our time value

## Bidirectional Attention

- compares tokens from same sequence in both directions
- bidirectional we don't have the masking tensor, more simple than self attention
- particularly useful for masked language modeling and bert
	- Bidirectional encoder representations from transformers

## Multi-head attention and scaled dot product

- several representations of attention for words
- instead of one tensor we can have multiple
- 3 time attention means we have 3 attention heads 
- one output from embedding layer creates 3 tensors
	- key
	- value
	- query
- doesn't represent the same atttention, different weights that are set randomly
- only want one attention at the end so we concat them
	- just glue them on to each other
	- pass this into a linear layer that translates into original size
- `d_k` adds a scaling factor. division value for the hidden state tensor size

