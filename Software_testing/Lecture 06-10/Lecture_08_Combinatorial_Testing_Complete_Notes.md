# Lecture 8: Combinatorial Testing — Complete Study Guide

**Course:** NPTEL Software Testing | **Instructor:** Prof. Rajib Mall, IIT Kharagpur

---

## Overview of This Lecture

This lecture introduces **Combinatorial Testing**, which is a **Black-Box Testing** technique. Before this lecture, you learned about two black-box techniques — Equivalence Class Testing and Special Value (Boundary Value) Testing. Now, Combinatorial Testing is added as the third black-box technique.

This lecture covers three combinatorial testing strategies:

1. Decision Table-Based Testing (DTT) — *explained in full detail*
2. Cause-Effect Graphing — *explained in full detail*
3. Pair-Wise Testing — *introduced briefly, covered fully in next lecture*

---

## Concept 1: Motivation Behind Combinatorial Testing

### 📌 What is the Problem?

In equivalence class testing and special value testing, when the **number of input parameters increases**, it becomes very difficult to design test cases. The behavior of a program is not affected by just one thing — it can be affected by many factors together.

### 🧠 What are these "factors"?

The transcript identifies three types of factors:

**a) Input Parameters** — The direct inputs a user provides to the program.

**b) Environment Configurations** — Settings that change how the program behaves. For example, a program might run in "expert mode", "novice mode", or "moderate mode". In each mode, the program behaves differently.

**c) State Variables** — The internal state of the system that affects behavior. For example, if a book is already "issued out" in a library system, the behavior of the "issue book" function will be different compared to when the book is available.

### 🎯 Why is this a problem?

When you have more than 2 or 3 factors (parameters), and each factor can take multiple values, the total number of combinations becomes **very large**. It becomes impractical to test all possible combinations of all factors.

For example: If you have 20 parameters and each takes 3 values, the number of combinations = 3²⁰ = about 3.5 billion. This is impossible to test manually.

### 🛠 Real-World Example from Transcript

Think of font settings in PowerPoint software. You can set: text font, font style, size, colour, small caps on/off, all caps on/off, superscript on/off, subscript on/off, etc. Each is a factor. For a specific combination (say: font = Body, style = Bold, size = 38, colour = Red, superscript = ON, all caps = ON), the font might get smudged or not display correctly. You need to test such combinations, but testing ALL of them is impractical.

### 🎯 Exam-Important Points

- Combinatorial testing is needed when the number of parameters is large.
- Factors include: input parameters, environment configurations, and state variables.
- Testing all combinations becomes impractical when factors exceed 2–3.
- Boolean variables are especially common in user interfaces and controller applications.

---

## Concept 2: Types of Combinatorial Testing

The transcript lists **three** combinatorial testing strategies:

1. **Decision Table-Based Testing (DTT)**
2. **Cause-Effect Graphing**
3. **Pair-Wise Testing**

All three are **Black-Box** techniques. This lecture explains DTT and Cause-Effect Graphing fully. Pair-wise testing is introduced at the end and will be covered in the next lecture.

---

## Concept 3: Decision Table-Based Testing (DTT)

### 📌 What is it?

Decision Table-Based Testing is a technique where you create a **decision table** from the problem description (the black-box specification). The table captures all possible combinations of input conditions and the actions (outputs) that should result from each combination.

### 🧠 How does it work?

A decision table has four parts:

- **Conditions** — These appear at the **top** of the table. They represent conditions on input parameters (e.g., "Is the printer recognized?" — Yes or No).
- **Actions** — These appear at the **bottom** of the table. They represent what the program should do (outputs).
- **Rules** — Each **column** of the table is called a **rule**. A rule is a specific combination of condition values and the corresponding actions. Each rule becomes one test case.
- **Assumption** — The inputs are assumed to be **independent** of each other.

### 🧠 Key Insight

- **Conditions = Inputs**
- **Actions = Outputs**
- **Rules = Test Cases**

So each column (rule) in the decision table directly gives you one test case with both input values and expected output.

### 🎯 Exam-Important Points

- DTT is applicable to requirements involving **conditional actions** (different actions based on different conditions).
- It can be automatically translated into code.
- Each column is a rule = a test case.
- Assumes independence of inputs.

---

## Concept 4: DTT Example 1 — The Triangle Problem

### 📌 Problem Description

A program reads three sides (a, b, c) of a triangle and outputs:
- "Not a triangle"
- "Scalene"
- "Isosceles"
- "Equilateral"

