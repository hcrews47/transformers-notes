# Preprocessing for NLP

## Stopwords

- words like in, a, an, the are not really useful for data. More noise than anything
- things like i, me, my, myself are all stopwords as well
- to optimize the speed convert, the nltk stopwords to a set from a list
- lower case all words in your corpus and then split by spaces
- `[word for word in tweet if word not in stopwords]` can use a list comprehension
- rejoin words to get the sentence back

## Tokens Introduction

- could be a word, a part of a word, a single character, a punctuation mark, special tokens like a url or a name, model-specific token
- some models even split words into "amaz" + "--ing"
- character level embeddings only need to know the alphabet, word level have to know the entire dictionary
	- model will not understand some words that the model has never seen before
	- could be difficult for some modernist writers who like to make up words
- character tokens are harder to embed context into the tokens
	- word tokens generally are better at context
- Special tokens can replace certain things, think like a web address or a twitter handle

### Model Specific tokens

- BERT has some special tokens like [PAD], [UNK] (unknown), [CLS], etc.
- Padding token allows to maintainn same-length seqs even when different sized sentences are fed in
- masking tokens are used in masked language modeling
- Bert uses 512 tokens per sequence, paddings are added to match this

## Stemming

- helps to simplify text before feeding to a model
- strips some characters to get it down to it's base
- model performance can be damaged by the nuances of adverb and such of a base word
- more complex models handle this in a more graceful way
- PorterStemmer and LancasterStemmer in nltk package
	- two very different stemmers
	- PorterStemmer is lightweight, Lancaster has a few more rules
	- Lancaster is more aggressive

## Lemmatization

- more advanced form of stemming
- goes down to real word roots instead of just cutting it off
- nltk you need to download 'wordnet', WordNetLemmatizer
- this just makes much more sense than stemming but is probably harder
- the lemmatizer requires that we also provide a Part of Speech
	- implement wordnet verb  argument
	- this doesn't make sense at the moment, need to be able to figure out what part of speech each is at runtime

## Unicode Normalization

### Canonical equivalence

- two characters that are rendered the same but are written differently
	- combined character sequences to create accents and such
	- could also be conjoined Korean characters
- need to find a singular format

### Compatibility Equivalence

- Font variances, circled variants
- superscript, subscript
- things can have the same meaning but be totally different for a model

### Composition and Decomposition

#### Canonical

- break things down to the decomp, most atomic state possible
- you could recombine everything if you want

#### Compatibility

- convert fancy fonts to base unicode fonts
- convert specialized fraction to the individual digits
- can't actually revert for fonting

### NFD and NFC

- there are 4 different "Normal Forms"
	- NFD, NFC, NFKD, NFKC
- `unicodedata` package 
- when you can't see the rendered difference, use canonical
- `unicodedata.normalize('NFD', c_with_cedilla) == c_plus_cedilla)
- can combine decomp and comp with NFC
	- this seems smart, revert to atomic state
- can apply to everything in your data

### NFKD and NFKC

- compatibility is referred to as (K)
- for font things you can't use NFD, have to use NFKD
- can do compatibility decomp, and then do canonical composition
	- NFKC, pretty much the ultimate for normalizing

*Basically just do the NFKC*


