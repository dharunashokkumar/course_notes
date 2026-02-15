# Lecture 11 — MC/DC Testing (Modified Condition/Decision Coverage)

**Course:** NPTEL Software Testing | **Instructor:** Prof. Rajib Mall, IIT Kharagpur

---

## Topics Covered in This Lecture

1. Recap: Basic Condition Testing (Simple Condition Coverage)
2. Does Basic Condition Coverage Subsume Decision Coverage?
3. Decision Testing is a Weak Criterion
4. Types of Condition Testing (Overview)
5. Condition/Decision Coverage
6. Multiple Condition Coverage (MCC)
7. Shortcomings of Condition Testing
8. Short-Circuit Evaluation
9. Exponential Complexity Problem with MCC
10. Introduction to MC/DC (Modified Condition/Decision Coverage)
11. Coverage Criteria Subsumption Hierarchy
12. What is MC/DC? — Definition, Main Idea, and Test Case Count

---

## 1. Recap: Basic Condition Testing (Simple Condition Coverage)

### 📌 What is it?

Before this lecture begins MC/DC, the professor does a quick recap of what was covered in the previous lecture — **basic condition testing** (also called simple condition coverage).

In any program, you will find `if` statements that contain **conditional expressions**. These conditional expressions can be made up of multiple smaller parts. Each smaller part is called an **atomic condition**.

**Example:**

```
if (a > 10 && b < 50)
```

Here there are **two atomic conditions**:
- `a > 10` — this is the first atomic condition
- `b < 50` — this is the second atomic condition

The full expression `(a > 10 && b < 50)` is the **decision** (also called the **branch expression**).

### 🧠 What does basic condition testing require?

Basic condition testing says: for **each atomic condition**, your test cases must make it evaluate to **TRUE at least once** and **FALSE at least once**.

You don't care about all possible combinations. You just care that individually, every atomic condition gets both values.

### 🛠 Example from Transcript

For `if (a > 10 && b < 50)`:

| Test Case | a   | b   | a > 10 | b < 50 | Full Decision |
|-----------|-----|-----|--------|--------|---------------|
| TC1       | 15  | 30  | TRUE   | TRUE   | TRUE          |
| TC2       | 5   | 60  | FALSE  | FALSE  | FALSE         |

- In TC1: first atomic condition is TRUE, second is TRUE
- In TC2: first atomic condition is FALSE, second is FALSE

So both atomic conditions have taken TRUE and FALSE values → **Basic condition coverage achieved.**

### 🎯 Exam Key Point

Basic condition coverage focuses on **individual atomic conditions** getting TRUE and FALSE. It does **not** worry about all possible combinations of atomic conditions.

---

## 2. Does Basic Condition Coverage Subsume Decision Coverage?

### 📌 What is this question about?

"Subsumption" means: if you achieve coverage A, does it automatically guarantee that coverage B is also achieved? If yes, then A **subsumes** B.

The question here is: **If you achieve basic condition coverage, do you automatically get decision coverage too?**

### 🧠 The Answer: NO

The transcript gives a very clear answer: **Basic condition coverage does NOT always subsume decision coverage.**

Sometimes the test cases that achieve basic condition coverage will also achieve decision coverage — but that is just lucky. It is **not guaranteed**.

### 🛠 Example from Transcript (where it fails)

For `if (a > 10 && b < 50)`:

| Test Case | a   | b   | a > 10 | b < 50 | Full Decision |
|-----------|-----|-----|--------|--------|---------------|
| TC1       | 15  | 60  | TRUE   | FALSE  | FALSE         |
| TC2       | 5   | 30  | FALSE  | TRUE   | FALSE         |

- First atomic condition: TRUE in TC1, FALSE in TC2 → covered
- Second atomic condition: FALSE in TC1, TRUE in TC2 → covered
- **Basic condition coverage is achieved**

BUT look at the full decision column — it is FALSE in both test cases! The decision **never becomes TRUE**. So **decision coverage is NOT achieved**.

### 🎯 Exam Key Point

> **Basic condition coverage does NOT subsume decision coverage.** You can have all atomic conditions take TRUE and FALSE individually, but the overall decision might not toggle between TRUE and FALSE.

### ⚠️ Common Confusion

Students often assume: "If I tested each atomic condition for TRUE and FALSE, the overall decision must also get TRUE and FALSE." This is wrong. The combination matters for the decision outcome, and basic condition coverage does not control combinations.

