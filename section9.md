# Question and Answering

## Overview

- open domain and reading comprehension
	- reading comprehension has a context
	- open domain is when there is a question without a context
- open: external access, web, pdf, etc.
- closed: all info that model provides must be encoded in model parameters

## Retrievers, Readers, and Generators

- ODQA: open domain question answering
- retriever: uses question to retrieve a set of context from external datasource
	- this makes it open book
- reader: takes a question and gets a context and then takes span of text from context to give an answer
- generator: takes place of reader model, takes question and produces an answer, doesn't need a context but can use one
- this gives us 3 models
	- retriever-reader
	- retriever-generator
	- generator
- first two use a data source, last one generates something based only on what it has been trained on

## SQuAD 2.0

- Stanford Questioning and Answering Dataset
- very comprehensive dataset

## Processing SQuAD Training Data

- python 3.10 introduces match-case

```python
match qa_pair:
	case {'answers': [{'text': answer}]}:
		pass
```

- if list is empty, it will return as false. Otherwise, case matches 
- underscore `_` is used as the else

## First Q&A model

- `from transformers import BertTokenizer, BertForQuestionAnswering`
- deepset makes some good ones
- tokenizer = BertTokenizer.from_pretrained('deepset/bert-base-cased-squad2')
- Pipelines class
	- allows us to use different parameters to setup a specific pipeline
	- things like 'sentiment-analysis', 'feature-extraction'
	- this could be interesting, curious to learn more about this
	- feed it the model and tokenizer
- can feed the pipeline the question and context and then it will return an answer
- pretty impressive stuff when the context is included 
