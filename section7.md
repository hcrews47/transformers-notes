# Long Text Classification with BERT

## Overview 

- so far we have restricted the length of input
- sentiment might not be clear from first 512 tokens
- could do a summarization model or could use windows
- also some special models you can use

## Using Windows

- BERT has maximum of 512 tokens
- can split text into 512 tokens at a time
- first break into input_ids and attention_mask
- can start breaking up input_ids into chunks or windows
- have to add your own start and close tokens to the inputs and attention_mask
- add your own padding tokens to inputs and mask
- input_dict will be `'input_ids': torch.Tensor([input_ids_chunk]).long()` and same for attention_mask except it's an `.int()`

## Window method with Pytorch

- should be more efficient and simpler
- return a 'pt' instead of 'tf'
- can use the split function on the tensors to chunk
- torch.cat to add tokens of torch.Tensor([101]) and torch.Tensor([102])

