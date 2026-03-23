---
{"dg-publish":true,"permalink":"/week-1/1-intro/expanded-topics/cosine-similarities/","dg-note-properties":{}}
---



Let's break open the black box and look at the actual math that powers that "47% match."

If you are working with machine learning libraries in Python (like `scikit-learn` or `numpy`), this exact calculation is running under the hood whenever you compare text.

To understand Cosine Similarity, we have to look at language through the lens of geometry.

### Step 1: Turning Text into Vectors (Coordinates)

Machine learning models cannot read English, so we first map our text into a mathematical space. Imagine a highly simplified universe where the dictionary only has two words: **"Python"** and **"Data"**.

If a Job Description says "Python Data Data", we can map it as coordinates based on word counts:

- **Vector A (Job):** `[1, 2]` _(1 Python, 2 Data)_
    

If your Resume says "Python Python Data", we map it as:

- **Vector B (Resume):** `[2, 1]` _(2 Python, 1 Data)_
    

### Step 2: Why Angles Matter More Than Distance

If a job description is one paragraph long, but your resume is five pages long, the geometric _distance_ between those two points in space will be massive. If we just measured the straight-line distance (Euclidean distance), the system would say your resume is a terrible match simply because it is longer.

**Cosine Similarity fixes this.** Instead of measuring the distance between the points, it measures the **angle (θ)** between the two vectors pointing to those coordinates.

- If two vectors point in the exact same direction, they share the same meaning, regardless of how long they are.
    

### Step 3: The Formula

To calculate the cosine of the angle between two vectors, we use this formula:

$$ Cosine Similarity=cos(\theta)=\frac{∥A∥ ∥B∥}{A⋅B} $$​

Here is what those pieces mean:

1. **The Numerator (A⋅B):** This is the **Dot Product**. You multiply the corresponding dimensions together and sum them up. It measures how much the two vectors push in the same direction.
    
2. **The Denominator (∥A∥∥B∥):** These are the **Magnitudes** (the lengths) of the vectors. Dividing by this normalizes the result, stripping away the impact of the document's length.
    

### Step 4: Decoding the Score

Because word counts (or term frequencies) are never negative, our Cosine Similarity score will always fall between 0 and 1:

- **Score = 1.0 (100%):** The angle is 0∘. The vectors point in the exact same direction. Perfect semantic match.
    
- **Score = 0.0 (0%):** The angle is 90∘ (orthogonal). The vectors share absolutely no common vocabulary or meaning.
    
- **Score = 0.47 (47%):** The angle between the job description and the resume is roughly 62∘. They share some relevant trajectory in that multi-dimensional space, but drift apart on specific keywords.