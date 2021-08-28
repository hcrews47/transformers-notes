# Reader-Retriever QA with Haystack

## Intro to Retriever-Reader and Haystack

- haystack gives easy access to the retriever
- deepset created the haystack library
- QA at scale
- Elasticsearch is a retriever approach
	- not specific to nlp
- FAISS: Facebook AI Similarity Search
	- best on unix systems?

## Elasticsearch

- splits data with nodes and shards
- can be combined to make the ELK stack
- cluster
	- contains everything in our deployment
	- nodes are individual processing units
- indexes
	- where we store data
	- within cluster but can span multiple nodes
	- an index is like a dataset with documents
	- documents can have different formats, schemaless
- sharding allows us to split across different nodes

## Elasticsearch in Haystack

- `pip install farm-haystack`
- `from haystack.document_store.elasticsearch import Elasticsearch`
- do this with elasticsearch running
- use `requests` to communicate with the elasticsearch instance
- `document_store.write_documents(input_data)` to write data

## Sparse Retrievers

- once we have data in the index, we can use sparse or dense vectors
- We just set up elasticsearch as our "knowledge" for the model
- we can use tf-idf in retrieval
	- words like "the" get penalized, unique do not
- `from haystack.retriever.sparse import TfidfRetriever`
- hit the document_store with the retriever
- `retriever.retrieve(query)` to receive a set of contexts related to a query

## Cleaning the index

- you could get some duplicates, don't want that with the contexts
- should do this before indexing data
- just use the `set()` function to delete duplicate contexts

## Implementing the BM25 Retriever

- tf-idf or bm25, bm25 is a variation of tf-idf
- essentially acts as normalization, also considers document length
	- short documents score better if they have the same scores
- typically preferred for sparse retrievers
- do the same things except with `ElasticsearchRetriever`

## What is FAISS?

- nearest neighbor algorithm gets slow after a while
	- find distance between vectors 
	- nearest neighbor is the brute force method
- FAISS is an alternative to the nearest neighbor similarity search
	- does efficient gpu computation
	- do a preprocessing step on each vector, PCA or L2 normalization
	- does an inverted file indexing (IVF)
		- clusters files based on similarity
		- creates an approximate nearest neighbors search
		- coarse quantizer
	- Fine quantizer
		- flat method: vectors are stored as they are
		- product quantization: reduce complexity or memory of vectors

## FAISS in Haystack

- need a sqlite database and a faiss index
	- sqlite creates a metadatabase 
- `from haystack.document_store.faiss import FAISSDocumentStore`
	- use this to create the data
- want to update embeddings with a dense passage retriever (DPR)
- need to give the DPR two different models
	- query model and passage model
	- question_encoder and a ctx_encoder (context)
	- want to use gpu if you can
- once embeddings are updated, you want to save the index as a .faiss object

## What is DPR?

- does more than just a similarity search
- converts contexts into special vectors
- converts the question into an approximate context
	- "what is the capital" becomes "the capital is"
- in tf-idf we give uncommon words large scores
- DPR uses dense vectors with lots of meaning encoded
- has a set of trainable neurons
	- tf-idf and bm25 are just algos without trainable neurons
- cons
	- have to train it, pre_trained means we don't have to do this as much
		- for specific language you'll have to retrain
	- not good with out-of-vocabulary words
	- need a lot more compute because it's a nerual net

## DPR Architecture

- have two unique bert encoder models (passage and question)
- create a passage vector and a question vector that are then dot producted together
	- $sim(q,p)=E_Q(q)^T E_P(p)$
- then optimize E_p and E_Q weights to increase similarity

## Retriever-Reader Stack

- use `FARMReader(model_name_or_path, use_gpu)`
- Load up document_store and DPR
	- can use Elasticsearch or FAISS document store
		- remember that elasticsearch is a sparse, exact match retrieval
		- FAISS is a dense nearest neighbor retrieval
- can still use a DPR on an elasticsearch document
- update_embeddings with the retriever
- can build an extractive QA pipeline with haystack
	- `from haystack.pipeline import ExtractiveQAPipeline`
	- just pass it a reader and retriever
	- `pipeline.run(query="......")`
 