### 🧠 How to set up the Decision Table

**Conditions identified:**
- C1: a < b + c (triangle inequality check 1)
- C2: b < a + c (triangle inequality check 2)
- C3: c < a + b (triangle inequality check 3)
- C4 (from transcript context): a == b
- C5: a == c
- C6: b == c

**Actions identified:**
- A1: Not a triangle
- A2: Scalene
- A3: Isosceles
- A4: Equilateral
- A5: Impossible (invalid combination)

**Rules (how it works):**
- If **any one** of C1, C2, C3 is **false** → Output is "Not a triangle" (A1).
- If **all** of a==b, b==c, a==c are **true** → Output is "Equilateral" (A4).
- If exactly two are equal → "Isosceles" (A3).
- If none are equal → "Scalene" (A2).

Each combination becomes a column in the decision table, and each column gives test case values for a, b, c along with the expected output.

---

## Concept 5: DTT Example 2 — Printer Diagnosis

### 📌 Problem Description

A printer diagnosis software takes conditions as input and outputs actions (what the user should do).

**Conditions:**
- Printer does not print (Yes/No)
- Red light is flashing (Yes/No)
- Printer is unrecognized (Yes/No)

**Actions (what the software recommends):**
- Check and replace ink
- Check for paper jam
- Check printer-computer cable
- Ensure printer software is installed

### 🧠 How it works

For every possible combination of the three conditions (Yes/No for each), the program outputs different actions. Since each condition is Boolean (Yes/No) and there are 3 conditions, the total number of rules = 2³ = 8 rules = 8 test cases.

The user may enter any combination of conditions, so ALL combinations must be represented in the decision table. Each column (rule) becomes a test case with the input values and expected actions.

---

## Concept 6: DTT Example 3 — In-Flight Meal Policy (Quiz)

### 📌 Problem Description

An airline has this policy:
- If the flight is **more than half-full** AND **ticket cost is more than Rs. 3000**, free meals are served **unless** it is a domestic flight.
- Meals are **charged** on all domestic flights.

### 🧠 Identifying Conditions and Actions

**Conditions (Inputs):**
- C1: Flight is more than half-full? (Yes/No)
- C2: Ticket cost more than Rs. 3000? (Yes/No)
- C3: Is it a domestic flight? (Yes/No)

**Actions (Outputs):**
- Free meal served
- Meal served but charged
- No meal served

### 📊 Step-by-Step Decision Table

Since there are 3 Boolean conditions → Total combinations = 2³ = **8 rules** initially.

Here is the logic from the transcript:

| Rule | Half-full? | >3000? | Domestic? | Action |
|------|-----------|--------|-----------|--------|
| 1 | No | No | No | No meal |
| 2 | No | No | Yes | No meal |
| 3 | No | Yes | No | No meal |
| 4 | No | Yes | Yes | No meal |
| 5 | Yes | No | No | Meal served (not free) |
| 6 | Yes | No | Yes | No meal |
| 7 | Yes | Yes | No | **Free meal** |
| 8 | Yes | Yes | Yes | Meal served (charged) |

### 🧠 Optimization — Reducing Redundant Test Cases

The transcript explains that some rules produce the **same action** regardless of one condition's value. These are called **"don't care" terms**.

For example:
- Rules 1 and 2 produce the same action (No meal) regardless of whether domestic or not, as long as half-full = No and >3000 = No. So they can be **combined** into one test case.
- Similarly, rules 3 and 4 can be combined.
- Rules 6 and 8: As long as it is more than half-full and domestic, whether >3000 or not, meals are served (charged). These combine.

After optimization: We get **4 test cases** instead of 8.

### 🎯 Exam-Important Points

- For **n Boolean parameters**, the total number of test cases = **2ⁿ**.
- Don't care terms allow us to **combine** rules and reduce the number of test cases.
- Reducing redundant columns in the decision table is an important optimization step.

---

## Concept 7: Rules About Decision Tables

### 📌 Two Important Properties

The transcript clearly states two properties that decision table rules MUST satisfy:

**a) Completeness:**
Every possible combination of condition values (including default combinations) must be present in the table. No combination should be missed.

**b) Consistency:**
There should NOT be two different actions for the same combination of conditions. If the same combination of inputs maps to two different outputs, something is wrong in the decision table formulation.

### 🎯 Exam-Important Points

