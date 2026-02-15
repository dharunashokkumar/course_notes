# Lecture 06 — Equivalence Class Partitioning and Boundary Value Testing

**Course:** NPTEL Software Testing | **Instructor:** Prof. Rajib Mall, IIT Kharagpur

---

## Overview of This Lecture

This lecture is entirely about **Black Box Testing strategies**. Specifically, it covers two very popular strategies:

1. **Equivalence Class Partitioning (ECP)**
2. **Boundary Value Testing** (introduced at the end)

The lecture teaches you how to identify equivalence classes, how to handle valid and invalid inputs, and what happens when a function takes multiple parameters. It also walks through several solved examples.

---

## Concept 1: What is Equivalence Class Partitioning?

### 🧠 Simple Explanation

Equivalence Class Partitioning is a **black box testing technique**. The idea is very simple:

- You look at the **input domain** of a function.
- You **divide (partition)** the input into groups called **equivalence classes**.
- Each group contains inputs that the software is expected to **treat in the same way**.
- You then pick **just one value** from each group to test.

**Why does this work?** Because if the software handles one value from a group correctly, it should handle all values in that group correctly. This saves a lot of testing effort.

### 🎯 Exam Important Points

- The **main problem** in equivalence partitioning is **designing the equivalence classes**. Once you have the classes, choosing a test value from each class is straightforward.
- Every unit (function) will have **at least two types** of equivalence classes: **valid** and **invalid**.
- Valid equivalence classes = inputs the function is designed to handle.
- Invalid equivalence classes = inputs the function is NOT designed to handle (but might receive by mistake).

---

## Concept 2: Valid and Invalid Equivalence Classes

### 🧠 Simple Explanation

For any function you are testing:

- **Valid equivalence classes** → These are the inputs that the function is supposed to accept and process correctly.
- **Invalid equivalence classes** → These are the inputs that should NOT be given to the function, but we test them to see if the software handles errors properly.

You must identify **both** valid and invalid classes. For each class (valid or invalid), you **select one representative value** and that forms your test case.

### 🛠 Example from Transcript

A function takes a value between **1 and 5000** and checks whether it is even or odd.

- **Valid equivalence classes:**
  - Odd numbers between 1 and 5000 (e.g., pick 3)
  - Even numbers between 1 and 5000 (e.g., pick 100)
- **Invalid equivalence classes:**
  - Values less than 1 (e.g., pick -5)
  - Values greater than 5000 (e.g., pick 6000)

So we get **2 valid classes** and **2 invalid classes** = 4 test cases total.

### 🎯 Exam Important Points

- If the input is a **range** (like 1 to 5000), you typically get 1 valid class and 2 invalid classes (below the range and above the range).
- If the valid range has **different scenarios** (like even/odd), each scenario becomes a separate valid equivalence class.

---

## Concept 3: Enumerated Input — Equivalence Classes

### 🧠 Simple Explanation

Sometimes the input is not a range of numbers but an **enumerated set** — meaning the input can be one of a fixed set of values, like {a, b, c}.

- **Valid equivalence class:** The input is one of {a, b, c} → This is 1 valid class.
- **Invalid equivalence class:** The input is anything that is NOT a, b, or c → This is 1 invalid class.

### 🎯 Exam Important Points

- For enumerated inputs: **1 valid** equivalence class and **1 invalid** equivalence class.
- For range inputs: typically **1 valid** and **2 invalid** (below range, above range).

---

## Concept 4: Identifying Equivalence Classes from Scenarios (Output-Based)

### 🧠 Simple Explanation

Sometimes you identify equivalence classes not just from the input values but from the **scenarios of operation** or the **output** of the function.

You look at the function description and ask: "What are the different things this function can do?" Each different behavior/scenario becomes an equivalence class.

### 🛠 Example: Issue Book Function

A function called `issueBook` takes a `bookId` as input.

Looking at what the function does (its output/behavior), you find different scenarios:

- The book is a **reference book** → cannot be issued
- The book is a **single volume** book → can be issued
- The book is a **multiple volume** book → can be issued (but handled differently)

Each of these scenarios is an equivalence class: **Reference book**, **Single volume**, **Multiple volume**.

### 🎯 Exam Important Points

- Equivalence classes can be identified from **input values** or from **output scenarios**.
- Looking at what the function does (different behaviors) helps you find the classes.

---

## Concept 5: Multiple Notions of Equivalence Classes for the Same Input

