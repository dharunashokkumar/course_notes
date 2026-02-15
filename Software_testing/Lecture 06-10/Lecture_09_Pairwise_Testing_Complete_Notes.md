# Lecture 9: Pair-wise Testing (All-Pairs Testing)

**Course:** NPTEL – Software Testing | **Instructor:** Prof. Rajib Mall, IIT Kharagpur

---

## Where Does This Lecture Fit?

Before this lecture, you learned several **black-box testing** strategies:

- Equivalence Class Testing
- Special Value (Boundary Value) Testing
- Decision Table-Based Testing
- Cause-Effect Graphing

All of these are black-box techniques (you don't look at the code). This lecture introduces **one more black-box technique** called **Pair-wise Testing** (also called **All-Pairs Testing**). It is a type of **combinatorial testing** that is useful when the **number of inputs is large**.

---

## Concept 1: The Problem — Too Many Combinations

### 🧠 Simple Explanation

Imagine you are testing a **font settings dialog** in a word processor. You can independently set many things: font type, font style (regular, italic, bold, bold italic), font size (many sizes), strikethrough ON/OFF, superscript ON/OFF, subscript ON/OFF, small caps ON/OFF, and so on.

Each of these is an **input parameter**, and each can take some values. If you want to test **every possible combination** of all these settings, the total number becomes **huge**.

### 🛠 Example from the Transcript

Suppose you have:

- 10 Boolean (ON/OFF) checkboxes → each takes 2 values → 2^10 = **1,024 combinations**
- 3 more parameters (font, font color, font size) each taking 10 values → 10^3 = **1,000 combinations**
- Total = 1,024 × 1,000 ≈ **1 million test cases**

Just for **one screen**! No tester can run a million test cases for one dialog box.

### 🎯 Exam Important Point

> **Exhaustive testing is impractical** when the number of input parameters is large, because the total combinations grow exponentially.

---

## Concept 2: The Combinatorial Testing Problem (Formal Statement)

### 🧠 Simple Explanation

The problem is formally stated as:

- You have **n input parameters**.
- Each parameter can take some number of valid values (some take 2, some take 3, some take 5, etc.).
- The **system is state-independent** — it behaves the same way regardless of history, only depending on the current combination of inputs.
- Depending on the combination of values, bugs may appear.

When n is large (like 20, 30, or 40), exhaustive testing of all combinations is **impossible**. We need a smart way to **reduce the number of test cases** and still catch almost all bugs.

### 🎯 Exam Important Point

> The system being tested is **assumed to be state-independent** for pair-wise testing. This means the system's behavior depends only on the current input values, not on past inputs.

---

## Concept 3: The Key Observation — Why Pair-wise Works

### 🧠 Simple Explanation

Researchers studied a large number of real-world software and found a crucial pattern:

**Most bugs are caused by the interaction of only 1 or 2 (or at most 3) parameters, NOT all parameters together.**

Think of it this way: When a programmer writes code with 40 parameters, they don't write `if` conditions that check all 40 at once. They write conditions like:

```
if (parameter1 == value_A AND parameter15 == value_B) then do_something
```

So the bug appears only when **a specific pair** of parameters has specific values — **regardless of what the other 38 parameters are set to**.

### 🛠 Example from the Transcript

Suppose you have parameters p1, p2, ..., p40, each taking 0 or 1.

A bug occurs **only when p1 = 1 AND p15 = 1**, regardless of the values of all other parameters. To find this bug, you just need one test case where p1 = 1 and p15 = 1. The values of p2, p3, ..., p14, p16, ..., p40 don't matter.

### 🎯 Exam Important Points

> - **Fault is caused by interaction among a few factors** — this is the main idea behind pair-wise testing.
> - By covering all pair-wise combinations of parameter values (every pair of parameters having every combination of their values), we can detect **80–90% of bugs**.
> - By covering 3-way combinations → about **90% or more** bugs found.
> - By covering 4-way or 5-way combinations → **almost all** bugs found.
> - **We do NOT need to cover all n-way combinations** for n parameters.

---

## Concept 4: t-way Interaction Faults

### 🧠 Simple Explanation

This is a terminology concept. A **t-way interaction fault** is a bug that is caused by some specific combination of **t** input parameters having specific values.

- **1-way fault (single-mode fault):** The bug happens when just **one specific parameter** has a certain value, no matter what other parameters are.
- **2-way fault (pair-wise fault / double-mode fault):** The bug happens when **two specific parameters** have certain values together.
- **3-way fault (multi-mode fault):** Three parameters need specific values for the bug to appear.

### 🎯 Exam Important Point

> - A **t-way fault** is caused by a specific combination of **t** input parameters.
> - Most real-world software faults are **1-way or 2-way** faults.
> - Up to **5-way** testing would catch virtually all bugs in practice.

---

## Concept 5: Single-Mode Bug (1-Way Fault) — Example

### 🧠 Simple Explanation

A single-mode bug means that **one parameter alone** triggers the bug, no matter what you do with other parameters.

### 🛠 Example from the Transcript

> "The printout always gets **smeared** when you choose the **duplex option** in the printer dialog box, regardless of the printer type and other selected options."

Here, the bug depends **only** on the duplex option being selected. It doesn't matter which printer model you pick or what other settings you choose. If duplex = ON → bug. If duplex = OFF → no bug regardless of other settings.

### 🎯 Exam Important Point

> A **single-mode (1-way) fault** is triggered by the setting of a **single parameter**, irrespective of all other parameter values.

---

## Concept 6: Double-Mode Bug (2-Way Fault) — Example

### 🧠 Simple Explanation

A double-mode bug means that **exactly two parameters** together, with specific values, cause the bug. Changing either one of them to a different value makes the bug disappear.

### 🛠 Example from the Transcript

> "The printout gets smeared **only when duplex is selected AND the printer model is 394**."

So: duplex = ON AND printer model = 394 → bug. Any other combination (duplex OFF, or model ≠ 394) → no bug.

### 🎯 Exam Important Point

> A **double-mode (2-way) fault** requires **two specific parameters** to have specific values simultaneously. This is what pair-wise testing is designed to catch.

---

## Concept 7: Multi-Mode Bug (3-Way or Higher)

### 🧠 Simple Explanation

A multi-mode fault is when **3 or more** parameters must all have specific values for the bug to appear.

### 🎯 Exam Important Point

> Multi-mode faults exist but are **rare** in practice. Most bugs are caught by 2-way (pair-wise) testing.

---

## Concept 8: Why Programs Have Mostly Pair-wise Faults — Code Perspective

### 🧠 Simple Explanation

The transcript gives a code-level explanation of why most faults are pair-wise.

When programmers write code, the `if` statements typically check only **a few parameters at a time**, not all of them. A programmer might forget to write one `if` case, or might write the wrong action for a specific condition.

### 🛠 Example from the Transcript

A program checks parameters x and y:

```
if (x == x1 AND y == y2) then output = f(x,y,z)
if (x == x2 AND y == y1) then output = g(x,y)
```

The programmer **missed** writing:

```
if (x == x2 AND y == y2) then output = f(x,y,z) - g(x,y)
```

So the problem occurs **only** when x = x2 AND y = y2 — a **2-way fault**. Other parameters (like z) don't determine whether the bug triggers; it's the pair (x, y) that matters.

### 🎯 Exam Important Point

> Programmers write `if` conditions involving only **a few parameters at a time**, which is why faults are typically caused by interactions of 2 or 3 parameters, not all parameters together.

---

## Concept 9: The Dramatic Reduction in Test Cases — Pair-wise Reductions Table

### 🧠 Simple Explanation

The power of pair-wise testing is in the **massive reduction** of test cases. The transcript gives a specific table:

| Number of Inputs | Values per Input | Exhaustive Combinations | Pair-wise Test Set Size |
|:---:|:---:|:---:|:---:|
| 7 | 2 (Boolean) | 2^7 = **128** | **8** |
| 13 | 3 | 3^13 = **1.6 × 10^6** (1.6 million) | **15** |
| 40 | 3 | 3^40 = **1.2 × 10^19** | **21** |

Look at the last row: 1.2 × 10^19 (a number no one can ever test in a lifetime) is reduced to just **21 test cases**!

### 🎯 Exam Important Points

> - Pair-wise testing **dramatically reduces** test cases while still catching most bugs.
> - The pair-wise test set size numbers (8, 15, 21) are **generated by tools**, not calculated by hand.
> - **Different tools may give different numbers** of pair-wise test cases because they use different algorithms.
> - Finding the **optimal (minimum) number** of pair-wise test cases is a **very hard (computationally difficult) problem**.
> - Tools may use **hill climbing**, **genetic algorithms**, and other **combinatorial optimization** techniques to find near-optimal solutions.

---

## Concept 10: Android Smartphone Testing Example

### 🧠 Simple Explanation

The transcript gives an example of testing an **Android smartphone's operating system** with environmental configuration parameters such as:

- Hard keyboard hidden (yes/no/undefined)
- Screen layout (large/normal/small)
- Orientation (landscape/portrait)

If you consider all the configuration variables and their values, there are **172,800 possible test cases** — way too many.

But with pair-wise testing (or 3-way, 4-way, 5-way testing), the number becomes very manageable, and you would still catch almost every bug.

### 🎯 Exam Important Point

> Pair-wise testing is particularly useful for **configuration testing** (like testing an OS across different hardware/environment settings).

---

## Concept 11: How to Manually Generate Pair-wise Test Cases — Step-by-Step Algorithm

This is the most **detailed and exam-important** part of the lecture. The transcript gives a step-by-step manual method.

---

### Step 1: Identify All Variables and Their Values

List all input parameters and the possible values each can take.

**Example:**

| Orientation | Screen | Keyboard |
|:---:|:---:|:---:|
| Portrait | Large | QWERTY |
| Landscape | Small | 12Key |
| | Normal | |

- Orientation: 2 values
- Screen: 3 values
- Keyboard: 2 values

Exhaustive testing = 2 × 3 × 2 = **12 test cases**. Pair-wise will be fewer.

---

### Step 2: Arrange Parameters from Most Values to Fewest

Rearrange the table so that the **leftmost column has the parameter with the most values**.

**After rearranging:**

| Screen (3 values) | Orientation (2 values) | Keyboard (2 values) |
|:---:|:---:|:---:|
| Large | Portrait | QWERTY |
| Small | Landscape | 12Key |
| Normal | | |

This is a **heuristic** (a practical rule of thumb). It doesn't guarantee the optimal solution but works well in practice.

---

### Step 3: Create the First Pair — Combine the First Two Parameters

Take the first parameter (most values) and the second parameter. Create rows such that **each value of the first parameter is matched with each value of the second parameter**.

Since Screen has 3 values and Orientation has 2 values, we need 3 × 2 = 6 rows:

| Screen | Orientation |
|:---:|:---:|
| Large | Portrait |
| Large | Landscape |
| Small | Portrait |
| Small | Landscape |
| Normal | Portrait |
| Normal | Landscape |

This ensures every pair of (Screen, Orientation) values is covered.

---

### Step 4: Add the Third Variable — Fill Alternately

Now add the third parameter (Keyboard) by writing its values **alternately** (cycling through them) down the rows:

| Screen | Orientation | Keyboard |
|:---:|:---:|:---:|
| Large | Portrait | QWERTY |
| Large | Landscape | 12Key |
| Small | Portrait | QWERTY |
| Small | Landscape | 12Key |
| Normal | Portrait | QWERTY |
| Normal | Landscape | 12Key |

---

### Step 5: Verify All Pairs Are Covered

Now **manually check** that every pair of values between the 2nd and 3rd parameters (Orientation & Keyboard) and between 1st and 3rd parameters (Screen & Keyboard) are present.

**Check Orientation & Keyboard pairs:**

| Pair Needed | Present? |
|:---:|:---:|
| (Portrait, QWERTY) | Yes — Row 1 |
| (Portrait, 12Key) | Need to check... Row 3 has (Portrait, QWERTY) — **Missing!** |
| (Landscape, QWERTY) | Need to check... Row 2 has (Landscape, 12Key) — **Missing!** |
| (Landscape, 12Key) | Yes — Row 2 |

Wait — actually looking at the **corrected slide** from the transcript, the final table after adjustment is:

| Screen | Orientation | Keyboard |
|:---:|:---:|:---:|
| Large | Portrait | QWERTY |
| Large | Landscape | 12Key |
| Small | Portrait | **12Key** |
| Small | Landscape | **QWERTY** |
| Normal | Portrait | QWERTY |
| Normal | Landscape | 12Key |

Some values in the Keyboard column were **adjusted** so that all pairs between Orientation and Keyboard are covered.

After adjustment:
- (Portrait, QWERTY) ✓ Row 1
- (Portrait, 12Key) ✓ Row 3
- (Landscape, QWERTY) ✓ Row 4
- (Landscape, 12Key) ✓ Row 2

Also check Screen & Keyboard pairs:
- (Large, QWERTY) ✓ Row 1
- (Large, 12Key) ✓ Row 2
- (Small, QWERTY) ✓ Row 4
- (Small, 12Key) ✓ Row 3
- (Normal, QWERTY) ✓ Row 5
- (Normal, 12Key) ✓ Row 6

All pairs covered!

---

### Step 6: Remove Duplicate Test Cases (If Any)

If there are any duplicate rows, remove them to reduce the test set further.

---

### 🎯 Exam Important Points for the Manual Algorithm

> 1. **Step 1:** Identify variables and their values.
> 2. **Step 2:** Arrange parameters from **greatest to least** number of values (leftmost = most values).
> 3. **Step 3:** Create all combinations of the first two parameters.
> 4. **Step 4:** Add the third parameter by writing values **alternately** (cycling).
> 5. **Step 5:** **Manually verify** all pairs are present. If any pair is missing, **adjust values** in existing rows or **add new rows**.
> 6. **Step 6:** Remove duplicate test cases.
> 7. This is a **heuristic method** — it does **NOT guarantee the optimal** (minimum) number of test cases.
> 8. You must **manually check** if all pairs are present.

---

## Concept 12: Tools for Pair-wise Test Case Generation

### 🧠 Simple Explanation

Since manually generating pair-wise test cases is tedious and error-prone, there are **small tools** available that do this automatically. You just give them the number of parameters and what values each can take, and they output the test cases.

### Tools Mentioned in the Transcript

1. **PICT** — Runs on Windows (and possibly Linux)
2. **Jenny** — Open source C program (small, around 100–200 lines of code, download and compile)
3. **AllPairs** — Another tool for generating pair-wise test cases

### 🎯 Exam Important Points

> - Tools exist for automatic pair-wise test case generation: **PICT, Jenny, AllPairs**.
> - Different tools **may produce different numbers** of test cases for the same inputs (because they use different algorithms internally).
> - Most of these tools are **small and simple** — many don't even have a GUI (graphical interface), they use a **text/command-line interface**.
> - Finding the **optimal number** of pair-wise test cases is computationally hard; tools give **near-optimal** results.

---

## Concept 13: Pair-wise Testing — Overall Summary

| Aspect | Detail |
|---|---|
| **Type of testing** | Black-box testing (combinatorial) |
| **When to use** | When number of input parameters is large |
| **Core idea** | Most faults are caused by interaction of 1–2 parameters |
| **t-way fault** | A fault caused by specific values of t parameters |
| **Pair-wise (2-way)** | Covers all pairs → catches ~80–90% of bugs |
| **3-way** | Catches ~90%+ bugs |
| **5-way** | Catches virtually all bugs |
| **Manual method** | Heuristic: arrange, combine, fill alternately, verify, adjust |
| **Optimal generation** | Very hard problem; use tools |
| **Tools** | PICT, Jenny, AllPairs |
| **System assumption** | System is state-independent |

---

## ⚠️ Common Confusions (Based on Transcript)

1. **Pair-wise ≠ Exhaustive Testing.** Pair-wise covers all pairs of two parameters, NOT all possible combinations of all parameters. That's the whole point — it's a smart reduction.

2. **The pair-wise test set size is NOT calculated by a formula.** It is generated by tools/algorithms. Different tools may give different sizes.

3. **The manual algorithm is a heuristic.** It doesn't guarantee the minimum number of test cases. You must manually verify and may need to add rows.

4. **"State-independent" is an important assumption.** Pair-wise testing assumes the system behaves the same regardless of past inputs. If the system has state, the problem becomes much more complex.

5. **Don't confuse pair-wise testing with decision table testing.** Decision tables list explicit rules for combinations of conditions. Pair-wise testing is for situations where the number of combinations is too large for a decision table.

---

## 📝 10 MCQs — Strictly from Lecture 9

---

### Q1. What type of testing technique is pair-wise testing?

**(A)** White-box testing
**(B)** Black-box testing
**(C)** Grey-box testing
**(D)** Unit testing

**Answer: (B)**
**Explanation:** The transcript clearly states that pair-wise testing is a **black-box testing** technique. It is a form of combinatorial testing applied when the number of inputs is large. No code/internal structure is examined.

---

### Q2. What is the main assumption about the system under test in pair-wise testing?

**(A)** The system must have a GUI
**(B)** The system must be written in Java
**(C)** The system is state-independent
**(D)** The system must have fewer than 10 inputs

**Answer: (C)**
**Explanation:** The transcript explicitly states that the system S is assumed to be **state-independent** — it behaves the same way regardless of history, depending only on the current combination of input values.

---

### Q3. According to the lecture, most software faults are caused by interaction of how many parameters?

**(A)** All parameters together
**(B)** At most 1 or 2 parameters
**(C)** Exactly 10 parameters
**(D)** At least half the parameters

**Answer: (B)**
**Explanation:** The transcript says that experimentally it has been shown that almost all bugs are found by considering combinations of **2 (or at most 3)** parameters. A majority of faults are simple (1-way) and pair-wise (2-way) faults.

---

### Q4. If there are 7 Boolean inputs, what is the size of the pair-wise test set (as given in the lecture)?

**(A)** 128
**(B)** 7
**(C)** 8
**(D)** 14

**Answer: (C)**
**Explanation:** The table in the transcript shows: 7 inputs, each with 2 values → exhaustive = 2^7 = 128 combinations, but the **pair-wise test set size is 8**. This is a massive reduction from 128 to just 8.

---

### Q5. What is a "single-mode bug" (1-way fault)?

**(A)** A bug that occurs only when all parameters are set to specific values
**(B)** A bug caused by the setting of a single parameter, regardless of all other parameters
**(C)** A bug that appears only in the first test case
**(D)** A bug found only by white-box testing

**Answer: (B)**
**Explanation:** The transcript defines a single-mode bug as one where **one specific parameter** being set to a certain value causes the problem, irrespective of the settings of all other parameters. The printer duplex example illustrates this.

---

### Q6. In the manual pair-wise test generation algorithm, what is the FIRST step?

**(A)** Run the PICT tool
**(B)** Identify all variables and their values
**(C)** Write all possible combinations
**(D)** Delete duplicate test cases

**Answer: (B)**
**Explanation:** The transcript clearly states that the first step is to **identify what the variables are** and what values they can take. Only then do you proceed to arrange, combine, and generate test cases.

---

### Q7. In the manual algorithm, how should the parameters be arranged?

**(A)** Alphabetically
**(B)** Randomly
**(C)** From the parameter with the greatest number of values to the least
**(D)** From the parameter with the least number of values to the greatest

**Answer: (C)**
**Explanation:** The transcript explicitly says to rearrange the table so that the **leftmost column has the parameter with the largest number of possible values** (greatest to least). This is a heuristic for the manual method.

---

### Q8. Which of the following is a tool for generating pair-wise test cases mentioned in the lecture?

**(A)** Selenium
**(B)** JUnit
**(C)** Jenny
**(D)** Eclipse

**Answer: (C)**
**Explanation:** The transcript mentions three tools: **PICT**, **Jenny** (an open-source C program), and **AllPairs**. Selenium, JUnit, and Eclipse are not mentioned in this lecture.

---

### Q9. Finding the minimum (optimal) number of pair-wise test cases is:

**(A)** Very easy and can be done by a simple formula
**(B)** A very hard computational problem
**(C)** Always exactly n × 2 where n is the number of parameters
**(D)** Impossible even with tools

**Answer: (B)**
**Explanation:** The transcript states that finding the optimal number of pair-wise test cases is a **very hard problem**. Different tools may produce different numbers of test cases depending on the algorithm used. Techniques like **hill climbing, genetic algorithms**, and other combinatorial optimization methods are used to get near-optimal results.

---

### Q10. For 40 inputs each taking 3 values, the exhaustive number of combinations is 1.2 × 10^19. What is the pair-wise test set size as given in the lecture?

**(A)** 40
**(B)** 100
**(C)** 21
**(D)** 1,000

**Answer: (C)**
**Explanation:** The table in the transcript shows that for 40 inputs with 3 values each, the exhaustive combinations = 1.2 × 10^19 (an astronomically large number), but the **pair-wise test set size is only 21** — a dramatic and practical reduction.

---

## 🔑 Final Revision Checklist for Lecture 9

- [ ] Pair-wise testing is a **black-box, combinatorial** technique
- [ ] Used when the number of inputs is **large**
- [ ] System must be **state-independent**
- [ ] **t-way fault** = fault caused by interaction of t parameters
- [ ] 1-way = single-mode, 2-way = double-mode (pair-wise), 3+ = multi-mode
- [ ] Most bugs caught with **2-way (pair-wise)** testing (~80–90%)
- [ ] Up to **5-way** catches virtually all bugs
- [ ] **Dramatic reduction**: 7 Boolean inputs → 128 exhaustive vs 8 pair-wise
- [ ] **Manual algorithm**: Identify → Arrange (descending values) → Combine first two → Add next alternately → Verify all pairs → Adjust → Remove duplicates
- [ ] Manual method is a **heuristic**, not optimal
- [ ] **Tools**: PICT, Jenny, AllPairs
- [ ] Different tools may give **different numbers** of test cases
- [ ] Optimal pair-wise generation is a **computationally hard** problem