- Rules must be **complete** — all combinations must be covered.
- Rules must be **consistent** — no conflicting actions for the same input.

---

## Concept 8: When to Use Decision Table-Based Testing

### 📌 Guidelines from the Transcript

DTT is **most appropriate** when the program has:

1. **A lot of decision making** — different actions take place depending on input conditions.
2. **Important logical relationships among input variables** — inputs interact with each other.
3. **Calculations involving subsets of input variables** — not all inputs are used for every calculation.
4. **Cause and effect relationships between input and output** — specific inputs "cause" specific outputs.
5. **Complex computation logic** — the logic is not simple.

### ⚠️ Limitation of Decision Tables

**Decision tables do NOT scale up very well.**

If the number of parameters is small (3, 4, 5), we can form the decision table. But if the number of parameters is, say, 20 and each takes 3 values, the number of columns = 3²⁰, which is far too many to handle manually.

### 🎯 Exam-Important Points

- DTT works well for problems with complex decision-making and logical relationships.
- The main limitation is **scalability** — it does not scale up for a large number of parameters.
- This limitation motivates cause-effect graphing and pair-wise testing.

---

## Concept 9: Cause-Effect Graphing

### 📌 What is it?

Cause-Effect Graphing is another combinatorial testing technique. It is essentially a **graphical method to help you develop a decision table** in a systematic way.

### 🧠 Key Idea (from Transcript)

The transcript states this clearly: *"Cause-effect graphing method actually helps us to develop the decision table."* It has a set of **notations** by which, given a problem, we can represent it as a cause-effect graph. Once the graph is ready, there is a **straightforward method** to convert it into a decision table.

### 📌 Terminology

- **Causes** = Inputs (the input conditions/parameters)
- **Effects** = Outputs (the actions/results)
- **Intermediate nodes** = Logical combinations of causes that lead to effects

### 🧠 How does it differ from DTT?

In DTT, you consider **all possible combinations** of input conditions.

In Cause-Effect Graphing, you look at **only the possible/valid ways** in which causes combine to produce effects. This means you don't need to consider every single combination — only the meaningful ones.

This is how it **avoids the combinatorial explosion** problem.

### 🎯 Exam-Important Points

- Cause-effect graphing is a systematic way to develop the decision table.
- Causes = Inputs, Effects = Outputs.
- It avoids combinatorial explosion by only considering valid combinations.
- Once the graph is made, each column of the resulting decision table is a test case.

---

## Concept 10: Cause-Effect Graph Example — Bank Deposit Interest Rate

### 📌 Problem Description

A bank offers interest rates based on two factors: deposit amount and deposit duration.

**If deposit < Rs. 1 Lakh:**
- Deposit up to 1 year → 6% interest
- Deposit over 1 year but less than 3 years → 7% interest
- Deposit 3 years and above → 8% interest

**If deposit >= Rs. 1 Lakh:**
- Deposit up to 1 year → 7% interest
- Deposit over 1 year but less than 3 years → 8% interest
- Deposit 3 years and above → 9% interest

### 📊 Step 1: Identify Causes and Effects

**Causes (Inputs):**
- C1: Deposit duration < 1 year
- C2: 1 year < Deposit duration < 3 years
- C3: Deposit duration >= 3 years
- C4: Deposit amount < 1 Lakh
- C5: Deposit amount >= 1 Lakh

**Effects (Outputs):**
- e1: Interest rate = 6%
- e2: Interest rate = 7%
- e3: Interest rate = 8%
- e4: Interest rate = 9%

### 📊 Step 2: Draw the Cause-Effect Graph

Causes and effects are represented as **circle nodes**. When specific combinations of causes produce an effect, they are connected through **intermediate nodes** (AND/OR logic nodes).

The connections are:

| Cause Combination | Effect |
|---|---|
| C1 (< 1 year) AND C4 (< 1 Lakh) | e1: 6% |
| C2 (1–3 years) AND C4 (< 1 Lakh) | e2: 7% |
| C1 (< 1 year) AND C5 (>= 1 Lakh) | e2: 7% |
| C3 (>= 3 years) AND C4 (< 1 Lakh) | e3: 8% |
| C2 (1–3 years) AND C5 (>= 1 Lakh) | e3: 8% |
| C3 (>= 3 years) AND C5 (>= 1 Lakh) | e4: 9% |

Notice that **e2 (7%)** can be produced by TWO different combinations, and **e3 (8%)** can also be produced by TWO different combinations. This is shown using intermediate AND nodes feeding into OR connections.

