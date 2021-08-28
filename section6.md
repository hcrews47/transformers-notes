# Project: Sentiment model with Tensorflow and transformers

## Overview

- use kaggle api to get data
- data preprocessing
- tensorflow input pipeline
- building and training model with sentiment and language classification
- making predictions

## Preprocessing

- can get the data from kaggle api. download key and all that
- still use pandas to read the data
- set your tokenizer to have a length 512
- use BertTokenizer with 'bert-base-cased'
- one-hot encode your training sentiment array

## Building a Dataset

- numpy can write to a numpy binary file
- Create a tensorflow dataset object with xids, xmask, and labels
	- (Xids, Xmask, labels) tuple
- need a two item tuple when feeding to tensorflow, inputs and outputs
	- inputs can be a dictionary of values
	- labels is the output value, input_ids and attention_mask is in input dicitonary

## Dataset shuffle, batch, split, and save

- take batch_sizes of something like 16
	- should shuffle data before batching
- shuffle argument around 10000
- define training, validation split
	- 90 to 10?
- dataset.take(size) takes a certain number of values
	- dataset.skip(size) skips the first number of values
- tf.data.experimental. save(train_ds, 'train') saves the data as a tensorflow file
- you need to save the `element_spec` in order to load the data

## Build and Save

- Initialize BERT model (tensorflow model)
- `from transformers import TFAutoModel`
	- load a pre-trained model such as 'bert-base-cased'
- models can have like 108 million parameters
- tf.keras.laers.Input(shape=(512,), name='input_ids', dtype='int32') to create an input
- then get the transformer
	- get `embeddings = bert.bert(input_ids, attention_mask=mask)[1]
- then get classifier head
	- tf.keras.layers.Dense(1024, activation='relu')(embeddings)
	- tf.keras.layers.Dense(5, activation='softmax', name='outputs')(x)
- now that we have all layers we need to initialize model
	- `tf.keras.Model(inputs=[input_ids, mask], outputs=y)
- don't necessarily need to optimize parameters in bert model, maybe test both
	- can freeze a layer
	- model.summary() to see the different layers, can select the Bert layer 
		- set the `model.layers[2].trainable=False`
	- this will lower your trainable params to something like 792k
- now need to optimize params
	- tf.keras.optimizer.Adam(lr=5e-5, decay=1e-6)
	- `tf.keras.losses.CategoricalCrossentropy()`
	- `tf.keras.metrics.CategoricalAccurary('accuracy')
	- `model.compile(optimizer=optimizer, loss=loss, metrics=[acc])
	- then can train on datasets when loaded with the proper element spec
	- use `model.fit` and then save the model

## Loading and Prediction

- `tf.keras.models.load_model('model_directory')
- import and load BertTokenizer
- tokenize data and set input_ids and attention_mask, no labels since real data now
- model.predict(tokenized_data)
- this will output an array of probabilities, you can then use np.argmax to find max value which will tell you the sentiment of the sentence
 
