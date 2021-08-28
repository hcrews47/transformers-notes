# Similarity

## Introduction

- nlp relies on measuring similarity in high dimensional spaces
	- really just distances between vectors
- first need to take sentences and turn them into representational vectors
- expressability is unparalleled?
- you should be able to do contextual additions and subtractions and get back to starting point
	- king-man+woman=queen
- dense vectors
	- every value in vector has a real meaningful value
	- sparse vector is something like a one-hot encoded vector
- end up with a very big tensor 512x768
	- want to convert this output into a sentence embedding
	- take a mean of all 512 tokens and compress into single vector
	- can just take the final hidden state, `last_hidden_state` tensor, outputted by BERT models

## Extracting the last hidden state tensor

- using pytorch and transformers library
	- AutoTokenizer, AutoModel
- sentence-transformers: team of people that create models that make sentence vectors
	- `sentence-transformers/bert-base-nli-mean-tokens`
- mean pooling operation to extract sentence embeddings from BERT

## Sentence Vectors with Mean Pooling

- need an attention_mask that matches the size of our embeddings
- `attention_mask.unsqueeze(-1).expand(embeddings.shape).float()` will switch up the size to match embeddings
- you can multiply the tensors after that  to get the masked embeddings
- now you can take a mean of all the embeddings
	- get the sum of the masked embeddings and the count of the mask
	- mean_pooled = sum/count
	- returns a tensor

## Using cosine similarity 

- use `sklearn.metrics.pairwise import cosine_similarity`
- can't use numpy on tensor that uses grad. need to detach tensor from pytorch `.detach()`
- cosine similarity gives us the distance between sentences?
- there's an easier way to do this with `sentence-transformers`

## Similarity with Sentence-Transformers

- use sentence-transformers model from huggingface
- `from sentence_transformers import SentenceTransformer`
- still use huggingface model under the hood
- create embeddings with `model.encode(sentences)
- `sklearn.metrics.pairwise import cosine_similarity`
- `cosine_similarity([embeddings[0]], embeddings[1:])`