### 📊 Step 3: Convert to Decision Table

Once the cause-effect graph is ready, it is a **straightforward translation** to the decision table. You write the conditions (causes), their values, and the corresponding actions (effects). Each valid combination becomes a column = a test case.

### 🎯 Exam-Important Points

- First identify all causes (inputs) and effects (outputs).
- Draw circles for causes and effects.
- Use intermediate nodes (AND logic) to connect causes to effects.
- Multiple cause combinations may produce the same effect.
- Once graph is drawn, convert directly to decision table.
- Each column of the decision table = one test case.

---

## Concept 11: Advantage of Cause-Effect Graphing Over DTT

### 📌 From the Transcript

The transcript explicitly states: Cause-effect graphing *"avoids the exponential combination of test cases that need to be considered in the case of decision table-based testing."*

In DTT, you consider ALL possible combinations of conditions. In cause-effect graphing, you only consider the valid/meaningful combinations that actually produce effects. This reduces the number of test cases.

### ⚠️ But There is Still a Limit

For very complicated problems where input conditions are, say, 50 and each takes two values, even cause-effect graphing can result in a very large number of test cases. 2⁵⁰ is a huge number. This is especially true in **user interfaces** and **embedded controller software** where there are many Boolean conditions.

This motivates the need for **pair-wise testing**.

---

## Concept 12: Introduction to Pair-Wise Testing (Brief)

### 📌 What the Transcript Says

In both decision table-based testing and cause-effect graphing, we were considering **all possible input combinations**. But when the number of parameters is very large (e.g., 50 parameters, each taking 2 values = 2⁵⁰ combinations), this is impossible.

Pair-wise testing is a technique where we do **not** consider all possible combinations. Instead, we test in a smarter way with **fewer test cases** while still achieving adequate testing.

### 🎯 Exam-Important Point

- Pair-wise testing will be covered in the next lecture (Lecture 9).
- It is motivated by the scalability limitation of DTT and cause-effect graphing.
- It does not test all combinations — it uses a reduced, effective set.

---

## Concept 13: E-Commerce Discount Quiz (from Transcript)

### 📌 Problem Description

An e-commerce site gives the following discounts:
- A **member** gets **10% discount** for purchases **lower than Rs. 2000**.
- A member gets **15% discount** for purchases **Rs. 2000 or more**.
- Purchase using **SBI card** gets **5% additional discount**.
- If the purchase amount **after all discounts exceeds Rs. 2000**, then **shipping is free**.

### 🧠 Identifying Conditions and Actions

**Conditions (Inputs):**
- Is the customer a member? (Yes/No)
- Is the purchase amount < Rs. 2000? (Yes/No)
- Is payment by SBI card? (Yes/No)

**Actions (Outputs):**
- What discount rate applies? (0%, 5%, 10%, 15%, or combinations like 10%+5%, 15%+5%)
- Is shipping free? (Yes/No — depends on final amount after discount)

The transcript asks you to form the decision table for this. Each combination of conditions maps to specific discount actions and shipping decision.

### 🎯 Exam-Important Point

This is a practice problem from the lecture. You should be able to identify conditions, actions, form the decision table, and derive test cases from it.

---

## Quick Revision Summary

| Topic | Key Point |
|---|---|
| Why Combinatorial Testing? | Too many parameter combinations to test individually |
| Factors that affect behavior | Input parameters, environment configs, state variables |
| Decision Table Structure | Conditions (top), Actions (bottom), Rules (columns) = Test Cases |
| For n Boolean params | Total test cases = 2ⁿ |
| Optimization in DTT | Combine rules with same actions using don't care terms |
| Rules must be | Complete (all combos covered) AND Consistent (no conflicting actions) |
| DTT Best For | Complex decision-making, logical relationships, cause-effect relationships |
| DTT Limitation | Does not scale well for large number of parameters |
| Cause-Effect Graphing | Systematic way to build the decision table |
| Causes = | Inputs |
| Effects = | Outputs |
| CE Graph Advantage | Avoids combinatorial explosion (only valid combos) |
| CE Graph Limitation | Still large for very many parameters (e.g., 50 Boolean inputs) |
| Pair-Wise Testing | Does not test all combinations; covered in next lecture |

---

## 10 MCQs — Strictly from Lecture 8

---

**Q1.** Combinatorial testing is which type of testing technique?

