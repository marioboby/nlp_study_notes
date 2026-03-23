---
{"dg-publish":true,"permalink":"/week-1/2-regex/regex-for-nlp/","dg-note-properties":{}}
---

## 1. Introduction to Regex in NLP

Regular expressions (Regex) are all about matching patterns in text to retrieve key information. If you are starting a career in Natural Language Processing (NLP), learning Regex is essential.

Many real-world NLP problems don't require fancy machine learning models; they can be solved completely—or partially in a hybrid approach—using regular expression pattern matching.

## 2. Prerequisites & Toolkit

Before diving into Regex, you need a basic development environment set up:

- **Python:** The core programming language used for these scripts.
    
- **Jupyter Notebook:** An interactive coding environment to write and test Python code.
    
- **Git Bash:** Recommended for Windows users to run Unix commands easily.
    
- **The `re` Module:** Python's built-in module for regular expressions.
    
- **[regex101: build, test, and debug regex](https://regex101.com/):** An interactive, visual web tool used to build and test Regex patterns before putting them into Python code.
    

---

## 3. Use Case 1: Customer Service Chatbot

When interacting with a chatbot, users type information in unpredictable formats. Regex helps standardize and extract this data.

### Extracting Phone Numbers

Users might enter a phone number as a continuous 10-digit string (e.g., 1234567890) or formatted with brackets and hyphens (e.g., (123)-456-7890).

- **Pattern for 10 digits:** `\d{10}` (`\d` matches any digit 0-9, and `{10}` specifies exactly 10 matches).
    
- **Pattern for formatted numbers:** `\(\d{3}\)-\d{3}-\d{4}` (The `\` acts as an escape character so the brackets are treated as literal text, not special Regex commands).
    
- **Combined Pattern:** `\(\d{3}\)-\d{3}-\d{4}|\d{10}` (The pipe `|` symbol acts as an "OR" operator to match either format).
    

### Extracting Email IDs

Email addresses consist of alphanumeric characters, an `@` symbol, a domain name, and a domain extension.

- **Pattern:** `[a-zA-Z0-9_]*@[a-zA-Z]*\.[a-zA-Z]*`
    
- `[a-zA-Z0-9_]` matches any lowercase letter, uppercase letter, number, or underscore.
    
- `*` matches zero or more of the preceding character.
    
- `\.` uses the escape character to literally match the dot before the domain extension (like .com).
    

### Extracting Order Numbers

A user might say "issue with my order #4567" or "order number 4567".

- **Pattern:** `order[^\d]*(\d*)`
    
- `[^\d]*` matches a sequence of any characters that are _not_ digits (skipping words like "number" or symbols like "#").
    
- `(\d*)` matches the sequence of digits. The parentheses `()` create a **Capture Group**, allowing you to extract _only_ the specific order number from the larger matched phrase.
    

---

## 4. Use Case 2: Information Extraction (IE)

Information extraction involves pulling structured data (like a person's age, name, and birthdate) from unstructured text, such as a Wikipedia article.

### Extracting Age

In a biographical text, age is often listed as "age 50".

- **Pattern:** `age (\d+)`
    
- `+` means one or more of the preceding character (ensuring at least one digit is matched).
    
- The parentheses `()` capture just the number, ignoring the word "age".
    

### Extracting Name, Birthdate, and Birthplace

By analyzing the HTML or text structure of a Wikipedia page, you can use contextual clues like line breaks to extract data.

- **Line Ends:** `\n` represents the end of a line.
    
- **Wildcards:** `.*` matches any character (`.`) zero or more times (`*`), effectively grabbing an entire sequence of text.
    
- **Strategy:** You can anchor your search using a known word (like "born"), capture everything up to the line break (`\n`), and then use the `strip()` function in Python to clean up any leftover white spaces.
    

---

## 5. Python Implementation Structure

To implement this in code, you repeatedly use Python's `re.findall()` method. To keep your code clean, you can wrap this logic in reusable functions.

- **Step 1:** Create a helper function `get_pattern_match(pattern, text)` that runs `re.findall()` and returns the first match if it exists.
    
- **Step 2:** Create a main function `get_personal_information(text)` that defines all your specific regex patterns (for age, name, etc.).
    
- **Step 3:** Pass the text into the main function to return a clean, structured Python Dictionary containing all the extracted data points.

refer to the cheat sheet for regex unique tokens [[Week-1/2 - Regex/Regex Unique Tokens\|here]]