---
{"dg-publish":true,"permalink":"/week-1/1-intro/three-categories-for-techniques-for-nlp/","dg-note-properties":{}}
---


Today, we are exploring the core toolbox of an NLP practitioner. We will be discussing the three foundational methodologies used to solve virtually any Natural Language Processing problem.

If you are building an NLP pipeline from scratch, you will inevitably rely on one—or a combination—of these three approaches. While it is highly recommended to have a baseline understanding of Python, classical machine learning, and deep learning, we are going to break these down conceptually so you can see exactly how they fit together.

---

## 1. Rules and Heuristics (The Deterministic Approach)

Before we jump into complex AI algorithms, we must appreciate the power of simple, rule-based systems. This approach involves zero machine learning. Instead, it relies on predefined logic and pattern matching, most commonly using **Regular Expressions (Regex)**.

- **Information Extraction (Emails):** When you book a flight, your email provider often extracts the flight number, destination, and time to create a neat summary widget at the top of your screen. How? It searches for specific, predictable string patterns (like `"booking ref:"`) and captures the text that immediately follows it.
    <br>
- **Knowledge Graphs (Search Engines):** When you search for a public figure like Elon Musk, search engines use heuristics to parse text from sources like Wikipedia, cleanly extracting structured attributes like age and birthdate based on consistent formatting rules.
    <br>
- **The Verdict:** Rules and heuristics are incredibly fast, highly accurate, and completely transparent. Never underestimate the power of a well-written Regex rule for structured tasks!
    

## 2. Statistical Machine Learning (The Probabilistic Approach)

Rules fall apart quickly when human language becomes unpredictable or messy. For more complex, dynamic tasks like text classification (e.g., Spam Detection), we move to statistical machine learning.

- **The Translation Problem:** Machine learning models are essentially massive mathematical functions; they cannot read English text.
    <br>
- **Text Vectorization:** To bridge this gap, we use techniques like the **Count Vectorizer**. This step converts the raw text into a numerical format (a vector) by counting the frequency of specific words in a document.
    <br>
- **The NLP Pipeline:** This creates a standard pipeline:
    <br>
    1. Take your Raw Text.
        <br>
    2. Preprocess it (e.g., lemmatization, removing punctuation).
        <br>
    3. Convert it to a Number Vector (using Count Vectorization or TF-IDF).
        <br>
    4. Feed it into a statistical algorithm, like a **Naive Bayes Classifier**, to predict an outcome.
        <br>
more on it later 
    
- **The Verdict:** This approach works exceptionally well for classifying text based on the presence of specific keywords (e.g., flagging an email as spam because it contains "55 million dollars," "urgent," and "free").
    

## 3. Deep Learning & Transformers (The Contextual Approach)

The main limitation of basic statistical ML is its inability to handle _unseen words_ or understand deep semantic context. If your spam model was trained heavily on the word "cash," it might fail if a new spam email uses the word "money" instead, because basic ML treats them as completely unrelated data points. This is where Deep Learning takes over.

- **Word and Sentence Embeddings:** Instead of just counting words, deep learning models (like Google's **BERT**) generate _embeddings_. These represent words or entire sentences as dense, multi-dimensional mathematical vectors based on their deep context.
    <br>
- **Cosine Similarity:** Because embeddings map language into a spatial graph, words with similar meanings (like "cash" and "money") end up placed very close to each other mathematically. We use a metric called **Cosine Similarity** to calculate the exact distance between these concepts.
    <br>
- **Hugging Face Transformers:** In the modern era, you can easily use pre-trained deep learning models from hubs like Hugging Face. As demonstrated in our text comparison, an embedding model instantly recognizes that the phrase _"hurry up for an offer to win cash"_ has a **70% cosine similarity** to _"rush for this great deal to win money,"_ while having nearly 0% similarity to an unrelated phrase like _"I love paratha."_
    <br>
- **The Verdict:** Deep learning handles nuance, synonyms, and complex sentence structures better than any prior technology, though it requires significantly more computational power.