### 🧠 Simple Explanation

For some inputs, there can be **more than one way** to define equivalence classes. This increases complexity.

### 🛠 Example: Fetch-Image Function

A function called `Fetch-image` takes a **URL** and returns an **image**.

You can define equivalence classes in **two different ways** for this single input parameter:

**Way 1 — Based on URL type (protocol):**
- http
- https
- ftp
- file

Each is a valid URL category → each becomes an equivalence class.

**Way 2 — Based on image type stored at the URL:**
- HTML
- GIF
- JPEG
- Plain Text

Each image format → each becomes another equivalence class.

So for a **single data item** (URL), we can construct **multiple types of equivalence classes** depending on different considerations.

### 🎯 Exam Important Points

- For some data items, you can have **multiple definitions of equivalence classes**.
- You must consider **all** of them for thorough testing.

---

## Concept 6: Equivalence Classes for Integer Ranges and Phone Numbers

### 🧠 Simple Explanation

**Example 1: Integer input between -99 and 99**

- Valid equivalence class: any value from -99 to 99 (inclusive)
- Invalid equivalence class 1: values greater than 99
- Invalid equivalence class 2: values less than -99
- Result: **1 valid** + **2 invalid** = 3 equivalence classes

**Example 2: Phone number (area code + suffix)**

- Area code: 11 to 999
- Suffix: any 6 digits (000000 to 999999)

- Valid equivalence class: area code is 11–999 AND suffix is 000000–999999
- Invalid equivalence classes can be **many types**:
  - Invalid format for the prefix
  - Invalid format for the suffix
  - Non-numeric characters for area code
  - Area code out of range
  - Suffix out of range

### 🎯 Exam Important Points

- When a function takes **structured input** (like a phone number with two parts), each part can have its own valid and invalid classes.
- You need to identify **different types** of invalid equivalence classes, not just one.

---

## Concept 7: Multiple Parameters — Weak, Strong, Robust, and Strong Robust Equivalence Class Testing

### 🧠 Simple Explanation

This is the **most important concept** in this lecture for exams.

When a function takes **two or more parameters**, we need to decide how to **combine** the equivalence classes of each parameter. There are four strategies:

---

### 7a. Weak Equivalence Class Testing

- You make test cases so that **each equivalence class of each parameter is covered at least once**.
- You do NOT need to test all combinations.
- This is the **minimum** level of testing.

**Example from transcript:**

Function takes two parameters:
- **Age**: equivalence classes are {5 to 30} and {greater than 30}
- **Years of education**: equivalence classes are {School}, {UG}, {PG}

Weak testing: Make enough test cases so that all classes of age (both) and all classes of education (all three) appear at least once. You might need just 3 test cases (since education has 3 classes, which is the larger number):

| Test Case | Age | Education |
|-----------|-----|-----------|
| 1 | 5 to 30 | School |
| 2 | > 30 | UG |
| 3 | > 30 | PG |

All classes of both parameters are covered.

---

### 7b. Strong Equivalence Class Testing

- You consider **all possible combinations** of the equivalence classes of the two parameters.
- If parameter 1 has 2 classes and parameter 2 has 3 classes → you get 2 × 3 = **6 test cases**.

**Using the same example:**

| Test Case | Age | Education |
|-----------|-----|-----------|
| 1 | 5 to 30 | School |
| 2 | 5 to 30 | UG |
| 3 | 5 to 30 | PG |
| 4 | > 30 | School |
| 5 | > 30 | UG |
| 6 | > 30 | PG |

---

### 7c. Robust Equivalence Class Testing (Weak Robust)

- Same as weak testing, but **also includes invalid values**.
- Invalid values = values outside the valid range (e.g., age less than 5, education is "none").
- You still don't need all combinations, just ensure each invalid class appears at least once.

---

### 7d. Strong Robust Equivalence Class Testing

- You consider **all possible combinations** including **invalid values**.
- So you combine valid AND invalid classes of all parameters.
- This gives the **maximum number** of test cases.

**Example:** If we add invalid classes like "age < 5" and "education = none":
- Age classes: {< 5}, {5 to 30}, {> 30}
- Education classes: {< School}, {School}, {UG}, {PG}
- Total combinations: 3 × 4 = 12 test cases

### 📊 Summary Table

