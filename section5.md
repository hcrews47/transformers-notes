# Language Classification

## Intro to Sentiment Analysis

- one of the most widely used apps of NLP
- positive, negative, and neutral statements
- 4 steps
	- text is tokenized
	- feed tokens to sentiment model
		- first layer is an embedding layer that outputs some word vectors
		- create word embeddings
	- ouptut activations are fed to transformer head which gives probabilities
		- softmax function gives us the probs
	- argmax finds the position of the greatest probability

## Prebuilt Flair Models

- Steps in python with flair
	- initialize modoel
	- tokenize
	- process with model
	- formatt outputs
- the flair english-sentiment model is a distillbert model
- `flair.data.Sentence(text)`
- predictions are added to `Sentence` object
	- predictions are given as a POSITIVE with a "confidence"
	- you can extract the model labels with `.get_labels()`
	- lots of functions and attributes with the flair.data.Label
- flair is great but hugging-face transformers is better

## Hugging Face Transformers Library

- most advanced library for nlp transformers
- sentiment analysis is a text classification model
- finbert is a financial language bert model
- `model_name = 'ProsusAI/finbert'`
- `from transformers import BertForSequenceClassification`
- `.from_pretrained()` to get a pretrained model
- every model has a tokenizer
	- `from transformers import BertTokenizer`
	- `tokenizer = BertTokenizer.from_pretrained(model_name)`
- model will output activations, not probabilities
	- mustt use a softmax ot get the probs
	- then take the argmax

## Tokenization and special tokens for BERT

- `tokens = tokenizer.encode_plus(txt, max_length=512, truncation=True, padding="max_length", add_special_tokens=True, return_tensors='pt')`
	- truncation cuts off at the max_length, not sure why we want to do that

## Making Predictions

- pass in tokens as kwargs dictionary
- `import torch.nn.functional as F` to get softmax function
- `F.softmax(output[0], dim=-1)`
	- -1 is final dimension
	- this gives us probabilities
- `torch.argmax(probs) gives us the largest value, or the index of it

