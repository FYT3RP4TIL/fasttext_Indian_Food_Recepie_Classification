# FastText Word Embeddings for Indian Food Recipes

This project demonstrates the use of FastText for creating and exploring word embeddings, with a special focus on Indian food recipes. We'll use pre-trained models for English and Hindi, and then train a custom model on Indian food recipe data.

## Table of Contents
1. [Installation](#installation)
2. [Using Pre-trained FastText Models](#using-pre-trained-fasttext-models)
   - [English Model](#english-model)
   - [Hindi Model](#hindi-model)
3. [Custom Training on Indian Food Recipes](#custom-training-on-indian-food-recipes)
   - [Data Preprocessing](#data-preprocessing)
   - [Training the Model](#training-the-model)
   - [Exploring the Custom Model](#exploring-the-custom-model)

## Installation

First, install the required libraries:

```bash
pip install fasttext pandas
```

## Using Pre-trained FastText Models

### English Model

Load the pre-trained English model and explore word embeddings:

```python
import fasttext

# Load the pre-trained English model
model_en = fasttext.load_model('C:\\Code\\nlp-tutorials\\downloads\\cc.en.300.bin')

# Get nearest neighbors for 'good'
print(model_en.get_nearest_neighbors('good'))

# Check the shape of the word vector
print(model_en.get_word_vector("good").shape)

# Get analogies
print(model_en.get_analogies("berlin", "germany", "france"))
```

Output:
```
[(0.7517593502998352, 'bad'),
 (0.7426098585128784, 'great'),
 (0.7299689054489136, 'decent'),
 ...]

(300,)

[(0.7303731441497803, 'paris'),
 (0.6408537030220032, 'france.'),
 (0.6393311023712158, 'avignon'),
 ...]
```

### Hindi Model

Load the pre-trained Hindi model and explore word embeddings:

```python
# Load the pre-trained Hindi model
model_hi = fasttext.load_model('C:\\Code\\nlp-tutorials\\downloads\\cc.hi.300.bin')

# Get nearest neighbors for "अच्छा" (good)
print(model_hi.get_nearest_neighbors("अच्छा"))
```

Output:
```
[(0.6697985529899597, 'बुरा'),
 (0.6132625341415405, 'अच्छे'),
 (0.608695387840271, 'अच्चा'),
 ...]
```

## Custom Training on Indian Food Recipes

### Data Preprocessing

1. Load the dataset:

```python
import pandas as pd
import re

df = pd.read_csv("Cleaned_Indian_Food_Dataset.csv")
print(df.shape)
print(df.head(3))
```

2. Define a preprocessing function:

```python
def preprocess(text):
    text = re.sub(r'[^\w\s\']',' ', text)
    text = re.sub(r'[ \n]+', ' ', text)
    return text.strip().lower()
```

3. Export preprocessed data to a text file:

```python
df.to_csv("food_recipes.txt", columns=["TranslatedInstructions"], header=None, index=False)
```

### Training the Model

Train a custom FastText model on the preprocessed data:

```python
import fasttext

model = fasttext.train_unsupervised("food_recipes.txt")
```

### Exploring the Custom Model

Explore the custom-trained model:

```python
# Get nearest neighbors for 'paneer'
print(model.get_nearest_neighbors("paneer"))

# Get nearest neighbors for 'halwa'
print(model.get_nearest_neighbors("halwa"))
```

Output:
```
[(0.6676578521728516, 'tikka'),
 (0.6331593990325928, 'bhurji'),
 (0.6316412687301636, 'tikkas'),
 ...]

[(0.7327786087989807, 'khoya'),
 (0.7155830264091492, 'sheera'),
 (0.6999987363815308, 'rabri'),
 ...]
```

This custom-trained model now provides word embeddings specifically tailored to Indian food recipes, allowing for more accurate and relevant word associations within this domain.

## Conclusion

This project demonstrates the versatility of FastText for working with word embeddings. We've shown how to use pre-trained models for English and Hindi, as well as how to train a custom model on domain-specific data. These techniques can be applied to various natural language processing tasks, particularly those involving specialized vocabularies or multilingual contexts.
