# Neural-Machine-Translation
In Machine Translation, our goal is to convert a sentence from the source language (e.g. English) to the
target language (e.g. Spanish). In this repository, we implement three different implementations to build a Neural Machine Translation (NMT) system. 
1. **Model RepeatVector** - One-shot translation using Repeatvector 
2. **Model Seq2seq** - Seq2seq architecture
3. **Model Seq2seq1** - Seq2seq architecture with a little tweaking

To check out the results from respective models,see the results text files.
## Model RepeatVector  
One-shot translation model where the input text is encoded to a vector through embedding and a RNN layer(in our case LSTM).Then this vector is repeated for Ty(number of output time steps)
We trained this model on anki dataset which has relatively shorter sentences ,so we didn't use attention.

![Model RepeatVector](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/src/model_repeatvector.jpeg)

We tried to tweak and mess around with the model parameters and tanulated few results:

![Model RepeatVector Results](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/src/model_repeatvector_comparisons.jpeg)

## Model Seq2seq 
Specifically, an seq2seq model first reads the source sentence using an encoder to build a "thought" vector, a sequence of numbers that represents the sentence meaning; a decoder, then, processes the sentence vector to emit a translation. This is often referred to as the encoder-decoder architecture. In this manner, NMT addresses the local translation problem in the traditional phrase-based approach: it can capture long-range dependencies in languages, e.g., gender agreements; syntax structures; etc.

![Model concept](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/src/seq2seq_representation.jpg)

**Embedding**
Given the categorical nature of words, the model must first look up the source and target embeddings to retrieve the corresponding word representations. For this embedding layer to work, a vocabulary is first chosen for each language. Note that one can choose to initialize embedding weights with pretrained word representations such as word2vec or Glove vectors. In general, given a large amount of training data we can learn these embeddings from scratch.
In our case, we decided to learn the embeddings from scratch.
**Encoder**
Once retrieved, the word embeddings are then fed as input into the main network, which consists of two multi-layer RNNs – an encoder for the source language and a decoder for the target language.The encoder RNN uses zero vectors as its starting states.
**Decoder**
The decoder also needs to have access to the source information, and one simple way to achieve that is to initialize it with the last hidden state of the encoder, encoder_state

![Model seq2seq](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/src/model_seq2seq.jpeg)

This model already worked fine with anki dataset so we added attention (luong style) to try it on data with longer sentences.
Luong Style Attention:

![Luong attention](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/src/attention.JPG)
## Model Seq2seq1
We tweaked with the original encoder-decoder structure by concatenating the encoded vector with the decoder embeddings and then gave processed it similarly as in the previous model.

![Model seq2seq1](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/src/model_seq2seq1.jpeg)

## Data Used
1. Anki - http://www.manythings.org/anki/spa-eng.zip
2. Data from cs224n assignment 4
##  Our pre-trained Models to check out the results:
To checkout our results or use our model as a pre-trained model , maintain the data consistency by using the cs224n assignment data files from here - https://drive.google.com/drive/folders/1iKU3rcOR-wpr6ucGo7g2_4Xb0pPhnTmZ?usp=sharing 

1. Model seq2seq (with attention)-[seq2seq model](https://drive.google.com/file/d/1-6MRmaS9JAPZ4NToToQXQUXVdSNf_HHI/view?usp=sharing)
  To use the above model however you would need to maintain the same set of vocabulary so run the [seq2seq notebook](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/nmt_seq2seq.ipynb) as it is (i.e. 100k samples,39486 english vocab and 56098 spanish vocab on cs224ndata)

2. Model seq2seq1 - Use the [seq2seq1 notebook](https://github.com/thebeyonder001/Neural-Machine-Translation/blob/master/nmt_seq2seq1.ipynb)
  - on anki dataset - [anki model](https://drive.google.com/file/d/1qrVwPP5yufADkde19XQ3JG6F-A3EhdBR/view?usp=sharing)
  ( 100k samples, 10807 english vocab and 20323 spanish vocab on anki with max length of sentences being 15 i.e. omitting the samples with legth more than that)
  - on cs224n dataset - [cs224n model](https://drive.google.com/file/d/1qrVwPP5yufADkde19XQ3JG6F-A3EhdBR/view?usp=sharing)
  ( 136510 samples, 30585 english vocab and 41637 spanish vocab on cs224n data(specifically trainsplit.txt from the above data link))
