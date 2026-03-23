---
{"dg-publish":true,"permalink":"/week-1/1-intro/nlp-tasks/","dg-note-properties":{}}
---


We are going to walk through **10 distinct NLP tasks**. I will be throwing some technical jargon your way—like TF-IDF or Doc2Vec—but don't worry about the deep mathematics today. For now, just understand that these are magical computational tools used to convert raw human text into numbers so our machine learning models can understand them.

---

### 1. Text Classification

This is the classic NLP task: assigning a predefined category to a raw block of text.

- **Customer Support Triage:** Companies (like TechSmith for their Camtasia software) receive thousands of bug reports. Instead of paying humans to read every ticket, NLP algorithms vectorize the text (using tools like TF-IDF) and use classifiers (like Naive Bayes) to automatically tag complaints as High, Medium, or Low severity. An issue like "won't import mp4" gets flagged as High and routed instantly to a human agent.
    <br>
- **Healthcare Document Sorting:** Hospitals scan massive piles of paperwork. By combining Optical Character Recognition (OCR) with NLP (like Doc2Vec and Logistic Regression), systems can automatically classify a scanned image as either a "Prescription" or a "Patient Medical Record."
    <br>
- **Hate Speech & Fake Profile Detection:** Platforms like Facebook and LinkedIn use advanced classification to monitor their networks. For context on the scale of this task, Facebook's algorithms reported **more than 8 million hate speeches in Q1 of 2020** across various racist, terrorist, and abusive groups, allowing them to automatically delete the violating content.
    

### 2. Text Similarity

This task measures how close two pieces of text are in underlying meaning, regardless of whether they use the exact same words.

- **Resume Screening:** A recruiter might receive 1,000 resumes for a Data Scientist role. By converting both the job description and the resumes into number vectors using a "Sentence Encoder," we can calculate the **Cosine Similarity** between them. If a resume scores a 47% match, it might be discarded, but a >50% match automatically triggers a phone interview.
    

### 3. Information Extraction

This involves pulling specific, structured data points out of unstructured text.

- **Email Parsing:** When Gmail reads your airline itinerary, it automatically extracts the takeoff time, destination, and confirmation number to build a clean widget at the top of your screen.
    <br>
- **Trending Topics:** Google News scans thousands of articles and extracts key entities (like person names, locations, or events) to automatically group stories into trending topics.
    

### 4. Information Retrieval

While it sounds like Extraction, Retrieval is fundamentally different. It is about searching through a massive database of documents and returning the most relevant whole documents based on a query.

- **Search Engines:** When you type "places near me," Google's retrieval engine ranks and returns the most relevant indexed webpages based on your location and query intent.
    

### 5. Chatbots

Conversational agents generally fall into three complexity tiers:

- **FAQ Bots:** The most basic tier. It matches your question to a hardcoded answer. There is no memory of past messages.
    <br>
- **Flow-Based Bots:** These maintain context. If you say "I want a pizza," and it asks "What size?", it remembers you are talking about pizza when you reply "Medium."
    <br>
- **Open-Ended Bots:** General, free-flowing conversational AI (like Siri or myself) that can handle chit-chat and context simultaneously.
    

### 6. Machine Translation

Translating text from one language to another while preserving semantic meaning.

- **Google Translate:** This relies heavily on Deep Learning, specifically the Encoder-Decoder architecture of Recurrent Neural Networks (RNNs), to understand the full context of a sentence before translating it.
    

### 7. Language Modeling

This task revolves around predicting the probability of the next word (or sequence of words) in a sentence.

- **Autocomplete:** When you are typing an email in Gmail and it suggests the rest of your sentence in gray text, you are interacting with a statistical or neural language model predicting your next likely keystrokes.
    

### 8. Text Summarization

Condensing a massive document into a few highly accurate sentences.

- **Executive Summaries & Titles:** Instead of reading a 50-page financial report, NLP can generate a 3-line abstract or an accurate headline, allowing professionals to digest vast amounts of information instantly.
    

### 9. Topic Modeling

An unsupervised learning task where the algorithm scans a massive volume of unlabeled documents and groups them by abstract themes.

- **Industry Clustering:** If you feed the algorithm thousands of finance documents, it will automatically group documents containing words like "revenue," "quarter," and "YOY growth." If fed pharmaceutical documents, it will group "clinical trial," "drug," and "placebo."
    

### 10. Voice Assistants

A multi-step pipeline that relies on Speech-to-Text, intent recognition (NLP), and Text-to-Speech to allow humans to control hardware or retrieve information purely via voice.

### Key Insights and Terminology

|Term/Concept|Description|
|---|---|
|**TF-IDF**|Converts text into numerical vectors by weighting term frequency inversely by document frequency.|
|**Naive Bayes Classifier**|A probabilistic classifier used for categorizing text based on features.|
|**OCR (Tesseract)**|Optical Character Recognition tool converting images of text into machine-readable text.|
|**Doc2Vec**|Technique to convert entire documents into fixed-length vectors.|
|**Sentence Transformer**|Model that encodes sentences into vectors for similarity comparisons.|
|**Cosine Similarity**|Metric to measure similarity between two vectors, often used in NLP for text similarity.|
|**Encoder-Decoder RNN**|Neural architecture used in sequence-to-sequence tasks like machine translation.|
|**Language Modeling**|Predicting the probability distribution of the next word(s) in a sequence.|
|**Topic Modeling**|Unsupervised technique to find abstract topics in large text corpora.|