| Type | Valid Only? | All Combinations? | Number of Test Cases |
|------|------------|-------------------|---------------------|
| Weak | Yes | No (just cover each class once) | max(m, n) |
| Strong | Yes | Yes | m × n |
| Weak Robust | Valid + Invalid | No | Covers each class once |
| Strong Robust | Valid + Invalid | Yes | (m + invalid_m) × (n + invalid_n) |

Where m = number of classes for parameter 1, n = number of classes for parameter 2.

### 🎯 Exam Important Points

- **Weak** = every class covered at least once, no exhaustive combinations.
- **Strong** = all combinations of valid classes.
- **Robust** = includes invalid classes too.
- **Strong Robust** = all combinations of both valid and invalid classes.
- Know the difference clearly — this is very likely to appear in exams.

### ⚠️ Common Confusions

- "Weak" does NOT mean bad testing. It just means fewer combinations.
- "Robust" refers to adding **invalid** input handling, not stronger combinations.
- "Strong" refers to testing **all combinations**, not testing with invalid inputs.

---

## Concept 8: Solved Example 1 — Bank Interest Rate (Single Parameter)

### 🧠 Problem Statement

A bank pays different rates of interest depending on the deposit period:

| Deposit Period | Interest Rate |
|---------------|---------------|
| Up to 15 days | 3% |
| Over 15 to 180 days | 4% |
| Over 180 days to 1 year | 6% |
| 1 year but less than 3 years | 7% |
| 3 years and above | 8% |

Design equivalence class test cases.

### 📊 Step-by-Step Solution

**Step 1: Identify valid equivalence classes**

Each interest rate scenario becomes one equivalence class:

| Class | Range |
|-------|-------|
| V1 | 0 to 15 days |
| V2 | 15 to 180 days |
| V3 | 180 days to 1 year |
| V4 | 1 year to less than 3 years |
| V5 | 3 years and above |

**Step 2: Identify invalid equivalence classes**

- I1: Negative deposit period (less than 0 days) — someone enters a negative number by mistake.
- There is no invalid class on the right side (3 years and above covers everything else).

**Step 3: Pick one value from each class**

| Test Case | Input (Deposit Period) | Expected Output |
|-----------|----------------------|-----------------|
| 1 | -5 days (invalid) | Error |
| 2 | 10 days | 3% |
| 3 | 100 days | 4% |
| 4 | 200 days | 6% |
| 5 | 2 years | 7% |
| 6 | 5 years | 8% |

### 🎯 Exam Important Points

- Each scenario (each different output/behavior) becomes one equivalence class.
- Always check for invalid inputs on **both sides** of the range.
- This is a **single parameter** problem, so weak and strong testing give the same result.

---

## Concept 9: Solved Example 2 — Bank Interest with Two Parameters (Principal + Period)

### 🧠 Problem Statement

A function takes **two parameters**: **Principal** and **Deposit Period**.

**If principal < 1 lakh:**
| Period | Rate |
|--------|------|
| Up to 1 year | 6% |
| 1 year to less than 3 years | 7% |
| 3 years and above | 8% |

**If principal ≥ 1 lakh:**
| Period | Rate |
|--------|------|
| Up to 1 year | 7% |
| 1 year to less than 3 years | 8% |
| 3 years and above | 9% |

### 📊 Step-by-Step Solution

**Step 1: Identify equivalence classes for each parameter**

**Principal:**
- P1: Less than 1 lakh
- P2: More than 1 lakh

**Deposit Period:**
- D1: Up to 1 year
- D2: 1 year to less than 3 years
- D3: 3 years and above

**Step 2: Weak equivalence class testing**

Cover each class at least once. Since period has 3 classes (the maximum), we need at least 3 test cases:

| Test | Principal | Period | Expected Rate |
|------|-----------|--------|---------------|
| 1 | < 1 lakh | ≤ 1 year | 6% |
| 2 | ≥ 1 lakh | 1–3 years | 8% |
| 3 | ≥ 1 lakh | ≥ 3 years | 9% |

**Step 3: Strong equivalence class testing**

All combinations: 2 × 3 = 6 test cases:

| Test | Principal | Period | Expected Rate |
|------|-----------|--------|---------------|
| 1 | < 1 lakh | ≤ 1 year | 6% |
| 2 | < 1 lakh | 1–3 years | 7% |
| 3 | < 1 lakh | ≥ 3 years | 8% |
| 4 | ≥ 1 lakh | ≤ 1 year | 7% |
| 5 | ≥ 1 lakh | 1–3 years | 8% |
| 6 | ≥ 1 lakh | ≥ 3 years | 9% |

