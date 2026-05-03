# Word2Vec_hindi

Welcome to **Word2Vec_hindi**  
This project is my attempt at implementing the **Word2Vec model from scratch**, specifically for the **Hindi language**.

The primary goal of this project is learning by building — understanding how word embeddings work by implementing them myself rather than relying on high-level libraries.

Feel free to explore the project, experiment with it, and raise issues or suggestions. While I may not implement every suggestion, I genuinely appreciate feedback and ideas.

---

## Project Status

This is an evolving project, and I will continue improving it as I deepen my understanding of NLP and representation learning.

---

## Latest Updates

- Combined 5 datasets:
   1. kapilverma/hindi-bible
   2. aiswaryaramachandran/hindienglish-corpora
   3. preetviradiya/english-hindi-dataset
   4. vaibhavkumar11/hindi-english-parallel-corpus
   5. disisbig/hindi-wikipedia-articles-172k   
- The combined text data has almost 82M tokens. The dataset was broken into a vocabulary of over 500K unique words.
- Now the context window has been increased to 5, which creates 10 instances of **(context, target)** pairs for each instance of a **context** word
  
---

## Datasets Used

1. **Hindi Bible**  
   Source: https://www.kaggle.com/datasets/kapilverma/hindi-bible  

2. **Hindi-English Corpora**  
   Source: https://www.kaggle.com/datasets/aiswaryaramachandran/hindienglish-corpora  

3. **English-Hindi Dataset**  
   Source: https://www.kaggle.com/datasets/preetviradiya/english-hindi-dataset

3. **IIT Bombay English-Hindi Translation Dataset**  
   Source: https://www.kaggle.com/datasets/vaibhavkumar11/hindi-english-parallel-corpus
   
3. **Hindi Wikipedia Articles - 172k**  
   Source: https://www.kaggle.com/datasets/disisbig/hindi-wikipedia-articles-172k 
   

---

## Dataset Preprocessing

The preprocessing pipeline includes:

- Concatenating Hindi text from all datasets  
- Removing punctuation and noise  
- Tokenizing text into words  
- Building a vocabulary from the corpus  

### Key Improvements

#### 1. Vocabulary Pruning

Instead of using all unique words, I now keep only words that appear more than 5 times in the dataset This helps:
- Reduce vocabulary size
- Improve training efficiency
- Remove noisy/rare words

#### 2. Context Window Update
- Previous window size: 3
- Current window size: 5
This allows the model to:
- Capture broader context
- Learn better semantic relationships

## Training Data Generation

For each word in a sentence:
- Treat it as the center (context) word
- Select surrounding words within the window as target words

#### Example
Sentence:
```text
आज सुबह मैंने अपने पुराने दोस्त के साथ बाजार में चाय पी
```
If the context word is: दोस्त

With a window size of 5, surrounding words are used to create pairs like:
```text
[दोस्त, सुबह]
[दोस्त, मैंने]
[दोस्त, अपने]
[दोस्त, पुराने]
[दोस्त, के]
[दोस्त, साथ]
[दोस्त, बाजार]
[दोस्त, में]
[दोस्त, चाय]
[दोस्त, पी]
```
This process is repeated for all words in the corpus to generate training pairs.

## Negative Sampling

In addition to positive pairs, I also generate negative samples:

Random words are selected from the vocabulary
These words do not appear in the context window of the center word
They are paired with the center word as negative examples

Example:
```text
[दोस्त, किताब]
[दोस्त, पहाड़]
[दोस्त, कंप्यूटर]
```
These pairs represent words that are unlikely to co-occur with the context word.

### Why Negative Sampling?

Negative sampling helps the model:
- Learn to distinguish between relevant and irrelevant word pairs
- Improve embedding quality
- Reduce computational cost compared to full softmax

This process is repeated for all words in the corpus to generate training data.

## Model Overview (Current)
- Architecture: Skip-gram style training
- Embeddings learned using word2vec training
- Optimizer: Adagrad
- Loss Function: BCEWithLogitsLoss
- Training uses both positive and negative word pairs

## Contributions
This is primarily a learning project, but suggestions and ideas are always welcome.

## References
- https://jalammar.github.io/illustrated-word2vec/
- https://medium.com/@manansuri/a-dummys-guide-to-word2vec-456444f3c673
- https://jaketae.github.io/study/word2vec/

## Author
```text
Abhishek Biswas
Software Developer | Interested in AI & Web Development
```
