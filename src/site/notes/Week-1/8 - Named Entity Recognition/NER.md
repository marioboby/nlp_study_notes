---
{"dg-publish":true,"permalink":"/week-1/8-named-entity-recognition/ner/","dg-note-properties":{}}
---


Today, we are exploring one of the most commercially valuable components of an NLP pipeline: **Named Entity Recognition (NER)**.

While Part-of-Speech tagging helps a machine understand grammar, NER helps a machine understand the _real world_. It bridges the gap between text and actual objects, people, locations, and concepts.

This is how NER works, its limitations, and how to manipulate it in spaCy.

---

# Named Entity Recognition (NER)

## 1. Real-World Applications of NER

Why do companies care about extracting entities from text?

- **Financial Search Engines:** If a news aggregator (like Bloomberg or Google News) wants to categorize an article under "Tesla News," it cannot just blindly search the text for the word "Tesla." The article might be a historical piece about the scientist _Nikola Tesla_. NER tells the system if the word "Tesla" in the text refers to a `PERSON` or an `ORG` (Organization), enabling highly accurate search indexing.
    
- **Recommendation Systems:** By extracting entities (like Actors, Directors, or Production Houses) from the descriptions of movies you watch on Disney+, the system can recommend new movies featuring those exact same entities.
    
- **Customer Support Routing:** If a user submits a blank text complaint like _"The Power BI dashboard is crashing,"_ an NER system can extract `Power BI` as a `PRODUCT` entity and automatically route the ticket to the specific engineering team responsible for that product.
    

## 2. Using Built-in NER in spaCy

When you load a pre-trained model (like `en_core_web_sm`), the NER component is already active in the pipeline. It scans your `Doc` object and populates the `doc.ents` attribute with everything it finds.



```Python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("Michael Bloomberg founded Bloomberg L.P. in 1982.")

for ent in doc.ents:
    print(f"{ent.text} -> {ent.label_}")
```

You can also use `spacy.explain(ent.label_)` to get a human-readable definition of the tag (e.g., `GPE` means Geopolitical Entity, `ORG` means Organization).

## 3. The Limitations of Pre-Trained Models

Pre-trained models are not magic; they are statistical engines that guess based on patterns they saw during training. They frequently make mistakes.

For example, if you feed the model _"Tesla is going to acquire Twitter for $45 billion"_:

- It correctly flags `$45 billion` as `MONEY`.
    
- However, the base model often misses "Twitter" or "Tesla" as an `ORG`.
    
- _Why?_ Because it relies heavily on capitalization and suffixes. If you change the text to `"Twitter Inc."`, the model suddenly recognizes it because it has been trained on the rule that Capitalized Word + `Inc.` = Organization.
    

## 4. Customizing Entities Manually (The `Span` Object)

If you know the model missed an entity, you can manually force it to recognize one. You do this by creating a **Span** (a slice of tokens) and defining its label.



```Python
from spacy.tokens import Span

doc = nlp("Tesla is going to acquire Twitter.")

# Create a Span for "Tesla" (tokens index 0 to 1) and label it ORG
s1 = Span(doc, 0, 1, label="ORG")

# Create a Span for "Twitter" (tokens index 5 to 6) and label it ORG
s2 = Span(doc, 5, 6, label="ORG")

# Set these custom entities back into the document
doc.set_ents([s1, s2], default="unmodified")
```

## 5. Three Approaches to Building Custom NER Systems

If you are working in a highly specialized field (like medicine) where the default model fails to recognize "Ibuprofen" as a `DRUG`, you have three options to build your own system:

1. **Simple Lookup (Database):** You maintain a massive database of every known drug. When you scan text, you literally do a string match against your database. It is fast and requires no machine learning, but it cannot handle misspellings or new, unknown drugs.
    
2. **Rule-Based NER:** You use spaCy's `EntityRuler` to define logical patterns. For example, you can write a Regex rule that says: _If you see 3 digits, a hyphen, 3 digits, a hyphen, and 4 digits... tag it as a `PHONE_NUMBER` entity._
    
3. **Machine Learning (CRF or BERT):** You gather thousands of documents, manually highlight the drugs yourself (creating training data), and train a sophisticated model (like a Conditional Random Field or a Transformer like BERT) to recognize the _context_ of drugs, allowing it to accurately guess new drugs it has never seen before.
    

---