---

## 3. Decision Testing is a Weak Testing Criterion

### 📌 What does this mean?

The professor clearly states that **decision testing (also called branch testing) is a weak testing criterion**.

Decision testing only requires the entire conditional expression to evaluate to TRUE at least once and FALSE at least once. It does not check what is happening with the individual atomic conditions inside the expression.

### 🧠 Why is it weak?

Imagine you have `if (a > 10 && b < 50 && c == 0)`. Decision testing only needs two test cases — one making the whole thing TRUE and one making it FALSE. But inside, there might be bugs related to individual conditions (like `c == 0`) that never get tested properly.

### 🎯 Exam Key Point

> Decision testing is weak because it only checks the overall result of the expression, not the individual atomic conditions.

This is why we need **condition testing** — to go deeper and test the atomic conditions.

---

## 4. Types of Condition Testing (Overview)

### 📌 The Four Types

The transcript lists the following types of condition testing:

1. **Basic Condition Coverage** — Each atomic condition evaluates to TRUE and FALSE
2. **Basic Condition/Decision Coverage** (also called Condition/Decision Coverage) — Achieves basic condition coverage AND also ensures decision coverage
3. **Multiple Condition Coverage (MCC)** — All possible combinations of truth values of atomic conditions are tested
4. **Modified Condition and Decision Coverage (MC/DC)** — Each condition independently shown to affect the decision outcome

### 🎯 Exam Key Point

Know all four types and their names. The lecture progressively builds from the weakest (basic condition) to the strongest practical technique (MC/DC).

---

## 5. Condition/Decision Coverage

### 📌 What is it?

Since basic condition coverage does not guarantee decision coverage, we need a stronger criterion that **combines both**.

**Condition/Decision Coverage** requires:
- Each atomic condition must take TRUE and FALSE values (this is the condition coverage part)
- The entire decision must also evaluate to TRUE and FALSE (this is the decision coverage part)

### 🧠 Simple Explanation

Think of it as: "I want the best of both worlds — I want every small piece (atomic condition) tested for TRUE and FALSE, AND I also want the full expression tested for TRUE and FALSE."

### 🛠 Example from Transcript

For `if (a > 50 or b < 30)`:

**First attempt — achieves basic condition coverage only:**

| Test Case | a   | b   | a > 50 | b < 30 | Full Decision |
|-----------|-----|-----|--------|--------|---------------|
| TC1       | 70  | 50  | TRUE   | FALSE  | TRUE          |
| TC2       | 30  | 20  | FALSE  | TRUE   | TRUE          |

- Atomic conditions: a > 50 gets TRUE and FALSE ✓, b < 30 gets FALSE and TRUE ✓
- But the full decision is TRUE in both cases! Decision coverage NOT achieved.

**Fix — add one more test case for condition/decision coverage:**

| Test Case | a   | b   | a > 50 | b < 30 | Full Decision |
|-----------|-----|-----|--------|--------|---------------|
| TC1       | 70  | 50  | TRUE   | FALSE  | TRUE          |
| TC2       | 30  | 20  | FALSE  | TRUE   | TRUE          |
| TC3       | 20  | 50  | FALSE  | FALSE  | FALSE         |

Now the decision gets both TRUE and FALSE → **Condition/Decision coverage is achieved.**

### 🎯 Exam Key Point

> Condition/Decision coverage may require **additional test cases** beyond what basic condition coverage needs, to ensure the full decision also toggles.

---

## 6. Multiple Condition Coverage (MCC)

### 📌 What is it?

Multiple Condition Coverage is the **strongest** of the condition testing techniques discussed so far. It requires that **all possible combinations** of truth values of the atomic conditions are exercised.

### 🧠 Simple Explanation

If you have 2 atomic conditions, each can be TRUE or FALSE. So the possible combinations are:
- TRUE, TRUE
- TRUE, FALSE
- FALSE, TRUE
- FALSE, FALSE

That is **4 combinations = 2² test cases**.

If you have 3 atomic conditions, you need **2³ = 8 test cases**.

For **n atomic conditions**, you need **2ⁿ test cases**.

### 🛠 Example from Transcript

For `if (a > 50 or b < 30)`:

| Test Case | a   | b   | a > 50 | b < 30 | Combination    |
|-----------|-----|-----|--------|--------|----------------|
| TC1       | 30  | 20  | FALSE  | TRUE   | F, T           |
| TC2       | 30  | 40  | FALSE  | FALSE  | F, F           |
| TC3       | 60  | 20  | TRUE   | TRUE   | T, T           |
| TC4       | 60  | 40  | TRUE   | FALSE  | T, F           |

All 4 combinations are covered → **Multiple Condition Coverage achieved.**

### 🎯 Exam Key Points

- MCC is the **strongest** condition testing technique.
- MCC ensures both **condition coverage** and **decision coverage** automatically.
- MCC requires **2ⁿ test cases** for n atomic conditions.
- The formula **2ⁿ** is critical for the exam.

---

## 7. Shortcomings of Condition Testing

### 📌 Two Major Problems

The professor identifies **two key shortcomings** of the condition testing techniques (particularly MCC):

### Problem 1: Compiler-Dependent Condition Evaluation (Short-Circuit Evaluation)

The way the condition expression is evaluated can vary from **compiler to compiler**. Compilers use **short-circuit evaluation**, which means they may not evaluate all atomic conditions.

This means even if you design test cases to set certain atomic conditions to specific values, the **compiler might never even check** those conditions!

### Problem 2: Dependencies Among Variables

Sometimes, certain combinations of truth values for atomic conditions are **impossible** due to the nature of the variables.

**Example from Transcript:**

```
if ((chr == 'A') || (chr == 'E'))
```

Here, `chr == 'A'` and `chr == 'E'` **cannot both be TRUE at the same time** — a character cannot be 'A' and 'E' simultaneously. So the combination (TRUE, TRUE) is **impossible**.

This means MCC cannot be fully achieved because some combinations are logically impossible.

### 🎯 Exam Key Points

- Short-circuit evaluation is **compiler-dependent** — different compilers may do it differently.
- Variable dependencies can make some truth value combinations **impossible**.
- These are reasons why MCC is **not always practical**.

---

## 8. Short-Circuit Evaluation

### 📌 What is Short-Circuit Evaluation?

Short-circuit evaluation is a technique used by compilers where they **stop evaluating** a conditional expression as soon as the result is determined, without checking the remaining conditions.

### 🧠 How Does It Work?

**Rule for AND (&&):**
- If the first condition is **FALSE**, the compiler **does not check** the second condition.
- Why? Because FALSE AND anything = FALSE. The result is already known.

**Example:** `if (a > 30 && b < 50)`
- If `a > 30` is FALSE → compiler skips `b < 50` entirely. The whole expression is FALSE.

**Rule for OR (||):**
- If the first condition is **TRUE**, the compiler **does not check** the second condition.
- Why? Because TRUE OR anything = TRUE. The result is already known.

**Example:** `if (a > 30 || b < 50)`
- If `a > 30` is TRUE → compiler skips `b < 50` entirely. The whole expression is TRUE.

### 🎯 Exam Key Points

- For **AND**: short-circuit happens when the first condition is **FALSE** (skip the rest).
- For **OR**: short-circuit happens when the first condition is **TRUE** (skip the rest).
- Short-circuit evaluation **varies from compiler to compiler** — you cannot assume all compilers behave the same way.
- Because of short-circuit evaluation, some test cases become **redundant** — the values you assign to later conditions **don't matter** because the compiler never checks them.

### ⚠️ Common Confusion

Students sometimes think: "I set b to TRUE, so it will be checked." But if the compiler short-circuits, `b` is never evaluated at all. Your test case for `b` has no effect in that case.

---

## 9. Exponential Complexity Problem with MCC

### 📌 The Core Problem

Even though MCC is the strongest testing, it has a **huge practical problem**: the number of test cases grows **exponentially**.

For **n atomic conditions** in a branch expression, you need **2ⁿ test cases**.

### 🧠 Why is this a Problem?

| n (atomic conditions) | 2ⁿ (test cases needed) |
|----------------------|------------------------|
| 2                    | 4                      |
| 3                    | 8                      |
| 5                    | 32                     |
| 10                   | 1,024                  |
| 20                   | 1,048,576              |
| 30                   | ~1 billion             |

The transcript specifically says: if n is 10, we need 1024 test cases. If n is 20 or 30, the number becomes impractical.

### 📌 Where Do Large Expressions Occur?

