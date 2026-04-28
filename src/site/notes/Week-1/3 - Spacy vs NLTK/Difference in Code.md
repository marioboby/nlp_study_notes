---
{"dg-publish":true,"permalink":"/week-1/3-spacy-vs-nltk/difference-in-code/","dg-note-properties":{}}
---



To verify your installations and see the architectural differences in action, we can run a simple "Hello World" script using both libraries. This will demonstrate how NLTK operates as a collection of functions while spaCy operates as a structured pipeline.

---
# Showcasing difference in tokenizing 
### 1. NLTK: The Functional Approach

In NLTK, you call specific functions for specific tasks. It treats the text as a raw string and returns lists of strings.



```run-python
import nltk
# Ensure you have the 'punkt' tokenizer data downloaded
# nltk.download('punkt')

text = "Dr. Strange loves pav bhaji. Hulk loves Delhi chat."

# Sentence Tokenization
sentences = nltk.sent_tokenize(text)
print(f"NLTK Sentences: {sentences}")

# Word Tokenization
words = nltk.word_tokenize(text)
print(f"NLTK Words: {words}")
```

### 2. spaCy: The Object-Oriented Approach

In spaCy, you load a language model once. When you pass text through it, it creates a `Doc` object. This object "knows" everything about the text—sentences, words, and grammar—without having to call separate functions for each.



```Python
import spacy

# Load the English model
nlp = spacy.load("en_core_web_sm")

text = "Dr. Strange loves pav bhaji. Hulk loves Delhi chat."

# Create the Doc object (The 'NLP Pipeline' runs here)
doc = nlp(text)

# Sentence Tokenization (Accessing the .sents attribute)
print("spaCy Sentences:")
for sent in doc.sents:
    print(sent.text)

# Word Tokenization (Iterating directly over the doc)
print("spaCy Words:")
tokens = [token.text for token in doc]
print(tokens)
```

### The Hierarchical Difference

When you tokenize, you are essentially breaking a large block of data into a hierarchy. Sentence tokenization identifies the boundaries of thought, while word tokenization identifies the boundaries of meaning.

Compare how these two logic styles handle different linguistic challenges. You can input custom text—such as sentences with titles (Dr., Mr.), abbreviations (etc., i.e.), or complex punctuation—to see how a "rule-based" functional approach differs from a "model-based" pipeline approach.

As you test different strings, notice how NLTK might struggle with "Dr." if not properly configured, whereas spaCy’s pre-trained model understands that the period after "Dr" is part of the title, not the end of the sentence. This is a primary reason why spaCy is frequently chosen for production-level software where edge-case accuracy is critical.

---

# But what ARE "Punkt" and "en_core_web_sm"

It is a great sign of an engineer when you want to know exactly what you are downloading into your system rather than just blindly running terminal commands!

When you download these two packages, you are downloading **pre-trained machine learning models**—essentially, the mathematical "brains" that these libraries use to understand text.

However, they are built very differently and serve different scopes. 

---

### 1. NLTK's `punkt` (The Sentence Boundary Expert)

The `punkt` package is a specialized, unsupervised machine learning model designed to do one thing exceptionally well: **figure out where a sentence ends.**

- **The Problem it Solves:** A naive Python script splits sentences at every period (`.`). But in English, periods are used for abbreviations (Dr., Mr., U.S.A., e.g., i.e.) or ellipses (...).
    
- **How it Works:** `punkt` is an algorithm (originally created by researchers Kiss and Strunk in 2006) that has read massive amounts of text to learn abbreviation patterns and collocations (words that frequently appear together). It uses this statistical knowledge to decide if a period marks the end of a thought or is just part of a title.
    
- **Why a Separate Download?** NLTK is modular. If you are building a tool for a language that doesn't use periods, or if you are only doing word-level analysis, you don't need `punkt`. NLTK makes you download it separately to save hard drive space and keep the core library lightweight.
    

---

### 2. spaCy's `en_core_web_sm` (The Complete Linguistic Brain)

Unlike `punkt` which is just a single tool, `en_core_web_sm` is a complete **pipeline**. It is a massive bundle of neural network weights, vocabulary rules, and statistical data that allows spaCy to perform almost every foundational NLP task at once.

We can literally decode the name to understand what it is:

- **`en` (English):** The language it was trained on. (spaCy has models like `es` for Spanish, `de` for German, etc.).
    
- **`core` (Core Vocabulary):** It is a general-purpose model. It understands standard grammar and vocabulary. (There are specialized models, like `scispacy`, which are trained specifically on medical or scientific papers).
    
- **`web` (Web Text):** This tells you _what_ it read to get smart. This specific model was trained on a massive dataset of blogs, news articles, and web comments (specifically, the OntoNotes 5 corpus).
    
- **`sm` (Small):** It is optimized for speed and low memory usage (it's only about 12 Megabytes). It comes with no word vectors. You can also download `md` (medium) or `lg` (large, around 500+ Megabytes) models if you need absolute state-of-the-art accuracy at the cost of processing speed.
    

**What it does:** When you pass text through `en_core_web_sm`, it doesn't just split sentences. In milliseconds, it assigns a Part-of-Speech tag (noun, verb) to every word, maps out the syntactic dependency (how the subject relates to the object), and performs Named Entity Recognition (identifying which words are Companies, People, or Locations).

---

### The TL;DR

- `punkt` is a specialized scalpel you pull out of the NLTK toolkit purely to chop text into sentences accurately.
    
- `en_core_web_sm` is a complete, lightweight artificial brain that reads the text once and instantly maps out all of its linguistic structure.
    
