---
{"dg-publish":true,"permalink":"/week-1/7-part-of-speech-pos/pos/","dg-note-properties":{}}
---


Before we can teach a machine to understand context, we need to teach it basic English grammar. Once a model knows whether a word is an action, an object, or a descriptive modifier, it can start doing some incredibly powerful text analysis.

This is how to identify and extract Parts of Speech using spaCy.

---

# Part of Speech (POS) Tagging

## 1. The Core 8 Parts of Speech

If you remember your middle school grammar classes, every word in an English sentence belongs to a specific category based on its function.

Here is a quick refresher on the fundamentals discussed in the lesson:

- **Noun:** A person, place, thing, or idea. (e.g., _Ahmed_, _Mars_, _fruits_)
    
- **Verb:** An action or state of being. (e.g., _ate_, _flew_, _cracked_)
    
- **Pronoun:** A substitute for a noun. (e.g., _he_, _she_, _they_, _it_)
    
- **Adjective:** Describes or modifies a noun. (e.g., _sweet_ fruits, _red_ Tesla, _smart_ engineer)
    
- **Adverb:** Describes a verb, adjective, or another adverb. (e.g., _slowly_ ate, _always_ scores)
    
- **Interjection:** Represents a strong emotion or expression. (e.g., _Wow!_, _Alas!_)
    
- **Conjunction:** Connects groups of words or clauses. (e.g., _and_, _but_, _or_)
    
- **Preposition:** Links a noun to another word, often indicating location or relationship. (e.g., _in_ the bus, _on_ the bus, _at_ the bus)
    

## 2. Part of Speech in spaCy (`token.pos_`)

When you load a pre-trained spaCy model (like `en_core_web_sm`), the pipeline includes a **Tagger**. This component automatically assigns a POS tag to every single token in your document.

You can access this broad category using `token.pos_`:



```Python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("Elon quickly bought a red Tesla.")

for token in doc:
    print(f"{token.text} -> {token.pos_}")
```

_Note: spaCy actually uses more than the standard 8 categories. For example, it distinguishes between a general `NOUN` (person, place, thing) and a `PROPN` (Proper Noun, like a specific name such as "Elon", اسم علم يعني)._

If you ever see a tag and don't know what it means, you can ask spaCy to explain it:



```Python
spacy.explain("PROPN") # Returns: "proper noun"
```

## 3. Detailed Tags (`token.tag_`)

While `pos_` gives you the broad category (e.g., VERB), sometimes you need deeper grammatical context, like knowing what tense the verb is in. spaCy provides this via the `tag_` attribute.

Let's look at how the model handles tense changes:

- **"He quits the job."**
    
    - `token.pos_` $\rightarrow$ `VERB`
        
    - `token.tag_` $\rightarrow$ `VBZ` (verb, 3rd person singular present)
        
- **"He quit the job."**
    
    - `token.pos_` $\rightarrow$ `VERB`
        
    - `token.tag_` $\rightarrow$ `VBD` (verb, past tense)
        

Because spaCy looks at the surrounding context, it is smart enough to know that "quit" without an "s" in this sentence happened in the past!

## 4. Real-World Application: Text Cleaning & Analysis

Why does any of this matter for a software engineer?

Imagine you are parsing a massive Microsoft earnings report. The raw text is full of garbage characters, spaces, and punctuation (`...`, `etc.`, `,`). You can use POS tags to instantly filter out the noise.



```Python
# Create an array of only meaningful words by filtering out punctuation, spaces, and 'X' (other/garbage)
filtered_tokens = []
for token in doc:
    if token.pos_ not in ["SPACE", "PUNCT", "X"]:
        filtered_tokens.append(token.text)
```

**Counting Parts of Speech:**

You can also do statistical analysis on a document. If you want to know exactly how many Proper Nouns are in a text, spaCy provides a highly efficient `doc.count_by()` method.



```Python
# Returns a dictionary of tag hashes and their occurrence counts
counts = doc.count_by(spacy.attrs.POS) 
```

you can use the vocab of the `en-core-web-sm` model to map each integer ID to the actual POS 

The `vocab` attribute in spaCy is a **`Vocab` object** that stores the vocabulary (vocabulary entries/lexemes) for your language model. Here's what it does:

**What is `doc.vocab`?**
- It's a vocabulary database containing all lexemes (unique word forms) the model has learned
- Each lexeme has an associated integer ID for efficient processing
- It's shared across all `Doc` objects from the same model

**How `doc.vocab[96].text` works:**
- `[96]` accesses the lexeme at index 96 in the vocabulary
- `.text` retrieves the actual string of that lexeme
- It returns what word/token corresponds to vocab ID 96

**What it's useful for:**
- **Reverse mapping**: Converting integer IDs back to words (as shown in your code)
- **Word vectors**: Access semantic similarity information (`doc.vocab[96].vector`)
- **Word frequencies**: Get occurrence counts (`doc.vocab[96].has_vector`)
- **Efficient storage**: spaCy uses integer IDs internally for memory efficiency, and vocab handles the ID↔text mapping

**In your notebook context:**
In the last two cells, you're:
1. Getting the vocabulary size and POS tag counts using `doc.count_by(spacy.attrs.POS)`
2. Using `doc.vocab[k].text` to convert the integer keys back into readable POS tag labels (like "NOUN", "VERB", etc.)

This is a common pattern—spaCy stores everything as integers for performance, and `vocab` provides the dictionary to look up what those integers represent.

---

### Interactive: The POS Explorer

To truly grasp how the Tagger component sees a sentence, use this interactive widget. You can type in different sentence structures to see how spaCy assigns grammatical meaning to each word in real-time.

![Pasted image 20260423213340.png](/img/user/Imgs/Pasted%20image%2020260423213340.png)