The professor mentions that **embedded controller applications** commonly have conditional expressions involving **10, 15, or even 20 or 30 variables**. In these systems, various parameters are sensed and actions are taken based on combinations of variable values.

### 🎯 Exam Key Points

- MCC is **meaningful only for 2 or 3 atomic conditions**.
- For larger expressions, MCC is **impractical** due to exponential growth.
- This motivates the need for **MC/DC** — which achieves similar bug detection with **linear** test cases.

### 📌 Example from Transcript: 5 Atomic Conditions

The expression `(((a || b) && c) || d) && e` has 5 atomic conditions.

- Without short-circuit: **2⁵ = 32 test cases** needed.
- With short-circuit evaluation: only **13 test cases** remain meaningful (the other 19 become redundant because of don't-care conditions marked as dashes).
- Short-circuit evaluation reduces the count, but **not always** to a manageable level, and it **varies by compiler**.

---

## 10. Introduction to MC/DC (Modified Condition/Decision Coverage)

### 📌 What is MC/DC?

MC/DC stands for **Modified Condition / Decision Coverage**. It is a condition coverage technique that was developed to solve the exponential test case problem of MCC.

### 🧠 Motivation

The professor asks: "Can we achieve as much fault detection capability as MCC with a reduced number of test cases — possibly **linear** in the number of atomic conditions?"

The answer is **yes — that is MC/DC**.

### 📌 Key Importance

The professor states that MC/DC has become an **important testing technique** and many standards, especially for **safety-critical software**, require MC/DC coverage. This means if you are building software for aircraft, medical devices, nuclear systems, etc., MC/DC coverage is often **mandatory**.

### 📌 Main Idea of MC/DC

MC/DC says: **test the important combinations of conditions, not all combinations**.

What are "important" combinations? Those where **each basic condition independently affects the outcome of the decision**.

This means:
- For any one atomic condition, **keep all other conditions constant** (same values).
- **Change only that one atomic condition** from TRUE to FALSE (or vice versa).
- The **decision outcome must change** (toggle from TRUE to FALSE or FALSE to TRUE).

If you can show this for **every** atomic condition, then MC/DC coverage is achieved.

### 📌 Three Requirements of MC/DC

To achieve MC/DC coverage, you must satisfy three requirements:

**Requirement 1:** Each basic condition must be made to evaluate to both **TRUE** and **FALSE** (just like basic condition coverage).

**Requirement 2:** When one atomic condition changes (from TRUE to FALSE or vice versa), all **other atomic conditions must remain the same** — their values should be held constant.

**Requirement 3:** The **compound condition (decision) as a whole** must evaluate to **TRUE for one test case** and **FALSE for the other** when only that one atomic condition changes.

### 🛠 How to Think About It

Imagine you have `if (A && B && C)`.

To test the independent effect of condition A:
- Set B and C to some fixed values.
- Make A = TRUE → note the decision result.
- Make A = FALSE → note the decision result.
- The decision result must be **different** in the two cases.

Repeat this process for B (keeping A and C constant) and for C (keeping A and B constant).

### 🎯 Exam Key Points

- MC/DC = Modified Condition/Decision Coverage.
- Main idea: Each condition **independently affects** the outcome.
- Keep other conditions **constant**, change one condition, and the decision must **toggle**.
- Three requirements: (1) each condition gets T and F, (2) other conditions held constant, (3) decision outcome toggles.
- MC/DC is mandated for **safety-critical software**.

---

## 11. Coverage Criteria Subsumption Hierarchy

### 📌 What is the Hierarchy?

The transcript presents a clear **subsumption hierarchy** (from weakest to strongest):

```
Statement Coverage (weakest)
    ↑
Decision / Branch Coverage
    ↑
Condition/Decision Coverage
    ↑
MC/DC (Modified Condition/Decision Coverage)
    ↑
Multiple Condition Coverage — MCC (strongest)
```

### 🧠 What Does This Mean?

- If you achieve **MCC**, you automatically get MC/DC, Condition/Decision, Decision, and Statement coverage — you don't need to do them separately.
- If you achieve **MC/DC**, you automatically get Condition/Decision, Decision, and Statement coverage.
- Each higher level **subsumes** all the levels below it.

### 📌 The Practical Trade-off

| Criterion             | Strength       | Test Cases Required | Practical?       |
|-----------------------|----------------|---------------------|------------------|
| Statement Coverage     | Weakest        | Very few            | Yes              |
| Decision Coverage      | Weak           | Few                 | Yes              |
| Condition/Decision     | Moderate       | Moderate            | Yes              |
| MC/DC                  | Strong         | **Linear (≈ n+1)**  | **Yes**          |
| MCC                    | Strongest      | **Exponential (2ⁿ)**| **No (for large n)** |

### 🎯 Exam Key Points

- MC/DC is **stronger** than Condition/Decision coverage.
- MC/DC is **weaker** than MCC.
- But MC/DC has **almost the same bug-detection effectiveness** as MCC.
- MC/DC requires only **linear** test cases — approximately **2×n** or **n+1** (the transcript mentions both "at least twice as much as n" and that it is linear).
- MCC requires **exponential (2ⁿ)** test cases — impractical for large n.

---

## 12. What is MC/DC? — Definition and Key Properties

### 📌 Formal Definition (as per Transcript)

**MC/DC** stands for **Modified Condition / Decision Coverage**.

**Terminology:**
- **Condition** = Atomic conditions in the expression (e.g., `a > 10`, `b < 50`)
- **Decision** = The complete conditional expression that controls program flow (e.g., the entire `if` expression)

**Main Idea:** Each condition must be shown to **independently affect** the outcome of a decision. The outcome of a decision changes as a result of changing a **single condition** while keeping all others constant.

### 📌 Number of Test Cases

The transcript explicitly states:
- The number of test cases is **at least twice** the number of atomic conditions.
- It is **linear** — not exponential like MCC.
- It is **not like 2ⁿ**, it will be approximately **2×n** or near about that.

### 📌 Bug Detection Effectiveness

The professor states that **experimentally**, MC/DC has been shown to have **almost as much bug detection effectiveness** as MCC, but with far fewer test cases.

### 🎯 Exam Key Points

- MC/DC = linear test cases, near-MCC effectiveness → **best practical choice**.
- MC/DC is **mandated by many standards** for safety-critical systems.
- Number of test cases ≈ **linear in n** (approximately 2n or n+1).
- If you do MC/DC testing, you do NOT need to worry about condition/decision coverage or basic condition coverage separately — MC/DC **subsumes** them.

---

## Quick Revision Summary

| Concept | Key Takeaway |
|---------|-------------|
| Basic Condition Coverage | Each atomic condition → TRUE and FALSE individually |
| Does it subsume Decision Coverage? | **NO** — decision may not toggle |
| Decision Testing | Weak criterion — only checks full expression |
| Condition/Decision Coverage | Combines both: atomic conditions + decision toggle |
| Multiple Condition Coverage (MCC) | All combinations → 2ⁿ test cases → strongest but impractical |
| Short-Circuit Evaluation | Compiler skips evaluating conditions when result is already known |
| Short-circuit for AND | First condition FALSE → skip rest |
| Short-circuit for OR | First condition TRUE → skip rest |
| Exponential Problem | 2ⁿ grows too fast for n > 3 |
| Embedded Controllers | Common example of large n (10-30 atomic conditions) |
| MC/DC | Each condition independently affects decision → linear test cases |
| MC/DC Requirements | (1) T/F for each condition, (2) others held constant, (3) decision toggles |
| MC/DC vs MCC | MC/DC ≈ same bug detection, but linear test cases |
| Subsumption Hierarchy | Statement < Decision < Cond/Decision < MC/DC < MCC |
| MC/DC in Standards | Required for **safety-critical software** |

---

## 10 MCQs — Lecture 11 (Strictly from Transcript)

---

### Q1. What does basic condition coverage require?

A) All possible combinations of atomic conditions to be tested
B) Each atomic condition to evaluate to both TRUE and FALSE
C) The entire decision to evaluate to TRUE and FALSE
D) Only one atomic condition to be tested

