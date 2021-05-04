# Emojional
Inspired by the current lack of existing emoji embedding models and their limited understanding of the nature of the evolving emotional content of the emoji, I have created novel emoji embeddings using their emotional content from their dictionary meanings. The subsequent emoji embeddings beat the state-of-the-art embeddings by up to 4% when tested on the task of sentiment analysis. 

## Creating the Dataset

I used the full emoji list, v13.1 from Unicode.org (1816 emojis)
I scraped the key emotive words from the online dictionaries Emojipedia and Emojis.Wiki and created a new dataset. 

The data looks like this:
🔮	crystal ball	future	magic	mysterious

## Change Dataset Format

In order for the model to train the data, the data needs to look like this:

* crystal ball	🔮	True
* magic	🔮	True
* mysterious	🔮	True

To achieve this I created a change dataset format script. It also has the functionality to shuffle the data.

## Negative Sampling

To make quality embeddings, I needed to create negative samples.

* ripe fruits🔮	False
* dirt	🔮	False
* approval	🔮	False

## Test Train Dev Split

In the original paper the authors used a training folder which contained the datasets train.txt, test.txt and dev.txt in a 91.8% train, 4.1% test, 4.1% develop split, and a testing folder which contained 20 identical true samples in train.txt, test.txt and dev.txt. The original layout and contents of these folders can be seen here. emoji2vec 
My full dataset consisted of [12205] true samples and [1000] of false samples. I also used the same 91.8% train, 4.1% test, 4.1% develop split.
8.2% of 12205 = 1000 (rounded down)

## My Data Folder

Total true samples = 12205
(91.8% train, 4.1% test, 4.1% develop split)

### Training Folder

* Train.txt consists of 11205 true samples.
* Test.txt consists of 500 true samples and 500 false samples.
* Dev.txt consists of 500 true samples and 500 false samples which are different from then test.txt.

Training folder uses all true samples available spread over the three datasets
The training folder also uses a total of 500 negative samples

### Testing Folder

* Train.txt uses 20 true samples
* Test.txt uses 20 true samples
* Develop.txt uses 20 true samples

My folders can be found here.

## Training

I used a repository developed by Piotr Wiercinski who has created a PyTorch implementation of emoji2vec. It can be found here.
The train test and dev files have been replaced in both training and testing folders. The file ‘phrase_embeddings.pkl’ in the ‘pre-trained’ folder (if it exists) needs to be deleted as this will allow a new dictionary to be created from the new dataset. I ran the file ‘presentation.ipynb’ to train the embeddings.
Here is the accuracy of my model when training:


## Testing

I used emoji2vec's original testing script and updated to current Python standards here.

### Results
I have evaluated the emoji embeddings on a list of emotive words, emoji similarity and analogies.
I compared my emoji embeddings to the state-of-the-art emoji embeddings on a Twitter sentiment analysis.
As you can see it performed really well for the most popular emojis:

## Visulization

### Homour

### Plutchicks Wheel of Emotion

### Similarities

### Analogy

### Visualizing Embeddings in 2D spaces

## Using the Emoji Embeddings

