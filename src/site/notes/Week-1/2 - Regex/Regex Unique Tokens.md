---
{"dg-publish":true,"permalink":"/week-1/2-regex/regex-unique-tokens/","dg-note-properties":{}}
---

### 1. Character Classes (The "What")

These tokens define _what_ kind of character you are looking for.

| **Token** | **Meaning**                                        | **Target Text** | **Regex Example** | **What it Matches**                   |
| --------- | -------------------------------------------------- | --------------- | ----------------- | ------------------------------------- |
| `.`       | Any single character (except a newline)            | `cat, cot, cut` | `c.t`             | `cat`, `cot`, `cut`                   |
| `\d`      | Any digit (0-9)                                    | `Agent 007`     | `\d\d\d`          | `007`                                 |
| `\D`      | Any NON-digit                                      | `Order 123`     | `\D+`             | `Order` (includes the space)          |
| `\w`      | Any word character (letters, numbers, underscores) | `User_1!`       | `\w+`             | `User_1` (stops at `!`)               |
| `\W`      | Any NON-word character (punctuation, spaces)       | `User_1!`       | `\W`              | `!`                                   |
| `\s`      | Any whitespace (space, tab, newline)               | `Hello World`   | `Hello\sWorld`    | `Hello World`                         |
| `\S`      | Any NON-whitespace                                 | `A B C`         | `\S`              | `A`, `B`, `C` (as individual matches) |

---

### 2. Quantifiers (The "How Many")

These tokens tell Regex _how many times_ the preceding character or group should appear.

|**Token**|**Meaning**|**Target Text**|**Regex Example**|**What it Matches**|
|---|---|---|---|---|
|`*`|Zero or more times|`color, colour`|`colou*r`|`color`, `colour`|
|`+`|One or more times|`goooal, goal`|`go+al`|`goooal`, `goal`|
|`?`|Zero or one time (makes it optional)|`apple, apples`|`apples?`|`apple`, `apples`|
|`{n}`|Exactly _n_ times|`12345`|`\d{3}`|`123`|
|`{n,}`|_n_ or more times|`12345`|`\d{4,}`|`12345`|
|`{n,m}`|Between _n_ and _m_ times|`123456`|`\d{2,4}`|`1234`|

---

### 3. Logic & Grouping (The "Rules")

These tokens allow you to build complex logic, extract specific data, or give multiple options.

| **Token** | **Meaning**                                                                     | **Target Text**    | **Regex Example**    | **What it Matches**             |
| --------- | ------------------------------------------------------------------------------- | ------------------ | -------------------- | ------------------------------- |
| `[]`      | Character Set: Match ANY character inside                                       | `bear, pear`       | `[bp]ear`            | `bear`, `pear`                  |
| `[^]`     | Negated Set: Match ANY character NOT inside                                     | `bear, pear, tear` | `[^bp]ear`           | `tear`                          |
| `()`      | Capture Group: Groups tokens and extracts them                                  | `ID: 99`           | `ID:\s(\d+)`         | Matches `ID: 99`, extracts `99` |
| \|        | Alternation (OR logic)                                                          | `cat or dog`       | cat\|dog             | `cat`, `dog`                    |
| `\`       | Escape Character: Treats special chars literally                                | `Cost: $5.00`      | `\$\d\.\d\d`         | `$5.00`                         |
| `(?:)`    | Non Capturing Group : Groups tokens for applying other rules without extracting | `Q1 ... S2`        | `(?:Q[1-4]\|S[1-2])` | `Q1`,`S2`                       |

---

### 4. Anchors (The "Where")

These tokens don't match characters; they match _positions_ in the text (like the very beginning or the very end).

|**Token**|**Meaning**|**Target Text**|**Regex Example**|**What it Matches**|
|---|---|---|---|---|
|`^`|Start of the string/line|`Hello World`|`^Hello`|`Hello` (only if it's the first word)|
|`$`|End of the string/line|`Hello World`|`World$`|`World` (only if it's the last word)|