**Answer: B**

**Explanation:** Basic condition coverage requires that each atomic condition individually takes both TRUE and FALSE values during testing. It does not require all combinations (that is MCC) and it does not specifically require the decision to toggle (that is decision coverage).

---

### Q2. Does basic condition coverage subsume decision coverage?

A) Yes, always
B) Yes, but only for OR expressions
C) No, basic condition coverage does not guarantee decision coverage
D) No, but decision coverage subsumes basic condition coverage

**Answer: C**

**Explanation:** The transcript clearly states that basic condition coverage does NOT subsume decision coverage. The example showed that both test cases could make the full decision FALSE even though each atomic condition took TRUE and FALSE values individually.

---

### Q3. For the expression `if (a > 50 or b < 30)`, test cases a=70,b=50 and a=30,b=20 achieve basic condition coverage. Why do they NOT achieve condition/decision coverage?

A) Because the atomic conditions don't take TRUE and FALSE
B) Because the decision evaluates to TRUE for both test cases
C) Because we need 4 test cases for any condition coverage
D) Because OR expressions cannot achieve condition/decision coverage

**Answer: B**

**Explanation:** With a=70,b=50 the decision is TRUE (70>50 is TRUE). With a=30,b=20 the decision is also TRUE (20<30 is TRUE). Since the decision never becomes FALSE, decision coverage is not achieved. An additional test case like a=20,b=50 is needed to make the decision FALSE.