**Step 4: Robust Strong equivalence class testing**

Add invalid inputs: negative period, non-integer value, negative principal, etc. Then consider ALL combinations of valid and invalid classes.

### 🎯 Exam Important Points

- Two-parameter problems are **very common** in NPTEL exams.
- Know how to calculate the number of test cases: weak = max(m, n), strong = m × n.
- Robust adds invalid classes; Strong Robust = all combinations including invalid.

---

## Concept 10: Solved Example 3 — Substring Function (String Parameters)

### 🧠 Problem Statement

A function called `substr` takes two string parameters:
- **s1**: maximum length 20
- **s2**: maximum length 5

The function checks whether **s2 is a substring of s1**.

### 📊 Step-by-Step Solution

**Step 1: Identify equivalence classes from scenarios (output)**

- Scenario 1: s2 **is** a substring of s1 → function displays "is a substring"
- Scenario 2: s2 is **NOT** a substring of s1 → function displays "is not a substring"

**Step 2: Identify invalid equivalence classes for s1**

- Invalid 1: s1 length is greater than 20
- Invalid 2: s1 contains an invalid character (e.g., a control character)

**Step 3: Identify invalid equivalence classes for s2**

- Invalid 1: s2 length is greater than 5
- Invalid 2: s2 contains an invalid character (e.g., not a character)

**Step 4: Combine**

For weak, strong, and robust testing, you combine the valid and invalid classes of s1 and s2 as described in Concept 7.

### 🎯 Exam Important Points

- For **string inputs**, invalid classes include: exceeding maximum length and containing invalid/control characters.
- Scenarios (what the function outputs) help identify valid equivalence classes.

---

## Concept 11: Special Value Testing (Introduction)

### 🧠 Simple Explanation

This concept is **introduced at the end** of this lecture and will continue in the next lecture.

In equivalence class testing, we pick **any one value** from each class. But experienced programmers know that programs often fail at **special values** — values where the programmer might not have handled the condition properly.

**Special Value Testing** means the tester uses their experience and intuition to pick values where they suspect the program might fail.

One important type of special value is the **boundary value** (which will be covered in detail in the next lecture).

Other special values are those where the tester has a **suspicion** or **knack** that the programmer might have made a mistake.

### 🎯 Exam Important Points

- Special value testing relies on the **tester's experience**.
- Boundary values are one kind of special value.
- The assumption in equivalence class testing is that **all inputs in one class are treated the same way** — but bugs often hide at boundaries or special points.

---

## Concept 12: The Core Assumption of Equivalence Class Testing

### 🧠 Simple Explanation

The fundamental assumption behind equivalence class testing is:

> **Any input from one equivalence class is processed in a similar way compared to any other input from the same equivalence class.**

This means if the software handles value X from class A correctly, it should handle all other values in class A correctly too. That is why we only need to test **one value per class**.

But this assumption can fail at **boundaries** and **special values**, which is why we need boundary value testing and special value testing as complementary techniques.

### 🎯 Exam Important Points

- This assumption is the **foundation** of equivalence class testing.
- If asked "What is the basic assumption of ECP?" — this is the answer.

---

## Complete Topic Summary

| # | Topic | Key Point |
|---|-------|-----------|
| 1 | Equivalence Class Partitioning | Divide inputs into groups; test one from each |
| 2 | Valid vs Invalid Classes | Every function has at least both types |
| 3 | Enumerated Input | 1 valid + 1 invalid class |
| 4 | Scenario-Based Classes | Identify from output/behavior |
| 5 | Multiple Notions | Same input can have multiple class definitions |
| 6 | Range and Structured Input | Range → 1 valid + 2 invalid; structured → multiple invalid types |
| 7 | Weak/Strong/Robust/Strong Robust | Four strategies for multi-parameter testing |
| 8 | Bank Interest (1 param) | Each rate scenario = one class |
| 9 | Bank Interest (2 params) | Weak vs Strong vs Robust demonstrated |
| 10 | Substring Function | String inputs with length and character validity |
| 11 | Special Value Testing | Tester's experience picks likely failure points |
| 12 | Core Assumption of ECP | All values in one class treated identically |

---

---

# 📝 10 MCQs — Lecture 06 (Strictly from Transcript)

---

### Q1. What is the main challenge in equivalence class partitioning?

