# Natural Language Processing with Python

by Steven Bird, Ewan Klein, and Edward Loper

## Chapter 1: Language Processing and Python

### Computing with Language: Texts and Words

* `concordance` permits us to see words in context
* `similar` shows other words that appear in similar contexts
* `common_contexts` to see shared context for two or more words
* `dispersion_plot` gives visual of where word is used in document

```python
def lexical_diversity(text): [1]
    return len(set(text)) / len(text) [2]

def percentage(count, total): [3]
    return 100 * count / total
```

### Computing with Language: Simple Statistics

* `FreqDist()` creates a frequency distribution
* `hapaxes` occur only once
* *collocation* is a sequence of words that occur together unusually often, i.e. red wine
    * `bigrams()` or `.collocations`

### Automatic Natural Language Understanding

* **word sense disambiguation** we want to work out which sense of a word was intended in a given context
