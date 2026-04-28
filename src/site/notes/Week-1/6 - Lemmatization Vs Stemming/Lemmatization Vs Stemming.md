---
{"dg-publish":true,"permalink":"/week-1/6-lemmatization-vs-stemming/lemmatization-vs-stemming/","dg-note-properties":{}}
---



Deciding whether to implement a lightweight stemmer or require a heavier lemmatizer dependency will be a critical design choice. Both techniques aim to solve the same problem—reducing variations of a word to its base form—but they do so using completely different computational philosophies.

---

# Stemming vs. Lemmatization

## 1. The Core Objective

In any NLP application (like a search engine or sentiment classifier), words like _talk_, _talking_, and _talked_ all carry the exact same semantic meaning. If your model treats them as three entirely separate mathematical entities, it wastes compute power and loses context.

The goal of the pre-processing stage is **Text Normalization**: mapping all variations of a word back to a single base representation.

زي في العربي، يكتب، كاتب، مكتوب، كتاب، كلهم جايين من جذر واحد "كتب"، الفرق بين الطريقتين في طريقة وصولهم للجذر ده, واحد منهم بصمجي بيشيل المقاطع من اول و اخر الكلمة, انما التاني فعلا مذاكر قاموس الانجليزي و عارف الحالات الشاذة للكلمات

## 2. Stemming: The Rule-Based Axe

Stemming is a fast, crude, heuristic approach. It does not know English grammar; it only knows character patterns.

- **How it works:** It uses hardcoded rules to chop off common prefixes and suffixes (like `-ing`, `-ed`, `-able`, `-ity`).
    
- **The Library:** We use **NLTK** for stemming (specifically algorithms like the `PorterStemmer` or `SnowballStemmer`) because spaCy refuses to support stemming due to its inaccuracy.
    
- **The Pros:** It is incredibly fast and computationally cheap.
    
- **The Cons:** Because it just blindly chops letters, it frequently outputs "non-words".
    
    - _Example 1 (Success):_ `talking` $\rightarrow$ `talk`
        
    - _Example 2 (Failure):_ `ability` $\rightarrow$ `abil`
        
    - _Example 3 (Failure):_ `ate` $\rightarrow$ `ate` (It fails completely on irregular verbs).
        



```Python
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()
print(stemmer.stem("eating"))     # Output: eat
print(stemmer.stem("ability"))    # Output: abil
```

![Pasted image 20260423200246.png](/img/user/Imgs/Pasted%20image%2020260423200246.png)

| Porter Stemming Algorithm                                                                                | Snowball Stemming (Porter2 stemming)                                                                                                                                                                                                     | Lancaster Stemming                                                                                           |
| -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| It is less aggressive in stemming and keeps more of the original word than some of the other algorithms. | This algorithm is more aggressive and efficient than the Porter algorithm. It applies a larger set of rules to reduce words to their base form.                                                                                          | This algorithm is the most aggressive among the three. It uses many rules to reduce a word to its base form. |
| Porter stemmers only stem English words                                                                  | the Snowball stemmer can stem texts in a number of other Roman script languages, such as Dutch, German, French, and even Russian<br><br>it is a better version of the Porter Stemmer since some issues of it were fixed in this stemmer. | Lancaster Stemming  only stem English words                                                                  |

## 3. Lemmatization: The Linguistic Scalpel

Lemmatization is a sophisticated, vocabulary-based approach. It maps a word back to its dictionary root, formally known as the **lemma**.

- **How it works:** It uses trained language models and built-in dictionaries to understand the part-of-speech context before transforming the word.
    
- **The Library:** We use **spaCy** for this, as lemmatization is a default component of its pre-trained pipelines (`en_core_web_sm`).
    
- **The Pros:** It produces highly accurate, readable dictionary words, successfully handling irregular verbs and complex grammar.
    
- **The Cons:** It requires loading a heavy language model into memory and is significantly slower than stemming.
    
    - _Example 1 (Success):_ `ate` $\rightarrow$ `eat` (Understands past tense).
        
    - _Example 2 (Success):_ `better` $\rightarrow$ `well` (Understands degrees of comparison).
        



```Python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("He ate better today and demonstrated great ability.")

for token in doc:
    print(f"{token.text} -> {token.lemma_}")
# Output: ate -> eat | better -> well | ability -> ability
```

## 4. Customizing the Lemmatizer (Handling Slang)

Because lemmatization relies on a formal dictionary, it will fail on internet slang. If a user types "bro" or "brah", the default model will just return "bro" and "brah", failing to normalize them.

You can intercept and customize this behavior by modifying the **Attribute Ruler** in the spaCy pipeline. This allows you to force specific words to map to a specific lemma.



```Python
# Extract the attribute ruler component from the pipeline
ruler = nlp.get_pipe("attribute_ruler")

# Add a custom rule: map 'bro' and 'brah' to the lemma 'brother'
ruler.add([{"TEXT": "bro"}, {"TEXT": "brah"}], {"LEMMA": "brother"})

doc = nlp("bro and brah")
for token in doc:
    print(token.lemma_) # Output: brother, brother
```
