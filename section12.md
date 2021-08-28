# Open-Domain QA

## ODQA Stack structure

<div align="center">
	<img src="assets/ODQA_structure.png" alt="ODQA" style="width: 200px;" />
</div>

- query goes to both retriever and reader
- retriever passes along to es doc_store and find closest matches
- retriever assigns scores and then passes contexts to reader
- reader tries to find answer based on each context

## Creating the database

- use meditations by marcus aurelieus for the project
- can split on `/n` characters
- set up elasticsearch to run locally and add book as contexts by paragraph

## Setup retriever and reader

- can set the FARMReader to have a context_window_size

