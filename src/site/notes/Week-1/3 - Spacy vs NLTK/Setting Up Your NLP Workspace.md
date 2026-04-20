---
{"dg-publish":true,"permalink":"/week-1/3-spacy-vs-nltk/setting-up-your-nlp-workspace/","dg-note-properties":{}}
---



## 1. The Foundation: Python & Shell

Before installing libraries, you must have a clean Python installation.

- **Python:** Ensure you are using a current version (3.8+).
    
- **The Shell:** While the Windows command prompt works, using a Unix-style terminal like **Git Bash** is highly recommended. It allows you to use standard `ls`, `cd`, and `grep` commands, which are the bread and butter of data processing and freelance software development.
    

## 2. Library Installation (The "Big Two")

### NLTK (Natural Language Toolkit)

NLTK is a modular library. When you install it via `pip`, you are only getting the core engine. You must manually download the "corpora" (the datasets and models) afterward.

1. **Core Library:**
<br>

    ```bash
    pip install nltk
    ```
<br>
2. **Downloading Data:**
    
    Inside a Python shell or Jupyter Notebook, run:
    
    ```python
    import nltk
    nltk.download('punkt') # Essential for tokenization
    ```
    
    _Note: Running `nltk.download()` without arguments opens a graphical downloader window where you can select specific packages like `WordNet` or `stopwords`._
    

### spaCy (The Industrial-Strength Library)

spaCy follows a two-step installation: the library itself and the specific language models it uses to understand context.

1. **Core Library:**
 
    ```bash
    pip install spacy
    ```
    
2. **Language Models:**
    
    You must download a specific model for the language you are processing. For English, the small model (`sm`) is usually sufficient for development:
    
    
    ```bash
    python -m spacy download en_core_web_sm
    ```
    

## 3. The Interactive Environment: Jupyter Notebooks

For documenting your research and summarizing complex algorithms (like Transformer architectures or Gaussian Mixture Models), Jupyter is the standard. It allows you to interleave live code with Markdown explanations, which is perfect for your Obsidian Digital Garden.

- **Installation:**
    
    
    ```
    pip install notebook
    ```
    

- **Launching:**
    
    Navigate to your project directory in your terminal and type:
    
    
    ```
    jupyter notebook
    ```
    

## 4. Professional Best Practice: Virtual Environments

As you scale your freelance projects, you will find that different clients require different library versions. To prevent "dependency hell," always use a virtual environment:

1. **Create:** `python -m venv nlp_env`
    
2. **Activate (Windows):** `source nlp_env/Scripts/activate`
    
3. **Activate (Unix/Mac):** `source nlp_env/bin/activate`
    

---

```bash
# --- Create & Activate Virtual Environment ---
python -m venv venv
venv\Scripts\activate

# --- Install Libraries ---
pip install --upgrade pip
pip install nltk spacy notebook

# --- Model & Resource Downloads ---
python -m spacy download en_core_web_sm
python -c "import nltk; nltk.download('punkt')"
```

---

