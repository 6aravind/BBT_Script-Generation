# Big Bang Theory - Script Generator

Inspired by [Neural Translation Model](https://github.com/macbrennan90/translation-model/blob/master/translation-model.ipynb).

## Pre-Processing:

The Transcripts are downloaded from this [website](https://bigbangtrans.wordpress.com) and saved as a JSON file with key as episode name.
First, we have to remove non dialogues like 'Story: Chuck Lorre & Bill Prady', 'Credit sequence.', 'Teleplay: Robert Cohen & Dave Goetsch' from transcripts and add markers to denote start and end of each episode.

We can't remove the puctuations because the colon after the character name at the start of the episode denotes that the character is speaking. To transfer this knowledge to the model we will replace this by character name + speaks i.e sheldon: -> sheldonspeaks.
Then, [gensim](https://radimrehurek.com/gensim/) is used to identify proper nouns and replace them with single a word, [Spacy](https://spacy.io) as a tokenizer, we are almost done with pre-processing.

We have to add markers to denote the start and end of every dialogue, so that later when the sequence length is decided it will be easier to add paddings. The next step will be to change the list of dialogues into input and output for training the model. The last word of every input excluding the padding and end of the dialogue marker will be the first word in the output. This way the model will be able to learn which has character to speak for a given input.</br>  Example: 

Input: sheldonspeaks how thoughtful . thank you . ramonaspeaks </br>
Output: ramonaspeaks mmm . no big deal , i enjoy spending time with you . </br>


## Word Embedding:

[Google word embeddings](https://code.google.com/archive/p/word2vec/) are used as starting point for common words, new words will be trained using gensim. The less frequent words are replaced by a 'unk' token, vector for this token is generated randomly. 'pad' token will take zero vector of embedding size. While training the model, both the input and output sequence must use same word embeddings so we will not training the weights of the embedding layer.

## Train the model:
