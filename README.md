# Emojional
Inspired by the current lack of existing emoji embedding models and their limited understanding of the nature of the evolving emotional content of the emoji, we have created novel emoji embeddings using their emotional content from their dictionary meanings. The subsequent emoji embeddings **are generally more accurate than the state-of-the-art embeddings** when tested on the task of sentiment analysis. 

As these embeddings were also trained on keywords, the subsequent embeddings are durable and can be used in different natural language tasks such as emotion, cyberbully and sarcasm detection successfully. The current embedding file contains all emojis as of [v13.1](https://unicode.org/emoji/charts/full-emoji-list.html) from Unicode.org (1816 emojis). The emoji embedding file will be updated when new emojis are added. 

## Creating the Dataset

We scraped the key emotive words from the online dictionaries [Emojipedia](https://emojipedia.org) and [Emojis.Wiki](https://emojis.wiki) and created a new dataset. 

The data looks like this:
🔮	crystal ball	future	magic	mysterious

In order for the model to train the data, the data needs to look like this:

* crystal ball	🔮	True
* magic	🔮	True
* mysterious	🔮	True

To achieve this we created a [change dataset format](https://github.com/elenabarry/emojional/blob/main/Helpful%20Scripts/Change_dataset_format.ipynb) script which also shuffles the data.

### Negative Sampling

To make quality embeddings, we needed to create negative samples.

* ripe fruits🔮	False
* dirt	🔮	False
* approval	🔮	False

## Test Train Dev Split

Our full dataset consists of 10854 true samples and 890 of false samples. We use a 91.8% train, 4.1% test, 4.1% develop split.

### Our Data Folder

The data we used to train the model can be found [here](https://github.com/elenabarry/emojional/tree/main/Data). 

#### Training Folder

* Train.txt consists of 9964 true samples.
* Test.txt consists of 445 true samples and 445 false samples.
* Dev.txt consists of 445 true samples and 445 false samples which are different from then test.txt.


#### Testing Folder

* Train.txt uses 20 true samples
* Test.txt uses 20 true samples
* Develop.txt uses 20 true samples

The testing folder contains 20 identical true samples. 

## Training

We used a [PyTorch implementation of emoji2vec](https://github.com/pwiercinski/emoji2vec_pytorch). The original implementation of emoji2vec can be found [here](https://github.com/uclnlp/emoji2vec). 

### Training your own dataset
The train test and dev files have been replaced in both training and testing folders. The file ‘phrase_embeddings.pkl’ in the ‘pre-trained’ folder (if it exists) needs to be deleted as this will allow a new dictionary to be created from the new dataset. We ran the file ‘presentation.ipynb’ to train. Our version of the file can be found here. 

## Testing
We used emoji2vec's original testing script and have updated it to current Python standards here.

### Results
We have evaluated the emoji embeddings on a list of emotive words, emoji similarity and analogies.
We compared our emoji embeddings to the state-of-the-art emoji embeddings on a Twitter sentiment analysis. Beating all other embeddings using Random Forests. 


## Mapping to Emotions and Key Words

Our resulting emoji embeddings are successful at predicting different emoji usage:

### Plutchicks Wheel of Emotion
![Screenshot 2021-05-30 at 15 55 08](https://user-images.githubusercontent.com/53048127/120110051-046bdc00-c164-11eb-984c-ce45643e8159.png)

### Humour
![Screenshot 2021-05-30 at 16 32 36](https://user-images.githubusercontent.com/53048127/120110279-bc998480-c164-11eb-8208-a8d8ff89adfe.png)

### Seasonal
![Screenshot 2021-05-30 at 15 51 04](https://user-images.githubusercontent.com/53048127/120110044-fa49dd80-c163-11eb-8ca6-a93a53ecacc6.png)

## Visulization

### Visualizing Embeddings in 2D spaces
![download](https://user-images.githubusercontent.com/53048127/117536197-93685700-aff1-11eb-80ae-6bc98a5a8bb4.png)

## Using the Emoji Embeddings

To use the embedding you need to download the emojional.bin file and include the following code within your model.
```python
import gensim

e2v = gensim.models.KeyedVectors.load_word2vec_format("emojional.bin", binary=True)
```

## References

