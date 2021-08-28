# Named Entity Recognition

## Intro to Spacy

- NER is extracting named entity from a text
- !python -m spacy download en_core_web_md downloads the model for us
- creates a doc object when run on a corpus
- displacy shows the doc
	- For the entities they are given a label about what they might be related to

## Using data from Reddit API

- classic http, id, secret stuff
- not complicated. skipping this bit
- you can get the frequency of oranizations and stuff

## NER with sentiment

- find entities with the highest sentiment
- basically just get entities and then see how sentiment matches up with them
- you can calculate the average sentiment of an entity

## NER with roBERTa

- you can do NER with transformer models
- spacy uses hugging face transformers behind the scenes
- `en_core_web_trf` is a roberta-base model so it uses a transformer
- `!python -m spacy download en_core_web_trf`
- the whole thing works the same
- typically works better than the traditional spacy model
