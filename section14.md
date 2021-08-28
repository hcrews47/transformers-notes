# Fine-Tuning Transformer Models

## Visual guide to BERT pre-training

- during pretraining we have a PreTraining head
	- adds two different head: Next sentence prediction and Masked Language Modeling

### Next Sentence Prediction NSP

- we have a feed forward NN
	- outputs two different values
	- one represents "isNext" label
	- one represents "notNext"
	- together this says if two sentences belong together or not
- all come from classification output

### Masked Language Modeling

- still a feed-forward network
- tries to guess a masked token based on previous tokens
- optimize parameters to get optimal prediction of masked token
- pass vocab (30k) through a softmax that gives a probability distribution of likelihood to be answer
- take output distributions and map through an argmax function to get token_id, or a spot in the vocab list

- The two heads are used to optimize our BERT model

## Introduction to Bert for pretraining code

- `BertForPreTraining` is a model that contains an MLM and NSP head
- BertForPreTraining outputs a `prediction_logits` (MLM) and a `seq_relationship_logits` (NSP)
	- the size of the prediction_logits should be the size of the vocabulary

## BERT pretraining for MLM

- taking prediction_logits and turning them into token predictions
- `tokenizer.get_vocab()` gives vocab
	- token_vocab['hello'] gives index number value of that word
- `prediction_logits[0][2]` for a sentences specific word gives us the vocab values for that word
- `torch.nn.functional.softmax(prediction_logits[0]2[], dim=-1)` gives the probs
	- take the argmax of this and you get the predicted word

## Bert pretraining for NSP

- need to break up into multiple sentences
- you can pass multiple sentences into a tokenizer
- `torch.argmax(outputs.seq_relationship_logits)` gives us the index
	- index 0 is 'IsNext'
	- index 1 is 'NotNext'

# MLM

## The Logic of MLM

- use the BertForMaskedLM from transformers
	- contains just the MLM head
- using this model we will only get the `logits` which are the vocab predictions of the masked text
- need to have some masked text
	- can mask say 15% of a text randomly
	- need to make sure that separator tokens are not masked
	- create a new tensor that is the same as the input_ids and then manipulate that
	- need to get input_ids where != 102 (the separator token)
- when you give a 'labels', the model will also output a 'loss'

## Fine-tuning with MLM - Data Preparation

- each paragraph in a book you're gonna want to separate with a `/n`
- all paragraphs still need to be the same length, set max_length=512, but this will truncate
- to do training: `inputs['labels'] = inputs.input_ids.detach().clone()` will be training data
- test data will be after you apply some masking
	- `rand = torch.rand(inputs.input_ids.shape)`
	- `mask_arr = (rand < .15) * ( inputs.input_ids != 101) * (inputs.input_ids != 102) * (inputs.input_ids != 0)
	- `torch.flatten(mask_arr.nonzero()).tolist()

## Fine-tuning with MLM - Training

- need to create a pytorch dataset object
	- will be creating a class that inherits `torch.utils.data.Dataset`
	- initialize with encodings
- once data is loaded and you are training the model, you want to get an optimizer
	- `from transformers import AdamW`
	- helps prevent over-fitting
	- `optim = AdamW(model.parameters(), lr=5e-5)`
- loop through batchs provided by dataloader
	- use tqdm so that you can see the progress training bar
	- want to initialize some gradient stuff: `optim.zero_grad()`
	- add batch input_ids, labels, attention_mask to the device
	- once you have outputs, you want to extract the loss
	- update params with `optim.step()`
- kinda takes a while, meditations took like 15s and it's not that big of a book

## Fine-tuning with MLM - Training with Trainer

- much easier version of what we just did
- `from transformers import TrainingArguments`
- pass in model, TrainingArguments, and pytorch dataset
- so simple and easy

# NSP

## The Logic of NSP

- trying to guess whether sentence b comes after sentence a
	- trying to figure out longer term relationships
- `BertForNextSentencePrediction`
- also need to include a labels tensor in order to get the loss

## Data Preparation

- want to assign a 50% probability of using the actual next sentence and 50% random sentence
- create a big list of sentences
- randomly assign sentence and next sentence, sometimes true, sometimes not
- tokenize the two lists of sentences, list a and list b
- also create labels tensor telling you if a combo of a and b is next sentence or not

## DataLoader

- create dataset class that inherits from `torch.utils.data.Dataset`
- similar process as with MLM
- Adam weighted decay optimizer

# MLM and NSP

## The Logic of MLM and NSP

- Bert was trained with both MLM and NSP
- you can use pretrained model and do some more training with them
- if you want the loss tensor, you need to add two labels tensors
	- do the same cloning process
	- MASK a few tokens
	- add some next-sentence label tensors
	- use "labels" and "next_sentence_labels"

## Data Preparation

- kinda just combine the two above
- takes like 17s to train on just Meditations
- can do this to any training set
 
