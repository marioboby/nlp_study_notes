---
{"dg-publish":true,"permalink":"/week-1/4-tokenization/tokenization/","dg-note-properties":{}}
---



In our NLP pipeline, tokenization sits squarely in the **Pre-processing** phase. Before our models can do anything intelligent with text, we must chop that text down into distinct, meaningful units called _tokens_.

This is how spaCy handles this under the hood.

---

# Tokenization in spaCy

## 1. Why Simple Rules Fail

You might ask: _"Why do I need a library to tokenize text? Can't I just split sentences by periods and words by spaces?"_

Let's look at this string: `Dr. Strange said "Let's go to N.Y.!"`

If you write a simple Python script to split by spaces and periods, your script will fail catastrophically. It will think "Dr." is a full sentence, "N.Y." is two sentences, and it will group punctuation directly into the words (like `"Let's` or `N.Y.!"`), which ruins your dataset.

You need an engine that understands the rules, abbreviations, and exceptions of the English language.

## 2. The spaCy `Doc` and `Token` Objects

To start tokenizing, we create a language component and feed it text.



```Python
import spacy

# Create a 'blank' English language object
nlp = spacy.blank("en") 

# Process the text
doc = nlp("Dr. Strange loves pav bhaji for $2.")
```

When you pass text into `nlp()`, it instantly applies its tokenization rules and returns a `Doc` object. This document is essentially a sequence of `Token` objects.

- **Indexing:** You can grab individual tokens just like a Python list (e.g., `doc[0]` returns `Dr.`).
    
- **Spans:** You can grab a slice of tokens (e.g., `doc[1:4]`), which returns a `Span` object.
    

## 3. The Power of Token Attributes

Because spaCy is object-oriented, every token comes packed with linguistic attributes that make extracting information incredibly easy.

If we look at the token `$`, we can check its attributes:

- `token.is_alpha` $\rightarrow$ False
    
- `token.is_punct` $\rightarrow$ False
    
- `token.is_currency` $\rightarrow$ True
    

If we look at the token `2`:

- `token.is_num` $\rightarrow$ False (it is a digit, not the spelled-out word "two")
    
- `token.like_num` $\rightarrow$ True
    

**Real-World Use Case:** If you have a massive text file of student records and want to extract all their email addresses, you don't need complex Regular Expressions. You can simply iterate through the `Doc` and use the built-in attribute: `if token.like_email: list.append(token.text)`.

## 4. Customizing the Tokenizer

Language is messy, and sometimes you need to teach spaCy new slang. For example, the word "gimme" technically represents two distinct words: "give" and "me".

You can add special rules to the tokenizer to split specific strings into multiple tokens:



```Python
from spacy.symbols import ORTH

# Tell the tokenizer to split "gimme" into "gim" and "me"
nlp.tokenizer.add_special_case("gimme", [{ORTH: "gim"}, {ORTH: "me"}])
```

_Note: Tokenization is strictly about splitting text. You cannot modify the underlying text (e.g., changing "gim" to "give") during this phase._

## 5. The Concept of Pipelines

When you run `spacy.blank("en")`, you are creating an empty pipeline that _only_ contains a tokenizer.

Because the pipeline is blank, it doesn't know how to do **Sentence Tokenization** (grouping words into sentences). If you try to run `for sent in doc.sents`, it will throw an error.

To fix this, you must add components to your pipeline:



```Python
nlp.add_pipe("sentencizer")
```

However, the basic sentencizer is a bit "dumb" and might still fail on complex edge cases like "Dr.". To get perfect sentence tokenization, you load the full, pre-trained pipeline (`spacy.load("en_core_web_sm")`) which includes a tagger, parser, and entity recognizer to fully understand the grammar before splitting.

---

### Interactive: The Tokenization Engine

To really understand how spaCy's tokenizer is so much smarter than a simple space-splitter, we need to see it in action. spaCy processes text by looking for **Prefixes** (like `$`, `"`), **Suffixes** (like `!`, `"`), and **Exceptions** (like `Let's` $\rightarrow$ `Let`, `'s`).

![Pasted image 20260420200639.png](/img/user/Imgs/Pasted%20image%2020260420200639.png)

![Pasted image 20260420200652.png](/img/user/Imgs/Pasted%20image%2020260420200652.png)