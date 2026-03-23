---
{"dg-publish":true,"permalink":"/week-1/1-intro/nlp-pipeline/","dg-note-properties":{}}
---

Up to this point, we have discussed individual NLP tasks and algorithms. Today, we are zooming out to look at the big picture: **The NLP Pipeline**.

Building an NLP application in the real world is not just about writing a machine learning algorithm; it is an end-to-end engineering process. To illustrate this, we will follow a single, real-world use case throughout this lecture: **Automating a Customer Support Ticketing System** for a software company (like TechSmith's Camtasia).

When users submit bug reports, the company needs to instantly classify them as _High_, _Medium_, or _Low_ priority. Here are the 6 sequential steps to build the pipeline that makes this happen.

---

### Step 1: Data Acquisition

Before you can train an AI, you need data. In an established company, this historical data (past tickets that humans have already tagged as High/Medium/Low) usually lives in a production database like MongoDB.

- **Data Transfer:** To avoid crashing the live database, the NLP team will export this data to a secure cloud storage location, like an Amazon S3 bucket or a Data Warehouse.
    <br>
- **The "Cold Start" Problem:** What if you are a startup and don't have historical data? You can acquire data through Web Scraping, using Public Datasets (like Google Dataset Search), or using Data Augmentation (programmatically generating 100 sample tickets out of 10 real ones).
    

### Step 2: Text Extraction & Cleanup

Raw data is incredibly messy. Your next job is to strip away the noise.

- **Discarding Irrelevant Information:** A machine learning model predicting ticket severity doesn't care about the `creator_name` or the `timestamp`. You drop those columns completely. You might also merge the `ticket_title` and `ticket_description` into one single block of text.
    <br>
- **Cleanup:** Users make typos. You run spelling correction algorithms (e.g., changing "cras" to "crash") and strip out invisible formatting characters like extra line breaks (`\n`).
    
![Pasted image 20260324011642.png](/img/user/Imgs/Pasted%20image%2020260324011642.png)
### Step 3: Pre-Processing

Now you must break the cleaned text block down into linguistic components.

- **Sentence Segmentation:** Splitting the paragraph into individual sentences. This is harder than just looking for periods; an algorithm must know that "Dr. Strange" does not signify the end of a sentence.
    <br>
- **Word Tokenization:** Splitting those sentences into individual words (tokens).
    <br>
- **Stemming & Lemmatization:** Reducing variations of a word to their base root. Stemming uses crude rules (chopping "eating" to "eat"), while Lemmatization uses grammatical rules to accurately map past-tense words (mapping "ate" to "eat").
    
|Term|Definition|Example(s)|
|---|---|---|
|Stemming|Rule-based truncation of word suffixes to get root form|eating → eat|
|Lemmatization|Grammar-aware conversion of word to base form|ate → eat|
|Tokenization|Splitting text into sentences or words|"Dr. Strange..." → ["Dr. Strange", "likes", "samosas"]|
|Preprocessing|Combined process of tokenization, stemming, lemmatization, and text cleaning|-|

### Step 4: Feature Engineering

Machine learning models are mathematical engines; they cannot read English text. You must translate your base words into numbers.

- **Vectorization:** We use techniques like TF-IDF (Term Frequency-Inverse Document Frequency) or Word Embeddings to convert words into numerical vectors.
    <br>
- **Contextual Features:** The goal is to represent the words in a way where mathematically similar numbers represent semantically similar concepts.
    

### Step 5: Model Building & Evaluation

You feed your numerical data into a classification algorithm (like Naive Bayes, Support Vector Machines, or Random Forest). Because model building is trial and error, you will test multiple algorithms and tune their "hyperparameters" to find the best fit.

To evaluate if your model is actually good, you use a **Confusion Matrix**. This compares your model's _Predictions_ against the _Ground Truth_.

- If your model predicts a ticket is "High Priority" and it actually _was_ "High Priority," that is a correct prediction. These sit perfectly on the diagonal line of the matrix.
    <br>
- Anything outside that diagonal line is where your model got "confused" (e.g., predicting a Low priority ticket as High).

| Confusion Matrix    | Description                                              |
| ------------------- | -------------------------------------------------------- |
| True Positive (TP)  | Correctly predicted positive class (e.g., high priority) |
| True Negative (TN)  | Correctly predicted negative class                       |
| False Positive (FP) | Incorrectly predicted positive (false alarm)             |
| False Negative (FN) | Incorrectly predicted negative (missed cases)            |

From this matrix, we calculate core performance metrics like **Precision**, **Recall**, and the **F1-Score**.

Since these metrics can be a bit unintuitive to grasp purely through text, I have generated an interactive calculator below. You can adjust the raw outcomes of a model's predictions to see exactly how it impacts the final evaluation scores.

### Step 6: Deployment & Monitoring

Once you are satisfied with the model's F1-Score, the job shifts from Data Science to Software Engineering.

- **Deployment:** You export the model and wrap it in a REST API (using frameworks like FastAPI or Flask). You host this on a cloud platform (AWS, Azure, or Databricks) so the live Camtasia website can send it data and receive predictions in real-time.
    <br>
- **Monitoring (Concept Drift):** Language evolves. If Camtasia releases a new feature, users will start using entirely new vocabulary in their tickets. Your model's accuracy will degrade over time (known as Concept Drift). You must monitor its performance and constantly loop back to Step 1 to retrain the model on fresh data.
    

---

### **Summary of the NLP Pipeline Steps Covered**

| Step Number | Step Name                 | Description                                                                                    |
| ----------- | ------------------------- | ---------------------------------------------------------------------------------------------- |
| 1           | Data Acquisition          | Obtain labeled/unlabeled data from databases, cloud storage, public datasets, or via scraping. |
| 2           | Text Extraction & Cleanup | Remove irrelevant fields, merge text fields, correct spelling, and remove noise from text.     |
| 3           | Preprocessing             | Sentence segmentation, word tokenization, stemming, and lemmatization to normalize text.       |
| 4           | Feature Engineering       | Convert cleaned, normalized text into numerical feature vectors suitable for ML models.        |
| 5           | Model Building            | Train machine learning classifiers (Naive Bayes, SVM, etc.) to predict ticket priority.        |
| 6           | Model Evaluation          | Use confusion matrix and metrics (accuracy, precision, recall, F1) to assess model quality.    |
| 7           | Model Deployment          | Export and deploy model on cloud with REST API service wrappers (FastAPI/Flask).               |
| 8           | Monitoring & Updating     | Continuously monitor model performance and retrain as needed to handle                         |

Building an NLP system is a continuous, iterative loop of data refinement.