(A) White-Box Testing
(B) Black-Box Testing
(C) Grey-Box Testing
(D) Unit Testing

**Answer: (B) Black-Box Testing**

*Explanation:* The transcript clearly states that combinatorial testing is a black-box testing technique, just like equivalence class testing and special value testing discussed in previous lectures.

---

**Q2.** Which of the following factors can affect the behavior of a program according to the transcript?

(A) Input parameters only
(B) Input parameters and environment configurations only
(C) Input parameters, environment configurations, and state variables
(D) Only state variables

**Answer: (C) Input parameters, environment configurations, and state variables**

*Explanation:* The transcript explicitly lists all three — input parameters, environment configurations (like expert/novice mode), and state variables (like whether a book is already issued) as factors affecting program behavior.

---

**Q3.** In a decision table, each column is called a:

(A) Condition
(B) Action
(C) Rule
(D) Factor

**Answer: (C) Rule**

*Explanation:* The transcript states: "each of these column here, we call it as a rule." Each rule specifies a combination of conditions and corresponding actions, and each rule becomes a test case.

---

**Q4.** For a decision table with 3 Boolean input conditions, how many test cases (rules) are possible before optimization?

(A) 3
(B) 6
(C) 8
(D) 9

**Answer: (C) 8**

*Explanation:* The transcript states: "for n parameters, if each one is a Boolean parameter, we need 2ⁿ test cases." With n = 3, that gives 2³ = 8 test cases.

---

**Q5.** Which two properties must the rules in a decision table satisfy?

(A) Completeness and Correctness
(B) Completeness and Consistency
(C) Consistency and Efficiency
(D) Correctness and Efficiency

**Answer: (B) Completeness and Consistency**

*Explanation:* The transcript states rules must be "complete" (every combination of values must be present) and "consistent" (no two different actions for the same combination of conditions).

---

**Q6.** What is the main limitation of decision table-based testing?

(A) It cannot handle Boolean conditions
(B) It does not produce test cases
(C) It does not scale up well for a large number of parameters
(D) It only works for numerical inputs

**Answer: (C) It does not scale up well for a large number of parameters**

*Explanation:* The transcript explicitly states: "Decision tables do not scale up very well." For example, 20 parameters with 3 values each gives 3²⁰ columns, which is too many.

---

**Q7.** In Cause-Effect Graphing, "Causes" represent:

(A) Outputs of the program
(B) Inputs of the program
(C) Internal states of the program
(D) Test cases

**Answer: (B) Inputs of the program**

*Explanation:* The transcript clearly defines: "we look at the input and we call them as the causes, and the output as the effect."

---

**Q8.** What is the main advantage of Cause-Effect Graphing over Decision Table-Based Testing?

(A) It produces more test cases
(B) It avoids the combinatorial explosion problem
(C) It does not need a decision table
(D) It works only for Boolean inputs

**Answer: (B) It avoids the combinatorial explosion problem**

*Explanation:* The transcript states cause-effect graphing "avoids the exponential combination of test cases" by only considering valid combinations of causes that produce effects, rather than all possible combinations.

---

**Q9.** In the bank deposit interest rate example, if the deposit amount is less than 1 Lakh and the duration is less than 1 year, the interest rate is:

(A) 7%
(B) 8%
(C) 6%
(D) 9%

**Answer: (C) 6%**

*Explanation:* The transcript states: "If depositing less than Rs. 1 Lakh, rate of interest: 6% for deposit up to 1 year."

---

**Q10.** Why is pair-wise testing needed, as motivated at the end of this lecture?

(A) Because decision tables are always incorrect
(B) Because cause-effect graphing cannot handle Boolean inputs
(C) Because for very large numbers of parameters, even cause-effect graphing produces too many test cases
(D) Because pair-wise testing is a white-box technique

**Answer: (C) Because for very large numbers of parameters, even cause-effect graphing produces too many test cases**

*Explanation:* The transcript explains that for 50 input parameters each taking 2 values, the total combinations = 2⁵⁰, which is a huge number. Both DTT and cause-effect graphing consider all combinations, so pair-wise testing is needed to test effectively with fewer test cases.

---

## What Else is in This Course (Upcoming Lectures)

| Lecture | Topic |
|---|---|
| **Lecture 9** | Pair-Wise Testing (continuation of combinatorial testing) |
| **Lecture 10 onwards** | Further testing techniques as per NPTEL syllabus |

---

*End of Lecture 8 Notes — Combinatorial Testing*
