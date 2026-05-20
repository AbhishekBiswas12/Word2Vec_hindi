# Word2Vec_hindi

Welcome to **Word2Vec_hindi**

This project is my attempt at implementing the **Word2Vec model completely from scratch**, specifically for the **Hindi language**.

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
- Generating over **1.5 Billion skip-gram training pairs**
- Training multiple embedding models with dimensions ranging from **300–400**
- Evaluating embeddings using:
  - cosine similarity
  - nearest-neighbor retrieval
  - analogy testing
  - embedding visualization using PCA and t-SNE

The current best-performing model:
- Embedding Size: **350**
- Training Loss: **~0.38**
- Validation Loss: **~0.47**

The model is now producing meaningful semantic separation between positive and negative word pairs.

---

# Latest Updates

- Combined 5 large Hindi datasets into a single training corpus
- Final corpus size reached approximately **82M tokens**
- Vocabulary built from words occurring atleast **2 times**
- Final vocabulary size exceeds **500K unique words**
- Context window size increased from **3 → 5**
- Generated approximately:
  - **1.5 Billion training skip-gram pairs**
  - **40M validation pairs**
  - **40M testing pairs**
- Implemented:
  - Skip-gram training
  - Negative sampling
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
- Generating skip-gram training pairs
- Generating negative samples

---

# Vocabulary Pruning

Instead of keeping every unique token, only words appearing atleast **2 times** are retained.

This helps:
- Reduce vocabulary size
- Improve training efficiency
- Remove noisy and corrupted tokens
- Improve embedding quality

---

# Context Window

- Previous context window size: **3**
- Current context window size: **5**

With a window size of 5:
- each center word can generate up to 10 positive pairs
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

In addition to positive pairs, negative samples are generated.

Random vocabulary words that do not appear in the context window are paired with the center word.

## Example

```text
[दोस्त, कंप्यूटर]
[दोस्त, पहाड़]
[दोस्त, विज्ञान]
```

These represent unlikely co-occurrences.

---

# Why Negative Sampling?

Negative sampling helps:
- Learn meaningful semantic separation
- Distinguish related vs unrelated words
- Scale training efficiently to very large vocabularies
- Avoid the computational cost of full softmax

---

# Model Architecture

Current training setup:

- Architecture: Skip-gram Word2Vec
- Framework: PyTorch
- Embedding dimensions tested:
  - 300
  - 350
  - 400
- Best-performing embedding size so far: **350**
- Optimizer: Adagrad
- Loss Function: BCEWithLogitsLoss
- Training uses:
  - positive skip-gram pairs
  - negative sampled pairs

---

# Current Results

The model now learns strong separation between positive and negative pairs.

Observed probability ranges:
- Positive pairs: ~0.94
- Negative pairs: ~0.07

The embeddings are beginning to capture:
- semantic similarity
- contextual relationships
- syntactic structure

---

# Embedding Evaluation

Current evaluation methods include:

## 1. Cosine Similarity

Used to retrieve semantically similar words.

Example goals:

```text
राजा → रानी, सम्राट, शासक
```

---

## 2. Analogy Testing

Evaluating vector arithmetic relationships such as:

```text
राजा - पुरुष + महिला ≈ रानी
```

---

## 3. Embedding Visualization

Using:
- PCA
- t-SNE

to visualize learned word clusters in 2D space.

---

# Future Improvements

Planned improvements include:

- Subsampling extremely frequent words (In progress)
- Improved negative sampling strategies

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