---

### Q4. How many test cases are needed for Multiple Condition Coverage (MCC) with n atomic conditions?

A) n + 1
B) 2 × n
C) 2ⁿ
D) n²

**Answer: C**

**Explanation:** MCC requires all possible combinations of truth values. With n atomic conditions, each can be TRUE or FALSE, giving 2ⁿ combinations. The transcript specifically states this and uses the example of 2 conditions needing 4 test cases and 5 conditions needing 32 test cases.

---

### Q5. What is short-circuit evaluation for an AND (&&) expression?

A) If the first condition is TRUE, the compiler skips the rest
B) If the first condition is FALSE, the compiler skips the rest
C) All conditions are always evaluated
D) Only the last condition is evaluated

**Answer: B**

**Explanation:** For AND expressions, if the first condition is FALSE, then FALSE AND anything = FALSE, so the compiler need not evaluate the remaining conditions. The transcript gives the example: `if (a > 30 && b < 50)` — if a > 30 is FALSE, the compiler does not check b < 50.

---

### Q6. What is the main shortcoming of Multiple Condition Coverage?

A) It does not detect any bugs
B) It requires exponential number of test cases (2ⁿ)
C) It does not cover decision outcomes
D) It only works for OR expressions

**Answer: B**

**Explanation:** The transcript states that even though MCC is the strongest testing, for n=10 it needs 1024 test cases, and for n=20 or n=30, it becomes completely impractical. MCC is meaningful only when n is small (2 or 3).

---

### Q7. What does MC/DC stand for?

A) Multiple Condition/Decision Coverage
B) Modified Condition/Decision Coverage
C) Modified Combination/Decision Coverage
D) Multiple Combination/Decision Criteria

**Answer: B**

**Explanation:** MC/DC stands for Modified Condition/Decision Coverage, as explicitly stated multiple times in the transcript. Do not confuse it with MCC (Multiple Condition Coverage).

---

### Q8. What is the main idea of MC/DC testing?

A) Test all possible combinations of conditions
B) Test only TRUE values of all conditions
C) Each condition must independently affect the decision outcome
D) Only test the first and last conditions

**Answer: C**

**Explanation:** The transcript states that in MC/DC, each basic condition must be shown to independently affect the outcome of the decision. You change one condition while keeping others constant, and the decision must toggle.

---

### Q9. In the coverage subsumption hierarchy, which of the following is correct?

A) MC/DC is stronger than MCC
B) Decision coverage is stronger than MC/DC
C) MC/DC is stronger than Condition/Decision coverage but weaker than MCC
D) Statement coverage is the strongest

**Answer: C**

**Explanation:** The hierarchy from the transcript is: Statement < Decision < Condition/Decision < MC/DC < MCC. So MC/DC is stronger than Condition/Decision coverage but weaker than MCC (Multiple Condition Coverage).

---

### Q10. Why is MC/DC preferred over MCC for practical testing?

A) MC/DC detects more bugs than MCC
B) MC/DC has almost the same bug detection effectiveness as MCC but requires only linear test cases
C) MC/DC requires no test cases at all
D) MC/DC is only used for basic programs

**Answer: B**

**Explanation:** The transcript states that experimentally, MC/DC has almost as much bug detection effectiveness as MCC, but the number of test cases is linear in the number of atomic conditions (approximately 2×n or n+1), compared to MCC's exponential 2ⁿ. MC/DC is also mandated by standards for safety-critical software.

---

*End of Lecture 11 — MC/DC Testing. The next lecture (Lecture 12) continues with MC/DC testing examples and test case design.*
