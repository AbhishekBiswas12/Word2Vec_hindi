# Word2Vec_hindi

Welcome to **Word2Vec_hindi**

This project is my attempt at implementing the **Word2Vec training pipeline from scratch using PyTorch**, specifically for the **Hindi language**.

The primary goal of this project is learning by building — understanding how word embeddings work internally by implementing the entire pipeline myself instead of relying on high-level NLP libraries.

The project currently includes:
- Dataset collection and preprocessing
- Vocabulary generation
- Skip-gram pair generation
- Negative sampling
- Custom PyTorch training pipeline
- Embedding evaluation and visualization

Feel free to explore the project, experiment with it, and raise issues or suggestions. While I may not implement every suggestion, I genuinely appreciate feedback and ideas.

---

# Project Status

This project has evolved from a small experimental implementation into a large-scale embedding training pipeline.

Current progress includes:
- Training on a corpus containing over **82M Hindi tokens**
- After subsampling corpus size: **~28M tokens**
- Generating over **1.5 Billion skip-gram training pairs**
- Evaluating embeddings using:
  - cosine similarity
  - nearest-neighbor retrieval
  - analogy testing
  - embedding visualization using PCA and t-SNE
- Embedding Size: **300**
  
The model is now producing meaningful semantic separation between positive and negative word pairs.

---

# Latest Updates

- Combined 5 large Hindi datasets into a single training corpus
- Final corpus size reached approximately **28M tokens**
- Vocabulary built from words occurring atleast **2 times**
- Final vocabulary size exceeds **500K unique words**
- Context window size: **10**
- Generated approximately:
  - **1.5 Billion training skip-gram pairs**
  - **42M validation pairs**
  - **28M testing pairs**
- Implemented:
  - Skip-gram training
  - Dynamic negative sampling
  - Frequent-word subsampling
  - BCEWithLogitsLoss training objective
  - Adagrad optimizer
- Added support for:
  - PCA embedding visualization
  - t-SNE embedding visualization
  - cosine similarity search
  - analogy-based embedding evaluation

---

# Datasets Used

## 1. Hindi Bible
Source:
https://www.kaggle.com/datasets/kapilverma/hindi-bible

## 2. Hindi-English Corpora
Source:
https://www.kaggle.com/datasets/aiswaryaramachandran/hindienglish-corpora

## 3. English-Hindi Dataset
Source:
https://www.kaggle.com/datasets/preetviradiya/english-hindi-dataset

## 4. IIT Bombay English-Hindi Translation Dataset
Source:
https://www.kaggle.com/datasets/vaibhavkumar11/hindi-english-parallel-corpus

## 5. Hindi Wikipedia Articles - 172k
Source:
https://www.kaggle.com/datasets/disisbig/hindi-wikipedia-articles-172k

---

# Dataset Preprocessing

The preprocessing pipeline currently includes:

- Combining Hindi text from multiple datasets
- Cleaning punctuation and noisy symbols
- Tokenizing text into words
- Building vocabulary mappings
- Removing extremely rare words
- Subsampling extremely frequent words
- Generating skip-gram training pairs
- Building a negative sampling distribution

---

# Vocabulary Pruning

Instead of keeping every unique token, only words appearing atleast **2 times** are retained.

This helps:
- Reduce vocabulary size
- Improve training efficiency
- Remove noisy and corrupted tokens
- Improve embedding quality

---

# Frequent Word Subsampling

Very frequent words can dominate the training corpus and generate a large number of relatively uninformative training pairs. To address this, frequent words are probabilistically discarded using the subsampling technique introduced in the original Word2Vec paper. This reduces the number of training pairs and gives relatively infrequent words more influence during training.

---

# Context Window

With a window size of 10:
- each center word can generate up to 20 positive pairs
- broader semantic context can be captured
- embeddings learn richer relationships

---

# Training Data Generation

For each word:
- The word is treated as the **center/context** word
- Neighboring words within the context window are treated as positive target words

## Example

Sentence:

```text
आज सुबह मैंने अपने पुराने दोस्त के साथ बाजार में चाय पी
```

If the center word is:

```text
दोस्त
```

Generated positive pairs:

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

This process is repeated across the entire corpus to generate training pairs.

---

# Negative Sampling

Instead of generating and storing negative examples beforehand, negative samples are generated dynamically during training.

Words are sampled from a unigram distribution raised to the power of 0.75, following the approach proposed in the original Word2Vec paper.

## Example

Positive pair:

```text
[दोस्त, बाजार]
```

Possible negative pairs:

```text
[दोस्त, कंप्यूटर]
[दोस्त, विज्ञान]
[दोस्त, पहाड़]
```
These sampled pairs are treated as negative examples during training, encouraging the model to assign higher scores to observed center-target pairs than to randomly sampled pairs.

# Why Negative Sampling?

Negative sampling helps:
- Learn meaningful semantic separation
- Distinguish related vs unrelated words
- Reduce memory requirements
- Scale training efficiently to very large vocabularies
- Avoid the computational cost of full softmax

---

# Model Architecture

Current training setup:

- Architecture: Skip-gram Word2Vec
- Framework: PyTorch
- Embedding dimension: **300**
- Optimizer: Adagrad
- Loss Function: BCEWithLogitsLoss
- Training uses:
  - positive skip-gram pairs
  - negative sampled pairs

---

# Current Results

The trained model achieves higher average logits for observed skip-gram pairs than for sampled negative pairs. These scores are used as diagnostic training metrics rather than as standalone measures of embedding quality. The embeddings are beginning to capture semantic similarity and syntactic relationship

---

# Future Improvements

Planned improvements include:
- Multiple negative samples per positive pair
- Intrinsic benchmark evaluation for Hindi embeddings
- Improved analogy testing

---

# Using the model

The model has been deployed to huggingface. You can use it by cloning the huggingface repository as follows:

```text
git clone https://huggingface.co/AbhishekBiswas12/word2vec-hindi
```

---

# Contributions

This is primarily a learning and research-oriented project, but suggestions, ideas, and feedback are always welcome.

---

# References

- https://jalammar.github.io/illustrated-word2vec/
- https://medium.com/@manansuri/a-dummys-guide-to-word2vec-456444f3c673
- https://jaketae.github.io/study/word2vec/

---

# Author

```text
Abhishek Biswas
Software Developer | Interested in AI, NLP, and Web Development
```