**(A)** Writing the test code
**(B)** Designing the equivalence classes
**(C)** Running the tests
**(D)** Fixing the bugs found

**Answer: (B)**

**Explanation:** The transcript explicitly states that the main problem in equivalence partitioning is **designing the equivalence classes**. Once the classes are identified, selecting one value from each class is straightforward.

---

### Q2. A function accepts an integer input in the range 1 to 5000. How many invalid equivalence classes are there?

**(A)** 1
**(B)** 2
**(C)** 3
**(D)** 0

**Answer: (B)**

**Explanation:** For a range input (1 to 5000), there are two invalid equivalence classes: values less than 1, and values greater than 5000. This is directly from the transcript example.

---

### Q3. If the input to a function is an enumerated set {a, b, c}, how many valid and invalid equivalence classes exist?

**(A)** 3 valid, 1 invalid
**(B)** 1 valid, 1 invalid
**(C)** 3 valid, 3 invalid
**(D)** 1 valid, 3 invalid

**Answer: (B)**

**Explanation:** The transcript states that for an enumerated set input, there is **1 valid equivalence class** (input is one of a, b, c) and **1 invalid equivalence class** (input is neither a, b, nor c).

---

### Q4. In the Fetch-image(URL) example, why are there multiple types of equivalence classes for the same input?

**(A)** Because the function has multiple parameters
**(B)** Because the URL can be categorized by protocol type (http, ftp, etc.) AND by image type (JPEG, GIF, etc.)
**(C)** Because invalid inputs are counted separately
**(D)** Because each URL is unique

**Answer: (B)**

**Explanation:** The transcript explains that for the Fetch-image function, the URL can be classified by protocol (http, https, ftp, file) and also by the type of image stored (HTML, GIF, JPEG, Plain Text). This gives **multiple definitions of equivalence classes** for a single input.

---

### Q5. In Weak Equivalence Class Testing for two parameters, what is the goal?

**(A)** Test all combinations of both parameters
**(B)** Ensure every equivalence class of every parameter is covered at least once
**(C)** Test only invalid inputs
**(D)** Test only boundary values

**Answer: (B)**

**Explanation:** Weak equivalence class testing ensures that **each equivalence class** for each parameter appears in at least one test case, without requiring all combinations.

---

### Q6. A function has parameter A with 2 equivalence classes and parameter B with 3 equivalence classes. How many test cases are needed for Strong Equivalence Class Testing?

**(A)** 2
**(B)** 3
**(C)** 5
**(D)** 6

**Answer: (D)**

**Explanation:** Strong equivalence class testing requires **all combinations**. So: 2 × 3 = 6 test cases. This is clearly described in the transcript for the age and education example.

---

### Q7. What does "Robust" add to equivalence class testing?

**(A)** More valid classes
**(B)** Invalid input values are also considered
**(C)** Boundary values are tested
**(D)** All combinations are tested

**Answer: (B)**

**Explanation:** The transcript explains that "Robust" testing means we also consider the **invalid values** (like negative inputs, out-of-range values) in addition to the valid equivalence classes.

---

### Q8. For the bank interest rate example (single parameter — deposit period), how many valid equivalence classes are identified?

**(A)** 3
**(B)** 4
**(C)** 5
**(D)** 6

**Answer: (C)**

**Explanation:** The transcript identifies 5 valid equivalence classes: 0–15 days, 15–180 days, 180 days–1 year, 1 year–less than 3 years, and 3 years and above. Each interest rate scenario is a separate class.

---

### Q9. For the substring function, which of the following is an invalid equivalence class for parameter s1 (max length 20)?

**(A)** s1 is a valid string of length 10
**(B)** s1 contains a control character
**(C)** s2 is longer than 5
**(D)** s2 is a substring of s1

**Answer: (B)**

**Explanation:** The transcript states that for s1, the invalid equivalence classes are: length greater than 20, and containing an invalid character such as a **control character**. Option (C) is about s2, not s1. Option (D) is a valid scenario.

---

### Q10. What is Special Value Testing based on?

**(A)** Random selection of inputs
**(B)** The tester's experience and intuition about where the program might fail
**(C)** Automated tool output
**(D)** Only boundary values

**Answer: (B)**

**Explanation:** The transcript states that in special value testing, the **tester has a knack for knowing where the program will fail** and enters only those values. It is based on the tester's experience and suspicion about likely failure points.

---

*End of Lecture 06 — Complete Notes and MCQs*
