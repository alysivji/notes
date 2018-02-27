# Natural Language Processing with Python

by Steven Bird, Ewan Klein, and Edward Loper

## Chapter 1: Language Processing and Python

### Computing with Language: Texts and Words

* `concordance` permits us to see words in context
* `similar` shows other words that appear in similar contexts
* `common_contexts` to see shared context for two or more words
* `dispersion_plot` gives visual of where word is used in document

```python
def lexical_diversity(text):
    return len(set(text)) / len(text)
```

### Computing with Language: Simple Statistics

* `FreqDist()` creates a frequency distribution
* `hapaxes` occur only once
* *collocation* is a sequence of words that occur together unusually often, i.e. red wine
    * `bigrams()` or `.collocations`

### Automatic Natural Language Understanding

* **word sense disambiguation** we want to work out which sense of a word was intended in a given context

## Chapter 2: Accessing Text Corpora and Lexical Resources

* **corpus** - large body of text
* **stylistics** - studying systematic differences between genres

* **conditional frequency distribution** - collection of frequency distributions, each one for a different "condition"

```python
def plural(word):
    if word.endswith('y'):
        return word[:-1] + 'ies'
    elif word[-1] in 'sx' or word[-2:] in ['sh', 'ch']:
        return word + 'es'
    elif word.endswith('an'):
        return word[:-2] + 'en'
    else:
        return word + 's'
```

* **stopwords** are high-frequency words that we want to filter out of a document before further processing

```python
from nltk.corpus import stopwords
stopwrods.words('english')

def content_fraction(text):
    stopwords = nltk.corpus.stopwords.words('english')
    content = [w for w in text if w.lower() not in stopwords]
    return len(content) / len(text)
```

* `nltk.corpus.words.words()` is the `/usr/share/dict/words` file from Unix
* `nltk.corpus.names` is the names corpus

* WordNet is a semantically-oriented dictionary of English, similar to a traditional thesaurus but with a richer structure
    * `from nltk.corpus import wordnet as wn`

* **hyponyms** - a word of more specific meaning than a general or superordinate term applicable to it
    * `nltk.app.wordnet()`

* meronyms - items to their components
* holonyms - things they are contained in
* entailments - relationships between words
* antonymy - relationships between lemmas

* use synsets to find semantically related documents
    * article about car is also an article about a vechicle

>  Two synsets linked to the same root may have several hypernyms in common. If two synsets share a very specific hypernym — one that is low down in the hypernym hierarchy — they must be closely related.
    * `.path_similarity()`

## Chapter 3: Processing Raw Text

`from nltk import word_tokenize`

> texts found on the web may contain unwanted material, and there may not be an automatic way to remove it. But with a small amount of extra work we can extract the material we need

### Natural Language Processing Pipeline

<img src="images/nltk_pipeline.png" />

> The Processing Pipeline: We open a URL and read its HTML content, remove the markup and select a slice of characters; this is then tokenized and optionally converted into an nltk.Text object; we can also lowercase all the words and extract the vocabulary.

* `nltk.Index()` converts this into a useful index

```python
def stem(word):
    """Stem word using regular expression"""

    regexp = r'^(.*?)(ing|ly|ed|ious|ies|ive|es|s|ment)?$'
    stem, suffix = re.findall(regexp, word)[0]
    return stem
```

### Normalizing Text

> By using lower(), we have normalized the text to lowercase so that the distinction between The and the is ignored. Often we want to go further than this, and strip off any affixes, a task known as stemming. A further step is to make sure that the resulting form is a known word in a dictionary, a task known as lemmatization

#### Tokenization

> Tokenization is the task of cutting a string into identifiable linguistic units that constitute a piece of language data.

* The function `nltk.regexp_tokenize()` is similar to `re.findall()`. However, `nltk.regexp_tokenize()` is more efficient for this task, and avoids the need for special treatment of parentheses

> Tokenization turns out to be a far more difficult task than you might have expected. No single solution works well across-the-board, and we must decide what counts as a token depending on the application domain.
>
> When developing a tokenizer it helps to have access to raw text which has been manually tokenized, in order to compare the output of your tokenizer with high-quality (or "gold-standard") tokens.
>
> A final issue for tokenization is the presence of contractions, such as didn't. If we are analyzing the meaning of a sentence, it would probably be more useful to normalize this form to two separate forms: did and n't (or not). We can do this work with the help of a lookup table.

#### Segmentation

A more general form of tokenization, i.e. sentence segmentation.

> Sentence segmentation is difficult because period is used to mark abbreviations, and some periods simultaneously mark an abbreviation and terminate a sentence, as often happens with acronyms like U.S.A.

#### Stemming

```python
porter = nltk.PorterStemmer()
lancaster = nltk.LancasterStemmer()
```

> Stemming is not a well-defined process, and we typically pick the stemmer that best suits the application we have in mind. The Porter Stemmer is a good choice if you are indexing some texts and want to support search using alternative forms of words

#### Lemmatization

> The WordNet lemmatizer is a good choice if you want to compile the vocabulary of some texts and want a list of valid lemmas (or lexicon headwords).

### Summary

* Texts found on the web may contain unwanted material (such as headers, footers, markup), that need to be removed before we do any linguistic processing.
* Tokenization is the segmentation of a text into basic units — or tokens — such as words and punctuation. Tokenization based on whitespace is inadequate for many applications because it bundles punctuation together with words. NLTK provides an off-the-shelf tokenizer nltk.word_tokenize().
* Lemmatization is a process that maps the various forms of a word (such as appeared, appears) to the canonical or citation form of the word, also known as the lexeme or lemma (e.g. appear).
