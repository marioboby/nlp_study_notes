---
{"dg-publish":true,"permalink":"/week-1/5-language-processing-pipeline-in-spacy/language-processing-pipeline-in-spacy/","dg-note-properties":{}}
---




Now that we understand how tokenization works, we can look at what happens to those tokens _after_ they are split apart.

Tokenization is just step one. To truly understand language, the text must pass through a **Language Processing Pipeline**.

---

# The Language Processing Pipeline

## 1. Blank vs. Pre-Trained Pipelines

In spaCy, there is a massive difference between an empty pipeline and a loaded one.

- **The Blank Pipeline (`spacy.blank("en")`):** 
	- This gives you an English tokenizer and absolutely nothing else.
    
	- If you pass the sentence _"Tesla is buying Twitter,"_ the blank pipeline can split the words apart, but it has no idea what a "Tesla" is. It is fast, but "dumb."
        
- **The Pre-Trained Pipeline (`spacy.load("en_core_web_sm")`):** 

	- This loads a fully trained machine learning model.
	- It comes packed with a sequence of intelligent components. As your text passes through this pipeline, each component adds a layer of linguistic metadata to the `Doc` object.
        

## 2. Core Pipeline Components

When you use a pre-trained pipeline, your text sequentially passes through several specialized models. Here are the three most important ones:

### A. The Tagger (Part of Speech - POS)

This component looks at the context of the sentence and assigns grammatical labels to every token.

- **Example:** In _"Dhaval eats an apple"_, the tagger identifies "Dhaval" as a _Proper Noun_, "eats" as a _Verb_, and "apple" as a _Noun_.
    
- _Note:_ It is context-aware. It knows that "apple" in _"Apple released a new phone"_ is a Proper Noun (a company), whereas in _"I ate an apple,"_ it is a common noun (a fruit).
    

### B. The Lemmatizer

As discussed in our pre-processing lecture, this component maps variations of a word back to their base dictionary form (the lemma). زي الكشف في المعجم زي ما قلنا

- **Example:** It maps the token "ate" $\rightarrow$ "eat", and "said" $\rightarrow$ "say".
    

### C. NER (Named Entity Recognition)

This is often the most commercially valuable component. The NER model scans the text for real-world objects and assigns them categories.

- **Example:** If the text says _"Tesla Inc is acquiring Twitter for $45 billion"_:
    
    - `Tesla Inc` $\rightarrow$ **ORG** (Organization)
        
    - `Twitter` $\rightarrow$ **ORG** 

    * `$45 billion` $\rightarrow$ **MONEY**
        
- **Visualization:** spaCy includes a built-in module called `displacy` (`from spacy import displacy`). You can use `displacy.render()` to generate beautiful, color-coded visual highlights of these entities directly in your Jupyter Notebook.
    

## 3. Customizing Your Pipeline

One of spaCy's greatest strengths is its modularity. You don't always want to run the full pipeline because running all those models requires more memory and compute time.

If you are building an application that _only_ extracts company names from text, you don't care about lemmatization or part-of-speech tagging. You can build a highly optimized custom pipeline by starting with a blank model and simply importing the exact component you need from a trained model:



```Python
import spacy

# 1. Load the heavy, pre-trained English model
nlp_english = spacy.load("en_core_web_sm")

# 2. Create a fast, blank pipeline
nlp = spacy.blank("en")

# 3. Add ONLY the NER component from the trained model into your blank one
nlp.add_pipe("ner", source=nlp_english)

# Now 'nlp' is perfectly optimized to only extract entities!
```

---

### Interactive: The Pipeline Visualizer

Understanding how each component layers data onto a sentence can be tricky. Use the visualizer below to toggle different pipeline components on and off. Notice how the underlying metadata changes depending on which mathematical model is currently active in the pipeline.

