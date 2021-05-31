# Emojional
Inspired by the current lack of existing emoji embedding models and their limited understanding of the nature of the evolving emotional content of the emoji, we have created novel emoji embeddings using their emotional content from their dictionary meanings. The subsequent emoji embeddings are **generally more accurate than the state-of-the-art embeddings** when tested on the task of sentiment analysis. 

As these embeddings were also trained on keywords, the subsequent embeddings are durable and can be used in different natural language tasks such as emotion, cyberbully and sarcasm detection successfully. The current embedding file contains all emojis as of [v13.1](https://unicode.org/emoji/charts/full-emoji-list.html) from Unicode.org (1816 emojis). The emoji embedding file will be updated when new emojis are added. 

## Creating the Dataset

We scraped the key emotive words from the online dictionaries [Emojipedia](https://emojipedia.org) and [Emojis.Wiki](https://emojis.wiki) and created a new dataset. [This](https://github.com/elenabarry/emojional/blob/main/Helpful%20Scripts/python_scraping.ipynb.zip) is the script used to scrape the websites. 

The data looks like this:
🔮	crystal ball	future	magic	mysterious

The full dataset can be found [here](https://github.com/elenabarry/emojional/blob/main/Data/emojional%20dataset.csv).

In order for the model to train the data, the data needs to look like this:

* crystal ball	🔮	True
* magic	🔮	True
* mysterious	🔮	True

To achieve this we created a [change dataset format](https://github.com/elenabarry/emojional/blob/main/Helpful%20Scripts/Change_dataset_format.ipynb) script which also shuffles the data.

### Negative Sampling

To make quality embeddings, we needed to create [negative samples](https://github.com/elenabarry/emojional/blob/main/Helpful%20Scripts/Negative_Sampling.ipynb).

* ripe fruits🔮	False
* dirt	🔮	False
* approval	🔮	False

## Test Train Dev Split

Our full dataset consists of 10854 true samples and 890 of false samples. We use a 91.8% train, 4.1% test, 4.1% develop split.

### Our Data Folder

The data used to train the model can be found [here](https://github.com/elenabarry/emojional/tree/main/Data). 

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
Download the repository of the [PyTorch implementation of emoji2vec](https://github.com/pwiercinski/emoji2vec_pytorch) and then replace the 'presentation.ipynb' file with [this](https://github.com/elenabarry/emojional/blob/main/PyTorch%20Emoji2vec/presentation.ipynb) version. You will need to download your own pretrained word vectors in binary format such as, [Google News word2vec](https://code.google.com/archive/p/word2vec/).

If the file ‘phrase_embeddings.pkl’ exists in the ‘pre-trained’ folder it needs to be deleted as this will allow a new dictionary to be created from the new dataset. You need to run the file ‘presentation.ipynb’ to train the emoji embeddings. Our **emojional embeddings** can be found [here](https://github.com/elenabarry/emojional/blob/main/Emojional%20Embeddings/emojional.bin). 

## Testing
Download the repository for [emoji2vec](https://github.com/uclnlp/emoji2vec) and replace these [files](https://github.com/elenabarry/emojional/tree/main/Original%20Emoji2vec) which have been updated to the current Python standards. Then add the emoji embeedings files you would like to test in the folder 'data/word2ec' as well as a copy of the Google News word2vec embeddings. Then run the file ['TwitterClassfication.ipynb'](https://github.com/elenabarry/emojional/blob/main/Original%20Emoji2vec/TwitterClassification.ipynb) to execute the testing. 

### Results
We compared our emoji embeddings to the state-of-the-art emoji embeddings on a Twitter sentiment analysis task. Our emojional embeddings generally beat other embeddings using Random Forests and score the second highest using Linear SVM. 

<img width="467" alt="Screenshot 2021-05-31 at 02 46 22" src="https://user-images.githubusercontent.com/53048127/120128657-73741f80-c1ba-11eb-8f0e-9930e157937b.png">

## Mapping to Emotions and Key Words
We have evaluated the emoji embeddings on a list of emotions, sensations, feelings and keywords. Each emoji embeddings can be seen to successfully display multiple senses.

### Plutchicks Wheel of Emotion
![Screenshot 2021-05-30 at 15 55 08](https://user-images.githubusercontent.com/53048127/120110051-046bdc00-c164-11eb-984c-ce45643e8159.png)

### Humour
![Screenshot 2021-05-30 at 16 32 36](https://user-images.githubusercontent.com/53048127/120110279-bc998480-c164-11eb-8208-a8d8ff89adfe.png)

### Seasonal
![Screenshot 2021-05-30 at 15 51 04](https://user-images.githubusercontent.com/53048127/120110044-fa49dd80-c163-11eb-8ca6-a93a53ecacc6.png)

## Visulization

### Visualizing Embeddings in 2D spaces
We also present our results in the form of t-SNE visualisation where you can see clusters of emotions in 2D space.
![download](https://user-images.githubusercontent.com/53048127/117536197-93685700-aff1-11eb-80ae-6bc98a5a8bb4.png)

## Using the Emoji Embeddings

To use the embedding you need to download the [emojional.bin](https://github.com/elenabarry/emojional/blob/main/Emojional%20Embeddings/emojional.bin) file and include the following code within your model.
```python
import gensim

e2v = gensim.models.KeyedVectors.load_word2vec_format("emojional.bin", binary=True)
```
