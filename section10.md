# Metrics for Language

## Exact Match

- you can check and see if the answer to a question is an exact match and then take the average
- this gives an accuracy but is pretty rigid
- you can clean the answer a bit by removing syntax and capitalization

## ROUGE in Python

- `from rouge import Rouge`
- give it a model output and a reference
	- gives an f-1 score, a precision, and recall for unigrams, bigrams, and l metric
- `rouge.get_scores(model_out, reference, avg=True)` to get the average score

## Applying ROUGE to QA

- `tqdm` is an interesting package that gives you a status bar for python code
- the rouge library does not consider punctuation, so you might want to clean outputs

## Recall, Precision, F1

- recall: number n-grams found in model and reference/number of n-grams in reference
	- doesn't take into account other words given in the output
	- model could game the system and just dump words
- precision: number of n-grams found in model and reference/number of n-grams in model
- F1: 2*((precision*recall)/(precision+recall))
	- combines the two and is a more balanced metric
	- still given as a percentage

## Longest Common Subsequence (LCS)

- l metric in the rouge package
- counts the longest shared sequence between the two
- use the LCS as the numerator in the recall and precision and still calculate F1 with the precision and recall calculated from that

## QA performance with ROUGE

- Recall oriented understudy for gisting evaluation
- it's a set of metrics
- N, L, S
	- N: number of matching n-grams between the predicted and reference
		- uses N as the different level of n-grams


